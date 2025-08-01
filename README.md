import { Inject, Injectable, Logger } from '@nestjs/common';
import { OrchestratorService } from '../../../shared/service/orchestrator.service';
import { OrchestratorSource } from '../../../shared/common/enums/orchestrator_source.enum';

import { ExtraHeaders } from 'src/digital-card/shared/interface/extra-headers';
import { IbSecuritySignaturesFactory } from 'src/digital-card/shared/service/security-signatures/ib-security-signatures-factory';
import { IbSecuritySignatures } from 'src/digital-card/shared/service/security-signatures/ib-security-signatures';

import { NotificationsServiceDelegate } from '../../notifications/services/notifications.service.delegate';
import { ConfigService } from '@nestjs/config';

import {
  Categories,
  ClientApplicationInformation,
  IPersonalDataServiceQualifier,
  PersonalDataInterface,
} from 'src/digital-card/shared/interface/personal-data.interface';
import { PersonalDataEnum } from 'src/digital-card/shared/common/enums/personal_data.enum';
import { BasicPersonalEnum } from 'src/digital-card/shared/common/enums/basic_personal.enum';
import { CreateBcsService } from 'src/digital-card/shared/common/services/create.bcs.service';
import { BasicPersonalRequestLibDto } from 'digital-card-lib/common/models/request/basic-personal-request-lib-dto';
import { AccountPersonalDataList } from 'src/digital-card/shared/common/models/AccountPersonalDataList.dto';
import { DigitalCardDto } from 'src/digital-card/shared/common/models/DigitalCardData.dto';
import { AccountPersonalData } from 'src/digital-card/shared/common/models/AccountPersonalData.dto';

import { EventTracingConstants } from '../../../shared/common/constants/event-tracing.constants';
import { AuditEventService } from '../../../shared/service/audit-event.service';
import { DigitalCardException } from '../../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../../shared/common/models/code-error.model';
import {
  BUSINESS_TAG,
  BUSINESS_TAG_FAILED,
  TEMP_DATA_TAG,
} from '../../../shared/common/constants/log.constants';
import { KeyMasterBcsService } from 'src/digital-card/shared/common/services/keymaster.bcs.service';

@Injectable()
export class CreateService {
  private readonly logger = new Logger(CreateService.name);
  private readonly moduleName: string = 'createService';
  private readonly loggerESLM = new Logger();

  constructor(
    private readonly orchestratorService: OrchestratorService,
    @Inject(IbSecuritySignaturesFactory)
    private readonly ibSecuritySignaturesLibFactory: IbSecuritySignaturesFactory,
    private readonly configService: ConfigService,
    private readonly notificationsServiceDelegate: NotificationsServiceDelegate,
    private readonly createBcsService: CreateBcsService,
    @Inject(IPersonalDataServiceQualifier)
    private readonly personalDataService: PersonalDataInterface,
    private readonly keyMasterBcsService: KeyMasterBcsService,
    private readonly auditService: AuditEventService,
  ) {}

  public async init(requestId: string): Promise<Object> {
    this.logger.log(`Initializing digital card create with requestId: ${requestId}`);
    this.loggerESLM.log(`Initializing digital card create with requestId: ${requestId}`, BUSINESS_TAG);

    return await this.orchestratorService.init(
      requestId,
      OrchestratorSource.CREATED_CARD.toString(),
    );
  }

