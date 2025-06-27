Gracias, requiero que me ayudes con los Logs de   @Get('/init')   @Get('/capture/layout') te comparto la clase controller y clase service  en donde esta la logica de su funcion 
/init ya tiene una implementacion de logs, me podrias apoyar con layout, adicional podrias agregar comentarios para validar que es lo que realiza cada bloque de codigo por favor


import { Response } from 'express';
import { InitResponseDto } from 'src/card-delivery/shared/common/api/model/initResponseDto';
import { SCOPE_API_READ } from 'src/card-delivery/shared/common/constants/journey-constants';
import { Headers, Controller, Get, HttpCode, Inject, UseGuards, Post, Body, Res, Req } from '@nestjs/common';
import { AuthorizationGuard, PermissionsGuard, PreAuthorize } from '@ib/ib-security-nestjs-lib';
import { JourneyServiceException } from 'src/card-delivery/shared/common/exceptions/custom-exception';
import { ExtraHeaders } from 'src/card-delivery/shared/interface/extra-headers';
import { CardPasswordDeliveryRequest } from '../../shared/common/api/model/cardPasswordDeliveryRequest';
import { Cookies } from '../../shared/common/decorator/cookies.decorator';
import { CardPasswordDeliveryUsecase, CardPasswordDeliveryUseCaseQualifier } from '../application/card-password-delivery.usecase';
import { CardPasswordUseCase, CardPasswordUseCaseQualifier } from '../application/card-password.usecase';
import { SignatureRequestDto } from '@ib/ib-mx-security-signatures-lib/dto/signatureRequestDto';

@Controller('v1/card-password')
@UseGuards(AuthorizationGuard, PermissionsGuard)
export class CardPasswordController {

  constructor(
    @Inject(CardPasswordUseCaseQualifier) private readonly cardPasswordUseCaseCommand: CardPasswordUseCase,
    @Inject(CardPasswordDeliveryUseCaseQualifier) private readonly cardPasswordDeliveryUseCaseCommand: CardPasswordDeliveryUsecase,
  ) {
  }

  @HttpCode(200)
  @PreAuthorize(SCOPE_API_READ)
  @Get('/init')
  async init(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
  ): Promise<InitResponseDto | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
    };

    const response = await this.cardPasswordUseCaseCommand.execute(requestId, extraHeaders);

    return response;
  }

  /**
   * Returns the layout to capture the nip of card
   * @param XChannelId 'MOBILE'
   * @param XOriginatingApplCode 'IB'
   * @param requestId 'example: 019283740192928374653'
   * @returns
   */
  @HttpCode(200)
  @PreAuthorize(SCOPE_API_READ)
  @Get('/capture/layout')
  async capture(
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
  ): Promise<InitResponseDto | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
    };

    const response = await this.cardPasswordUseCaseCommand.capture(requestId, extraHeaders);

    return response;
  }

  @HttpCode(302)
  @PreAuthorize(SCOPE_API_READ)
  @Post('/delivery')
  async delivery(
    @Res() response: Response,
    @Body() request: CardPasswordDeliveryRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('x-otp-token') XOtp: string,
    @Headers('x-auth-token') xAuthToken: string,
    @Cookies('request_id') requestId: string,
    @Req() req : Request
  ): Promise<void | JourneyServiceException> {
    const extraHeaders: ExtraHeaders = {
      'x-channel-id': XChannelId,
      'x-originating-appl-code': XOriginatingApplCode,
      'x-otp-token': XOtp,
      'x-auth-token': xAuthToken
    };

    const params = {
      headers: { 'x-otp-token': XOtp },
      req: { 'cookies': { 'request_id': requestId } }
    }

    extraHeaders.authorization = req.headers['Authorization'];

    await SignatureRequestDto.validateParams(params);

    return this.cardPasswordDeliveryUseCaseCommand.execute(requestId, request, extraHeaders)
      .then(result => {
        response.setHeader('location', result.url);
        response.end();
      });

  }


}


-----------------------------------

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
export const CardPasswordDeliveryUseCaseQualifier = Symbol('CardPasswordDeliveryUseCaseQualifier');

@Injectable()
export class CardPasswordDeliveryUsecase {
  private readonly logger = new Logger(CardPasswordDeliveryUsecase.name);
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
    const personalDataResponse: ClientApplicationInformation = await this.personalDataService.getByRequestId(requestId, this.source)

    const basicPersonal: Categories = await this.personalDataService.getCategory(personalDataResponse.categories, ClientAppDataCategory.BASIC_PERSONAL);
    await IbSecuritySignatures.validateEllave(
      this.ibSecuritySignaturesLibFactory, extraHeaders, requestId, this.moduleName, this.country, new Object(basicPersonal) as Categories);

    const initResponse: BusinessProcess = await this.orchestratorService.init(requestId, this.source)

    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }

    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, 'cardPassword');

    await this.libStepsService.executeStep({
      stepName: moduleConfig.steps.delivery.stepName,
      personalDataResponse: personalDataResponse,
      flow: initResponse.flow,
      source: this.source,
      request: request,
      headers: new Map(Object.entries(extraHeaders))
    })

    const deliveryResponse: BusinessProcess = await this.orchestratorService.delivery(requestId, this.finalState, this.source)

    if (deliveryResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_002);
    }

    return { url: deliveryResponse.next_step }
  }

}


