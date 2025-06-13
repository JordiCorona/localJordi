import { UseGuards, Controller, Inject, Post, Get, Headers, Put, Body, HttpCode } from '@nestjs/common';
import { Cookies } from '../../../shared/common/decorator/cookies.decorator';

import {
  UpdateStatusRequest
} from 'digital-card-lib/journey/model/updateStatusRequest';
import { DigitalCardQueryServices } from 'src/digital-card/shared/service/digital-card-query.service';
import { DigitalCreditCardCaseCommandQualifier } from '../application/response/digital-card-query-response.interface';
import { CVVRequest } from "src/digital-card/modules/card-operations/application/request/digital-card-request.interface";
import { CardOperationsService } from '../services/card-operations.service';
import { REQUEST_ID, SCOPES_DIGITAL_CARD_READ, SCOPES_DIGITAL_CARD_WRITE} from '../../../shared/common/constants/api.environment';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';


@UseGuards(AuthorizationGuard, PermissionsGuard)
@Controller('v1/operations')
class CardOperationsController{

  constructor(
    @Inject(DigitalCreditCardCaseCommandQualifier) private digitalCreditCardCase: DigitalCardQueryServices,
    private readonly cardOperationsService: CardOperationsService,
  ) {
  }

  @PreAuthorize(SCOPES_DIGITAL_CARD_READ, SCOPES_DIGITAL_CARD_WRITE)
  @Get('/cards')
  async digitalCardQuery(
    @Headers('x-channel-id') XChannelId: string ="IB",
    @Cookies(REQUEST_ID) requestId: string,
  ): Promise<object> {

    return await this.digitalCreditCardCase.digitalCardQuery("");
  }


  @HttpCode(200)
  //@PreAuthorize(SCOPES_DIGITAL_CARD_READ, SCOPES_DIGITAL_CARD_WRITE)
  @Put('/status')
  async statusCard(
    @Cookies(REQUEST_ID) requestId: string,
    @Headers('x_auth_token') x_auth_token: string,
    @Body() updateStatusRequestLib: UpdateStatusRequest
  ): Promise<object> {
    return await this.cardOperationsService.updateStatus(requestId, updateStatusRequestLib, x_auth_token);
  }

  @PreAuthorize(
    SCOPES_DIGITAL_CARD_READ,
    SCOPES_DIGITAL_CARD_WRITE,
  )
  @Post('/cvv')
  async cvv(
    @Headers('x-channel-id') XChannelId: string ="IB",
    @Cookies(REQUEST_ID) requestId: string,
    @Body() cvv: CVVRequest
  ): Promise<object> {

    return await this.digitalCreditCardCase.cvv(cvv, requestId);
  }
}



export { CardOperationsController };


import { Injectable } from '@nestjs/common';
import { DigitalCardQueryInterface } from '../interface/digital-card-query.interface';
import {
  DigitalCardQueryServiceResponse,
  DigitalCardResponse
} from 'src/digital-card/modules/card-operations/application/response/digital-card-query-response.interface';
import { StatusCardRequest, CVVRequest } from "src/digital-card/modules/card-operations/application/request/digital-card-request.interface";
import { AuditEventService } from '../../shared/service/audit-event.service';
import EventTracing from '../../shared/common/models/EventTracing';
import { ConfigService } from '@nestjs/config';
import { IUseCaseCommand } from '../interface/use-case-command.interface';
import { ExtraHeaders } from '../interface/extra-headers';
@Injectable()
class DigitalCardQueryServices implements DigitalCardQueryInterface,IUseCaseCommand {
    private readonly clientHeaderKafka: string;
    private readonly epmCode: string;
  constructor(
    private readonly auditEventService: AuditEventService,
    configService: ConfigService
    ) {
        this.clientHeaderKafka = configService.get('DIGITAL-CARD.CLIENT_HEADER_KAFKA');
        this.epmCode = configService.get('DIGITAL-CARD.EPM_CODE');
    }

  execute(requestId: string, extraHeaders: ExtraHeaders): Promise<object> {
  
    throw new Error('Method not implemented.');
  }

  async digitalCardQuery(
    requestId: string,
  ): Promise<DigitalCardQueryServiceResponse> {
    //EventTracing
    const auditRequest: EventTracing = {
        request_id: requestId,
        flow: this.clientHeaderKafka,
        domain: 'SERVICING',
        source: this.clientHeaderKafka,
        date: new Date().toISOString(),
        state: "DEATILS",
        action: 'Get details digital card',
        state_detail: 'COMPLETED',
        state_detail_code: this.epmCode
      };
    this.auditEventService.sendEvent(auditRequest);
    //Build Response
    const response: DigitalCardQueryServiceResponse = {
      data: {
        key: 'Key',
        id: 'Id',
        type: 'VISA',
        alias: 'Tarjeta de Crédito *** 0008',
        display_id: '*** 1001',
        description: 'Tarjeta de Crédito',
        status: 'ACTIVE',
        expiry_date: '118',
        holder_name: 'JUAN SOLEDAD',
        display: '1234567',
        card_description: 'SCOTIA COOL',
      },
      notifications: null,
    };

    return response;
  }
//Update status card digital LOCK, UNLOCK
  async statusDigitalCardQuery(listStatusCardRequest: Array<StatusCardRequest>, requestId: string): Promise<DigitalCardResponse> {

    listStatusCardRequest.forEach(statusCardRequest => {
    //EventTracing
    const auditRequest: EventTracing = {
        request_id: requestId,
        flow: this.clientHeaderKafka,
        domain: 'SERVICING',
        source: this.clientHeaderKafka,
        date: new Date().toISOString(),
        state: statusCardRequest.card_status.status,
        action: 'The digital card is '+ statusCardRequest.card_status.status + ', reason: '+ statusCardRequest.card_status.reason,
        state_detail: 'COMPLETED',
        state_detail_code: this.epmCode
      };
      this.auditEventService.sendEvent(auditRequest);
    });
    //Build Response
    const response: DigitalCardResponse = {
      data: {
        confirmation_no: 'H23456789',
        update_status: 'success',
        update_date_time: '06-11-2024'
      },
      notifications: null,
    };

    return response;
  }

  //Get CVV
  async cvv(cvvRequest: CVVRequest, requestId: string): Promise<DigitalCardResponse> {

    //EventTracing
    const auditRequest: EventTracing = {
        request_id: requestId,
        flow: this.clientHeaderKafka,
        domain: 'SERVICING',
        source: this.clientHeaderKafka,
        date: new Date().toISOString(),
        state: "",
        action: 'The cvv is generated 2343',
        state_detail: 'COMPLETED',
        state_detail_code: this.epmCode
      };
      this.auditEventService.sendEvent(auditRequest);

    //Build Response
    const response: DigitalCardResponse = {
      data: {
        status: '8374',
        fecha_activacion: '06-11-2024',
        fecha_expiracion: '06-11-2024'
      },
      notifications: null,
    };

    return response;
  }
}

export { DigitalCardQueryServices };
