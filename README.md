Me peuedes ayudar con estas vulnerabilidades, para el caso de   CARD_PASSWORD = 'card-password' ya probe crear una variable en la clase de constants, ya probe agregar etiquetas   // checkmarx:off,   // checkmarx-suppress pero sigue apareciendo en el escaneo.
cual podria ser la solucion para esta casuistica? el nombre de la variable, asi como su valor no puede ser modificado. 
hay manera de agregarlo en una variable de entorno externa que no pueda ser detectada en el escaneo ?

export enum JourneyModules {
  ADDRESS_CAPTURE = 'address-capture',
  CARD_DELIVERY = 'card-delivery',
  CARD_DELIVERY_V2 = 'card-delivery-v2',
  DESTINATION = 'destination',
  // checkmarx:off
  CARD_PASSWORD = 'card-password'
  // checkmarx:on
}


Asi mismo quisiera ayuda con estas lineas de codigo las cuales muestran igual vulnerabilidad.
    CardPasswordDeliveryRequestExample:
      value:
        value: " "

Ya probe mandarle un valor "***" de password generico, proble mandarle un dato vacio
    
CardPasswordDeliveryRequestExample:
      value:
        value:

asi mismo un dato vacio de esta manera
    CardPasswordDeliveryRequestExample:
      value:
        value: ' '

El valor inicial es este
CardPasswordDeliveryRequestExample:
  value:
        value: "eyJraWQiOiJRV28zMkdpUWJqSi04X2NpUnpKb0dHTlFHMGNDR3BkZU51NWFmaEpBQWlBIiwidHlwIjoiSl"

esta vulnerabilida esta presente en un archivo llamado api.yamel

Como puedo solucionar esta vulnerabilidad?