  async create(requestId: string, extraHeaders: ExtraHeaders): Promise<any> {
    this.loggerESLM.log(`Starting the digital card creation process for requestId: ${requestId}`, BUSINESS_TAG);
    try {
      this.loggerESLM.log(`Customer information is obtained for requestId: ${requestId}`, TEMP_DATA_TAG);

      let personalDataResponse: ClientApplicationInformation =
        await this.personalDataService.getByRequestId(
          requestId,
          PersonalDataEnum.DIGITAL_CARD.toString(),
        );

      const basicPersonal: Categories = this.personalDataService.getCategory(
        personalDataResponse.categories,
        PersonalDataEnum.BASIC_PERSONAL.toString(),
      );

      const productSummaryCat: Categories = this.personalDataService.getCategory(
        personalDataResponse.categories,
        PersonalDataEnum.PRODUCT_SUMMARY.toString(),
      );

      const cards = productSummaryCat.transaction_data as AccountPersonalDataList;
      let card = this.getCard(cards);

      const cif = basicPersonal.transaction_data[BasicPersonalEnum.CLIENT_ID.toString()];
      const userKey = basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()];

      await IbSecuritySignatures.validateEllave(
        this.ibSecuritySignaturesLibFactory,
        extraHeaders,
        requestId,
        this.moduleName,
        this.configService.get('COUNTRY'),
        cif,
        userKey,
      );

      this.loggerESLM.log(`Ellave is validated for requestId: ${requestId}`, BUSINESS_TAG);

      const basicPersonalRequestLibDto: BasicPersonalRequestLibDto = {
        user: userKey,
        client_id: cif,
        account_id: card.account_id,
        x_auth_token: extraHeaders['x-auth-token'],
      };

      const containtDigitalCard = cards.accounts.find(element =>
        element.product_id.includes(PersonalDataEnum.DIGITAL_D),
      );

      let responseDigitalCard;
      let digitalCard;

      if (!containtDigitalCard) {
        responseDigitalCard = await this.createBcsService.createDigitalCard(basicPersonalRequestLibDto);

        if (
          responseDigitalCard.notifications &&
          responseDigitalCard.notifications[0].code === 'I_CEN_BS_001'
        ) {
          digitalCard = responseDigitalCard.data as DigitalCardDto;
          this.loggerESLM.log(
            `Successful card digital activating for requestId: ${requestId}`,
            BUSINESS_TAG,
          );
        } else {
          this.loggerESLM.log(
            `Error activating digital card for requestId: ${requestId}`,
            BUSINESS_TAG_FAILED,
          );
          throw new DigitalCardException(`Error activating digital card`, CodeError.TDG_001);
        }
      } else {
        if (!containtDigitalCard.account_id_masked) {
          const basicPersonalRequestLibDtoForKeyMaster: BasicPersonalRequestLibDto = {
            user: userKey,
            client_id: cif,
            account_id: containtDigitalCard.account_id,
            x_auth_token: extraHeaders['x-auth-token'],
          };

          const cardClean = await this.keyMasterBcsService.getDesSurrogate(
            basicPersonalRequestLibDtoForKeyMaster,
          );

          digitalCard = {
            card_number: this.maskCardNumber(cardClean),
            card_number_key: cardClean,
          } as DigitalCardDto;
        } else {
          digitalCard = {
            card_number: containtDigitalCard.account_id_masked,
            card_number_key: containtDigitalCard.account_id,
          } as DigitalCardDto;
        }
      }

      await this.savePersonalData(productSummaryCat, digitalCard, requestId);
      await this.registerEventTracing(requestId);
      this.notificationsServiceDelegate.sendNotifications(requestId, personalDataResponse);

      const responseDelivery = await this.delivery(requestId);

      this.loggerESLM.log(`Digital card created successfully for requestId: ${requestId}`, BUSINESS_TAG);
      return { next_step: responseDelivery };
    } catch (error) {
      this.loggerESLM.log(`Error creating digital card for requestId: ${requestId}`, BUSINESS_TAG_FAILED);
      throw new DigitalCardException(`Error processing digital card ${error}`, CodeError.TDG_001);
    }
  }

  private getCard(cards: AccountPersonalDataList): any {
    return cards.accounts.find(
      element => !element.product_id.includes(PersonalDataEnum.DIGITAL_D),
    );
  }

  private savePersonalData(
    productOriginationCat: Categories,
    digitalCard: DigitalCardDto,
    requestId: string,
  ) {
    this.loggerESLM.log(`Starting saving digital card to personalData for requestId: ${requestId}`, BUSINESS_TAG);

    const cards = productOriginationCat.transaction_data as AccountPersonalDataList;
    const card = this.getCard(cards);

    const digCard: AccountPersonalData = {
      type: `${card.type} ${PersonalDataEnum.DIGITAL.toString()}`,
      description: card.description,
      product_id: `${card.product_id}${PersonalDataEnum.DIGITAL_D}`,
      account_id: digitalCard.card_number_key,
      branch_id: card.branch_id,
      account_id_masked: digitalCard.card_number,
    };

    cards.accounts.push(digCard);
    this.personalDataService.saveCategory(
      requestId,
      productOriginationCat,
      PersonalDataEnum.DIGITAL_CARD.toString(),
    );

    this.loggerESLM.log(
      `Digital card successfully saved in personalData for requestId: ${requestId}`,
      TEMP_DATA_TAG,
    );
  }

  private maskCardNumber(cardNumber: string): string {
    if (!cardNumber || cardNumber.length < 8) {
      return '************';
    }
    return `${cardNumber.slice(0, 4)}********${cardNumber.slice(-4)}`;
  }

  async delivery(requestId: string): Promise<string> {
    const deliveryResponse = await this.orchestratorService.delivery(
      requestId,
      OrchestratorSource.DETAILS_COMPLETED.toString(),
      OrchestratorSource.CREATED_CARD.toString(),
    );

    this.logger.log(
      `Delivery in digital card create requestId: ${requestId} - finalState: ${OrchestratorSource.DETAILS_COMPLETED.toString()} - nextStep ${deliveryResponse.next_step}`,
    );

    return deliveryResponse.next_step;
  }

  private async registerEventTracing(requestId: string) {
    const event = this.auditService.createEvent(requestId, EventTracingConstants.STEP_CREATE);
    await this.auditService.sendEvent(event);
  }
}
