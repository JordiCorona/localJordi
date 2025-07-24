responses:
  '200':
    description: Successful operation
    content:
      application/json:
        examples:
          ActivacionTarjeta:
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
                      action: successActivateCard
                      promotionCreative: success-activar-tarjeta
                      zoneId: success-activar-tarjeta
                      parameters:
                        - key: flow_name
                          value: activacion-tdc
                          isPersisted: false
                        - key: step_name
                          value: tarjeta_activada
                          isPersisted: false
                  parameters:
                    - key: step_name
                      value: activacion
                      isPersisted: false
                    - key: segment
                      value: ''
                      isPersisted: true
                    - key: card
                      value: VISA CLÁSICA
                      isPersisted: true
                    - key: request_id
                      value: ''
                      isPersisted: true
                    - key: flow_name
                      value: activacion-tdc
                      isPersisted: false
                    - key: environment
                      value: ''
                      isPersisted: true
                    - key: operating_system
                      value: ''
                      isPersisted: true
                navigation:
                  title: Tarjeta digital
                components:
                  - type: Container
                    data:
                      components:
                        - type: Image
                          data:
                            srcImage: digitalCard.svg
                            width: 48
                            height: 48
                            alt: ''
                            align: center
                            margins:
                              top: 60
                        - type: Text
                          data:
                            text: "Activa tu\nTarjeta de Crédito digital"
                            component: h1
                            font: headline24
                            alignment: center
                            margins:
                              top: 18
                              bottom: 12
                        - type: Text
                          data:
                            text: "Podrás consultar el número, vigencia y CVV dinámico para tus compras en línea."
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
                            isEnabled: true
                            hint: null
                            name: ActivateCard
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
                              value: Solicitando con e-llave
                            successState:
                              state: success
                              value: Listo
                            failureState:
                              state: failure
                              value: Error al activar
                          margins:
                            bottom: 0
