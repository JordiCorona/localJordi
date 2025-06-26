
Tengo un error en  this.loggerESLM.log , me podrias ayudar a reviosarlo: Property 'loggerESLM' does not exist on type 'typeof IbSecuritySignatures'.ts(2339)


import { CustomerData } from '@ib/ib-mx-security-signatures-lib';
import { Logger } from '@nestjs/common';
import * as EllaveConfig from 'src/card-delivery/shared/common/files/ellave-resources/ellave-config.json';
import { CodeError } from "../../common/constants/code-error.constants";
import { BusinessException } from "../../common/exceptions/custom-exception";
import { ExtraHeaders } from "../../interface/extra-headers";
import { Categories } from "../../interface/personal-data.interface";
import { IbSecuritySignaturesFactory } from "./ib-security-signatures-factory";
import { jwtDecode } from 'jwt-decode';
import { BUSINESS_TAG_FAILED, BUSINESS_TAG } from 'src/card-delivery/shared/common/constants/log.constants';

let eLlaveCountryConfiguration: any;

export class IbSecuritySignatures {

  private readonly logger = new Logger(IbSecuritySignatures.name);
  private readonly loggerESLM = new Logger(); 

  static async validateEllave(ibSecuritySignaturesLibFactory: IbSecuritySignaturesFactory, extraHeaders: ExtraHeaders
    , requestId: string
    , moduleName: string
    , ibCountry: string
    , basicPersonal: Categories
  ): Promise<any> {

    return await IbSecuritySignatures.validate(ibCountry.toUpperCase(), moduleName)
      .then(async (res) => {
        if (res === 'OK') {
          LOGGER.log(`Ellave configuration loaded for country '${ibCountry}' : ${JSON.stringify(eLlaveCountryConfiguration)}`);

          const operationType = eLlaveCountryConfiguration.modules[`${moduleName}`].operationType;
          const operationUrl = eLlaveCountryConfiguration.modules[`${moduleName}`].operationUrl;
          const instance = ibSecuritySignaturesLibFactory.getInstance();

          const decoded = jwtDecode(extraHeaders["authorization"]);
          const claims : Map<string, string> = new Map<string, string>([
            ['cif', decoded["cif"]],
            ['passwordLastChangeTime', decoded["passwordLastChangeTime"]],
            ['sid', decoded["sid"]]
          ]);


          await instance.validateEllave(
            operationType,
            operationUrl,
            extraHeaders["x-otp-token"],
            requestId,
            new CustomerData(basicPersonal.transaction_data['client_id'], basicPersonal.transaction_data['user_key'] || '',
              claims,
              )
          ).then(response => {
            this.loggerESLM.log(`Successful eLlave signature validation for home delivery. requestId: ${requestId}`, BUSINESS_TAG);
            LOGGER.log(`Received reponse: ${JSON.stringify(response)}`);
          }).catch(error => {
            this.loggerESLM.error(`Failed eLlave signature validation. requestId: ${requestId} - ${error.message}`, BUSINESS_TAG_FAILED);
            throw new BusinessException(error.message, CodeError.CRD_SSIG_001)
          });
        } else {
          throw new BusinessException('IbSecuritySignatures not pass validations', CodeError.CRD_SSIG_001)
        }
      });

  }

  private static validate(ibCountry: string, moduleName: string): Promise<any> {
    const failStatus = 'FAIL';

    if (!moduleName || moduleName.trim().length === 0) {
      throw new Error(`Invalid moduleName parameter.`);
    }

    eLlaveCountryConfiguration = EllaveConfig.country[`${ibCountry}`];

    if (!eLlaveCountryConfiguration) {
      LOGGER.log(`No e-llave configuration for country ${ibCountry}`);
      return Promise.resolve(failStatus);
    }

    if (!eLlaveCountryConfiguration.enable) {
      LOGGER.log(`Disabled e-llave configuration for country ${ibCountry}`);
      return Promise.resolve(failStatus);
    }

    if (!eLlaveCountryConfiguration.modules[`${moduleName}`]) {
      LOGGER.log(`No module found for country ${ibCountry}`);
      return Promise.resolve(failStatus);
    }

    return Promise.resolve('OK');
  }

}

