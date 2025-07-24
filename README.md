lo tengo de la siguiente manera solo que swagger no lo reconoce, puedes detectar el error por favor 

openapi: 3.0.2
info:
  title: Journey Digital Card
  version: '1.0'
  description: Journey template definitions end points modules, creation and client validation and operations
servers:
  - url: http://localhost:4000
tags:
  - name: create
    description: Client Validation and Create Digital Card
  - name: details
    description: Details Digital Card
  - name: operations
    description: Details Digital Card

components:
  parameters:
    request_id:
      name: request_id
      in: cookie
      required: true
      description: Identifier of the request application in the flow
      schema:
        type: string

    opaque_token:
      name: opaque_token
      in: cookie
      required: true
      description: Security token for authentication
      schema:
        type: string

    X-AUTH-TOKEN:
      name: X-AUTH-TOKEN
      in: header
      required: true
      description: Authentication token for user session
      schema:
        type: string

    x-originating-appl-code:
      name: x-originating-appl-code
      in: header
      required: true
      description: Originating application code
      schema:
        type: string

    x-channel-id:
      name: x-channel-id
      in: header
      required: true
      description: Channel identifier
      schema:
        type: string

    X-CLIENT-VERSION:
      name: X-CLIENT-VERSION
      in: header
      required: true
      description: Client version
      schema:
        type: string

    X-DEVICE-MODEL:
      name: X-DEVICE-MODEL
      in: header
      required: true
      description: Device model
      schema:
        type: string

    X-CLIENT-TYPE:
      name: X-CLIENT-TYPE
      in: header
      required: true
      description: Client type
      schema:
        type: string

    X-IMEI:
      name: X-IMEI
      in: header
      required: true
      description: Device IMEI
      schema:
        type: string

    X-OS-TYPE:
      name: X-OS-TYPE
      in: header
      required: true
      description: OS Type
      schema:
        type: string

    x-b3-traceid:
      name: x-b3-traceid
      in: header
      required: false
      description: Trace ID
      schema:
        type: string

    x-b3-spanid:
      name: x-b3-spanid
      in: header
      required: false
      description: Span ID
      schema:
        type: string

    x-country-code:
      name: x-country-code
      in: header
      required: true
      description: Country Code
      schema:
        type: string

    x-api-version:
      name: x-api-version
      in: header
      required: true
      description: API Version
      schema:
        type: string

    x-user-context:
      name: x-user-context
      in: header
      required: true
      description: Encoded user context
      schema:
        type: string

    operation-type:
      name: operation-type
      in: header
      required: true
      description: Operation type
      schema:
        type: string

paths:
  /v1/create/init:
    get:
      tags:
        - create
      summary: Init module operations and return initial layout
      description: init call orchestrator to initialize process
      operationId: create-init
      parameters:
        - $ref: '#/components/parameters/request_id'
        - $ref: '#/components/parameters/X-AUTH-TOKEN'
        - $ref: '#/components/parameters/x-originating-appl-code'
        - $ref: '#/components/parameters/x-channel-id'
        - $ref: '#/components/parameters/X-CLIENT-VERSION'
        - $ref: '#/components/parameters/X-DEVICE-MODEL'
        - $ref: '#/components/parameters/X-CLIENT-TYPE'
        - $ref: '#/components/parameters/X-IMEI'
        - $ref: '#/components/parameters/X-OS-TYPE'
        - $ref: '#/components/parameters/x-b3-spanid'
        - $ref: '#/components/parameters/x-country-code'
        - $ref: '#/components/parameters/x-api-version'
        - $ref: '#/components/parameters/x-user-context'
        - $ref: '#/components/parameters/operation-type'
