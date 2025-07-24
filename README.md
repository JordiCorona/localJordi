paths:
  /v1/create/init:
    get:
      tags:
        - create
      summary: Init module operations and return initial layout
      operationId: create-init
      parameters:
        - $ref: '#/components/parameters/HeaderCookieRequestId'
        - $ref: '#/components/parameters/HeaderCookieOpaqueToken'
        - $ref: '#/components/parameters/HeaderAuthToken'
        - $ref: '#/components/parameters/HeaderOriginatingApp'
        - $ref: '#/components/parameters/HeaderChannelId'
        - $ref: '#/components/parameters/HeaderClientVersion'
        - $ref: '#/components/parameters/HeaderDeviceModel'
        - $ref: '#/components/parameters/HeaderClientType'
        - $ref: '#/components/parameters/HeaderIMEI'
        - $ref: '#/components/parameters/HeaderOSType'
        - $ref: '#/components/parameters/HeaderTraceId'
        - $ref: '#/components/parameters/HeaderSpanId'
        - $ref: '#/components/parameters/HeaderCountryCode'
        - $ref: '#/components/parameters/HeaderApiVersion'
        - $ref: '#/components/parameters/HeaderUserContext'
        - $ref: '#/components/parameters/HeaderOperationType'
