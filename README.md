openapi: "3.0.2"
info:
  title: Dynamic Layout Adapter
  version: "1.0"
servers:
  - url: http://localhost:8080
security:
  - bearerAuth: []
paths:
  /layout:
    post:
      tags:
        - "dynamic layout adapter"
      summary: Create different SDUIs according to their respective configuration.
      description: Create different SDUIs according to their respective configuration.
        It must have the scope dynamic.layout.adapter.ib.service.read in the token.
      operationId: sduiLayout
      security:
        - bearerAuth: []
      parameters:
        - name: source
          in: header
          required: true
          schema:
            type: string
            minLength: 1
            maxLength: 25
          description: Identification of the transaction by source (DASHBOARD / SERVICING, ...)
        - name: country_code
          in: header
          required: true
          schema:
            type: string
            minLength: 1
            maxLength: 2
        - name: module_name
          in: header
          required: true
          schema:
            type: string
            minLength: 1
            maxLength: 60
        - name: segment_id
          in: header
          required: true
          schema:
            type: string
            minLength: 1
            maxLength: 30
        - name: banking_segment
          in: header
          required: true
          schema:
            type: string
            minLength: 1
            maxLength: 30
      responses:
        '200':
          description: Ok
          content:
            'application/json':
              examples:
                apiResponseExampleAffluentRmActive:
                  $ref: '#/components/examples/ApiResponseExampleAffluentRmActive'
                apiResponseExampleAffluentRmNoActive:
                  $ref: '#/components/examples/ApiResponseExampleAffluentRmNoActive'
        '400':
          description: Bad Request
          content:
            'application/json':
              examples:
                apiResponseExampleBadRequest:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '500':
          description: Internal Server Error
          content:
            'application/json':
              examples:
                ApiResponseError500Example:
                  $ref: '#/components/examples/ApiResponseError500Example'
        '401':
          description: Unauthorized
          content:
            'application/json':
              examples:
                ApiResponseError401Example:
                  $ref: '#/components/examples/ApiResponseError401Example'
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: "Please enter your JWT token for authentication."
  examples:
    ApiRequestExampleMX:
      value:
        segment_id: "4"
        banking_segment: "4"
        rm_assigned: true
    ApiResponseExampleAffluentRmActive:
      value:
        {
          "layout": {
            "components": [
              {
                "type": "Carousel",
                "data": {
                  "pages": [
                    {
                      "index": 0,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "servideAgentCollTools",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Te damos la bienvenida a tu asistencia en línea",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "${ADVISOR_NAME} es tu Gerente de Relación, te ayudará con asesoría y contratación de productos y servicios.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Siguiente",
                            "type": "continue",
                            "action": "nextSlideChannels"
                          }
                        }
                      }
                    },
                    {
                      "index": 1,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "twoPeopleWithTableGreen",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Conoce a tu equipo financiero",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "Comunícate con tu asesor de inversiones y asistente personal de forma segura para que te asesoren sobre cómo proteger e incrementar tu patrimonio.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Siguiente",
                            "type": "continue",
                            "action": "nextSlideProducts"
                          }
                        }
                      }
                    },
                    {
                      "index": 2,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "payEquity4Characters",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Acceso a productos exclusivos",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "Puedes acceder a cuentas, tarjetas de crédito o bonos de inversión exclusivos para tu perfil.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Listo",
                            "type": "submit",
                            "action": "nextServices"
                          }
                        }
                      }
                    }
                  ],
                  "headerButtonAction": "close"
                }
              }
            ]
          }
        }
    ApiResponseExampleAffluentRmNoActive:
      value:
        {
          "layout": {
            "components": [
              {
                "type": "Carousel",
                "data": {
                  "pages": [
                    {
                      "index": 0,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "womanSodaBlue",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Te damos la bienvenida a tu asistencia en línea",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "El objetivo es asesorarte en la contratación de productos y servicios con mensajes o videollamadas seguras.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Siguiente",
                            "type": "continue",
                            "action": "nextSlideChannels"
                          }
                        }
                      }
                    },
                    {
                      "index": 1,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "twoPeopleWithTableGreen",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Conoce a tu equipo financiero",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "Comunícate con tu asesor de inversiones y asistente personal de forma segura para que te asesoren sobre cómo proteger e incrementar tu patrimonio.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Siguiente",
                            "type": "continue",
                            "action": "nextSlideProducts"
                          }
                        }
                      }
                    },
                    {
                      "index": 2,
                      "image": {
                        "data": {
                          "srcUrlImage": "www.scotiabank.com/photo/manager-relationship.png",
                          "srcImage": "payEquity4Characters",
                          "width": 360,
                          "height": 270,
                          "alignment": "center",
                          "margins": {
                            "bottom": 36
                          }
                        }
                      },
                      "heading": {
                        "data": {
                          "text": "Acceso a productos exclusivos",
                          "alignment": "left",
                          "margins": {
                            "bottom": 30
                          }
                        }
                      },
                      "description": {
                        "data": {
                          "text": "Puedes acceder a cuentas, tarjetas de crédito o bonos de inversión exclusivos para tu perfil.",
                          "alignment": "left",
                          "margins": {
                            "bottom": 46
                          }
                        }
                      },
                      "bottomNavigation": {
                        "data": {
                          "nextButton": {
                            "text": "Listo",
                            "type": "submit",
                            "action": "nextServices"
                          }
                        }
                      }
                    }
                  ],
                  "headerButtonAction": "close"
                }
              }
            ]
          }
        }
    ApiResponseError400Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Bad Request"
        details: ["Bad Request"]
    ApiResponseError401Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Unauthorized"
        details: ["Unauthorized"]
    ApiResponseError500Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Internal Server Error"
        details: ["Internal Server Error"]