const LOGGER = new Logger(IbSecuritySignatures.name);


-------------------------------------------------------------------------

import { Response } from 'express';
import { InitResponseDto } from 'src/card-delivery/shared/common/api/model/initResponseDto';
import { SCOPE_API_READ } from 'src/card-delivery/shared/common/constants/journey-constants';
import { Headers, Controller, Get, HttpCode, Inject, UseGuards, Post, Body, Res, Req } from '@nestjs/common';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';
import { JourneyServiceException } from 'src/card-delivery/shared/common/exceptions/custom-exception';
import { ExtraHeaders } from 'src/card-delivery/shared/interface/extra-headers';
import { CardPasswordDeliveryRequest } from '../../shared/common/api/model/cardPasswordDeliveryRequest';
import { Cookies } from '../../shared/common/decorator/cookies.decorator';
import { CardPasswordDeliveryUsecase, CardPasswordDeliveryUseCaseQualifier } from '../application/card-password-delivery.usecase';
import { CardPasswordUseCase, CardPasswordUseCaseQualifier } from '../application/card-password.usecase';
import { SignatureRequestDto } from '@ib/ib-mx-security-signatures-lib/dto/signatureRequestDto';

@Controller('v1/card-password')
@UseGuards(AuthorizationGuard, PermissionsGuard)
export class CardPasswordController {

  constructor(
    @Inject(CardPasswordUseCaseQualifier) private readonly cardPasswordUseCaseCommand: CardPasswordUseCase,
    @Inject(CardPasswordDeliveryUseCaseQualifier) private readonly cardPasswordDeliveryUseCaseCommand: CardPasswordDeliveryUsecase,
  ) {
  }

  @HttpCode(200)
  @PreAuthorize(SCOPE_API_READ)
  @Get('/init')
  async init(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
  ): Promise<InitResponseDto | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
    };

    const response = await this.cardPasswordUseCaseCommand.execute(requestId, extraHeaders);

    return response;
  }

  /**
   * Returns the layout to capture the nip of card
   * @param XChannelId 'MOBILE'
   * @param XOriginatingApplCode 'IB'
   * @param requestId 'example: 019283740192928374653'
   * @returns
   */
  @HttpCode(200)
  @PreAuthorize(SCOPE_API_READ)
  @Get('/capture/layout')
  async capture(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
  ): Promise<InitResponseDto | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
    };

    const response = await this.cardPasswordUseCaseCommand.capture(requestId, extraHeaders);

    return response;
  }

  @HttpCode(302)
  @PreAuthorize(SCOPE_API_READ)
  @Post('/delivery')
  async delivery(
    @Res() response: Response,
    @Body() request: CardPasswordDeliveryRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('x-otp-token') XOtp: string,
    @Headers('x-auth-token') xAuthToken: string,
    @Cookies('request_id') requestId: string,
    @Req() req : Request
  ): Promise<void | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'x-otp-token': XOtp,
      'x-auth-token': xAuthToken
    };

    const params = {
      headers: { 'x-otp-token': XOtp },
      req: { 'cookies': { 'request_id': requestId } }
    }

    extraHeaders.authorization = req.headers['Authorization'];

    await SignatureRequestDto.validateParams(params);

    return this.cardPasswordDeliveryUseCaseCommand.execute(requestId, request, extraHeaders)
      .then(result => {
        response.setHeader('location', result.url);
        response.end();
      });

  }


}

