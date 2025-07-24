responses:
  '200':
    description: Successful operation
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/LayoutResponse'
        examples:
          MX_LayResE2:
            value:
              layout:
                analytics:
                  events:
                    - eventId: product_flow
                      event: load
                      parameters:
                        - key: meta_event
                          value: detalle-tarjeta
                          isPersisted: false
                    - eventId: journey_click
                      action: copyText
                      promotionCreative: copiar-numero
                      zoneId: datos-tarjeta
                      parameters:
                        - key: flow_name
                          value: tarjeta-digital
                          isPersisted: false
                        - key: step_name
                          value: detalle-tarjeta
                          isPersisted: false
                  parameters:
                    - key: step_name
                      value: detalle-tarjeta
                      isPersisted: false
                    - key: segment
                      value: ""
                      isPersisted: false
                    - key: card
                      value: VISA CLÁSICA
                      isPersisted: false
                    - key: request_id
                      value: ""
                      isPersisted: false
                    - key: flow_name
                      value: tarjeta-digital
                      isPersisted: false
                    - key: environment
                      value: ""
                      isPersisted: false
                    - key: operating_system
                      value: ""
                      isPersisted: false
                navigation:
                  title: Tarjeta digital
                components:
                  - type: Text
                    data:
                      text: Número de tarjeta digital
              screens:
                - id: CVVDigitalCard
                  sections:
                    - id: CVVComponent
                      type: CVV
                      data:
                        cvv: 123
                        cardNumber: 1234567812345678
                        expirationDate: 12/27
                        copyButton:
                          text: Copiar número
                          accessibilityText: Copiar número
                          icon:
                            type: copy
                            style: outlined
                            color: primary
                          action:
                            type: copyText
                            accessibilityText: Copiar número
                            parameters:
                              - key: copy_text
                                value: 1234567812345678
                                isPersisted: false
                        secureText: Código de seguridad (CVV)
                        description: Este es el código de seguridad de tu tarjeta
                        headerText: CVV
                        footnoteText: Este código solo estará visible por 30 segundos
