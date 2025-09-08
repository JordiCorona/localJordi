Tengo este error
There were issues with the specification. The option can be disabled via validateSpec (Maven/Gradle) or --skip-validate-spec (CLI).
 | Error count: 4, Warning count: 2
Errors: 
	-attribute components.version is unexpected
	-Duplicate field paths in `C:\ProyectosDigital\tools\merge master\ib-advisor-monitor-service/src/main/resources/api.yml`
	-attribute components.title is unexpected
	-attribute components.description is unexpected
Warnings: 
	-attribute components.version is unexpected
	-Duplicate field paths in `C:\ProyectosDigital\tools\merge master\ib-advisor-monitor-service/src/main/resources/api.yml`
	-attribute components.title is unexpected
	-attribute components.description is unexpected


te comparto la clase me ayudas a revisar por favor 

openapi: 3.0.2
info:
  title: Advisor Service API
  version: "1.0"
servers:
  - url: http://localhost:8080
security:
  - bearerAuth: []
paths:
  /advisors/status:
    post:
      tags:
        - "advisors"
      summary: Get advisor statuses by IDs
      description: Receives a list of advisor IDs and returns their statuses (mocked data for now).
      operationId: getAdvisorStatusByIds
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AdvisorRequest'
            examples:
              ApiRequestAdvisors:
                $ref: '#/components/examples/ApiRequestAdvisors'
      responses:
        '200':
          description: Successful response with advisor statuses
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AdvisorResponse'
              examples:
                ApiResponsePostAdvisors:
                  $ref: '#/components/examples/ApiResponsePostAdvisors'
        '400':
          description: Bad Request
          content:
            application/json:
              examples:
                ApiResponseError400Example:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '500':
          description: Internal Server Error
          content:
            application/json:
              examples:
                ApiResponseError500Example:
                  $ref: '#/components/examples/ApiResponseError500Example'
        '401':
          description: Unauthorized
          content:
            application/json:
              examples:
                ApiResponseError401Example:
                  $ref: '#/components/examples/ApiResponseError401Example'

components:
  schemas:
    AdvisorRequest:
      type: object
      properties:
        advisors:
          type: array
          items:
            type: object
            properties:
              id:
                type: string
                example: "s123456"
    AdvisorResponse:
      type: object
      properties:
        advisors:
          type: array
          items:
            type: object
            properties:
              id:
                type: string
                example: "s123456"
              status:
                type: string
                enum: ["online", "offline"]
                example: "online"
  examples:
    ApiRequestAdvisors:
      value:
        advisors:
          - id: "s123456"
          - id: "s123457"
          - id: "s123458"
    ApiResponsePostAdvisors:
      value:
        advisors:
          - id: "s123456"
            status: "online"
          - id: "s123457"
            status: "offline"
          - id: "s123458"
            status: "online"
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
  title: Advisor Monitor Service
  description: API para monitorear el estado del servicio Advisor Monitor.
  version: 1.0.0
paths:
  /test:
    get:
      summary: Verifica si el servicio está corriendo
      operationId: testServiceStatus
      responses:
        '200':
          description: El servicio está corriendo correctamente
          content:
            text/plain:
              schema:
                type: string
                example: Advisor Monitor Service Running
