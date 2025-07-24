necesito que me ayudes a modificar este json.

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
                            "value": "detalle-tarjeta",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "copyText",
                    "promotionCreative": "copiar-numero",
                    "zoneId": "datos-tarjeta",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "detalle-tarjeta",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "tapCVV",
                    "promotionCreative": "generar-cvv-dinamico",
                    "zoneId": "datos-tarjeta",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "detalle-tarjeta",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "showTurnOn",
                    "promotionCreative": "toggle",
                    "zoneId": "encendido-tarjeta",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "detalle-tarjeta",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "turnOn",
                    "promotionCreative": "prender-y-generar-cvv",
                    "zoneId": "opciones",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "tarjeta-apagada",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "showTurnOff",
                    "promotionCreative": "toggle",
                    "zoneId": "apagado-tarjeta",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "detalle-tarjeta",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "turnOff",
                    "promotionCreative": "apagar-y-generar-cvv",
                    "zoneId": "opciones",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "tarjeta-prendida",
                            "isPersisted": false
                        }
                    ]
                },
                {
                    "eventId": "journey_click",
                    "action": "dismissSheet",
                    "promotionCreative": "por-ahora-no",
                    "zoneId": "opciones",
                    "parameters": [
                        {
                            "key": "flow_name",
                            "value": "tarjeta-digital",
                            "isPersisted": false
                        },
                        {
                            "key": "step_name",
                            "value": "opciones",
                            "isPersisted": false
                        }
                    ]
                }
            ],
            "parameters": [
                {
                    "key": "step_name",
                    "value": "detalle-tarjeta",
                    "isPersisted": false
                },
                {
                    "key": "segment",
                    "value": "",
                    "isPersisted": false
                },
                {
                    "key": "card",
                    "value": "VISA CLÁSICA",
                    "isPersisted": false
                },
                {
                    "key": "request_id",
                    "value": "",
                    "isPersisted": false
                },
                {
                    "key": "flow_name",
                    "value": "tarjeta-digital",
                    "isPersisted": false
                },
                {
                    "key": "environment",
                    "value": "",
                    "isPersisted": false
                },
                {
                    "key": "operating_system",
                    "value": "",
                    "isPersisted": false
                }
            ]
        },
        "navigation": {
            "title": "Tarjeta digital"
        },
        "screens": [
            {
                "type": "BottomSheet",
                "event": "showTurnOff",
                "data": {
                    "header": "Al apagarla no podrás hacer compras",
                    "title": "Al apagarla no podrás hacer compras.",
                    "body": {
                        "text": "Los cargos recurrentes se harán con normalidad."
                    },
                    "buttons": {
                        "labelButtonPrimary": "Apagar",
                        "labelButtonSecondary": "Ahora no",
                        "actionPrimary": "turnOff",
                        "actionSecondary": "dismissSheet",
                        "alwaysStackButtons": true
                    }
                }
            },
            {
                "type": "BottomSheet",
                "event": "showTurnOn",
                "data": {
                    "header": "Al prenderla ya se podrán hacer compras",
                    "title": "Al prenderla ya se podrán hacer compras",
                    "body": {
                        "text": "Los cargos recurrentes se harán con normalidad."
                    },
                    "buttons": {
                        "labelButtonPrimary": "Prender",
                        "labelButtonSecondary": "Ahora no",
                        "actionPrimary": "turnOn",
                        "actionSecondary": "dismissSheet",
                        "alwaysStackButtons": true
                    }
                }
            },
            {
                "type": "BottomSheet",
                "event": "screenTapCVV",
                "data": {
                    "header": "Tu tarjeta está apagada",
                    "title": "Tu tarjeta está apagada",
                    "body": {
                        "text": "Para generar tu código de seguridad prende tu tarjeta digital."
                    },
                    "buttons": {
                        "labelButtonPrimary": "Prender y generar CVV",
                        "labelButtonSecondary": "Por ahora no",
                        "actionPrimary": "turnOn",
                        "actionSecondary": "dismissSheet",
                        "alwaysStackButtons": true
                    }
                }
            },
            {
                "type": "SnackBar",
                "event": "copyText",
                "data": {
                    "header": "Número de tarjeta copiado",
                    "body": {
                        "text": "Número de tarjeta copiado"
                    },
                    "buttons": {}
                }
            }
        ],
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
                                    "top": 18
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "Número de tarjeta digital",
                                "component": "p",
                                "font": "body1",
                                "alignment": "center",
                                "margins": {
                                    "top": 12
                                }
                            }
                        },
                        {
                            "type": "StyledTextWithIcon",
                            "data": {
                                "text": "ZP4UPX2gvTOATzf/B4xM2ZeO63rOkp42HUUA/5hOR80=UNR9L5qCYA9wZXHq0tY9+sbfvQ==",
                                "srcImage": "copy.svg",
                                "iconHeight": 21,
                                "iconWidth": 21,
                                "iconAlignment": "end",
                                "font": "headline21",
                                "component": "h1",
                                "alignment": "center",
                                "action": "copyText",
                                "margins": {
                                    "top": 12
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "richText": "Vigencia: [b]05/30[/b]",
                                "component": "p",
                                "font": "body1",
                                "alignment": "center",
                                "margins": {
                                    "top": 24,
                                    "bottom": 25
                                }
                            }
                        },
                        {
                            "type": "CVVDigitalCard",
                            "data": {
                                "generateCVVTextState": {
                                    "text": "Generar CVV dinámico",
                                    "component": "h1",
                                    "font": "subtitle2"
                                },
                                "generatingCVVTextState": {
                                    "text": "Generando...",
                                    "component": "h1",
                                    "font": "subtitle2"
                                },
                                "showCVVTextState": {
                                    "title": "El CVV es único para cada compra",
                                    "timerText": "Expirará en",
                                    "componentTitle": "p",
                                    "fontTitle": "caption",
                                    "componentTimer": "h1",
                                    "fontTimer": "subtitle2"
                                },
                                "name": "switchTDC",
                                "action": "tapCVV",
                                "screenAction": "screenTapCVV",
                                "time": 120,
                                "alignment": "center",
                                "margins": {
                                    "top": 25,
                                    "bottom": 28
                                }
                            }
                        },
                        {
                            "type": "LineSeparator",
                            "data": {
                                "type": "solid",
                                "height": 4,
                                "margins": {
                                    "top": 0,
                                    "start": 0,
                                    "end": 0,
                                    "bottom": 0
                                }
                            }
                        },
                        {
                            "type": "ToggleDescription",
                            "data": {
                                "name": "switchTDC",
                                "toggleChecked": true,
                                "cardType": "CREDIT",
                                "textOn": "Tu tarjeta está prendida",
                                "textOff": "Tu tarjeta está apagada",
                                "textSwitchingOn": "Encendiendo...",
                                "textSwitchingOff": "Apagando...",
                                "turnOnCard": "turnOn",
                                "turnOffCard": "turnOff",
                                "isDisabled": false,
                                "progressEnabled": true,
                                "screenAction": "showTurnOff",
                                "action": "showTurnOff",
                                "font": "body2",
                                "component": "p",
                                "countAppear": 3,
                                "keyPreference": "timesOnOffCard",
                                "margins": {
                                    "top": 20,
                                    "bottom": 20
                                }
                            }
                        },
                        {
                            "type": "LineSeparator",
                            "data": {
                                "type": "solid",
                                "height": 4,
                                "margins": {
                                    "top": 0,
                                    "start": 0,
                                    "end": 0,
                                    "bottom": 0
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "Marca",
                                "component": "p",
                                "font": "body1",
                                "alignment": "leading",
                                "margins": {
                                    "top": 30
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "VISA",
                                "component": "h1",
                                "font": "headline18",
                                "alignment": "leading"
                            }
                        }
                    ],
                    "margins": {
                        "top": 0,
                        "start": 0,
                        "end": 0,
                        "bottom": 60
                    }
                }
            }
        ]
    }
}

