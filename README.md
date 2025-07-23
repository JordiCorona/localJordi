Por que me esta dando un bad request, pudes revisar si ves algun error ?
 
este es el json de entrada 

[
    {
        "card_type": "CREDIT",
        "card_status": {
            "status": "LOCKED",
            "reason": "Por robo"
        }
    }
]

este es el response 

{
    "code": "400",
    "timestamp": "2025-07-23T18:08:12.720Z",
    "layout": {
        "type": "page",
        "data": {
            "title": "Tarjeta Digital",
            "icon": "error.svg",
            "iconWidth": 80,
            "iconHeight": 80,
            "labelHeading": "Hubo un problema con el servicio",
            "labelText": "Intenta nuevamente en unos minutos.",
            "labelCodeError": "Código: PAGE_DEFAULT_ERROR_SDUI",
            "labelButtonPrimary": "Entendido",
            "actionPrimary": "backToHome",
            "linkPrimary": null,
            "urlPrimary": "https://www.scotiabank.com",
            "labelButtonSecondary": null,
            "actionSecondary": null,
            "linkSecondary": null,
            "urlSecondary": null
        }
    }
}

y este es el LOG

[Nest] 1  - 07/23/2025, 6:08:06 PM     LOG [AuthorizationGuard] Introspection authorization enabled: true
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [AuthorizationGuard] Consuming token info with opaqueToken cookie
[Nest] 1  - 07/23/2025, 6:08:06 PM     LOG [TokenService] TokenInfo service.
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://passport-oauth-uat.apps.cloud.bns/oauth2/v1/tokeninfo
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [AuthorizationGuard] Adding bearer token to request headers
[Nest] 1  - 07/23/2025, 6:08:06 PM     LOG [PermissionsGuard] Introspection preAuthorize enabled: true
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [PermissionsGuard] Requested scopes: digital.card.ib.journey.read,digital.card.ib.journey.write
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [PermissionsGuard] Available scopes: baas:mx:reports:all,cards.plastic-card.BDRQ.activation.write,baas:mx:mortgages:all,baas:mx:payroll:all,identity.verification.ib.journey.write,notification.delivery-status.bdrq.delivery-status.read,baas:ib:keymaster:map,compliance.compliance.ib.journey.read,digital.card.ib.journey.read,baas:mx:locations:all,client.kyc.ib.journey.write,client.kyc.ib.journey.read,baas:mx:investment-products:all,baas:mx:quick-response:write,client.card-management.ib.journey.write,fulfillment.fulfillment.ib.journey.write,baas:mx:loans:all,baas:mx:all:all,ca:baas:contents:read,landing.store-front.ib.journey.write,baas:mx:statements:all,digital.card.ib.journey.write,notification.delivery-history.bdrq.history.read,baas:mx:token-information:read,baas:mx:preauthorized-payments:all,multiproduct-offer.ib.journey.write,baas:mx:summary:all,fulfillment.fulfillment.ib.journey.read,baas:mx:references:all,client.card-management.ib.journey.read,client.product-configuration.ib.journey.read,notification.delivery-history.bdrq.history.write,baas:mx:bill-payments:allbaas:mx:payees:all,baas:mx:saving-programs:all,multiproduct-offer.ib.journey.read,compliance.compliance.ib.journey.write,baas:mx:investment-funds:all,landing.store-front.ib.journey.read,baas:mx:transfers:all,baas:mx:customers:all,cards.plastic-card.BDRQ.inquiry.read,client.product-configuration.ib.journey.write,baas:mx:accounts:all,test:test:test:test,baas:mx:credit:all,identity.verification.ib.journey.read
[Nest] 1  - 07/23/2025, 6:08:06 PM     LOG [PermissionsGuard] Introspection validated: true
[Nest] 1  - 07/23/2025, 6:08:06 PM   ERROR [BusinessCustomFilter] Exception: property 0 should not exist,The value card status is required.,The value card type is required. - status: 400
BadRequestException: Bad Request Exception
    at ValidationPipe.exceptionFactory (/app/node_modules/@nestjs/common/pipes/validation.pipe.js:101:20)
    at ValidationPipe.transform (/app/node_modules/@nestjs/common/pipes/validation.pipe.js:74:30)
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
[Nest] 1  - 07/23/2025, 6:08:06 PM     LOG [ServerDrivenUiService] Getting data from server driven ui http://sdui-service.ibwork.svc/digital-card/mx/error/400.json
[Nest] 1  - 07/23/2025, 6:08:06 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://sdui-service.ibwork.svc/digital-card/mx/error/400.json
[Nest] 1  - 07/23/2025, 6:08:08 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://sdui-service.ibwork.svc/digital-card/mx/error/400.json
[Nest] 1  - 07/23/2025, 6:08:10 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://sdui-service.ibwork.svc/digital-card/mx/error/400.json
[Nest] 1  - 07/23/2025, 6:08:12 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://sdui-service.ibwork.svc/digital-card/mx/error/400.json
[Nest] 1  - 07/23/2025, 6:08:12 PM   ERROR [ServerDrivenUiService] Error not found SDUI JSON, category ERROR
[Nest] 1  - 07/23/2025, 6:08:12 PM   ERROR [ServerDrivenUiService] AxiosError: Request failed with status code 404
[Nest] 1  - 07/23/2025, 6:08:12 PM   ERROR [ErrorDesignStrategy] Error not found SDUI JSON - name: 400
[Nest] 1  - 07/23/2025, 6:08:12 PM   ERROR [ErrorDesignStrategy] SduiException: Error not found SDUI JSON, category ERROR
{"level":30,"time":"2025-07-23T18:08:12.722Z","pid":1,"hostname":"ib-digital-card-journey-574d945d44-b2rsv","req":{"id":243289,"method":"PUT","url":"/digital-card/v1/operations/status","headers":{"host":"ib-digital-card-journey.ibprse.svc","x-b3-flags":"0","x-request-id":"87a5e1643b72cc3ab5388cb2c0ea7ff8","x-real-ip":"127.0.0.1","x-forwarded-host":"ib-security-manager.ibwork.uat.npe-k8s.ib.bns","x-forwarded-port":"443","x-forwarded-proto":"https","x-forwarded-scheme":"https","x-scheme":"https","content-length":"154","x-auth-token":"460a0d06-3c97-4bda-b186-e29d846c51de","x-originating-appl-code":"BJFT","x-channel-id":"Mobile","x-client-version":"14","x-device-model":"a123456789asdf21","x-client-type":"IOS","x-imei":"345621234567890986","x-os-type":"14","x-country-code":"MX","x-api-version":"1.0","x-user-context":"eyAic3ViLWNoYW5uZWwiIDogIk1vYmlsZSBJT1MiLCAib2ZmaWNlci1pZCIgOiAiNDU2IiwgInRyYW5zaXQtaWQiIDogIjU1NiIsIHRlbGxlci1pZCIgOiAiNzQ2NCIsICIgb3JpZ2luYXRpbmctaXAtYWRkcmVzcyI6IjEyMi4xMjIuMTIuMTEifQ==","operation-type":"MTB","content-type":"application/json","user-agent":"PostmanRuntime/7.26.10","accept":"*/*","cache-control":"no-cache","postman-token":"3abda2dc-125c-457f-8c10-a08654680c9b","accept-encoding":"gzip, deflate, br","x-ruxit-forwarded-for":"127.0.0.1","x-envoy-attempt-count":"1","cookie":"opaque_token=tE6WQ8rjycPOzSqSlrXKZRNT-IO5VWMNZTLKSYLNKHI;request_id=720250723180735826394","x-forwarded-client-cert":"By=spiffe://cluster.local/ns/ibprse/sa/ib-digital-card-journey;Hash=7e03b898cc85e1d164aca79ad4879f76713675c88209d7ba8dd1fd6cdfff1f3d;Subject=\"\";URI=spiffe://cluster.local/ns/ibwork/sa/ib-security-manager","x-b3-traceid":"41200de3d3e82a81","x-b3-spanid":"9d0e76eb8385ef25","x-b3-parentspanid":"9e360837f45033bd","x-b3-sampled":"0","x-dynatrace-application":"v=2;appId=;rid=353565317;rpid=447680114"},"body":[{"card_type":"CREDIT","card_status":{"status":"LOCKED","reason":"Por robo"}}]},"res":{"statusCode":400,"headers":{"x-powered-by":"Express","content-type":"application/json; charset=utf-8","content-length":"530","etag":"W/\"212-YiBw8KBgyq/k9VAsVFbOwXOhuoE\""}},"responseTime":6204,"spanId":"eab05b78ee61d6e7","traceId":"000000000000000041200de3d3e82a81","msg":"request completed"}
