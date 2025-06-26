import { ExtraHeaders } from '../../shared/interface/extra-headers';
import { Inject, Injectable, Logger } from '@nestjs/common';
import { Categories, ClientApplicationInformation, IPersonalDataServiceQualifier, PersonalDataInterface } from '../../shared/interface/personal-data.interface';
import { ConfigService } from '@nestjs/config';
import { GetServerDrivenUiLayoutUseCase, GetServerDrivenUiLayoutUseCaseQualifier } from '../../sdui/application/get-server-driven-ui-layout.usecase';
import { ModuleConfigurationService } from '../service/module-configuration.service';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import BusinessProcess from '../../shared/common/models/BusinessProcess';
import { FlowConfig } from '../models/module/configuration';
import { SelectDestinationInitResponse } from '../../shared/common/api/model/selectDestinationInitResponse';
import { BranchRequest } from 'src/card-delivery/shared/common/models/BranchRequest';
import { validate } from 'class-validator';
import { IbCatalogServiceInterface } from 'src/card-delivery/shared/interface/ib-catalog-service.interface';
import { IbCatalogService } from 'src/card-delivery/shared/service/ib-catalog-service';
import { LibStepsService } from '../../shared/service/lib-steps.service';
import { LibStepsInterface } from '../../shared/interface/lib-steps.interface';
import { ClientAppDataCategory } from '../../shared/common/enums/client-app-data-category';
import { RetrieveBranchListResponseDto } from 'src/card-delivery/shared/common/api/ib/catalog-service/model/retrieveBranchListResponseDto';
import { BranchDto } from 'src/card-delivery/shared/common/api/ib/catalog-service/model/branchDto';
import { BranchAddressDto } from 'src/card-delivery/shared/common/models/BranchAddressDto';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../shared/common/constants/code-error.constants';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED, TEMP_DATA_TAG } from '../../shared/common/constants/log.constants';

const SelectDestinationInitUseCaseQualifier = Symbol('SelectDestinationInitUseCaseQualifier');

