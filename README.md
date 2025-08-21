{
  "client": {
    "id": "12345",
    "name": "Roberto Barrios",
    "avatarImage": "flyings.svg",       // opcional si tiene foto
    "status": "online"                  // online | offline
  },
  "advisors": [
    {
      "id": "a1",
      "name": "Alejandra Ortiz Herrera",
      "role": "Relationship Manager",
      "status": "online",                // online | offline
      "avatarImage": "avatar1.svg",      // opcional
      "priority": 1,                     // orden de aparición
      "actions": {
        "onClick": "showAdvisorInfo",    // acción para el avatar
        "onAreaClick": "AdvisorsContactInformation"
      }
    },
    {
      "id": "a2",
      "name": "Juan Pérez",
      "role": "Investment Advisor",
      "status": "offline",
      "avatarImage": null,
      "priority": 2,
      "actions": {
        "onClick": "showAdvisorInfo",
        "onAreaClick": "HeaderAction"
      }
    },
    {
      "id": "a3",
      "name": "María López",
      "role": "Support Specialist",
      "status": "online",
      "avatarImage": null,
      "priority": 3,
      "actions": {
        "onClick": "showAdvisorInfo",
        "onAreaClick": "AdvisorsContactInformation"
      }
    }
  ]
}
🗂️ Explicación de los campos
client

id → identificador único del cliente.

name → nombre que aparece en el Avatar principal.

avatarImage → si tiene un ícono o foto.

status → estado de conexión.

advisors[]

id → identificador único del asesor.

name → nombre completo (se mostrará en los textos del layout).

role → cargo/rol (ej. Relationship Manager).

status → si está online/offline.

avatarImage → si tiene foto o ícono.

priority → orden en que deben mostrarse (1–3).

actions → mapeo de las acciones de negocio que el front debe ejecutar al interactuar.

🎯 Beneficio de este modelo
El backend solo entrega datos limpios y estructurados.

El frontend ya sabe mapearlos al layout JSON (Avatar, Text, Role, Status, Acciones).

Si mañana cambian estilos, el backend no se toca, solo el layout renderer.

👉 ¿Quieres que te prepare un response ejemplo realista con 2 asesores (como el caso de tu JSON) para que lo uses como payload inicial en tu draft de back-end?







Dijiste:
cual es la funcion de         "onClick": "showAdvisorInfo",  


ChatGPT dijo:
¡Muy buena observación! 🔍

El campo que mencionas:

json
Copiar
Editar
"onClick": "showAdvisorInfo"
📌 Función de este campo
Es un mapeo de acción de negocio que el backend devuelve como metadata.

No ejecuta nada por sí mismo → su propósito es indicar al frontend qué acción debe realizar cuando el usuario haga clic en ese componente (en este caso, el Avatar del asesor).

⚙️ Flujo típico
Backend responde con "onClick": "showAdvisorInfo".

Frontend tiene una tabla de acciones predefinidas (un action dispatcher).

Ejemplo:

javascript
Copiar
Editar
if (action === "showAdvisorInfo") {
   openAdvisorModal(advisorId); // abre modal con datos del asesor
}
Cuando el usuario da clic → el front lee "showAdvisorInfo" y dispara la lógica correspondiente (abrir detalle, mostrar modal, redirigir a otra vista, etc.).

📖 Comparación con otros campos de acción
"onClick": "showAdvisorInfo" → se dispara al hacer clic sobre el Avatar del asesor.

"onAreaClick": "AdvisorsContactInformation" → se dispara al hacer clic en la zona de detalle/texto del asesor.

👉 Esto permite que diferentes partes del mismo bloque hagan acciones distintas, aunque estén relacionadas con el mismo asesor.

✅ En conclusión:
"onClick": "showAdvisorInfo" es un identificador de acción que instruye al frontend qué hacer cuando se hace clic en el avatar del asesor, típicamente mostrar más información de ese asesor.

