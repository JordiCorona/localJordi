Hola, me ayudas a revisar este json por favor. 
{
  "layout": {
    "screens": [
      {
          "type": "BottomSheet",
          "event": "screenMeetAdvisorTeam",
          "data": {
              "header": "${HEADING_TEXT}",
              "body": {
                  "text": "${DESCRIPTION_TEXT}",
                  "list": {
                      "typeOfList": "unorder",
                      "spacing": 16,
                      "items": []
            }
          },
              "buttons": {
                  "alwaysStackButtons": true,
                  "labelButtonSecondary": "${DESCRIPTION_BUTTON}",
                  "actionSecondary": "${ACTION_BUTTON}"
          }
        }
      },

      {
          "type": "Carousel",
          "event": "load",
          "data": {
              "body": {
                  "carousel": {
                      "data": {
                          "pages": [
                  {
                                  "index": 0,
                                  "image": {
                                      "srcUrlImage": "${SRC_URL_IMAGE}",
                                      "srcImage": "${SRC_IMAGE}",
                                      "width": 360,
                                      "height": 270,
                                      "fullWidth": true
                    },
                                  "heading": "${HEADING_TEXT}",
                                  "description": "${DESCRIPTION_TEXT}",
                                  "nextButton": {
                                    "text": "${DESCRIPTION_BUTTON}",
                                    "type": "continue",
                                    "action": "${ACTION_BUTTON}"
                    }
                  },
                  {
                                  "index": 1,
                                  "image": {
                              "srcUrlImage": "${SRC_URL_IMAGE}",
                                "srcImage": "${SRC_IMAGE}",
                                "width": 360,
                                "height": 270,
                                "fullWidth": true
                    },
                                  "heading": "${HEADING_TEXT}",
                                  "description": "${DESCRIPTION_TEXT}",
                                  "nextButton": {
                                    "text": "${DESCRIPTION_BUTTON}",
                                    "type": "continue",
                                    "action": "${ACTION_BUTTON}"
                    }
                  },
                  {
                                  "index": 2,
                                  "image": {
                                      "srcUrlImage": "${SRC_URL_IMAGE}",
                                      "srcImage": "${SRC_IMAGE}",
                                      "width": 360,
                                      "height": 270,
                                      "fullWidth": true
                    },
                                  "heading": "${HEADING_TEXT}",
                                  "description": "${DESCRIPTION_TEXT}",
                                  "nextButton": {
                                    "text": "${DESCRIPTION_BUTTON}",
                                    "type": "submit",
                                    "action": "${ACTION_BUTTON}"
                    }
                  }
                ],
                          "headerButtonAction": "close"
              }
            }
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
                      "type": "Text",
                      "data": {
                          "text": "${HEADING_TEXT}",
                          "font": "serifHeadline28",
                          "component": "h1",
                          "alignment": "start",
                          "margins": {
                              "bottom": 16
                }
              }
            },
            {
                      "type": "Text",
                      "data": {
                          "text": "${DESCRIPTION_TEXT}",
                          "font": "body2",
                          "component": "p",
                          "alignment": "start",
                          "margins": {
                              "bottom": 4
                }
              }
            },
            {
                      "type": "Text",
                      "data": {
                          "richText": "${ACTION_TEXT}",
                          "disableRichTextUnderline": false,
                          "alignment": "start"
              }
            }
          ],
              "margins": {
                  "top": 32,
                  "start": 16,
                  "end": 16,
                  "bottom": 0
          }
        }
      }



-------------------------------------------


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
                            "typeOfList": "unorder",
                            "spacing": 16,
                            "items": [
                                {
                                    "richText": "[b]Gerente de relación[/b]\nEs tu principal punto de conexión con el banco, te ofrece asesoría sobre créditos, inversiones, seguros y pagos. Estará disponible para brindarte soluciones personalizadas y asegurar que recibas el mejor servicio."
                                },
                                {
                                    "richText": "[b]Especialistas de inversiones[/b]\nOfrece asesoría en gestión de inversiones sofisticadas y diversificación de portafolio."
                                },
                                {
                                    "richText": "[b]Gerente de sucursal[/b]\nOfrece asistencia con problemas de servicio o te apoya en ausencia de tu Gerente de relación."
                                },
                                {
                                    "richText": "[b]El horario de atención:[/b]\nLun. -Vi. 8:00 a.m. - 6:00 p.m.\nSáb. 10:00 a.m. - 5:00 p.m."
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
                              "text": "Alejandra Ortiz Herrera",
                              "font": "headline18",
                              "alignment": "start",
                              "widthFull": false,
                              "margins": {
                                  "start": 0,
                                  "bottom": 0
                              }
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
                                    "widthFull": false,
                                    "values": {
                                      "srcImage": "flyings.svg",
                                      "text": "Roberto Barrios",
                                      "status": "online"
                                    },
                                    "business": {
                                      "action": "showAdvisorInfo"
                                    },
                                    "format": {
                                      "backgroundColor": "brandPurple",
                                      "size": {
                                        "width": 48,
                                        "height": 48
                                      }
                                    },
                                    "position": {
                                      "alignment": "center",
                                      "margins": {
                                        "start": 0,
                                        "end": 0
                                      }
                                    }
                                  }
                                },
                                {
                                  "type": "Vertical",
                                  "data": {
                                    "widthFull": false,
                                    "components": [
                                      {
                                        "type": "Text",
                                        "data": {
                                          "text": "Estado:",
                                          "font": "caption",
                                          "alignment": "start",
                                          "margins": {
                                            "start": 0
                                          }
                                        }
                                      },
                                      {
                                        "type": "Text",
                                        "data": {
                                          "text": "En línea",
                                          "font": "subtitle2",
                                          "alignment": "start",
                                          "margins": {
                                            "start": 0
                                          }
                                        }
                                      }
                                    ],
                                    "position": {
                                      "alignment": "start",
                                      "margins": {
                                        "top": 0,
                                        "start": 16,
                                        "bottom": 0,
                                        "end": 0
                                      }
                                    }
                                  }
                                }
                              ],
                              "position": {
                                "alignment": "top",
                                "margins": {
                                  "top": 24,
                                  "start": 0,
                                  "bottom": 0,
                                  "end": 0
                                }
                              }
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
                              "text": "Lunes a viernes de 8:30 a 16:00 h",
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
                                "bottom": 0,
                                "top": 16,
                                "start": 62,
                                "end": 0
                              }
                            }
                          },
                          {
                            "type": "Text",
                            "data": {
                              "richText": "[a action=\"advisorCall\"]3188669019[/a]",
                              "actionParameters": [
                                  {
                                      "name": "advisorCall",
                                      "parameters": {
                                          "number": "3188669019"
                                      }
                                  }
                              ],
                              "disableUnderline": true,
                              "font": "subtitle2",
                              "alignment": "start",
                              "margins": {
                                "bottom": 0,
                                "top": 0,
                                "start": 62,
                                "end": 0
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
                                "bottom": 0,
                                "top": 16,
                                "start": 62,
                                "end": 0
                              }
                            }
                          },
                          {
                            "type": "Text",
                            "data": {
                                "richText": "[a action=\"advisorEmail\"]alejandra.ortiz@scotiabank.com[/a]",
                                "actionParameters": [
                                    {
                                        "name": "advisorEmail",
                                        "parameters": {
                                            "email": "alejandra.ortiz@scotiabank.com"
                                        }
                                    }
                                ],
                                "font": "subtitle2",
                                "disableUnderline": true,
                                "alignment": "start",
                                "margins": {
                                    "bottom": 0,
                                    "top": 0,
                                    "start": 62,
                                    "end": 0
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
                                "bottom": 0,
                                "top": 16,
                                "start": 62,
                                "end": 0
                              }
                            }
                          },
                          {
                            "type": "Text",
                            "data": {
                              "richText": "[b]Sucursal Plaza Scotiabank[/b]\nBlvd. M. Ávila Camacho 1, Lomas de Chapultepec 10009, Miguel Hidalgo, Ciudad de México.\n",
                              "font": "subtitle2",
                              "alignment": "start",
                              "margins": {
                                "bottom": 0,
                                "top": 0,
                                "start": 62,
                                "end": 0
                              }
                            }
                          }
                        ],
                        "position": {
                          "alignment": "start",
                          "margins": {
                              "top": 30
                          }
                        }
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
        ],
        "components": [
            {
                "type": "Container",
                "data": {
                    "components": [
                        {
                            "type": "Text",
                            "data": {
                                "text": "Conéctate con tu equipo especializado",
                                "font": "serifHeadline28",
                                "component": "h1",
                                "alignment": "start",
                                "margins": {
                                    "top": 0,
                                    "start": 16,
                                    "bottom": 0,
                                    "end": 16
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "text": "Nuestros especialistas están disponibles para apoyarte en tus objetivos financieros.",
                                "font": "body2",
                                "component": "p",
                                "alignment": "start",
                                "margins": {
                                    "top": 16,
                                    "start": 16,
                                    "bottom": 0,
                                    "end": 16
                                }
                            }
                        },
                        {
                            "type": "Text",
                            "data": {
                                "richText": "[a action=\"screenMeetAdvisorsTeam\"]Conoce a tu equipo[/a]",
                                "alignment": "start",
                                "font": "subtitle2",
                                "disableUnderline": false,
                                "margins": {
                                    "top": 4,
                                    "start": 16,
                                    "bottom": 0,
                                    "end": 16
                                }
                            }
                        },
                        {
                            "type": "AccordionUserList",
                            "data": {
                                "components": [
                                    {
                                        "type": "Horizontal",
                                        "data": {
                                            "components": [
                                                
                                                {
                                                    "type": "Avatar",
                                                    "name": "Avatar",
                                                    "data": {
                                                        "values": {
                                                            "text": "Roberto Barrios",
                                                            "status": "offline"
                                                        },
                                                        "business": {
                                                            "action": "showAdvisorInfo",
                                                        },
                                                        "format": {
                                                            "backgroundColor": "brandPurple",
                                                            "size": {
                                                                "width": 48,
                                                                "height": 48
                                                            }
                                                        },
                                                        "position": {
                                                            "alignment": "center",
                                                            "margins": {
                                                                "top": 24,
                                                                "start": 16,
                                                                "bottom": 24,
                                                                "end": 0
                                                            }
                                                        }
                                                    }
                                                },
                                                {
                                                  "type": "ActionArea",
                                                  "data": {
                                                    "components": [
                                                        {
                                                            "type": "Horizontal",
                                                            "data": {
                                                                "components": [
                                                                    {
                                                                        "type": "Vertical",
                                                                        "data": {
                                                                            "widthFull": false,
                                                                            "components": [
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Body 1 Alejandra Ortiz Herrera Body 1 Alejandra Ortiz Herrera Body 1 ",
                                                                                        "font": "subtitle3",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 4,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                },
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Relationship Manager",
                                                                                        "font": "caption",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 0,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "start",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 8,
                                                                                    "bottom": 0,
                                                                                    "end": 0
                                                                                }
                                                                            }
                                                                        }
                                                                    },
                                                                    {
                                                                        "type": "Spacer",
                                                                        "data": {}
                                                                    },
                                                                    {
                                                                        "type": "Horizontal",
                                                                        "data": {
                                                                            "components": [
                                                                                {
                                                                                    "type": "Image",
                                                                                    "data": {
                                                                                        "srcImage": "chevronRight",
                                                                                        "widthFull": false,
                                                                                        "width": 16,
                                                                                        "height": 16,
                                                                                        "alt": null,
                                                                                        "align": "end",
                                                                                        "margins": {
                                                                                            "top": 0,
                                                                                            "start": 8,
                                                                                            "bottom": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "end",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 0,
                                                                                    "bottom": 0,
                                                                                    "end": 16
                                                                                }
                                                                            }
                                                                        }
                                                                    }
                                                                    
                                                                ],
                                                                "position": {
                                                                    "alignment": "center",
                                                                    "margins": {
                                                                        "top": 0,
                                                                        "start": 0,
                                                                        "bottom": 0,
                                                                        "end": 0
                                                                    }
                                                                }
                                                            }
                                                        }
                                                    ],
                                                    "business": {
                                                      "action": "AdvisorsContactInformation"
                                                    },
                                                    "position": {
                                                        "alignment": "center",
                                                        "margins": {
                                                            "top": 0,
                                                            "start": 0,
                                                            "bottom": 0,
                                                            "end": 0
                                                        }
                                                    }
                                                  }
                                                }
                                                
                                            ],
                                            "position": {
                                                "alignment": "center",
                                                "margins": {
                                                    "top": 0,
                                                    "start": 0,
                                                    "bottom": 0,
                                                    "end": 0
                                                }
                                            }
                                            
                                        }
                                    },
                                    {
                                        "type": "LineSeparator",
                                        "data": {
                                            "type":  "solid",
                                            "height":  1,
                                            "margins": {
                                                "top": 0,
                                                "start": 0,
                                                "bottom": 0,
                                                "end": 0
                                            }
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
                                                            "srcImage": "flyingsgray.svg",
                                                            "text": "Roberto Barrios",
                                                            "status": "online"
                                                        },
                                                        "business": {
                                                            "action": "showAdvisorInfo",
                                                        },
                                                        "format": {
                                                            "backgroundColor": "brandPurple",
                                                            "size": {
                                                                "width": 48,
                                                                "height": 48
                                                            }
                                                        },
                                                        "position": {
                                                            "alignment": "center",
                                                            "margins": {
                                                                "top": 24,
                                                                "start": 16,
                                                                "bottom": 24,
                                                                "end": 0
                                                            }
                                                        }
                                                    }
                                                },
                                                {
                                                  "type": "ActionArea",
                                                  "data": {
                                                    "components": [
                                                        {
                                                            "type": "Horizontal",
                                                            "data": {
                                                                "components": [
                                                                    {
                                                                        "type": "Vertical",
                                                                        "data": {
                                                                            "widthFull": false,
                                                                            "components": [
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Body 2 Alejandra Ortiz Herrera",
                                                                                        "font": "subtitle3",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 4,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                },
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Relationship Manager",
                                                                                        "font": "caption",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 0,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "start",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 8,
                                                                                    "bottom": 0,
                                                                                    "end": 0
                                                                                }
                                                                            }
                                                                        }
                                                                    },
                                                                    {
                                                                        "type": "Spacer",
                                                                        "data": {}
                                                                    },
                                                                    {
                                                                        "type": "Horizontal",
                                                                        "data": {
                                                                            "components": [
                                                                                {
                                                                                    "type": "Image",
                                                                                    "data": {
                                                                                        "srcImage": "signifier.svg",
                                                                                        "backgroundColor": "brandOrange",
                                                                                        "widthFull": false,
                                                                                        "width": 8,
                                                                                        "height": 8,
                                                                                        "align": "end",
                                                                                        "margins": {
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "bottom": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                },
                                                                                {
                                                                                    "type": "Image",
                                                                                    "data": {
                                                                                        "srcImage": "chevronRight",
                                                                                        "widthFull": false,
                                                                                        "width": 16,
                                                                                        "height": 16,
                                                                                        "alt": null,
                                                                                        "align": "end",
                                                                                        "margins": {
                                                                                            "top": 0,
                                                                                            "start": 8,
                                                                                            "bottom": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "end",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 0,
                                                                                    "bottom": 0,
                                                                                    "end": 16
                                                                                }
                                                                            }
                                                                        }
                                                                    }
                                                                    
                                                                ],
                                                                "position": {
                                                                    "alignment": "center",
                                                                    "margins": {
                                                                        "top": 0,
                                                                        "start": 0,
                                                                        "bottom": 0,
                                                                        "end": 0
                                                                    }
                                                                }
                                                            }
                                                        }
                                                    ],
                                                    "business": {
                                                      "action": "HeaderAction"
                                                    },
                                                    "position": {
                                                        "alignment": "center",
                                                        "margins": {
                                                            "top": 0,
                                                            "start": 0,
                                                            "bottom": 0,
                                                            "end": 0
                                                        }
                                                    }
                                                  }
                                                }
                                                
                                            ],
                                            "position": {
                                                "alignment": "center",
                                                "margins": {
                                                    "top": 0,
                                                    "start": 0,
                                                    "bottom": 0,
                                                    "end": 0
                                                }
                                            }
                                            
                                        }
                                    },
                                    {
                                        "type": "LineSeparator",
                                        "data": {
                                            "type":  "solid",
                                            "height":  1,
                                            "margins": {
                                                "top": 0,
                                                "start": 0,
                                                "bottom": 0,
                                                "end": 0
                                            }
                                        }
                                    },
                                ],
                                "headerComponents": [
                                    {
                                        "type": "Horizontal",
                                        "data": {
                                            "components": [
                                                
                                                {
                                                    "type": "Avatar",
                                                    "name": "Avatar",
                                                    "data": {
                                                        "values": {
                                                            "srcImage": "flyings.svg",
                                                            "text": "Roberto Barrios",
                                                            "status": "online"
                                                        },
                                                        "business": {
                                                            "action": "showAdvisorInfo",
                                                        },
                                                        "format": {
                                                            "backgroundColor": "brandPurple",
                                                            "size": {
                                                                "width": 48,
                                                                "height": 48
                                                            }
                                                        },
                                                        "position": {
                                                            "alignment": "center",
                                                            "margins": {
                                                                "top": 24,
                                                                "start": 16,
                                                                "bottom": 24,
                                                                "end": 0
                                                            }
                                                        }
                                                    }
                                                },
                                                {
                                                  "type": "ActionArea",
                                                  "data": {
                                                    "components": [
                                                        {
                                                            "type": "Horizontal",
                                                            "data": {
                                                                "components": [
                                                                    {
                                                                        "type": "Vertical",
                                                                        "data": {
                                                                            "widthFull": false,
                                                                            "components": [
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Header Alejandra Ortiz Herrera",
                                                                                        "font": "subtitle3",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 4,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                },
                                                                                {
                                                                                    "type": "Text",
                                                                                    "data": {
                                                                                        "widthFull": false,
                                                                                        "text": "Relationship Manager",
                                                                                        "font": "caption",
                                                                                        "alignment": "start",
                                                                                        "margins": {
                                                                                            "bottom": 0,
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "start",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 8,
                                                                                    "bottom": 0,
                                                                                    "end": 0
                                                                                }
                                                                            }
                                                                        }
                                                                    },
                                                                    {
                                                                        "type": "Spacer",
                                                                        "data": {}
                                                                    },
                                                                    {
                                                                        "type": "Horizontal",
                                                                        "data": {
                                                                            "components": [
                                                                                {
                                                                                    "type": "Image",
                                                                                    "data": {
                                                                                        "srcImage": "signifier.svg",
                                                                                        "backgroundColor": "brandOrange",
                                                                                        "widthFull": false,
                                                                                        "width": 8,
                                                                                        "height": 8,
                                                                                        "align": "end",
                                                                                        "margins": {
                                                                                            "top": 0,
                                                                                            "start": 0,
                                                                                            "bottom": 0,
                                                                                            "end": 0
                                                                                        }
                                                                                    }
                                                                                }
                                                                            ],
                                                                            "position": {
                                                                                "alignment": "end",
                                                                                "margins": {
                                                                                    "top": 0,
                                                                                    "start": 0,
                                                                                    "bottom": 0,
                                                                                    "end": 16
                                                                                }
                                                                            }
                                                                        }
                                                                    }
                                                                    
                                                                ],
                                                                "position": {
                                                                    "alignment": "center",
                                                                    "margins": {
                                                                        "top": 0,
                                                                        "start": 0,
                                                                        "bottom": 0,
                                                                        "end": 0
                                                                    }
                                                                }
                                                            }
                                                        }
                                                    ],
                                                    "business": {
                                                      "action": "AdvisorsContactInformation"
                                                    },
                                                    "position": {
                                                        "alignment": "center",
                                                        "margins": {
                                                            "top": 0,
                                                            "start": 0,
                                                            "bottom": 0,
                                                            "end": 0
                                                        }
                                                    }
                                                  }
                                                }
                                                
                                            ],
                                            "position": {
                                                "alignment": "center",
                                                "margins": {
                                                    "top": 0,
                                                    "start": 0,
                                                    "bottom": 0,
                                                    "end": 0
                                                }
                                            }
                                            
                                        }
                                    }
                                ],
                                "footerComponents": [ ],
                                "format": {
                                    "cardStyle": "flatSolid",
                                    "withParentsMargins": false,
                                    "expandableFormat": {
                                        "textExpanded": "Ver menos asesores",
                                        "textUnexpanded": "Ver más asesores",
                                        "toggleActionPosition": "footer",
                                        "expandable": false,
                                        "resume": true,
                                        "iconExpanded": {
                                            "srcImage": "chevronDown",
                                            "width": 16,
                                            "height": 16
                                        },
                                        "iconUnexpanded": {
                                            "srcImage": "chevronUp",
                                            "width": 16,
                                            "height": 16
                                        }
                                    }
                                },
                                "values": {
                                    "textExpanded": "",
                                    "textUnexpanded": "",
                                },
                                "position": {
                                    "alignment": "start",
                                    "margins": {
                                        "bottom": 16,
                                        "top": 32,
                                        "start": 16,
                                        "end": 16
                                    }
                                },
                                "accessibility": {
                                    "identifier": "accordion advisor ",
                                    "enabled": true,
                                    "hint": "Lista de asesores",
                                    "label": "label ?"
                                }
                            }
                        }
                    ],
                    "margins": {
                        "bottom": 16,
                        "top": 56,
                        "start": 0,
                        "end": 0
                    }
                }
            }
        ]
    }
}
    ]
  }
}
