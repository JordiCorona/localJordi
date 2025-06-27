import { Logger } from '@nestjs/common';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from '../../shared/common/constants/log.constants';

export class DestinationSelectUseCase extends ActionBaseUseCase {
  private readonly loggerESLM = new Logger('DestinationSelectUseCase');
  private readonly finalStatesConfig: object;

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) readonly orchestratorService: OrchestratorInterface,
    @Inject(DestinationService) private readonly destinationService: DestinationInterface,
    private readonly configService: ConfigService
  ) {
    super(orchestratorService);
    this.finalStatesConfig = JSON.parse(configService.get('CARD_DELIVERY.DESTINATION_SELECT_FINAL_STATES') ?? '{}');
  }

  async execute(params: UseCaseParams, request: DestinationSelectRequest): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start DestinationSelectUseCase - requestId: ${params.requestId}`, BUSINESS_TAG);

    try {
      // 🛡️ Validación de datos de entrada
      await this.destinationService.validateRequest(request);

      // 🔄 Inicialización del flujo de orquestación
      const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
      if (initResponse.error) {
        this.loggerESLM.error(`Orchestrator init failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
        throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
      }

      // 📌 Determina el estado final basado en la opción seleccionada
      const finalState = this.destinationService.getFinalState(this.finalStatesConfig, request.option);

      // 💾 Guarda la opción seleccionada en la base de datos de categorías
      await this.destinationService.saveSelectedOption(params.requestId, params.source, request.option);

      this.loggerESLM.log(`Destination selection completed - requestId: ${params.requestId}`, BUSINESS_TAG);

      // ⏭️ Redirige a la siguiente ruta (estado final)
      return this.getOrchestratorNextStep(params, finalState);
    } catch (error) {
      this.loggerESLM.error(`DestinationSelectUseCase failed - requestId: ${params.requestId} - ${error.message}`, BUSINESS_TAG_FAILED);
      throw error;
    }
  }
}
