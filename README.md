openapi: '3.0.3'
info:
  title: IB Client Interaction Service API
  description: API for saving and retrieving user interaction data.
  version: 1.0.0
servers:
  - url: http://localhost:8080
security:
  - bearerAuth: []

paths:
  /v1/interactions:
    get:
      tags:
        - Client Interaction
      summary: Retrieve  user interaction.
      parameters:
        - name: client_id
          in: header
          required: true
          description: Client identifier
          schema:
            type: string
        - name: feature
          in: header
          schema:
            type: string
        - name: channel
          in: header
          schema:
            type: string
        - name: country
          in: header
          schema:
            type: string
        - name: start_date
          in: header
          schema:
            type: string
            format: date-time
        - name: end_date
          in: header
          schema:
            type: string
            format: date-time
      responses:
        '200':
          description: List of user interaction records
          content:
            application/json:
              schema:
                items:
                  $ref: '#/components/schemas/ClientInteraction'
              examples:
                successResponseExample:
                  $ref: '#/components/examples/SuccessResponseExample'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                errorResponseExample:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '500':
          description: Internal server error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                errorResponseExample:
                  $ref: '#/components/examples/ApiResponseError500Example'
        '401':
          description: Unauthorized
          content:
            application/json:
              examples:
                ApiResponseError401Example:
                  $ref: '#/components/examples/ApiResponseError401Example'
  /v1/interactions/save-interaction:
    post:
      tags:
        - Client Interaction
      summary: Save a new user interaction
      operationId: saveInteraction
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ClientInteraction'
            examples:
              exampleRequest:
                $ref: '#/components/examples/SuccessClientInteractionRequestExample'
      responses:
        '201':
          description: Interaction successfully created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ClientInteraction'
              examples:
                exampleRequest:
                  $ref: '#/components/examples/SuccessResponseExample'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                errorResponseExample:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '500':
          description: Internal server error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                errorResponseExample:
                  $ref: '#/components/examples/ApiResponseError500Example'
        '401':
          description: Unauthorized
          content:
            application/json:
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
  schemas:
    ClientInteraction:
      type: object
      required:
        - client_id
        - country
        - channel
        - interactions
      properties:
        client_id:
          type: string
          description: Identifier of the client
        country:
          type: string
          description: Country code of user
        channel:
          type: string
          description: Source of interaction (web, app, etc)
        interactions:
          type: array
          description: List of feature interactions
          items:
            $ref: '#/components/schemas/Interaction'
    Interaction:
      type: object
      required:
        - feature
        - action
      properties:
        feature:
          type: string
          description: Name of the used feature
        action:
          type: string
          description: Action performed by the user


    ErrorResponse:
      type: object
      properties:
        timestamp:
          type: string
        message:
          type: string
        details:
          type: array
          items:
            type: string

  examples:
    SuccessClientInteractionRequestExample:
      summary: A valid client interaction request
      value:
        client_id: "client-123"
        country: "MX"
        channel: "APP"
        interactions:
          - feature: "AccountView"
            action: "ViewBalance"
    SuccessResponseExample:
      summary:  Success response
      value:
        client_id: "client-123"
        country: "MX"
        channel: "MOBILE"
        created_at: "2025-06-10T08:00:00Z"
        updated_at: "2025-06-11T11:00:00Z"
        Interactions:
          -  feature: "AccountView"
             action: "ViewBalance"
             last_time: "2025-06-11-t10:45:00Z"
             count: 4

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




openapi: 3.0.2
info:
  title: Advisor Monitor Service
  description: API para monitorear el estado del servicio Advisor Monitor.
  version: 1.0.0
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