@Injectable()
class SelectDestinationInitUsecase {
  private readonly logger = new Logger(SelectDestinationInitUsecase.name);
  private readonly source: string;

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(GetServerDrivenUiLayoutUseCaseQualifier) private readonly serverDrivenUiUseCase: GetServerDrivenUiLayoutUseCase,
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorService: OrchestratorInterface,
    private readonly moduleConfigurationService: ModuleConfigurationService,
    @Inject(IbCatalogService) private readonly ibCatalogService: IbCatalogServiceInterface,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface,
    configService: ConfigService
  ) {
    this.source = configService.get('CARD_DELIVERY.SELECT_DESTINATION_HEADER_KAFKA');
  }

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<SelectDestinationInitResponse> {
    this.logger.log(`Starting Select Destination Init. requestId: ${requestId}`, BUSINESS_TAG);
    await this.validateHeaders(extraHeaders);
    this.logger.log(`Headers validated. requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorService.init(requestId, this.source);
    if (initResponse.error) {
      this.logger.error(`Orchestration init failed. requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }
    this.logger.log(`Orchestration init succeeded. Flow: ${initResponse.flow}. requestId: ${requestId}`, BUSINESS_TAG);

    const flowConfig: FlowConfig = this.moduleConfigurationService.getFlowConfiguration(initResponse.flow);

    let personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(requestId, this.source);
    this.logger.log(`Fetched personal data. requestId: ${requestId}`, TEMP_DATA_TAG);

    const layoutParams: object = {
      address: {},
      branch: {},
      cardInformation: {}
    };

    personalDataResponse = await this.libStepsService.executeStep({
      stepName: flowConfig.stepName,
      personalDataResponse: personalDataResponse,
      flow: initResponse.flow,
      source: this.source
    });
    this.logger.log(`Executed step: ${flowConfig.stepName}. requestId: ${requestId}`, BUSINESS_TAG);

    await this.saveInitialDeliveryType(personalDataResponse, flowConfig);

    if (flowConfig.hasAddressOption) {
      const residenceAddressCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.RESIDENCE_ADDRESS);
      layoutParams['address'] = residenceAddressCategory.transaction_data;
      this.logger.log(`Added address layout. requestId: ${requestId}`, TEMP_DATA_TAG);
    }

    if (flowConfig.hasBranchOption) {
      const regulatoryCategory: Categories = await this.createOrGetRegulatoryCategory(personalDataResponse, extraHeaders);
      layoutParams['branch'] = regulatoryCategory.transaction_data;
      await this.saveCustomerGeolocData(requestId, extraHeaders, personalDataResponse);
      this.logger.log(`Added branch layout and geo data. requestId: ${requestId}`, TEMP_DATA_TAG);
    }

    layoutParams['cardInformation'] = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.CARD_INFORMATION)?.transaction_data;

    const layout: object = await this.serverDrivenUiUseCase.executeWithParams(requestId, 'select-destination/layout.json', layoutParams);
    this.logger.log(`Layout generated successfully. requestId: ${requestId}`, BUSINESS_TAG);

    return {
      data: {
        layout: layout
      }
    };
  }

  private async saveInitialDeliveryType(personalDataResponse: ClientApplicationInformation, flowConfig: FlowConfig) {
    const cardInformationCategory = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.CARD_INFORMATION);

    if (!cardInformationCategory.transaction_data['delivery_type']) {
      cardInformationCategory.transaction_data['delivery_type'] = flowConfig.initialDeliveryType;
      await this.personalDataService.saveCategory(personalDataResponse.request_id, cardInformationCategory, this.source);
      this.logger.log(`Initial delivery type '${flowConfig.initialDeliveryType}' saved. requestId: ${personalDataResponse.request_id}`, TEMP_DATA_TAG);
    }
  }

  private async createOrGetRegulatoryCategory(personalDataResponse: ClientApplicationInformation, extraHeaders: ExtraHeaders) {
    let regulatoryCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.REGULATORY);
    if (!regulatoryCategory) {
      regulatoryCategory = await this.createRegulatoryCategory(personalDataResponse.request_id, extraHeaders);
    }
    return regulatoryCategory;
  }

  private async createRegulatoryCategory(requestId: string, extraHeaders: ExtraHeaders) {
    let ibCatalogServiceResponse: RetrieveBranchListResponseDto = null;
    let nearestBranch: BranchDto = null;
    try {
      ibCatalogServiceResponse = await this.ibCatalogService.getNearestBranch(extraHeaders);
      nearestBranch = BranchAddressDto.validateBranch(ibCatalogServiceResponse.data);
    } catch (err) {
      nearestBranch = BranchAddressDto.validateBranch(null);
    }

    const residenceAddressCategory: Categories = {
      category_transaction_data: ClientAppDataCategory.REGULATORY,
      transaction_data: {
        branch_code: nearestBranch.branchId,
        branch_name: nearestBranch.branchName,
        branch_address: BranchAddressDto.getBranchAddress(nearestBranch.address)
      }
    };

    await this.personalDataService.saveCategory(requestId, residenceAddressCategory, this.source);
    this.logger.log(`Regulatory category saved. requestId: ${requestId}`, TEMP_DATA_TAG);

    return residenceAddressCategory;
  }

  private async validateHeaders(extraHeaders: ExtraHeaders) {
    let branchRequest = new BranchRequest();
    branchRequest.latitude = extraHeaders.latitude;
    branchRequest.longitude = extraHeaders.longitude;

    await validate(branchRequest).then((errors) => {
      if (errors.length > 0) {
        this.logger.error(`Error validating headers: ${errors}`, BUSINESS_TAG_FAILED);
        throw new Error(`${errors}`);
      }
    });
  }

  private async saveCustomerGeolocData(requestId: string, extraHeaders: ExtraHeaders, personalDataResponse: ClientApplicationInformation) {
    this.logger.log('Start saveCustomerGeolocData', TEMP_DATA_TAG);
    await this.createBasicPersonalCategory(requestId, personalDataResponse, extraHeaders);
    this.logger.log('End saveCustomerGeolocData', TEMP_DATA_TAG);
  }

  private async createBasicPersonalCategory(requestId: string, personalDataResponse: ClientApplicationInformation, extraHeaders: ExtraHeaders) {
    this.logger.log('Start createBasicPersonalCategory', TEMP_DATA_TAG);

    let basicPersonalCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.BASIC_PERSONAL);

    if (!basicPersonalCategory) {
      basicPersonalCategory = {
        category_transaction_data: ClientAppDataCategory.BASIC_PERSONAL,
        transaction_data: {
          location_latitude: extraHeaders.latitude,
          location_longitude: extraHeaders.longitude
        }
      };
    } else {
      basicPersonalCategory.transaction_data['location_latitude'] = extraHeaders.latitude;
      basicPersonalCategory.transaction_data['location_longitude'] = extraHeaders.longitude;
    }

    await this.personalDataService.saveCategory(requestId, basicPersonalCategory, this.source);
    this.logger.log('End createBasicPersonalCategory', TEMP_DATA_TAG);
  }
}

export {
  SelectDestinationInitUsecase,
  SelectDestinationInitUseCaseQualifier
};
