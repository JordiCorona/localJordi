import { Logger, BadRequestException, Inject } from '@nestjs/common';
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
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from '../../shared/common/constants/log.constants';

export class DestinationConfirmUseCase extends ActionBaseUseCase {
  private readonly loggerESLM = new Logger('DestinationConfirmUseCase');
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
    private readonly configService: ConfigService
  ) {
    super(orchestratorService, globalModuleConfigurationService);
    this.finalStatesConfig = JSON.parse(configService.get('CARD_DELIVERY.DESTINATION_CONFIRM_FINAL_STATES') ?? '{}');
    this.country = configService.get('CARD_DELIVERY.COUNTRY');
  }

  async execute(params: UseCaseParams, request: DestinationSelectRequest, extraHeaders: ExtraHeaders): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start DestinationConfirmUseCase - requestId: ${params.requestId}`, BUSINESS_TAG);

    try {
      // ✅ Validación del request (estructura y campos requeridos)
      await this.destinationService.validateRequest(request);

      // 🔄 Inicializa el flujo del orquestador
      const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
      if (initResponse.error) {
        this.loggerESLM.error(`Orchestrator init failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
        throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
      }

      // 📋 Obtiene la configuración del módulo y el paso actual
      const flowConfig: FlowConfig = this.globalModuleConfigurationService.getConfiguration(params.module, initResponse.flow);
      const currentStep: Step = this.globalModuleConfigurationService.getStep(flowConfig, params.subModule);

      // 🔐 Validación de OTP si se requiere para este paso
      if (this.globalModuleConfigurationService.needValidateOtpSignatures(currentStep, { request })) {
        this.loggerESLM.log(`Validating OTP signature - requestId: ${params.requestId}`, BUSINESS_TAG);
        await this.validateSecuritySignatures(params, extraHeaders);
      }

      // 💾 Guarda la opción seleccionada (ej: domicilio, sucursal)
      await this.destinationService.saveSelectedOption(params.requestId, params.source, request.option);

      // 🧠 Ejecuta el paso correspondiente si está configurado (puede hacer lógica de negocio u orquestación)
      if (currentStep.execution_step) {
        const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source);

        await this.libStepsService.executeStep({
          stepName: currentStep.execution_step,
          personalDataResponse: personalDataResponse,
          flow: initResponse.flow,
          source: params.source,
          request: request
        });
      }

      // 🔚 Define el siguiente estado final basado en la opción seleccionada
      params.finalState = this.destinationService.getFinalState(this.finalStatesConfig, request.option);

      this.loggerESLM.log(`DestinationConfirmUseCase success - requestId: ${params.requestId}`, BUSINESS_TAG);
      return await this.getNextStepResponse(flowConfig, currentStep, params);
    } catch (error) {
      this.loggerESLM.error(`DestinationConfirmUseCase failed - requestId: ${params.requestId} - ${error.message}`, BUSINESS_TAG_FAILED);
      throw error;
    }
  }

  // 🔐 Método auxiliar para validar firmas OTP
  async validateSecuritySignatures(params: UseCaseParams, extraHeaders: ExtraHeaders) {
    if (!extraHeaders['x-otp-token']) {
      throw new BadRequestException('x-otp-token header, is required');
    }

    const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source);
    const basicPersonal: Categories = await this.personalDataService.getCategory(
      personalDataResponse.categories,
      ClientAppDataCategory.BASIC_PERSONAL
    );

    await IbSecuritySignatures.validateEllave(
      this.ibSecuritySignaturesLibFactory,
      extraHeaders,
      params.requestId,
      this.signaturesModuleName,
      this.country,
      basicPersonal as Categories
    );
  }
}
