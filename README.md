ayudame con la modificacion de este bloque de codigo en el Yaml, actualmente se encuentra de esta forma 
responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
              examples:
                MX_LayResE1:
                  value:
                    layout:
                      screenName: screen_name_analytics,
                      navigation:
                        title: Tarjeta digital
                        theme: primary
                      screens:
                        - type: Modal
                          event: load
                          data:
                            header: ¿Eres cliente?
                            title: ¿Eres cliente?
                            body:
                              text: Ya eres cliente si tienes alguna cuenta, crédito o préstamo.
                            buttons:
                              labelButtonPrimary: No soy cliente
                              labelButtonSecondary: Soy cliente
                              actionPrimary: SaveIsNotClient
                              actionSecondary: SaveIsClient
                              alwaysStackButtons: true
                      components:
                        - type: Container
                          data:
                            components:
                              - type: ModalDialogue
                                data:
                                  title: ¿Eres cliente?
                                  text: Ya eres cliente si tienes alguna cuenta, crédito o préstamo.
                                  primaryButtonText: No soy cliente
                                  secondaryButtonText: Soy cliente
                                  primaryButtonAction: SaveIsNotClient
                                  secondaryButtonAction: SaveIsClient
                                  alwaysStackButtons: true
                                  isVisible: true
                              - type: Text
                                data:
                                  text: Comenzaremos con tu solicitud
                                  component: h1
                                  font: headline24
                                  alignment: start
                                  margins:
                                    bottom: 30
                              - type: Form
                                data:
                                  submit: SendCurp
                                  components:
                                    - type: TextField
                                      data:
                                        name: curp
                                        labelText: CURP
                                        type: text
                                        fieldType: password
                                        defaultValue: ''
                                        placeholder: 18 caracteres
                                        trackingEnabled: true
                                        isRequired: true
                                        errorMsg: El campo es requerido
                                        validations:
                                          - validator: RequiredValidator
                                            errorMessage: El campo es requerido
                                          - value: '18'
                                            validator: MaxLengthValidator
                                            errorMessage: Debe tener 18 caracteres
                                          - value: '18'
                                            validator: MinLengthValidator
                                            errorMessage: Debe tener 18 caracteres
                                          - value: ^[A-Z][AEIOUX][A-Z]{2}[0-9]{2}(0[1-9]|1[0-2])(0[1-9]|1[0-9]|2[0-9]|3[0-1])[HM](AS|BC|BS|CC|CS|CH|CL|CM|DF|DG|GT|GR|HG|JC|MC|MN|MS|NT|NL|OC|PL|QT|QR|SP|SL|SR|TC|TS|TL|VZ|YN|ZS|NE)[B-DF-HJ-NP-TV-Z]{3}[0-9A-Z][0-9]$
                                            validator: RegexValidator
                                            errorMessage: Revisa que sea correcto
                                          - value: '18'
                                            validator: ValidateAgeMinFromCurpValidator
                                            errorMessage: Debes ser mayor de 18 años
                                          - value: '105'
                                            validator: ValidateAgeMaxFromCurpValidator
                                            errorMessage: Debes ser menor de 105 años
                                        tooltip:
                                          id: CURP
                                          closeButtonLabel: Entendido
                                          header: CURP
                                          buttonTitle: Entendido
                                          body:
                                            richText: Encuéntrala al frente de tu INE o IFE o [a url="https://www.gob.mx/curp/"]consúltala con tus datos personales.[/a]
                                            text: Encuéntrala al frente de tu INE o IFE o consúltala con tus datos personales.
                                            linkText: consúltala con tus datos personales.
                                            type: emphasis
                                            linkURL: https://www.gob.mx/curp/
                                            target: _blank
                                            image:
                                              srcImage: idCurp.svg
                                              width: 312
                                              height: 150
                                              alt: Curp
                                              align: center
                                    - type: Consent
                                      data:
                                        consentId: Mx_curp
                                    - type: BottomNavigation
                                      data:
                                        nextButton:
                                          text: Continuar
                                          type: continue
                                          action: SendCurp
                                          isDisabled: true
                MX_LayResE2:
                  value:
                    layout:
                      navigation:
                        title: Datos de contacto
                        theme: secondary
                      components:
                        - type: Container
                          data:
                            components:
                              - type: Text
                                data:
                                  text: Confirma tu celular
                                  component: h1
                                  font: headline24
                                  alignment: start
                                  margins:
                                    bottom: 30
                              - type: Text
                                data:
                                  text: Ingresa el código que hemos enviado a tu celular ${CELLPHONE}.
                                  component: p
                                  font: body2
                                  alignment: start
                                  margins:
                                    bottom: 36
                              - type: Form
                                data:
                                  components:
                                    - type: PinCode
                                      data:
                                        name: code
                                        labelText: Código
                                        regEx: '[0-9]*'
                                        type: text
                                        maxLength: 6
                                        minLength: 6
                                        isRequired: true
                                        placeholder: ''
                                        trackingEnabled: true
                                        defaultValue: ''
                                        errorRegExMsg: Debe tener 6 dígitos
                                        margins:
                                          bottom: 36
                                        validations:
                                          - validator: RequiredValidator
                                            errorMessage: Debe tener 6 dígitos
                                          - value: '6'
                                            validator: MaxLengthValidator
                                            errorMessage: Debe tener 6 dígitos
                                          - value: '6'
                                            validator: MinLengthValidator
                                            errorMessage: Debe tener 6 dígitos
                                          - value: '[0-9]*'
                                            validator: RegexValidator
                                            errorMessage: Debe tener 6 dígitos
                                    - type: CountdownButton
                                      data:
                                        name: count
                                        labelText: Reenviar código
                                        seconds: 60
                                        action: reSendOtpCellphone
                                        margins:
                                          bottom: 42
                                    - type: Consent
                                      data:
                                        consentId: iden_otp_cell_facial
                                        margins:
                                          bottom: 48
                                    - type: BottomNavigation
                                      data:
                                        backButton:
                                          text: Atrás
                                          type: back
                                          action: backToContactInfo
                                        nextButton:
                                          text: Continuar
                                          type: continue
                                          action: verificationOtpCellphone

