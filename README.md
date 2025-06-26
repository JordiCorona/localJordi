
-----------------------------------------------------------------------------------------------

✅ 1. Endpoint: /card-password/init
Ubicación: Método execute en clase CardPasswordUseCase.

🎯 Objetivo del log:
Loggear el inicio y fin del proceso de obtención del layout inicial para la captura de contraseña de tarjeta.

🧩 Ubicación sugerida:
Dentro del método execute, antes y después de la llamada a orchestratorClient.init(...).

📌 Código modificado con ESLM logs:
ts
Copiar
Editar
import { Logger } from '@nestjs/common';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

@Injectable()
export class CardPasswordUseCase implements ICardPasswordUseCase {
  private readonly loggerESLM = new Logger('CardPasswordUseCase'); // ESLM logger
  ...

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<UseCaseResponse | InitResponseDto | any> {
    this.loggerESLM.log(`Start CardPassword INIT - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword INIT failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    this.loggerESLM.log(`CardPassword INIT success - requestId: ${requestId}`, BUSINESS_TAG);

    ...
  }
}
✅ 2. Endpoint: /address-capture/save
Ubicación: Método execute en clase AddressCaptureSaveUseCase.

🎯 Objetivo del log:
Loggear el inicio y resultado del proceso de guardado de la dirección del usuario.

🧩 Ubicación sugerida:
Log de inicio al entrar al método execute.

Log de éxito tras guardar los datos.

Log de error si ocurre fallo en el orchestratorService.init.

📌 Código modificado con ESLM logs:
ts
Copiar
Editar
import { Logger } from '@nestjs/common';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

@Injectable()
class AddressCaptureSaveUseCase extends ActionBaseUseCase implements AddressCaptureSave {
  private readonly loggerESLM = new Logger('AddressCaptureSaveUseCase');

  async execute(params: UseCaseParams, request: AddressCaptureRequest): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start ADDRESS SAVE - requestId: ${params.requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      this.loggerESLM.error(`AddressCapture SAVE failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    ...
    this.loggerESLM.log(`AddressCapture SAVE successful - requestId: ${params.requestId}`, BUSINESS_TAG);

    return this.getNextStepResponse(flowConfig, currentStep, params);
  }
}







Listo. He agregado los logs ESLM en el método execute de CardPasswordUseCase, en las siguientes partes clave:

Al inicio del método: Start CardPassword INIT

Si ocurre un error en el init del orquestador: CardPassword INIT failed

Cuando el init es exitoso: CardPassword INIT success

Si deseas que también incluya logs ESLM en capture o que aplique el mismo patrón en el método save de AddressCaptureSaveUseCase, dímelo y lo hago en el código directamente.










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
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

export const CardPasswordUseCaseQualifier = Symbol('CardPasswordUseCaseQualifier');

const INITIAL_STATE = 'CARD_DELIVERY.CARD_PASSWORD_HEADER_KAFKA';

@Injectable()
export class CardPasswordUseCase implements ICardPasswordUseCase {
  private readonly kafkaHeader: string;
  private readonly moduleProps = { name: 'cardPassword' };
  private readonly loggerESLM = new Logger('CardPasswordUseCase');

