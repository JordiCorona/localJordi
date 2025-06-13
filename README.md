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

-----------------------------------

import { Inject, Injectable, Logger } from '@nestjs/common';
import { UpdateStatusRequest} from 'digital-card-lib/journey/model/updateStatusRequest';
import { StatusBcsService } from '../../../shared/common/services/status.bcs.service';
import { GroupStatusRequestLibDto} from 'digital-card-lib/operation/models/request/group-status-request-lib-dto';
import { CardRequestLibDto } from 'digital-card-lib/operation/models/request/card-request-lib-dto';
import { ResponseLibDto} from 'digital-card-lib/common/models/response/response-lib-dto';

import { UPDATE_STATUS_REASON } from '../../../shared/common/constants/api.environment';
import { EventTracingConstants } from '../../../shared/common/constants/event-tracing.constants';
import { AuditEventService } from '../../../shared/service/audit-event.service';
import { DigitalCardException } from '../../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../../shared/common/models/code-error.model';
import { BasicPersonalRequestLibDto } from 'src/digital-card/shared/common/models/basic-personal-request-lib-dto';
import { Categories } from 'src/digital-card/shared/interface/personal-data.interface';
import { ClientApplicationInformation, IPersonalDataServiceQualifier, PersonalDataInterface } from 'src/digital-card/shared/interface/personal-data.interface';
import { PersonalDataEnum } from 'src/digital-card/shared/common/enums/personal_data.enum';
import { BasicPersonalEnum } from 'src/digital-card/shared/common/enums/basic_personal.enum';
import { AccountPersonalDataList } from 'src/digital-card/shared/common/models/AccountPersonalDataList.dto';

@Injectable()
export class CardOperationsService {
  constructor(private readonly statusBcsService: StatusBcsService,
              private readonly auditService: AuditEventService,
              @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface) {
  }
  private readonly logger = new Logger(CardOperationsService.name);


  async updateStatus(requestId: string, updateStatusRequest: UpdateStatusRequest, x_auth_token?: string, token?: string): Promise<ResponseLibDto> {
    //TODO extraer el key de personal data, por el momento lo recibiremos como parametro
    const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(requestId, PersonalDataEnum.DIGITAL_CARD.toString(),token);
    this.logger.log("personalDataResponse >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "+JSON.stringify(personalDataResponse));
    const basicPersonal: Categories = this.personalDataService.getCategory(personalDataResponse.categories, PersonalDataEnum.BASIC_PERSONAL.toString());

    const productOriginationCat: Categories =this.personalDataService.getCategory(personalDataResponse.categories, PersonalDataEnum.PRODUCT_SUMMARY.toString());

    const cards =productOriginationCat.transaction_data as AccountPersonalDataList;

    const digitalCard= cards.accounts.find(account=> account.product_id.charAt((account.product_id.length)-1)==='D');
    this.logger.log(`Update digital card details with requestId: ${requestId} and key ${digitalCard.account_id}`);
    let request: GroupStatusRequestLibDto = new GroupStatusRequestLibDto();
    request.cards = [];
    request.cards.push(this.mapUpdateRequestToCardRequestLibDto(updateStatusRequest, digitalCard.account_id))
    await this.registerEventUpdateState(requestId, updateStatusRequest.status)

    const basicPersonalRequestLibDto: BasicPersonalRequestLibDto = {
          user: basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()],
          client_id: basicPersonal.transaction_data[BasicPersonalEnum.CLIENT_ID.toString()],
          //Recuperar account id de tarjeta digital en personal data, este dato lo debe guardar previamente la creación de tdc
          account_id: digitalCard.account_id,
          x_auth_token: x_auth_token
        }

    request.basic_personal = basicPersonalRequestLibDto;

    const cardStatusResponse: any =  await this.statusBcsService.cardStatus(request, token);

    this.logger.log("cardStatusResponse >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "+JSON.stringify(cardStatusResponse));

    if ( cardStatusResponse.data===null ) {
      throw new DigitalCardException(`Error updating state to account ${basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()]}`, CodeError.TDG_002)
    }
    return cardStatusResponse;
  }

  private mapUpdateRequestToCardRequestLibDto (updateStatusRequest: UpdateStatusRequest, key: string): CardRequestLibDto {

    return {
      key : key,
      card_type: updateStatusRequest.type,
      card_status: {
        status: updateStatusRequest.status,
        reason: UPDATE_STATUS_REASON
      }
    }
  }

  private async registerEventUpdateState(requestId: string, status: string) {
    let event = this.auditService.createEvent(requestId, status.toUpperCase() == 'UNLOCKED'
      ? EventTracingConstants.STEP_STATUS_ON : EventTracingConstants.STEP_STATUS_OFF);
    await this.auditService.sendEvent(event)
  }
}
