Me podrias ayudar, estoy realizando escaneo de mi proyecto en checkmarx.
el cual presenta ciertas vulnerabilidades por contraseñas hardcodeadas. como este caso:   'password_hash': 'passwordHash',

    {
      'category_transaction_data': 'CARD_INFORMATION',
      'transaction_data': {
        'cards': [
          {
            'account_number': 'encrypted_value',
            'account_branch_id': '102345',
            'password': 'encrypted_password',
            'password_hash': 'passwordHash',
            'type': '02',
            'delivery': {
              'commission': '180',
              'phone': '32453534',
              'type': 'CUSTOM_ADDRESS|BRANCH'
            }
          }
        ]
      }
    }
  ]
}

Necesito mitigar las vulnerabilidades, me ayudar ?
me podrias dar la solucion. 
ya sea lineas de codigo o reemplazo del valor 

te comparto toda la clase

import { ConfigService } from '@nestjs/config';
import { CardDeliveryCompleteUseCase } from '../../../../src/card-delivery/card-delivery/application/card-delivery-complete.usecase';
import { FlowConfig } from '../../../../src/card-delivery/shared/domain/flow-config';
import { GlobalModuleConfigurationInterface } from '../../../../src/card-delivery/shared/interface/global-module-configuration.interface';
import { LibStepsInterface } from '../../../../src/card-delivery/shared/interface/lib-steps.interface';
import { DELIVERY_CARD_DELIVERY, INIT_CARD_DELIVERY } from '../../shared/common/constants/orchestrator-mock-steps';
import { ExtraHeaders } from '../../shared/interface/extra-headers';
import { OrchestratorInterface } from '../../shared/interface/orchestrator.interface';
import { ClientApplicationInformation, PersonalDataInterface } from '../../shared/interface/personal-data.interface';

describe('CardDeliveryCompleteUseCase', () => {

  const globalModuleConfigurationService: jest.Mocked<GlobalModuleConfigurationInterface> = {
    getConfiguration: jest.fn(),
    getInitialStep: jest.fn(),
    getStep: jest.fn(),
    getNextStep: jest.fn(),
    needValidateOtpSignatures: jest.fn()
  };

  const personalDataService: jest.Mocked<PersonalDataInterface> = {
    getByRequestId: jest.fn(),
    save: jest.fn(),
    saveCategory: jest.fn(),
    getCategory: jest.fn()
  };

  const mockOrchestratorService: jest.Mocked<OrchestratorInterface> = {
    init: jest.fn(),
    delivery: jest.fn(),
  };

  const libStepsService: jest.Mocked<LibStepsInterface> = {
    clientApplicationInformationMapToLibApplicationData: jest.fn(),
    libApplicationDataMapToClientApplicationInformation: jest.fn(),
    executeStep: jest.fn()
  };

  let extraHeaders: ExtraHeaders;
  let configService: ConfigService;

  beforeAll(() => {
    configService = new ConfigService();
    configService.set('CARD_DELIVERY.CRIENVTDDA', '1111111111111111');
    configService.set('CARD_DELIVERY.CRIENVTDDB', '1111111111111111');
  });

  test('use case success operation without nex-steps', async () => {

    // Arrange
    jest.spyOn(mockOrchestratorService, 'init').mockImplementation(jest.fn(() => Promise.resolve(INIT_CARD_DELIVERY)));
    jest.spyOn(personalDataService, 'getByRequestId').mockImplementation(() => Promise.resolve(responseApplicationData));
    jest.spyOn(globalModuleConfigurationService, 'getConfiguration').mockImplementation(() => flowConfiguration);
    jest.spyOn(globalModuleConfigurationService, 'getStep').mockImplementation(() => flowConfiguration.steps[1]); // step without next-steps
    jest.spyOn(mockOrchestratorService, 'delivery').mockImplementation(jest.fn(() => Promise.resolve(DELIVERY_CARD_DELIVERY)));

    const useCase = new CardDeliveryCompleteUseCase(
      personalDataService,
      mockOrchestratorService,
      globalModuleConfigurationService,
      libStepsService,
      configService
    );

    // Act
    const result = await useCase.execute('some-request-id', 'some-module', extraHeaders);

    // Assert
    expect(result).not.toBeNull();
  });

  test('use case success operation with nex-steps', async () => {

    // Arrange
    jest.spyOn(mockOrchestratorService, 'init').mockImplementation(jest.fn(() => Promise.resolve(INIT_CARD_DELIVERY)));
    jest.spyOn(personalDataService, 'getByRequestId').mockImplementation(() => Promise.resolve(responseApplicationData));
    jest.spyOn(globalModuleConfigurationService, 'getConfiguration').mockImplementation(() => flowConfiguration);
    jest.spyOn(globalModuleConfigurationService, 'getStep').mockImplementation(() => flowConfiguration.steps[0]); // step with next-steps
    jest.spyOn(globalModuleConfigurationService, 'getNextStep').mockImplementation(() => flowConfiguration.steps[0]); // step with next-steps
    jest.spyOn(mockOrchestratorService, 'delivery').mockImplementation(jest.fn(() => Promise.resolve(DELIVERY_CARD_DELIVERY)));

    const useCase = new CardDeliveryCompleteUseCase(
      personalDataService,
      mockOrchestratorService,
      globalModuleConfigurationService,
      libStepsService,
      configService
    );

    // Act
    const result = await useCase.execute('some-request-id', 'some-module', extraHeaders);

    // Assert
    expect(result).not.toBeNull();
  });
});

