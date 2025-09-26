{
  "layout": {
    "screens": [
      {
        "type": "BottomSheet",
        "event": "screenMeetAdvisorsTeam",
        "data": {
          "header": "Conoce a tus especialistas",
          "body": {
            "text": "Encuentra al experto ideal para resolver tus consultas y obtener asesorías especializadas.",
            "list": {
              "typeOfList": "unordered",
              "spacing": 16,
              "items": [
                {
                  "richText": "[b]Gerente de relación[/b]\n${descripcionGerenteRelacion}",
                  "action": "showAdvisorInfo"
                },
                {
                  "richText": "[b]Especialistas de inversiones[/b]\n${descripcionInversiones}",
                  "action": "showAdvisorInfo"
                },
                {
                  "richText": "[b]Gerente de sucursal[/b]\n${descripcionGerenteSucursal}",
                  "action": "showAdvisorInfo"
                },
                {
                  "richText": "[b]El horario de atención:[/b]\n${horarioAtencion}"
                }
              ]
            }
          },
          "buttons": {
            "alwaysStackButtons": true,
            "labelButtonSecondary": "Entendido",
            "actionSecondary": "close"
          }
        }
      },
      {
        "type": "BottomSheet",
        "event": "showAdvisorInfo",
        "data": {
          "body": {
            "components": [
              {
                "type": "Vertical",
                "data": {
                  "components": [
                    {
                      "type": "Text",
                      "data": {
                        "text": "${nombreAsesor}",
                        "font": "headline18",
                        "alignment": "start"
                      }
                    },
                    {
                      "type": "Horizontal",
                      "data": {
                        "components": [
                          {
                            "type": "Avatar",
                            "name": "Avatar",
                            "data": {
                              "values": {
                                "srcImage": "${avatarImage}",
                                "text": "${nombreAsesor}",
                                "status": "${estadoConexion}"
                              },
                              "format": {
                                "backgroundColor": "brandPurple",
                                "size": {
                                  "width": 48,
                                  "height": 48
                                }
                              }
                            }
                          },
                          {
                            "type": "Vertical",
                            "data": {
                              "components": [
                                {
                                  "type": "Text",
                                  "data": {
                                    "text": "Estado:",
                                    "font": "caption",
                                    "alignment": "start"
                                  }
                                },
                                {
                                  "type": "Text",
                                  "data": {
                                    "text": "${estadoConexion}",
                                    "font": "subtitle2",
                                    "alignment": "start"
                                  }
                                }
                              ],
                              "position": {
                                "alignment": "start",
                                "margins": {
                                  "start": 16
                                }
                              }
                            }
                          }
                        ]
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "text": "Horario",
                        "font": "caption",
                        "alignment": "start",
                        "margins": {
                          "top": 16,
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "text": "${horarioAsesor}",
                        "font": "subtitle2",
                        "alignment": "start",
                        "margins": {
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "text": "Número de contacto",
                        "font": "caption",
                        "alignment": "start",
                        "margins": {
                          "top": 16,
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "richText": "[a action=\"advisorCall\"]${telefonoAsesor}[/a]",
                        "actionParameters": [
                          {
                            "name": "advisorCall",
                            "parameters": {
                              "number": "${telefonoAsesor}"
                            }
                          }
                        ],
                        "disableRichTextUnderline": true,
                        "font": "subtitle2",
                        "alignment": "start",
                        "margins": {
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "text": "Correo",
                        "font": "caption",
                        "alignment": "start",
                        "margins": {
                          "top": 16,
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "richText": "[a action=\"advisorEmail\"]${correoAsesor}[/a]",
                        "actionParameters": [
                          {
                            "name": "advisorEmail",
                            "parameters": {
                              "email": "${correoAsesor}"
                            }
                          }
                        ],
                        "font": "subtitle2",
                        "disableRichTextUnderline": true,
                        "alignment": "start",
                        "margins": {
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "text": "Ubicación",
                        "font": "caption",
                        "alignment": "start",
                        "margins": {
                          "top": 16,
                          "start": 62
                        }
                      }
                    },
                    {
                      "type": "Text",
                      "data": {
                        "richText": "[b]${sucursalAsesor}[/b]\n${direccionSucursal}",
                        "font": "subtitle2",
                        "alignment": "start",
                        "margins": {
                          "start": 62
                        }
                      }
                    }
                  ]
                }
              }
            ]
          },
          "buttons": {
            "alwaysStackButtons": true,
            "labelButtonSecondary": "Entendido",
            "actionSecondary": "close"
          }
        }
      }
    ]
  }
}
