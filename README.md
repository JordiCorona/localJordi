De lo que me has ayudado te he compartido las clases para @Post('/delivery') y te he compartido el execute   return await this.selectDestinationDeliveryUseCaseCommand.execute(requestId, request, extraHeaders) el cual se encuentra en la clase selectDestinationDeliveryUseCaseCommand
requiere que me ayudes a lo mismo con @Get('/init') el cual el .execute se encuentra en esta clase selectDestinationInitUseCaseCommand te comparto la clase, me puedes ayudar con un analisis de este proceso, asi como los Logs ELSM en cada evento por favor


import { Body, Controller, Get, Headers, HttpCode, Inject, Post, Req, Res, UseGuards } from '@nestjs/common';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';
import { SelectDestinationInitUsecase, SelectDestinationInitUseCaseQualifier } from '../application/select-destination-init.usecase';
import { Cookies } from '../../shared/common/decorator/cookies.decorator';
import { ExtraHeaders } from '../../shared/interface/extra-headers';
import { Response } from 'express';
import { SelectDestinationInitResponse } from '../../shared/common/api/model/selectDestinationInitResponse';
import { SelectDestinationDeliveryUsecase, SelectDestinationDeliveryUseCaseQualifier } from '../application/select-destination-delivery.usecase';
import { SelectDestinationDeliveryRequest } from '../../shared/common/api/model/selectDestinationDeliveryRequest';
import { SCOPE_API_READ } from 'src/card-delivery/shared/common/constants/journey-constants';
import { SignatureRequestDto } from '@ib/ib-mx-security-signatures-lib/dto/signatureRequestDto';
import { SelectDestinationDeliveryEnum } from '../../shared/common/enums/select-destination-delivery-enum';

@Controller('v1/select-destination')
@UseGuards(AuthorizationGuard, PermissionsGuard)
class SelectDestinationController {

  constructor(
    @Inject(SelectDestinationInitUseCaseQualifier) private readonly selectDestinationInitUseCaseCommand: SelectDestinationInitUsecase,
    @Inject(SelectDestinationDeliveryUseCaseQualifier) private readonly selectDestinationDeliveryUseCaseCommand: SelectDestinationDeliveryUsecase
  ) {
  }

  @HttpCode(200)
  @Get('/init')
  @PreAuthorize(SCOPE_API_READ)
  async init(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('latitude') latitude: string,
    @Headers('longitude') longitude: string,
    @Cookies('request_id') requestId: string
  ): Promise<SelectDestinationInitResponse> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'latitude': latitude,
      'longitude': longitude,
    };
    return await this.selectDestinationInitUseCaseCommand.execute(requestId, extraHeaders)
  }

  @PreAuthorize(SCOPE_API_READ)
  @Post('/delivery')
  async delivery(
    @Res() response: Response,
    @Body() request: SelectDestinationDeliveryRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('x-otp-token') XOtp: string,
    @Cookies('request_id') requestId: string,
    @Req() req : Request
  ): Promise<void> {
    let extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'x-otp-token': XOtp,
    };
    if(request.source === SelectDestinationDeliveryEnum.SOURCE_NEXT_BUTTON
      && request.selectedOption === SelectDestinationDeliveryEnum.SELECTED_OPTION_ADDRESS){
      const params =  {
        headers: {'x-otp-token':XOtp},
        req: {'cookies': {'request_id': requestId}}
      }
  
      await SignatureRequestDto.validateParams(params);
    }

    extraHeaders.authorization = req.headers['Authorization'];


    return await this.selectDestinationDeliveryUseCaseCommand.execute(requestId, request, extraHeaders)
      .then(result => {
        response.status(302).setHeader('location', result.url);
        response.end();
      })
  }

}