const responseApplicationData: ClientApplicationInformation = {
  'request_id': '{{requestId}}',
  'categories': [
    {
      'category_transaction_data': 'BASIC_PERSONAL',
      'transaction_data': {
        'client_id': 'CIF_USUARIO'
      }
    },
    {
      'category_transaction_data': 'REGULATORY',
      'transaction_data': {
        'branch_code': '242001',
        'branch_name': 'SUCURSAL CIUDAD DE MEXICO',
        'branch_address': 'Miguel Ángel De Quevedo 478, Col. Barrio Santa Catarina, Coyoacan'
      }
    },
    {
      'category_transaction_data': 'RESIDENCE_ADDRESS',
      'transaction_data': {
        'streetName': 'CALLE MIGUEL HIDALGO',
        'externalNumber': '67',
        'interiorNumber': 'A',
        'neighborhoodOcr': 'BELLAVISTA',
        'neighborhoodCode': '54',
        'neighborhood': 'BELLAVISTA',
        'municipallyCode': '2',
        'municipallyDescription': 'MEXICALI',
        'cityCode': '2',
        'cityDescription': 'MEXICALI',
        'stateCode': 2,
        'stateDescription': 'BAJA CALIFORNIA',
        'countryCode': 'MX',
        'countryDescription': 'MEXICO',
        'postalCode': '21150'
      }
    },
    {
      'category_transaction_data': 'CARD_INFORMATION',
      'transaction_data': {
        'cards': [
          {
            'account_number': 'encrypted_value',
            'account_branch_id': '102345',
            'password': 'encrypted_password',
            'password_hash': 'passwordHash',
            'type': '02',
            'delivery': {
              'commission': '180',
              'phone': '32453534',
              'type': 'CUSTOM_ADDRESS|BRANCH'
            }
          }
        ]
      }
    }
  ]
}

const flowConfiguration: FlowConfig = {
  'name': 'CD',
  'initial_step': "expression:: |#{ card_information.delivery.type == 'BRANCH' ? 'successfully-processed-branch' : 'successfully-processed-address' }",
  'steps': [
    {
      'name': 'loader-processing',
      'layout': 'card-delivery/finish-process-address.json',
      'is_loader': true,
      'final_state': null,
      'next_steps': [
        {
          'expression': "expression:: |#{card_information.delivery.type == 'ADDRESS'}",
          'step': 'successfully-processed-address'
        },
        {
          'expression': "expression:: |#{card_information.delivery.type == 'BRANCH'}",
          'step': 'successfully-processed-branch'
        }
      ]
    },
    {
      'name': 'successfully-processed-address',
      'layout': 'card-delivery/finish-process-address.json',
      'is_loader': false,
      'final_state': 'CARD_DELIVERY_COMPLETED',
      'next_steps': []
    },
    {
      'name': 'successfully-processed-branch',
      'layout': 'card-delivery/finish-process-branch.json',
      'is_loader': false,
      'final_state': 'CARD_DELIVERY_COMPLETED',
      'next_steps': []
    }
  ]
}