  constructor(
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorClient: OrchestratorInterface,
    private readonly serverDrivenUiService: GetServerDrivenUiLayoutUseCase,
    configClient: ConfigService,
    private readonly moduleConfigurationService: ModuleConfigurationService,
  ) {
    this.kafkaHeader = configClient.get(INITIAL_STATE);
  }

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<UseCaseResponse | InitResponseDto | any> {
    this.loggerESLM.log(`Start CardPassword INIT - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword INIT failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    this.loggerESLM.log(`CardPassword INIT success - requestId: ${requestId}`, BUSINESS_TAG);

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const initalStep = moduleConfig.steps[moduleConfig.initialStep];

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
    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const layout: object = await this.serverDrivenUiService.execute(requestId, moduleConfig.steps.capture.layout);

    const initResponseDto: InitResponseDto = this.getInitResponseDto(layout, moduleConfig.steps.capture.nextStep);

    return initResponseDto;
  }
}









save


Listo. He implementado los logs ESLM en el método execute de la clase AddressCaptureSaveUseCase, específicamente:

Al inicio del proceso:

ts
Copiar
Editar
this.loggerESLM.log(`Start AddressCapture Save - requestId: ${params.requestId}`, BUSINESS_TAG);
Si hay error en el init del flujo:

ts
Copiar
Editar
this.loggerESLM.error(`AddressCapture Save failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
Al final exitoso del proceso:

ts
Copiar
Editar
this.loggerESLM.log(`AddressCapture Save success - requestId: ${params.requestId}`, BUSINESS_TAG);
¿Te gustaría que también lo implemente en el método capture de CardPasswordUseCase?









Herramientas



Listo. He implementado los logs ESLM en el método execute de la clase AddressCaptureSaveUseCase, específicamente:

Al inicio del proceso:

ts
Copiar
Editar
this.loggerESLM.log(`Start AddressCapture Save - requestId: ${params.requestId}`, BUSINESS_TAG);
Si hay error en el init del flujo:

ts
Copiar
Editar
this.loggerESLM.error(`AddressCapture Save failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
Al final exitoso del proceso:

ts
Copiar
Editar
this.loggerESLM.log(`AddressCapture Save success - requestId: ${params.requestId}`, BUSINESS_TAG);



import { UseCaseParams } from '../../shared/domain/use-case-params';
import { AddressCaptureRequest } from '../../shared/common/api/model/addressCaptureRequest';
import { AddressCaptureSave } from './interface/address-capture-save.interface';
import { Inject, Injectable, Logger } from '@nestjs/common';
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
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

@Injectable()
class AddressCaptureSaveUseCase extends ActionBaseUseCase implements AddressCaptureSave {
  private readonly loggerESLM = new Logger('AddressCaptureSaveUseCase');

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) orchestratorService: OrchestratorInterface,
    @Inject(IGlobalModuleConfigurationQualifier) globalModuleConfigurationService: GlobalModuleConfigurationInterface,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface) {

    super(orchestratorService, globalModuleConfigurationService);
  }

  async execute(params: UseCaseParams, request: AddressCaptureRequest): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start AddressCapture Save - requestId: ${params.requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      this.loggerESLM.error(`AddressCapture Save failed - requestId: ${params.requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
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

    this.loggerESLM.log(`AddressCapture Save success - requestId: ${params.requestId}`, BUSINESS_TAG);
    return this.getNextStepResponse(flowConfig, currentStep, params);
  }
}

export {
  AddressCaptureSaveUseCase
}

-------------------------------------------------------------------------------------------------CARD CAPTURE

card capture 


Aquí tienes la clase CardPasswordUseCase con los logs ESLM integrados correctamente en el método execute (para /init) y también en capture (por si lo necesitas después):

ts
Copiar
Editar
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
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

export const CardPasswordUseCaseQualifier = Symbol('CardPasswordUseCaseQualifier');

const INITIAL_STATE = 'CARD_DELIVERY.CARD_PASSWORD_HEADER_KAFKA';

@Injectable()
export class CardPasswordUseCase implements ICardPasswordUseCase {
  private readonly kafkaHeader: string;
  private readonly loggerESLM = new Logger('CardPasswordUseCase');
  private readonly moduleProps = { name: 'cardPassword' };

  constructor(
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorClient: OrchestratorInterface,
    private readonly serverDrivenUiService: GetServerDrivenUiLayoutUseCase,
    configClient: ConfigService,
    private readonly moduleConfigurationService: ModuleConfigurationService,
  ) {
    this.kafkaHeader = configClient.get(INITIAL_STATE);
  }

  async execute(requestId: string, extraHeaders: ExtraHeaders): Promise<UseCaseResponse | InitResponseDto | any> {
    this.loggerESLM.log(`Start CardPassword Init - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword Init failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const initalStep = moduleConfig.steps[moduleConfig.initialStep];

    const layout: object = await this.serverDrivenUiService.execute(requestId, initalStep.layout);

    this.loggerESLM.log(`CardPassword Init success - requestId: ${requestId}`, BUSINESS_TAG);
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
    this.loggerESLM.log(`Start CardPassword Capture - requestId: ${requestId}`, BUSINESS_TAG);

    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword Capture failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const layout: object = await this.serverDrivenUiService.execute(requestId, moduleConfig.steps.capture.layout);

    this.loggerESLM.log(`CardPassword Capture success - requestId: ${requestId}`, BUSINESS_TAG);

    return this.getInitResponseDto(layout, moduleConfig.steps.capture.nextStep);
  }
}
Cambios realizados:
✅ Agregados loggerESLM.log() al inicio y éxito del método execute.

✅ Agregado loggerESLM.error() si ocurre error durante el init.

✅ Lo mismo en el método capture como buena práctica.
