import { Inject, Injectable, Logger } from '@nestjs/common';
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
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

const INITIAL_STATE = 'CARD_DELIVERY.CARD_PASSWORD_HEADER_KAFKA';

@Injectable()
export class CardPasswordUseCase implements ICardPasswordUseCase {
  private readonly kafkaHeader: string;
  private readonly moduleProps = { name: 'cardPassword' };
  private readonly logger = new Logger(CardPasswordUseCase.name);
  private readonly loggerESLM = new Logger();

  constructor(
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorClient: OrchestratorInterface,
    private readonly serverDrivenUiService: GetServerDrivenUiLayoutUseCase,
    configClient: ConfigService,
    private readonly moduleConfigurationService: ModuleConfigurationService,
  ) {
    this.kafkaHeader = configClient.get(INITIAL_STATE);
  }

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<UseCaseResponse | InitResponseDto | any> {
    // Log de inicio del flujo INIT
    this.loggerESLM.log(`Start CardPassword INIT - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    // Validación de error en respuesta
    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword INIT failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    // Obtención de configuración de pasos del módulo
    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const initalStep = moduleConfig.steps[moduleConfig.initialStep];

    // Log de éxito
    this.loggerESLM.log(`CardPassword INIT success - requestId: ${requestId}`, BUSINESS_TAG);

    // Llama al servicio que obtiene el layout a presentar al usuario
    const layout: object = await this.serverDrivenUiService.execute(requestId, initalStep.layout);

    return this.getInitResponseDto(layout, initalStep.nextStep);
  }

  getInitResponseDto(layout: object, next_step?: string): InitResponseDto {
    const initResponseDto: InitResponseDto = new InitResponseDto();
    initResponseDto.data = new InitResponseDtoData();
    initResponseDto.data.layout = layout;

    if (next_step !== undefined) {
      initResponseDto.data.nextStep = next_step;
    }

    return initResponseDto;
  }

  async capture(requestId: string, extraHeaders: ExtraHeaders): Promise<InitResponseDto> {
    // Log de inicio de captura de NIP
    this.loggerESLM.log(`Start CardPassword CAPTURE - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword CAPTURE failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    // Obtiene configuración del paso de captura del flujo
    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const layout: object = await this.serverDrivenUiService.execute(requestId, moduleConfig.steps.capture.layout);

    // Log de éxito
    this.loggerESLM.log(`CardPassword CAPTURE success - requestId: ${requestId}`, BUSINESS_TAG);

    // Construye DTO de respuesta con layout y siguiente paso (si aplica)
    const initResponseDto: InitResponseDto = this.getInitResponseDto(layout, moduleConfig.steps.capture.nextStep);

    return initResponseDto;
  }
}