----------------------------------------------
import { Inject, Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { GetServerDrivenUiLayoutUseCase } from 'src/card-delivery/sdui/application/get-server-driven-ui-layout.usecase';
import BusinessProcess from 'src/card-delivery/shared/common/models/BusinessProcess';
import { InitResponseDto } from 'src/card-delivery/shared/common/api/model/initResponseDto';
import { InitResponseDtoData } from 'src/card-delivery/shared/common/api/model/initResponseDtoData';
import { UseCaseResponse } from 'src/card-delivery/shared/domain/use-case-response';
import { ExtraHeaders } from 'src/card-delivery/shared/interface/extra-headers';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from 'src/card-delivery/shared/interface/orchestrator.interface';
import { ICardPasswordUseCase } from '../interface/card-password-use-case-command.interface';
import { ModuleConfigurationService } from 'src/card-delivery/select-destination/service/module-configuration.service';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../shared/common/constants/code-error.constants';
export const CardPasswordUseCaseQualifier = Symbol('CardPasswordUseCaseQualifier');

const INITIAL_STATE = 'CARD_DELIVERY.CARD_PASSWORD_HEADER_KAFKA';

@Injectable()
export class CardPasswordUseCase implements ICardPasswordUseCase {
  private readonly kafkaHeader: string;
  private readonly moduleProps={name:'cardPassword'};

  constructor(
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorClient: OrchestratorInterface,
    private readonly serverDrivenUiService: GetServerDrivenUiLayoutUseCase,
    configClient: ConfigService,
    private readonly moduleConfigurationService: ModuleConfigurationService,
  ) {
    this.kafkaHeader = configClient.get(INITIAL_STATE);
  }

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<UseCaseResponse | InitResponseDto | any> {
    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }
    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const initalStep = moduleConfig.steps[moduleConfig.initialStep]

    const layout: object = await this.serverDrivenUiService.execute(requestId, initalStep.layout);
    return this.getInitResponseDto(layout, initalStep.nextStep);
  }

  getInitResponseDto(layout: object, next_step?:string):InitResponseDto{
    const initResponseDto:InitResponseDto = new InitResponseDto();

    initResponseDto.data = new InitResponseDtoData();
    initResponseDto.data.layout = layout;

    if(next_step!==undefined){
      initResponseDto.data.nextStep = next_step;
    }

    return initResponseDto;

  }

  async capture(requestId: string, extraHeaders: ExtraHeaders): Promise<InitResponseDto> {
    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const layout: object = await this.serverDrivenUiService.execute(requestId, moduleConfig.steps.capture.layout);

    const initResponseDto:InitResponseDto = this.getInitResponseDto(layout, moduleConfig.steps.capture.nextStep);

    return initResponseDto;
  }

}

-------------------------------
import { InitModuleUseCase } from '../../shared/application/init-module.usecase';
import { LayoutModuleUseCase } from '../../shared/application/layout-module.usecase';
import { ConfigService } from '@nestjs/config';
import { BaseV2Controller } from '../../shared/controller/base-v2.controller';
import { Body, Controller, Get, Headers, HttpCode, Inject, Param, Post, Res, UseGuards } from '@nestjs/common';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';
import { JourneyModules } from '../../shared/common/enums/journey-modules';
import { SCOPE_API_READ, SCOPE_API_WRITE } from '../../shared/common/constants/journey-constants';
import { Cookies } from '../../shared/common/decorator/cookies.decorator';
import { AddressCaptureRequest } from '../../shared/common/api/model/addressCaptureRequest';
import { UseCaseParams } from '../../shared/domain/use-case-params';
import { AddressCaptureSaveUseCase } from '../application/address-capture-save.usecase';
import { Response } from 'express';
import { UseCaseResponse } from '../../shared/domain/use-case-response';
import { IbCatalogServiceInterface } from 'src/card-delivery/shared/interface/ib-catalog-service.interface';
import { ExtraHeaders } from 'src/card-delivery/shared/interface/extra-headers';
import { IbCatalogService } from 'src/card-delivery/shared/service/ib-catalog-service';

@Controller('v1/address-capture')
@UseGuards(AuthorizationGuard, PermissionsGuard)
export class AddressCaptureController extends BaseV2Controller {

