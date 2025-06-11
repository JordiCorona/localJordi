import { Controller, Get, Headers, HttpCode, Post, Res, UseGuards } from '@nestjs/common';
import { Cookies } from '../../../shared/common/decorator/cookies.decorator';
import { REQUEST_ID, SCOPES_DIGITAL_CARD_READ, SCOPES_DIGITAL_CARD_WRITE} from '../../../shared/common/constants/api.environment';
import { CreateService } from '../services/create.service';
import { Response } from 'express';
import { SduiService } from '../../../sdui/infrastructure/sdui.service';
import { SubModuleName } from '../../../shared/common/enums/sub-module-name.enum';
import { SduiBaseController } from '../../../sdui/controller/sdui.base.controller';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';
import { ExtraHeaders } from 'src/digital-card/shared/interface/extra-headers';
import { FeatureToggleDecorator } from '@ib/ib-feature-toggle-lib';
import { DIGITAL_CARD_CREATE_INIT_V1 } from '../../../shared/common/constants/feature.toggle';

@UseGuards(AuthorizationGuard, PermissionsGuard)
@Controller('v1/create')
export class CreateController extends SduiBaseController{

  constructor(
    private readonly createService: CreateService,
    sduiService: SduiService,
  ) {
    super(sduiService, SubModuleName.CREATE)
  }

  @HttpCode(200)
  //@PreAuthorize(SCOPES_DIGITAL_CARD_READ, SCOPES_DIGITAL_CARD_WRITE)
  @Get('/init')
  //@FeatureToggleDecorator(DIGITAL_CARD_CREATE_INIT_V1)
  async init(
    @Cookies(REQUEST_ID) requestId: string
  ) {
    return await this.createService.init(requestId);
  }

  @HttpCode(302)
 /* @PreAuthorize(
    SCOPES_DIGITAL_CARD_READ,
    SCOPES_DIGITAL_CARD_WRITE,
  )*/
  @Post()
  async create(
    @Cookies(REQUEST_ID) requestId: string,
    @Res({ passthrough: true }) res: Response,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('x-otp-token') XOtp: string,
    @Headers('x-auth-token') XAuthToken: string
  ) {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'x-country-code': '',
      'x-otp-token': XOtp,
      'x-auth-token': XAuthToken
    };

    const response  = await this.createService.create(requestId,extraHeaders);

    res.set({ "location":response.next_step });
  }
}
