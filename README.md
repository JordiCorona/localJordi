Me puedes ayudar por favor, requiero realizar un edn point que devuelva la informacion de de una lista de asesores.
En un futuro se debe consumir un API externa el cual consumiendolo responda la informacion a detalle.
De momento necesito simular el como seria el response por lo cual requiero mokear los datos que responderan.

Este seria el Json de entrada.:
{
  "advisors": [
    {
      "id": "s123456",
    },
    {
      "id": "s123457",
    },
    {
      "id": "s123458",
    }
  ]
}

Seria un Array de Ids, y deberia responder esto: 

{
  "advisors": [
    {
      "id": "s123456",
      "status": "online"
    },
    {
      "id": "s123457",
      "status": "offline",
    },
    {
      "id": "s123458",
      "status": "online",
    }
  ]

Me podrias ayudar con el codigo y las clases necesarias por favor 
