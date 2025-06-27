Gracias, me podrias ayudar con el end point /save, te comparto la clase controller 

  @HttpCode(200)
  @PreAuthorize(SCOPE_API_WRITE)
  @Post('/save')
  async save(
    @Res() response: Response,
    @Body() request: AddressCaptureRequest,
    @Headers('x-channel-id') XChannelId: string = 'MOBILE',
    @Headers('x-originating-appl-code') XOriginatingApplCode: string = 'IB',
    @Headers('path') path: string,
    @Cookies('request_id') requestId: string
  ): Promise<object | void> {
    const params: UseCaseParams = {
      requestId,
      source: this.source,
      module: this.module,
      subModule: path
    }
    const useCaseResponse: UseCaseResponse = await this.addressCaptureSaveUseCase.execute(params, request);

    this.prepareResponse(response, useCaseResponse)
  }

  
  requiero que agregues Logs ESLM en eventos, asi como  que le agregues lineas de codigo en los eventos que suceden, te comparto la clase con la funcionalidad
  
import { UseCaseParams } from '../../shared/domain/use-case-params';
import { AddressCaptureRequest } from '../../shared/common/api/model/addressCaptureRequest';
import { AddressCaptureSave } from './interface/address-capture-save.interface';
import { Inject, Injectable } from '@nestjs/common';
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

@Injectable()
class AddressCaptureSaveUseCase extends ActionBaseUseCase implements AddressCaptureSave {
  constructor(
    @Inject(IPersonalDataServiceQualifier) private readonly personalDataService: PersonalDataInterface,
    @Inject(IOrchestratorServiceQualifier) orchestratorService: OrchestratorInterface,
    @Inject(IGlobalModuleConfigurationQualifier) globalModuleConfigurationService: GlobalModuleConfigurationInterface,
    @Inject(LibStepsService) private readonly libStepsService: LibStepsInterface) {

    super(orchestratorService, globalModuleConfigurationService);
  }

  async execute(params: UseCaseParams, request: AddressCaptureRequest): Promise<UseCaseResponse> {
    const initResponse: BusinessProcess = await this.orchestratorService.init(params.requestId, params.source);
    if (initResponse.error) {
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

    return this.getNextStepResponse(flowConfig, currentStep, params);
  }
}

export {
  AddressCaptureSaveUseCase
}
