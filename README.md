Gracias, requiero que me ayudes con los Logs de   @Get('/init')   @Get('/capture/layout') te comparto la clase controller y clase service  en donde esta la logica de su funcion Add commentMore actions
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


----------------------------------------


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
  private readonly moduleProps={name:'cardPassword'};
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
    this.loggerESLM.log(`Start CardPassword INIT - requestId: ${requestId}`, BUSINESS_TAG);
    const initResponse: BusinessProcess = await this.orchestratorClient.init(requestId, this.kafkaHeader);

    if (initResponse.error) {
      this.loggerESLM.error(`CardPassword INIT failed - requestId: ${requestId} - ${initResponse.error_message}`, BUSINESS_TAG_FAILED);
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }
    const moduleConfig = this.moduleConfigurationService.getAppFlowConfigModule(initResponse.flow, this.moduleProps.name);
    const initalStep = moduleConfig.steps[moduleConfig.initialStep]

    this.loggerESLM.log(`CardPassword INIT success - requestId: ${requestId}`, BUSINESS_TAG);

    const layout: object = await this.serverDrivenUiService.execute(requestId, initalStep.layout);
    return this.getInitResponseDto(layout, initalStep.nextStep);
  }

  getInitResponseDto(layout: object, next_step?:string):InitResponseDto{
    const initResponseDto:InitResponseDto = new InitResponseDto();

    initResponseDto.data = new InitResponseDtoData();
    initResponseDto.data.layout = layout;

    if(next_step!==undefined){
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

    const initResponseDto:InitResponseDto = this.getInitResponseDto(layout, moduleConfig.steps.capture.nextStep);

    return initResponseDto;
  }

}
