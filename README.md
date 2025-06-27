Gracias, me podrias ayudar con el end point /select , te comparto la clase controller 

@Controller('v2/destination')
@UseGuards(AuthorizationGuard, PermissionsGuard)
class DestinationController extends BaseV2Controller {

  constructor(
    private readonly destinationSelectUseCase: DestinationSelectUseCase,
    private readonly destinationConfirmUseCase: DestinationConfirmUseCase,
    private readonly destinationGeolocationSaveUseCase: DestinationGeolocationSaveUseCase,
    initModuleUseCase: InitModuleUseCase,
    layoutModuleUseCase: LayoutModuleUseCase,
    configService: ConfigService
  ) {
    super(initModuleUseCase, layoutModuleUseCase, JourneyModules.DESTINATION, configService.get('CARD_DELIVERY.SELECT_DESTINATION_HEADER_KAFKA'))
  }

  @HttpCode(302)
  @PreAuthorize(SCOPE_API_WRITE)
  @Post('/select')
  async select(
    @Res() response: Response,
    @Body() request: DestinationSelectRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Cookies('request_id') requestId: string,
  ): Promise<void> {
    const params: UseCaseParams = {
      requestId,
      source: this.source,
      module: this.module
    }
    const useCaseResponse: UseCaseResponse = await this.destinationSelectUseCase.execute(params, request);

    this.prepareResponse(response, useCaseResponse)
  }
  


requiero que agregues Logs ESLM en eventos, asi como  que le agregues lineas de codigo en los eventos que suceden, te comparto la clase con la funcionalidad
  
import { Inject } from '@nestjs/common';
import { IOrchestratorServiceQualifier, OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import { UseCaseParams } from '../../shared/domain/use-case-params';
import { UseCaseResponse } from '../../shared/domain/use-case-response';
import { DestinationSelectRequest } from '../../shared/common/api/model/destinationSelectRequest';
import BusinessProcess from '../../shared/common/models/BusinessProcess';
import { FlowStepException } from '../../shared/common/exceptions/custom-exception';
import { CodeError } from '../../shared/common/constants/code-error.constants';
import { ConfigService } from '@nestjs/config';
import { ActionBaseUseCase } from '../../shared/application/action-base.usecase';
import { IPersonalDataServiceQualifier, PersonalDataInterface } from '../../shared/interface/personal-data.interface';
import { DestinationService } from '../service/destination.service';
import { DestinationInterface } from '../service/interface/destination.Interface';

export class DestinationSelectUseCase extends ActionBaseUseCase {

  private readonly finalStatesConfig: object;

  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) readonly orchestratorService: OrchestratorInterface,
    @Inject(DestinationService) private readonly destinationService: DestinationInterface,
    private readonly configService: ConfigService) {

    super(orchestratorService);
    this.finalStatesConfig = JSON.parse(configService.get('CARD_DELIVERY.DESTINATION_SELECT_FINAL_STATES') ?? '{}');
  }

  async execute(params: UseCaseParams, request: DestinationSelectRequest): Promise<UseCaseResponse> {

    await this.destinationService.validateRequest(request)

    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
      throw new FlowStepException(initResponse.error_message, CodeError.CRD_FLW_001);
    }
    const finalState = this.destinationService.getFinalState(this.finalStatesConfig, request.option)

    await this.destinationService.saveSelectedOption(params.requestId, params.source, request.option)

    return this.getOrchestratorNextStep(params, finalState);
  }
}
