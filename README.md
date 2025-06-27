import { Logger } from '@nestjs/common';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

@Injectable()
class AddressCaptureSaveUseCase extends ActionBaseUseCase implements AddressCaptureSave {
  private readonly loggerESLM = new Logger('AddressCaptureSaveUseCase');

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) orchestratorService: OrchestratorInterface,
    @Inject(IGlobalModuleConfigurationQualifier) globalModuleConfigurationService: GlobalModuleConfigurationInterface,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface
  ) {
    super(orchestratorService, globalModuleConfigurationService);
  }

  async execute(params: UseCaseParams, request: AddressCaptureRequest): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start AddressCapture Save - requestId: ${params.requestId}`, BUSINESS_TAG);

    // 🔸 Inicializa el flujo de orquestador
    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      this.loggerESLM.error(`AddressCapture Save failed at init - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    // 🔸 Obtiene la configuración del flujo y paso actual
    const flowConfig: FlowConfig = this.globalModuleConfigurationService.getConfiguration(params.module, initResponse.flow);
    const currentStep = this.globalModuleConfigurationService.getStep(flowConfig, params.subModule);

    // 🔹 Si hay paso de ejecución definido
    if (currentStep.execution_step) {
      const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(params.requestId, params.source);

      await this.libStepsService.executeStep({
        stepName: currentStep.execution_step,
        personalDataResponse: personalDataResponse,
        flow: initResponse.flow,
        source: params.source,
        request: request
      });
    } else {
      // 🔹 Si no hay paso definido, guarda la categoría de datos básicos
      const basicPersonalCategory: Categories = {
        category_transaction_data: ClientAppDataCategory.RESIDENCE_ADDRESS,
        transaction_data: request
      };
      await this.personalDataService.saveCategory(params.requestId, basicPersonalCategory, params.source);
    }

    this.loggerESLM.log(`AddressCapture Save success - requestId: ${params.requestId}`, BUSINESS_TAG);

    // 🔸 Retorna la respuesta del siguiente paso
    return this.getNextStepResponse(flowConfig, currentStep, params);
  }
}

export {
  AddressCaptureSaveUseCase
}