export { SelectDestinationController };





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
    await this.validateHeaders(extraHeaders);

    const initResponse: BusinessProcess = await this.orchestratorService.init(requestId, this.source);
    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const flowConfig: FlowConfig = this.moduleConfigurationService.getFlowConfiguration(initResponse.flow);

    let personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(requestId, this.source);

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
    })

    await this.saveInitialDeliveryType(personalDataResponse, flowConfig)

    if (flowConfig.hasAddressOption) {
      const residenceAddressCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.RESIDENCE_ADDRESS)
      layoutParams['address'] = residenceAddressCategory.transaction_data
    }

    if (flowConfig.hasBranchOption) {
      const regulatoryCategory: Categories = await this.createOrGetRegulatoryCategory(personalDataResponse, extraHeaders)
      layoutParams['branch'] = regulatoryCategory.transaction_data
      await this.saveCustomerGeolocData(requestId, extraHeaders, personalDataResponse);
    }

    layoutParams['cardInformation'] = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.CARD_INFORMATION)?.transaction_data

    const layout: object = await this.serverDrivenUiUseCase.executeWithParams(requestId, 'select-destination/layout.json', layoutParams)

    return {
      data: {
        layout: layout
      }
    }
  }

  private async saveInitialDeliveryType(personalDataResponse: ClientApplicationInformation, flowConfig: FlowConfig) {

    const cardInformationCategory = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.CARD_INFORMATION)

    if (!cardInformationCategory.transaction_data['delivery_type']) {
      cardInformationCategory.transaction_data['delivery_type'] = flowConfig.initialDeliveryType
    }
    await this.personalDataService.saveCategory(personalDataResponse.request_id, cardInformationCategory, this.source)
  }

  private async createOrGetRegulatoryCategory(personalDataResponse: ClientApplicationInformation, extraHeaders: ExtraHeaders) {
    let regulatoryCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.REGULATORY)
    if (!regulatoryCategory) {
      regulatoryCategory = await this.createRegulatoryCategory(personalDataResponse.request_id, extraHeaders);
    }
    return regulatoryCategory
  }

  private async createRegulatoryCategory(requestId: string, extraHeaders: ExtraHeaders) {
    // Go to Bsc Service to get nearest branch
    let ibCatalogServiceResponse:RetrieveBranchListResponseDto = null;
    let nearestBranch:BranchDto = null;
    try{
      ibCatalogServiceResponse = await this.ibCatalogService.getNearestBranch(extraHeaders);
      nearestBranch = BranchAddressDto.validateBranch(ibCatalogServiceResponse.data);
    }catch(err){
      nearestBranch = BranchAddressDto.validateBranch(null);
    }

    const residenceAddressCategory: Categories = {
      category_transaction_data: ClientAppDataCategory.REGULATORY,
      transaction_data: {
        branch_code: nearestBranch.branchId,
        branch_name: nearestBranch.branchName,
        branch_address: BranchAddressDto.getBranchAddress(nearestBranch.address)
      }
    }

    await this.personalDataService.saveCategory(requestId, residenceAddressCategory, this.source)

    return residenceAddressCategory
  }

  private async validateHeaders(extraHeaders: ExtraHeaders){
    let branchRequest = new BranchRequest();

    branchRequest.latitude = extraHeaders.latitude;
    branchRequest.longitude = extraHeaders.longitude;

    await validate(branchRequest).then((errors) => {
      if (errors.length > 0) {
        this.logger.log(`Error validating headers: ${errors}`);
        throw new Error(`${errors}`);
      }
    });
  }

  private async saveCustomerGeolocData(requestId: string, extraHeaders: ExtraHeaders, personalDataResponse: ClientApplicationInformation){
    this.logger.log('Start saveCustomerGeolocData');

    await this.createBasicPersonalCategory(requestId, personalDataResponse, extraHeaders);

    this.logger.log('End saveCustomerGeolocData');
  }

  private async createBasicPersonalCategory(requestId: string, personalDataResponse: ClientApplicationInformation, extraHeaders: ExtraHeaders) {
    this.logger.log('Get basic personal category start');

    let basicPersonalCategory: Categories = this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.BASIC_PERSONAL);

    if (!basicPersonalCategory) {
      basicPersonalCategory = {
        category_transaction_data: ClientAppDataCategory.BASIC_PERSONAL,
        transaction_data: {
          location_latitude: extraHeaders.latitude,
          location_longitude: extraHeaders.longitude
        }
      }
    }else{
      basicPersonalCategory.transaction_data['location_latitude'] = extraHeaders.latitude;
      basicPersonalCategory.transaction_data['location_longitude'] = extraHeaders.longitude;
    }

    await this.personalDataService.saveCategory(requestId, basicPersonalCategory, this.source);

    this.logger.log('Get basic personal category end');
  }

}

export {
  SelectDestinationInitUsecase,
  SelectDestinationInitUseCaseQualifier
};