  private addressCaptureSaveUseCase: AddressCaptureSaveUseCase
  constructor(
    initModuleUseCase: InitModuleUseCase,
    layoutModuleUseCase: LayoutModuleUseCase,
    addressCaptureSaveUseCase: AddressCaptureSaveUseCase,
    @Inject(IbCatalogService) private readonly ibCatalogService: IbCatalogServiceInterface,
    configService: ConfigService
  ) {
    super(initModuleUseCase, layoutModuleUseCase, JourneyModules.ADDRESS_CAPTURE, configService.get('CARD_DELIVERY.ADDRESS_CAPTURE_HEADER_KAFKA'))
    this.addressCaptureSaveUseCase = addressCaptureSaveUseCase
  }
  
  @HttpCode(200)
  @Get('/zip-code/:zipCode')
  @PreAuthorize(SCOPE_API_READ)
  async getZipCode(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
    @Param('zipCode') zipCode: string
  ){
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
    };

    return await this.ibCatalogService.getSuburbsByZipCode(extraHeaders, zipCode);
  }

  @HttpCode(200)
  @PreAuthorize(SCOPE_API_WRITE)
  @Post('/save')
  async save(
    @Res() response: Response,
    @Body() request: AddressCaptureRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('path') path: string,
    @Cookies('request_id') requestId: string
  ): Promise<object | void> {
    const params: UseCaseParams = {
      requestId,
      source: this.source,
      module: this.module,
      subModule: path
    }
    const useCaseResponse: UseCaseResponse = await this.addressCaptureSaveUseCase.execute(params, request);

    this.prepareResponse(response, useCaseResponse)
  }


}

---------------------------------------------------------------
import { UseCaseParams } from '../../shared/domain/use-case-params';
import { AddressCaptureRequest } from '../../shared/common/api/model/addressCaptureRequest';
import { AddressCaptureSave } from './interface/address-capture-save.interface';
import { Inject, Injectable } from '@nestjs/common';
import { Categories, ClientApplicationInformation, IPersonalDataServiceQualifier, PersonalDataInterface } from '../../shared/interface/personal-data.interface';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import { LibStepsService } from '../../shared/service/lib-steps.service';
import { LibStepsInterface } from '../../shared/interface/lib-steps.interface';
import BusinessProcess from '../../shared/common/models/BusinessProcess';
import { ClientAppDataCategory } from '../../shared/common/enums/client-app-data-category';
import { FlowConfig } from '../../shared/domain/flow-config';
import { GlobalModuleConfigurationInterface, IGlobalModuleConfigurationQualifier } from '../../shared/interface/global-module-configuration.interface';
import { UseCaseResponse } from '../../shared/domain/use-case-response';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../shared/common/constants/code-error.constants';
import { ActionBaseUseCase } from '../../shared/application/action-base.usecase';

@Injectable()
class AddressCaptureSaveUseCase extends ActionBaseUseCase implements AddressCaptureSave {
  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) orchestratorService: OrchestratorInterface,
    @Inject(IGlobalModuleConfigurationQualifier) globalModuleConfigurationService: GlobalModuleConfigurationInterface,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface) {

    super(orchestratorService, globalModuleConfigurationService);
  }

  async execute(params: UseCaseParams, request: AddressCaptureRequest): Promise<UseCaseResponse> {
    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const flowConfig: FlowConfig = this.globalModuleConfigurationService.getConfiguration(params.module, initResponse.flow)
    const currentStep = this.globalModuleConfigurationService.getStep(flowConfig, params.subModule);

    if (currentStep.execution_step) {
      const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source)

      await this.libStepsService.executeStep({
        stepName: currentStep.execution_step,
        personalDataResponse: personalDataResponse,
        flow: initResponse.flow,
        source: params.source,
        request: request }
      )
    } else {
      const basicPersonalCategory: Categories = {
        category_transaction_data: ClientAppDataCategory.RESIDENCE_ADDRESS,
        transaction_data: request
      }
      await this.personalDataService.saveCategory(params.requestId, basicPersonalCategory, params.source)
    }

    return this.getNextStepResponse(flowConfig, currentStep, params);
  }
}

export {
  AddressCaptureSaveUseCase
}

