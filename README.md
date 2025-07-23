paths:
  /v1/create/init:
    get:
      tags:
        - create
      summary: Init module operations and return initial layout
      description: init call orchestrator to initialize process
      operationId: create-init
      parameters:
        - name: request_id, opaque_token
          in: cookie
          required: true
          description: identifier of request application in the flow
          schema:
            type: string
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PathResponse'
              examples:
                LayoutResponse:
                  $ref: '#/components/examples/PathResponseSuccess'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                TechnicalException401:
                  $ref: '#/components/examples/TechnicalException401'
        '403':
          description: Forbidden
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                TechnicalException401:
                  $ref: '#/components/examples/TechnicalException401'
        '500':
          description: Internal Server Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                InitException500:
                  $ref: '#/components/examples/TechnicalException500'
        '503':
          description: Service Unavailable
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                InitException500:
                  $ref: '#/components/examples/TechnicalException500'
        '504':
          description: Gateway Timeout
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                InitException500:
                  $ref: '#/components/examples/TechnicalException500'

header create init 

Cookie:{{request_id}}
//Cookie:opaque_token={{opaqueToken}}
X-AUTH-TOKEN:{{x_auth_token}}
//pid:8c679c2bc69a32cc5fc0b1a95ff3d6b451af22385869350924025fedfa153460-CC
//token:eyJraWQiOiIyZGZQTWttcUwwLWJXRENDVjAzOXRpNjljRWdubHMweS1DZkhydTFULXc4IiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJjaWYiOiI1MDYzMDU3MzkiLCJzdWIiOiJMZWFwNTA2MzA1NzM5IiwibGFzdEFjY2Vzc1RpbWUiOiIyMDI1LTA1LTA3IDExOjAyOjU2IiwiaXNzIjoiaHR0cHM6Ly9wYXNzcG9ydC1vYXV0aC1pc3QuYXBwcy5jbG91ZC5ibnMiLCJ1c2VyTmFtZSI6IkRhcnRoIiwidXNlcktleSI6IkxlYXA1MDYzMDU3MzkiLCJyZmMiOiJNT0NBNzMwNDIzIiwicGFzc3dvcmRMYXN0Q2hhbmdlVGltZSI6IjIwMjUtMDUtMDEgMTE6MDI6NTYiLCJjbGllbnRfaWQiOiJhdXRoZW50aWNhdGVfbXgiLCJzaWQiOiI3ZWMxNTlmMy0zNjc5LTRmNGUtYTI5MS0zMTE3MTJhYWU3OWIiLCJ1c2VyTGFzdE5hbWUiOiJWYWRlciIsImF6cCI6ImF1dGhlbnRpY2F0ZV9teCIsInNjb3BlIjoidGVzdDp0ZXN0OnRlc3Q6dGVzdCIsImV4cCI6MTc0NjcyNDM3NiwiaWF0IjoxNzQ2NzIzNzc3LCJqdGkiOiJlOTMyYzNiNy1kN2JiLTRhMDQtOTkyZi03OGNhODgzZDY2YjYifQ.EHLLLPFjdVLBFXg2UBiq7-6pZyASHIKq0osgdCdFjmuQI12rBEe1L1mfIM0brD9lgIiTURfngA6jGUOe84YR24YF-W9exNhsR4gUHZJoj0exdELHciRJjy2DjuQemx2zzwhDHexQBxKNz4raIQA9D4TKxD6Yfac_ZtbgOQ7Gj9DMReYXh8dN98bzZlKBMKVoZtatmswA_2FFT9vv6gfZZ-HV6t_P4IjKUr90DShJvVGA-JebeiwKMwFCmjVqb_G-QY421ll5M-aPbC_G6EayVXMIYCMYsLTHxyejnK6v00HfoX6HswEwRlmHMlMlGakKT9RkzIQdgWW8saCmaEwQrQ
//os-name:IOS
//os-version:14
//device-id:a123456789asdf21
x-originating-appl-code:BJFT
x-channel-id:Mobile
X-CLIENT-VERSION:14
X-DEVICE-MODEL:a123456789asdf21
X-CLIENT-TYPE:IOS
X-IMEI:345621234567890986
X-OS-TYPE:14
x-b3-traceid:41200de3d3e82a81
x-b3-spanid:41200de3d3e82a82
x-country-code:MX
x-api-version:1.0
x-user-context:eyAic3ViLWNoYW5uZWwiIDogIk1vYmlsZSBJT1MiLCAib2ZmaWNlci1pZCIgOiAiNDU2IiwgInRyYW5zaXQtaWQiIDogIjU1NiIsIHRlbGxlci1pZCIgOiAiNzQ2NCIsICIgb3JpZ2luYXRpbmctaXAtYWRkcmVzcyI6IjEyMi4xMjIuMTIuMTEifQ==
operation-type:MTB

{
    "key": "103953249166826586835",
    "request_id": "720250723192919775910",
    "next_step": "created-card",
    "state": "CREATED_CARD",
    "flow": "DC",
    "version": 1,
    "error": false
}
