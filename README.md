components:
  parameters:
    HeaderCookieRequestId:
      name: request_id
      in: cookie
      required: true
      description: Identifier of the request application in the flow
      schema:
        type: string

    HeaderCookieOpaqueToken:
      name: opaque_token
      in: cookie
      required: true
      description: Security token for authentication
      schema:
        type: string

    HeaderAuthToken:
      name: X-AUTH-TOKEN
      in: header
      required: true
      description: Authentication token for user session
      schema:
        type: string

    HeaderOriginatingApp:
      name: x-originating-appl-code
      in: header
      required: true
      description: Originating application code
      schema:
        type: string
        example: BJFT

    HeaderChannelId:
      name: x-channel-id
      in: header
      required: true
      description: Channel identifier
      schema:
        type: string
        example: Mobile

    HeaderClientVersion:
      name: X-CLIENT-VERSION
      in: header
      required: true
      description: Client version
      schema:
        type: string
        example: "14"

    HeaderDeviceModel:
      name: X-DEVICE-MODEL
      in: header
      required: true
      description: Device model
      schema:
        type: string
        example: a123456789asdf21

    HeaderClientType:
      name: X-CLIENT-TYPE
      in: header
      required: true
      description: Client type
      schema:
        type: string
        example: IOS

    HeaderIMEI:
      name: X-IMEI
      in: header
      required: true
      description: Device IMEI
      schema:
        type: string
        example: 345621234567890986

    HeaderOSType:
      name: X-OS-TYPE
      in: header
      required: true
      description: OS Type
      schema:
        type: string
        example: "14"

    HeaderTraceId:
      name: x-b3-traceid
      in: header
      required: false
      description: Trace ID
      schema:
        type: string
        example: 41200de3d3e82a81

    HeaderSpanId:
      name: x-b3-spanid
      in: header
      required: false
      description: Span ID
      schema:
        type: string
        example: 41200de3d3e82a82

    HeaderCountryCode:
      name: x-country-code
      in: header
      required: true
      description: Country Code
      schema:
        type: string
        example: MX

    HeaderApiVersion:
      name: x-api-version
      in: header
      required: true
      description: API Version
      schema:
        type: string
        example: "1.0"

    HeaderUserContext:
      name: x-user-context
      in: header
      required: true
      description: Encoded user context
      schema:
        type: string
        example: eyAic3Vi...

    HeaderOperationType:
      name: operation-type
      in: header
      required: true
      description: Operation type
      schema:
        type: string
        example: MTB