el cual es este json de respuesta.

{
  "layout": {
    "screenName": "screen_name_analytics,",
    "navigation": {
      "title": "Tarjeta digital",
      "theme": "primary"
    },
    "screens": [
      {
        "type": "Modal",
        "event": "load",
        "data": {
          "header": "¿Eres cliente?",
          "title": "¿Eres cliente?",
          "body": {
            "text": "Ya eres cliente si tienes alguna cuenta, crédito o préstamo."
          },
          "buttons": {
            "labelButtonPrimary": "No soy cliente",
            "labelButtonSecondary": "Soy cliente",
            "actionPrimary": "SaveIsNotClient",
            "actionSecondary": "SaveIsClient",
            "alwaysStackButtons": true
          }
        }
      }
    ],
    "components": [
      {
        "type": "Container",
        "data": {
          "components": [
            {
              "type": "ModalDialogue",
              "data": {
                "title": "¿Eres cliente?",
                "text": "Ya eres cliente si tienes alguna cuenta, crédito o préstamo.",
                "primaryButtonText": "No soy cliente",
                "secondaryButtonText": "Soy cliente",
                "primaryButtonAction": "SaveIsNotClient",
                "secondaryButtonAction": "SaveIsClient",
                "alwaysStackButtons": true,
                "isVisible": true
              }
            },
            {
              "type": "Text",
              "data": {
                "text": "Comenzaremos con tu solicitud",
                "component": "h1",
                "font": "headline24",
                "alignment": "start",
                "margins": {
                  "bottom": 30
                }
              }
            },
            {
              "type": "Form",
              "data": {
                "submit": "SendCurp",
                "components": [
                  {
                    "type": "TextField",
                    "data": {
                      "name": "curp",
                      "labelText": "CURP",
                      "type": "text",
                      "fieldType": "password",
                      "defaultValue": "",
                      "placeholder": "18 caracteres",
                      "trackingEnabled": true,
                      "isRequired": true,
                      "errorMsg": "El campo es requerido",
                      "validations": [
                        {
                          "validator": "RequiredValidator",
                          "errorMessage": "El campo es requerido"
                        },
                        {
                          "value": "18",
                          "validator": "MaxLengthValidator",
                          "errorMessage": "Debe tener 18 caracteres"
                        },
                        {
                          "value": "18",
                          "validator": "MinLengthValidator",
                          "errorMessage": "Debe tener 18 caracteres"
                        },
                        {
                          "value": "^[A-Z][AEIOUX][A-Z]{2}[0-9]{2}(0[1-9]|1[0-2])(0[1-9]|1[0-9]|2[0-9]|3[0-1])[HM](AS|BC|BS|CC|CS|CH|CL|CM|DF|DG|GT|GR|HG|JC|MC|MN|MS|NT|NL|OC|PL|QT|QR|SP|SL|SR|TC|TS|TL|VZ|YN|ZS|NE)[B-DF-HJ-NP-TV-Z]{3}[0-9A-Z][0-9]$",
                          "validator": "RegexValidator",
                          "errorMessage": "Revisa que sea correcto"
                        },
                        {
                          "value": "18",
                          "validator": "ValidateAgeMinFromCurpValidator",
                          "errorMessage": "Debes ser mayor de 18 años"
                        },
                        {
                          "value": "105",
                          "validator": "ValidateAgeMaxFromCurpValidator",
                          "errorMessage": "Debes ser menor de 105 años"
                        }
                      ],
                      "tooltip": {
                        "id": "CURP",
                        "closeButtonLabel": "Entendido",
                        "header": "CURP",
                        "buttonTitle": "Entendido",
                        "body": {
                          "richText": "Encuéntrala al frente de tu INE o IFE o [a url=\"https://www.gob.mx/curp/\"]consúltala con tus datos personales.[/a]",
                          "text": "Encuéntrala al frente de tu INE o IFE o consúltala con tus datos personales.",
                          "linkText": "consúltala con tus datos personales.",
                          "type": "emphasis",
                          "linkURL": "https://www.gob.mx/curp/",
                          "target": "_blank",
                          "image": {
                            "srcImage": "idCurp.svg",
                            "width": 312,
                            "height": 150,
                            "alt": "Curp",
                            "align": "center"
                          }
                        }
                      }
                    }
                  },
                  {
                    "type": "Consent",
                    "data": {
                      "consentId": "Mx_curp"
                    }
                  },
                  {
                    "type": "BottomNavigation",
                    "data": {
                      "nextButton": {
                        "text": "Continuar",
                        "type": "continue",
                        "action": "SendCurp",
                        "isDisabled": true
                      }
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    ]
  }
}

ayudame a modificar el codigo de tal forma que quede de esta manera 
{
    "layout": {
        "analytics": {
            "events": [
                {
                    "eventId": "product_flow",
                    "event": "load",
                    "parameters": [
                        {
                            "key": "meta_event",
                            "value": "activacion",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "activateCard",
                    "promotionCreative": "slide-activar-tarjeta",
                    "zoneId": "boton-slider",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "activacion-tdc",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "activacion",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "successActivateCard",
                    "promotionCreative": "success-activar-tarjeta",
                    "zoneId": "success-activar-tarjeta",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "activacion-tdc",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "tarjeta_activada",
                            "isPersisted": false
                        }
                    ]
                }
            ],
            "parameters": [
                {
                    "key": "step_name",
                    "value": "activacion",
                    "isPersisted": false
                },
                {
                    "key": "segment",
                    "value": "",
                    "isPersisted": true
                },
                {
                    "key": "card",
                    "value": "VISA CLÁSICA",
                    "isPersisted": true
                },
                {
                    "key": "request_id",
                    "value": "",
                    "isPersisted": true
                },
                {
                    "key": "flow_name",
                    "value": "activacion-tdc",
                    "isPersisted": false
                },
                {
                    "key": "environment",
                    "value": "",
                    "isPersisted": true
                },
                {
                    "key": "operating_system",
                    "value": "",
                    "isPersisted": true
                }
            ]
        },
        "navigation": {
            "title": "Tarjeta digital"
        },
        "components": [
            {
                "type": "Container",
                "data": {
                    "components": [
                        {
                            "type": "Image",
                            "data": {
                                "srcImage": "digitalCard.svg",
                                "width": 48,
                                "height": 48,
                                "alt": "",
                                "align": "center",
                                "margins": {
                                    "top": 60
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "Activa tu\nTarjeta de Crédito digital",
                                "component": "h1",
                                "font": "headline24",
                                "alignment": "center",
                                "margins": {
                                    "top": 18,
                                    "bottom": 12
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "Podrás consultar el número, vigencia y CVV dinámico para tus compras en línea.",
                                "component": "p",
                                "font": "body2",
                                "alignment": "center",
                                "margins": {
                                    "bottom": 30
                                }
                            }
                        },
                        {
                            "type": "Spacer",
                            "data": {}
                        },
                        {
                            "type": "SlideToComplete",
                            "data": {
                                "title": "activate",
                                "isEnabled": true,
                                "hint": null,
                                "name": "ActivateCard",
                                "isVisible": true,
                                "isDisabledTimer": true,
                                "action": "activateCard",
                                "normalState": {
                                    "state": "normal",
                                    "value": "Activar tarjeta"
                                },
                                "slidingState": {
                                    "state": "sliding",
                                    "value": "Activar tarjeta"
                                },
                                "loadingState": {
                                    "state": "loading",
                                    "value": "Solicitando con e-llave"
                                },
                                "successState": {
                                    "state": "success",
                                    "value": "Listo"
                                },
                                "failureState": {
                                    "state": "failure",
                                    "value": "Error al activar"
                                }
                            },
                            "margins": {
                                "bottom": 0
                            }
                        }
                    ]
                }
            }
        ]
    }
}