Es un json de respuesya el cual quiero que mantenga el mismo formato que el siguiente: 
esto es para un archivo YAML 
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LayoutResponse'
              examples:
                MX_LayResE1:
                  value:
                    layout:
                      analytics:
                        events:
                          - eventId: product_flow
                            event: load
                            parameters:
                              - key: meta_event
                                value: activacion
                                isPersisted: false
                          - eventId: journey_click
                            action: activateCard
                            promotionCreative: slide-activar-tarjeta
                            zoneId: boton-slider
                            parameters:
                              - key: flow_name
                                value: activacion-tdc
                                isPersisted: false
                              - key: step_name
                                value: activacion
                                isPersisted: false
                          - eventId: journey_click
                            action: SuccessActivateCard
                            promotionCreative: success-activar-tarjeta
                            zoneId: success-activar-tarjeta
                            parameters:
                              - key: flow_name
                                value: activacion-tdc
                                isPersisted: false
                              - key: step_name
                                value: tarjeta activada
                                isPersisted: false
                        parameters:
                          - key: step_name
                            value: activacion
                            isPersisted: false
                          - key: segment
                            value: ""
                            isPersisted: false
                          - key: card
                            value: VISA CLASICA
                            isPersisted: false
                          - key: request_id
                            value: ""
                            isPersisted: false
                          - key: flow_name
                            value: activacion-tdc
                            isPersisted: false
                          - key: environment
                            value: ""
                            isPersisted: false
                          - key: operating_system
                            value: ""
                            isPersisted: false
                      navigation:
                          title: Tarjeta digital
                      component:
                          - type: Container
                            data:
                              components:
                                - type: Image
                                  data:
                                    srcImage: digitalCard.svg
                                    width: 48
                                    height: 48
                                    alt: ""
                                    aling: center
                                    margins:
                                      top: 60
                                - type: Text
                                  data:
                                    text: Activa tu tarjeta de credito digital
                                    component: h1
                                    font: headline24
                                    alignment: center
                                    margins:
                                      top: 18
                                      botton: 12
                                - type: text
                                  data:
                                    text: Podrás consultar el número, vigencia y CVV dinámico para tus compras en línea.
                                    component: p
                                    font: body2
                                    alignment: center
                                    margins:
                                      bottom: 30
                                - type: Spacer
                                  data: {}
                                - type: SlideToComplete
                                  data:
                                    title: activate
                                    isenabled: true
                                    hint: null
                                    name: activateCard
                                    isVisible: true
                                    isDisabledTimer: true
                                    action: activateCard
                                    normalState:
                                      state: normal
                                      value: Activar tarjeta
                                    slidingState:
                                      state: sliding
                                      value: Activar tarjeta
                                    loadingState:
                                      state: loading
                                      value: solicitando con e-llave
                                    succesState:
                                      state: Success
                                      valie: Listo
                                    failureState:
                                      state: failure
                                      value: Error al activar
                                  margins:
                                    bottom: 0
