import { Inject, Injectable, Logger } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { ClientAppDataCategory } from 'src/card-delivery/shared/common/enums/client-app-data-category';
import { UseCaseResponse } from 'src/card-delivery/shared/domain/use-case-response';
import { ExtraHeaders } from 'src/card-delivery/shared/interface/extra-headers';
import { IbSecuritySignatures } from 'src/card-delivery/shared/service/security-signatures/ib-security-signatures';
import { IbSecuritySignaturesFactory } from 'src/card-delivery/shared/service/security-signatures/ib-security-signatures-factory';
import { ModuleConfigurationService } from '../../select-destination/service/module-configuration.service';
import { CardPasswordDeliveryRequest } from '../../shared/common/api/model/cardPasswordDeliveryRequest';
import { CodeError } from '../../shared/common/constants/code-error.constants';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import BusinessProcess from '../../shared/common/models/BusinessProcess';
import { LibStepsInterface } from '../../shared/interface/lib-steps.interface';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import { Categories, ClientApplicationInformation, IPersonalDataServiceQualifier, PersonalDataInterface } from '../../shared/interface/personal-data.interface';
import { LibStepsService } from '../../shared/service/lib-steps.service';
import { BUSINESS_TAG, BUSINESS_TAG_FAILED } from 'src/card-delivery/shared/common/constants/log.constants';

export const CardPasswordDeliveryUseCaseQualifier = Symbol('CardPasswordDeliveryUseCaseQualifier');

@Injectable()
export class CardPasswordDeliveryUsecase {
  private readonly logger = new Logger(CardPasswordDeliveryUsecase.name);
  private readonly loggerESLM = new Logger();
  private readonly moduleName: string = 'cardPasswordDelivery';
  private readonly country: string;
  private readonly source: string;
  private readonly finalState: string;

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) private readonly orchestratorService: OrchestratorInterface,
    private readonly moduleConfigurationService: ModuleConfigurationService,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface,
    @Inject(IbSecuritySignaturesFactory) private readonly ibSecuritySignaturesLibFactory: IbSecuritySignaturesFactory,
    configService: ConfigService
  ) {
    this.source = configService.get('CARD_DELIVERY.CARD_PASSWORD_HEADER_KAFKA');
    this.finalState = configService.get('CARD_DELIVERY.CARD_PASSWORD_FINAL_STATE');
    this.country = configService.get('CARD_DELIVERY.COUNTRY');
  }

  async execute(requestId: string, request: CardPasswordDeliveryRequest, extraHeaders: ExtraHeaders): Promise<UseCaseResponse> {
    this.loggerESLM.log(`Start CardPassword Delivery - requestId: ${requestId}`, BUSINESS_TAG);

    // Obtener datos personales guardados previamente
    const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(requestId, this.source);

    // Obtener datos básicos personales necesarios para la validación
    const basicPersonal: Categories = await this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.BASIC_PERSONAL);

    // Validar firma eLlave
    await IbSecuritySignatures.validateEllave(
      this.ibSecuritySignaturesLibFactory, extraHeaders, requestId, this.moduleName, this.country, new Object(basicPersonal) as Categories);

    // Inicializar el proceso de negocio
    const initResponse: BusinessProcess = await this.orchestratorService.init(requestId, this.source);
    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword Delivery init failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    // Obtener configuración del módulo para ejecutar el paso correspondiente
    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, 'cardPassword');

    // Ejecutar paso definido por configuración
    await this.libStepsService.executeStep({
      stepName: moduleConfig.steps.delivery.stepName,
      personalDataResponse: personalDataResponse,
      flow: initResponse.flow,
      source: this.source,
      request: request,
      headers: new Map(Object.entries(extraHeaders))
    });

    // Ejecutar transición a estado final del proceso de negocio
    const deliveryResponse: BusinessProcess = await this.orchestratorService.delivery(requestId, this.finalState, this.source);
    if (deliveryResponse.error) {
      this.loggerESLM.error(`CardPassword Delivery final step failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_002);
    }

    this.loggerESLM.log(`CardPassword Delivery success - requestId: ${requestId}`, BUSINESS_TAG);
    return { url: deliveryResponse.next_step };
  }
}
