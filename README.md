Gracias, me podrias ayudar con el end point /select , te comparto la clase controller 

@HttpCode(200)
  @PreAuthorize(SCOPE_API_WRITE)
  @Post('/confirm')
  async confirm(
    @Res() response: Response,
    @Body() request: DestinationSelectRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('x-otp-token') XOtp: string,
    @Headers('path') path: string,
    @Cookies('request_id') requestId: string,
    @Req() req : Request
  ): Promise<void> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'x-otp-token': XOtp
    };

    extraHeaders.authorization = req.headers['Authorization'];

    const params: UseCaseParams = {
      requestId,
      source: this.source,
      module: this.module,
      subModule: path
    }
    const useCaseResponse: UseCaseResponse = await this.destinationConfirmUseCase.execute(params, request, extraHeaders);

    this.prepareResponse(response, useCaseResponse)
  }

requiero que agregues Logs ESLM en eventos solo en la clase DestinationConfirmUseCase, asi como  que le agregues lineas de codigo en los eventos que suceden, te comparto la clase con la funcionalidad
  
import { BadRequestException, Inject } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { ActionBaseUseCase } from '../../shared/application/action-base.usecase';
import { DestinationSelectRequest } from '../../shared/common/api/model/destinationSelectRequest';
import { CodeError } from '../../shared/common/constants/code-error.constants';
import { ClientAppDataCategory } from '../../shared/common/enums/client-app-data-category';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import BusinessProcess from '../../shared/common/models/BusinessProcess';
import { FlowConfig, Step } from '../../shared/domain/flow-config';
import { UseCaseParams } from '../../shared/domain/use-case-params';
import { UseCaseResponse } from '../../shared/domain/use-case-response';
import { ExtraHeaders } from '../../shared/interface/extra-headers';
import { GlobalModuleConfigurationInterface, IGlobalModuleConfigurationQualifier } from '../../shared/interface/global-module-configuration.interface';
import { LibStepsInterface } from '../../shared/interface/lib-steps.interface';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import { Categories, ClientApplicationInformation, IPersonalDataServiceQualifier, PersonalDataInterface } from '../../shared/interface/personal-data.interface';
import { LibStepsService } from '../../shared/service/lib-steps.service';
import { IbSecuritySignatures } from '../../shared/service/security-signatures/ib-security-signatures';
import { IbSecuritySignaturesFactory } from '../../shared/service/security-signatures/ib-security-signatures-factory';
import { DestinationService } from '../service/destination.service';
import { DestinationInterface } from '../service/interface/destination.Interface';

export class DestinationConfirmUseCase extends ActionBaseUseCase {

  private readonly signaturesModuleName: string = 'selectDestinationDeliveryV2';
  private readonly finalStatesConfig: object;
  private readonly country: string;

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) readonly orchestratorService: OrchestratorInterface,
    @Inject(IGlobalModuleConfigurationQualifier) readonly globalModuleConfigurationService: GlobalModuleConfigurationInterface,
    @Inject(DestinationService) private readonly destinationService: DestinationInterface,
    @Inject(IbSecuritySignaturesFactory) private readonly ibSecuritySignaturesLibFactory: IbSecuritySignaturesFactory,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface,
    private readonly configService: ConfigService) {

    super(orchestratorService, globalModuleConfigurationService);

    this.finalStatesConfig = JSON.parse(configService.get('CARD_DELIVERY.DESTINATION_CONFIRM_FINAL_STATES') ?? '{}');
    this.country = configService.get('CARD_DELIVERY.COUNTRY');
  }

  async execute(params: UseCaseParams, request: DestinationSelectRequest, extraHeaders: ExtraHeaders): Promise<UseCaseResponse> {

    await this.destinationService.validateRequest(request)

    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const flowConfig: FlowConfig = this.globalModuleConfigurationService.getConfiguration(params.module, initResponse.flow)
    const currentStep: Step = this.globalModuleConfigurationService.getStep(flowConfig, params.subModule);

    if (this.globalModuleConfigurationService.needValidateOtpSignatures(currentStep, { request })) {
      await this.validateSecuritySignatures(params, extraHeaders)
    }

    await this.destinationService.saveSelectedOption(params.requestId, params.source, request.option)

    if (currentStep.execution_step) {
      const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source)

      await this.libStepsService.executeStep({
        stepName: currentStep.execution_step,
        personalDataResponse: personalDataResponse,
        flow: initResponse.flow,
        source: params.source,
        request: request
      }
      )
    }

    params.finalState = this.destinationService.getFinalState(this.finalStatesConfig, request.option);

    return await this.getNextStepResponse(flowConfig, currentStep, params);
  }

  async validateSecuritySignatures(params: UseCaseParams, extraHeaders: ExtraHeaders) {
    if (!extraHeaders['x-otp-token']) {
      throw new BadRequestException('x-otp-token header, is required')
    }
    const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source)
    const basicPersonal: Categories = await this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.BASIC_PERSONAL);
    await IbSecuritySignatures.validateEllave(
      this.ibSecuritySignaturesLibFactory, extraHeaders, params.requestId, this.signaturesModuleName, this.country, basicPersonal as Categories);
  }
}
