[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [AuthorizationGuard] Introspection authorization enabled: true
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [AuthorizationGuard] Consuming token info with opaqueToken cookie
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [TokenService] TokenInfo service.
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://passport-oauth-uat.apps.cloud.bns/oauth2/v1/tokeninfo
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [AuthorizationGuard] Adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PermissionsGuard] Introspection preAuthorize enabled: true
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [PermissionsGuard] Requested scopes: digital.card.ib.journey.read,digital.card.ib.journey.write
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [PermissionsGuard] Available scopes: baas:mx:reports:all,cards.plastic-card.BDRQ.activation.write,baas:mx:mortgages:all,baas:mx:payroll:all,identity.verification.ib.journey.write,notification.delivery-status.bdrq.delivery-status.read,baas:ib:keymaster:map,compliance.compliance.ib.journey.read,digital.card.ib.journey.read,baas:mx:locations:all,client.kyc.ib.journey.write,client.kyc.ib.journey.read,baas:mx:investment-products:all,baas:mx:quick-response:write,client.card-management.ib.journey.write,fulfillment.fulfillment.ib.journey.write,baas:mx:loans:all,baas:mx:all:all,ca:baas:contents:read,landing.store-front.ib.journey.write,baas:mx:statements:all,digital.card.ib.journey.write,notification.delivery-history.bdrq.history.read,baas:mx:token-information:read,baas:mx:preauthorized-payments:all,multiproduct-offer.ib.journey.write,baas:mx:summary:all,fulfillment.fulfillment.ib.journey.read,baas:mx:references:all,client.card-management.ib.journey.read,client.product-configuration.ib.journey.read,notification.delivery-history.bdrq.history.write,baas:mx:bill-payments:allbaas:mx:payees:all,baas:mx:saving-programs:all,multiproduct-offer.ib.journey.read,compliance.compliance.ib.journey.write,baas:mx:investment-funds:all,landing.store-front.ib.journey.read,baas:mx:transfers:all,baas:mx:customers:all,cards.plastic-card.BDRQ.inquiry.read,client.product-configuration.ib.journey.write,baas:mx:accounts:all,test:test:test:test,baas:mx:credit:all,identity.verification.ib.journey.read
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PermissionsGuard] Introspection validated: true
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [CreateController] Bea --->: Bearer eyJraWQiOiJmS0Y0alIxY0JIVmJWVVNrLTBCZGxOZnp6blBwT3k3d1FWNm1nX0VVSUNFIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJVc3I1MDYzMzE1OTgiLCJjaWYiOiI1MDYzMzE1OTgiLCJsYXN0QWNjZXNzVGltZSI6IjIwMjUtMDctMjIgMTg6NTA6MzEiLCJpc3MiOiJodHRwczovL3Bhc3Nwb3J0LXUuc2NvaW50bmV0Lm5ldCIsImlwQWRkcmVzcyI6IjAzMTc1OTMwLTAzOEYtNERDMS04MDVCLUJBNkVGMUY2OThBNSIsInVzZXJOYW1lIjoiRVJORVNUTyIsImNsaWVudF9pZCI6IjNkMTYyNTJlLTRiNjQtNGZjMy1hODU3LTg4NWE5ZDg5N2QyYiIsInVzZXJLZXkiOiJVc3I1MDYzMzE1OTgiLCJyZmMiOiJBQUxFOTIxMDIzUks5IiwicGFzc3dvcmRMYXN0Q2hhbmdlVGltZSI6IjIwMjUtMDMtMDUgMTg6MjE6MTAiLCJzaWQiOiI4ZTEwOTBlNC0xYzk1LTRkOGUtODkwNi0wNzcyYzhmZTIzYWYiLCJhenAiOiIzZDE2MjUyZS00YjY0LTRmYzMtYTg1Ny04ODVhOWQ4OTdkMmIiLCJ1c2VyTGFzdE5hbWUiOiJBTkRSQURFIiwic2NvcGUiOiJiYWFzOm14OnJlcG9ydHM6YWxsIGNhcmRzLnBsYXN0aWMtY2FyZC5CRFJRLmFjdGl2YXRpb24ud3JpdGUgYmFhczpteDptb3J0Z2FnZXM6YWxsIGJhYXM6bXg6cGF5cm9sbDphbGwgaWRlbnRpdHkudmVyaWZpY2F0aW9uLmliLmpvdXJuZXkud3JpdGUgbm90aWZpY2F0aW9uLmRlbGl2ZXJ5LXN0YXR1cy5iZHJxLmRlbGl2ZXJ5LXN0YXR1cy5yZWFkIGJhYXM6aWI6a2V5bWFzdGVyOm1hcCBjb21wbGlhbmNlLmNvbXBsaWFuY2UuaWIuam91cm5leS5yZWFkIGRpZ2l0YWwuY2FyZC5pYi5qb3VybmV5LnJlYWQgYmFhczpteDpsb2NhdGlvbnM6YWxsIGNsaWVudC5reWMuaWIuam91cm5leS53cml0ZSBjbGllbnQua3ljLmliLmpvdXJuZXkucmVhZCBiYWFzOm14OmludmVzdG1lbnQtcHJvZHVjdHM6YWxsIGJhYXM6bXg6cXVpY2stcmVzcG9uc2U6d3JpdGUgY2xpZW50LmNhcmQtbWFuYWdlbWVudC5pYi5qb3VybmV5LndyaXRlIGZ1bGZpbGxtZW50LmZ1bGZpbGxtZW50LmliLmpvdXJuZXkud3JpdGUgYmFhczpteDpsb2FuczphbGwgYmFhczpteDphbGw6YWxsIGNhOmJhYXM6Y29udGVudHM6cmVhZCBsYW5kaW5nLnN0b3JlLWZyb250LmliLmpvdXJuZXkud3JpdGUgYmFhczpteDpzdGF0ZW1lbnRzOmFsbCBkaWdpdGFsLmNhcmQuaWIuam91cm5leS53cml0ZSBub3RpZmljYXRpb24uZGVsaXZlcnktaGlzdG9yeS5iZHJxLmhpc3RvcnkucmVhZCBiYWFzOm14OnRva2VuLWluZm9ybWF0aW9uOnJlYWQgYmFhczpteDpwcmVhdXRob3JpemVkLXBheW1lbnRzOmFsbCBtdWx0aXByb2R1Y3Qtb2ZmZXIuaWIuam91cm5leS53cml0ZSBiYWFzOm14OnN1bW1hcnk6YWxsIGZ1bGZpbGxtZW50LmZ1bGZpbGxtZW50LmliLmpvdXJuZXkucmVhZCBiYWFzOm14OnJlZmVyZW5jZXM6YWxsIGNsaWVudC5jYXJkLW1hbmFnZW1lbnQuaWIuam91cm5leS5yZWFkIGNsaWVudC5wcm9kdWN0LWNvbmZpZ3VyYXRpb24uaWIuam91cm5leS5yZWFkIG5vdGlmaWNhdGlvbi5kZWxpdmVyeS1oaXN0b3J5LmJkcnEuaGlzdG9yeS53cml0ZSBiYWFzOm14OmJpbGwtcGF5bWVudHM6YWxsYmFhczpteDpwYXllZXM6YWxsIGJhYXM6bXg6c2F2aW5nLXByb2dyYW1zOmFsbCBtdWx0aXByb2R1Y3Qtb2ZmZXIuaWIuam91cm5leS5yZWFkIGNvbXBsaWFuY2UuY29tcGxpYW5jZS5pYi5qb3VybmV5LndyaXRlIGJhYXM6bXg6aW52ZXN0bWVudC1mdW5kczphbGwgbGFuZGluZy5zdG9yZS1mcm9udC5pYi5qb3VybmV5LnJlYWQgYmFhczpteDp0cmFuc2ZlcnM6YWxsIGJhYXM6bXg6Y3VzdG9tZXJzOmFsbCBjYXJkcy5wbGFzdGljLWNhcmQuQkRSUS5pbnF1aXJ5LnJlYWQgY2xpZW50LnByb2R1Y3QtY29uZmlndXJhdGlvbi5pYi5qb3VybmV5LndyaXRlIGJhYXM6bXg6YWNjb3VudHM6YWxsIHRlc3Q6dGVzdDp0ZXN0OnRlc3QgYmFhczpteDpjcmVkaXQ6YWxsIGlkZW50aXR5LnZlcmlmaWNhdGlvbi5pYi5qb3VybmV5LnJlYWQiLCJ1c2VyTWlkZGxlTmFtZSI6IkxVTkEiLCJleHAiOjE3NTMyMTIxMTUsImlhdCI6MTc1MzIxMTUxNSwianRpIjoiNjIyNjRlYzEtZTdlYy00MTg3LWIyNTYtYWEyOWQ0OGQ1Y2MzIn0.h4hiAko1kNk9gDYF0I2kkes0HykV_-oejd4Zi7p44FyqURC5-SYLQouHQoteFzyIZSZ-C_NuV0LaLuENnVQVqxQytD3Zce7oxA40GQAPWahl1hCqlG2jkTHXQH5ClstFJYjuDHUt2ynLKvrsA21lR98wrBC8y0e27oetCYu6WGnTStwOFiguK3s50rhH1ieTYLl3bkU_WrOX8yzOYdzrlPvN6lHGbIiL5E3lubcaCJDkedlWWPgJxPE9iYZE6KHunoFgE2_vruYocljhFO37xjDR14qJ8TUHmxRMoaIQF9jj5GMBpbaFT8eLVOF-_PygHhc0HuRtm9c9CfDlPJvITQ
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG Starting the digital card creation process for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "SIEM_BUSINESS_LOG"
  ],
  "siem": true,
  "success": true
}
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [CreateService] requestId >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "72025072219110093355"
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG Customer information is obtained for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "TEMP_DATA_TAG"
  ],
  "siem": true
}
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PersonalDataService] requestId in getByRequestId >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "72025072219110093355"
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PersonalDataService] url >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "http://ib-client-application-data.ibclie.svc/ib-client-application-data/data?request_id=72025072219110093355"
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PersonalDataService] Init call service GET: -> http://ib-client-application-data.ibclie.svc/ib-client-application-data/data?request_id=72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:55 PM     LOG [PersonalDataService] source >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "DIGITAL_CARD"
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:55 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://passport-oauth-uat.apps.cloud.bns/oauth2/v1/token
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [TokenService] Client credentials token generated
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [PersonalDataService] Success call to service -> http://ib-client-application-data.ibclie.svc/ib-client-application-data/data?request_id=72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [CreateService] personalDataResponse >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"request_id":"72025072219110093355","categories":[{"category_transaction_data":"BASIC_PERSONAL","transaction_data":{"first_name":"ERNESTO","last_name":"ANDRADE","second_last_name":"LUNA","client_id":"9d3089f0-825f-4f37-b0d8-e1634c80dd90","segment_id":"","banking_segment":"","gender_code":"M","birthdate":"1992-10-23","nationality_code":"MX","nationality_description":"Mexico","country_birth_code":"MX","country_birth_description":"Mexico","state_birth_code":"08","state_birth_description":"CHIHUAHUA","publicity_flag":null,"identifications":[{"sequence":"01","type":"03","number":"AALE921023RK9","expiration_date":null,"expedition_date":null,"country_code":"MX","country_description":"Mexico"},{"sequence":"02","type":"05","number":"AALE921023HDFNNR05","expiration_date":"2001-01-01","expedition_date":null,"country_code":"MX","country_description":"Mexico"},{"sequence":"03","type":"01","number":"2312541231121","expiration_date":"2029-12-10","expedition_date":null,"country_code":"MX","country_description":"Mexico"}],"user_key":"Usr506331598"}},{"category_transaction_data":"CONTACT","transaction_data":{"phone":"5579509933","email":"eandrade@scotiabank.com.mx"}},{"category_transaction_data":"DEVICE_INFORMATION","transaction_data":{"operating_system":"IOS","operating_system_version":"14","device_number":"a123456789asdf21"}},{"category_transaction_data":"PRODUCT_SUMMARY","transaction_data":{"accounts":[{"type":"TARJETA DE CREDITO","description":"VISA CLÁSICA","product_id":"9240","subproduct_id":"9100","account_id":"54a156e6-a009-4bfc-a6d1-63f7b6e03ae6","account_id_masked":"************7025","branch_id":"100001","date_opened":"2025-03-06"}]}}]}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [CreateService] basicPersonal >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"category_transaction_data":"BASIC_PERSONAL","transaction_data":{"first_name":"ERNESTO","last_name":"ANDRADE","second_last_name":"LUNA","client_id":"9d3089f0-825f-4f37-b0d8-e1634c80dd90","segment_id":"","banking_segment":"","gender_code":"M","birthdate":"1992-10-23","nationality_code":"MX","nationality_description":"Mexico","country_birth_code":"MX","country_birth_description":"Mexico","state_birth_code":"08","state_birth_description":"CHIHUAHUA","publicity_flag":null,"identifications":[{"sequence":"01","type":"03","number":"AALE921023RK9","expiration_date":null,"expedition_date":null,"country_code":"MX","country_description":"Mexico"},{"sequence":"02","type":"05","number":"AALE921023HDFNNR05","expiration_date":"2001-01-01","expedition_date":null,"country_code":"MX","country_description":"Mexico"},{"sequence":"03","type":"01","number":"2312541231121","expiration_date":"2029-12-10","expedition_date":null,"country_code":"MX","country_description":"Mexico"}],"user_key":"Usr506331598"}}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [CreateService] productSummaryCat >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"category_transaction_data":"PRODUCT_SUMMARY","transaction_data":{"accounts":[{"type":"TARJETA DE CREDITO","description":"VISA CLÁSICA","product_id":"9240","subproduct_id":"9100","account_id":"54a156e6-a009-4bfc-a6d1-63f7b6e03ae6","account_id_masked":"************7025","branch_id":"100001","date_opened":"2025-03-06"}]}}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbSecuritySignatures] Ellave configuration loaded for country 'MX' : {"enable":true,"modules":{"createService":{"operationType":"CREATE_DIGITAL_CREDIT_CARD","operationUrl":"/digital-card/create"}}}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [BaasSecuritySignaturesService] [ib-mx-security-signatures-lib] Start validating OTP
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [BaasSecuritySignaturesService] operationType: CREATE_DIGITAL_CREDIT_CARD - operationUrl: '/digital-card/create' - requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [AssertionTokenFlowService] Assertion token code flow enabled: true
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [AssertionTokenFlowService] Consuming get assertion token endpoint
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://passport-oauth-uat.apps.cloud.bns/oauth2/v1/token
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [AssertionTokenFlowService] Opaque token: eyJraWQiOiJmS0Y0alIxY0JIVmJWVVNrLTBCZGxOZnp6blBwT3k3d1FWNm1nX0VVSUNFIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJjaWYiOiI1MDYzMzE1OTgiLCJzdWIiOiI2MDVlYjBlNC0xYzlmLTRkZDQtYjRhNS00NGEyMWZmNDU5YmMiLCJhenAiOiI2MDVlYjBlNC0xYzlmLTRkZDQtYjRhNS00NGEyMWZmNDU5YmMiLCJzY29wZSI6ImNsaWVudC5hcHBsaWNhdGlvbi1kYXRhLmliLnNlcnZpY2UucmVhZCBjdXN0b21lcnMucHJvZmlsZS5iamZ0LmlucXVpcnkucmVhZCBub3RpZmljYXRpb24uc2VuZC1zbXMuYmpmdC5zZW5kLXNtcy53cml0ZSBiYWFzOmliOmtleW1hc3RlcjptYXAgZGlnaXRhbC5jYXJkLmliLmpvdXJuZXkucmVhZCBjbGllbnQuYXBwbGljYXRpb24tZGF0YS5pYi5zZXJ2aWNlLndyaXRlIGJhYXM6bXg6Y3VzdG9tZXJzOnJlYWQgY2FyZC5jcmVhdGUtZW5yb2xsbWVudC5iamZ0LmVucm9sbG1lbnQud3JpdGUgYmFhczpteDpzZW5kLWVtYWlsOndyaXRlIGRpZ2l0YWwuY2FyZC5pYi5qb3VybmV5LndyaXRlIGNhcmQuY2FyZC1ncm91cC1zdGF0dXMuYmpmdC5zdGF0dXMud3JpdGUiLCJpc3MiOiJodHRwczovL3Bhc3Nwb3J0LXUuc2NvaW50bmV0Lm5ldCIsImV4cCI6MTc1MzIxNDUxNiwiaWF0IjoxNzUzMjExNTE2LCJqdGkiOiIzOWUxZTkxOS1iMzU2LTQ4NjEtYjQwYy0zZjRiMWFjMzI4ODciLCJwYXNzd29yZExhc3RDaGFuZ2VUaW1lIjoiMjAyNS0wMy0wNSAxODoyMToxMCIsImNsaWVudF9pZCI6IjYwNWViMGU0LTFjOWYtNGRkNC1iNGE1LTQ0YTIxZmY0NTliYyIsInNpZCI6IjhlMTA5MGU0LTFjOTUtNGQ4ZS04OTA2LTA3NzJjOGZlMjNhZiJ9.PHjt_Zs0rIKbaQ4MnLUXr7queVHHTRcWsBkpnXls5hYRTI6-enAJ7QNCr0n4lmeffb_d7NJuzhmUhK5yBzgqayPb86qtEF4V3iye25C9AyvCe4mRLb2HrutXJPaQ1gXtv1H2SczsHsyVwq-8Q6WomDpqsFnsLxDK7Xzn3c3MQJIwBS2p-cYthWeJUoLNKo3OLzldMx5vsbzYQyCNwgIcA80w1HlVXpdO2ABOjGjNEgRHiGn0dL8bL8SL6UC02GPztKwwVjR0Ka6SldSlid_6Up06QuKkskjTT_hjkI1fh_tJUUmsq6AEcq3sPu3Pcqdn1yy248_4oAQBVJOkR21n_w
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://baas-security-signatures.external.svc/v3/security/signatures
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [AssertionTokenFlowService] Assertion token code flow enabled: true
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [AssertionTokenFlowService] Consuming get assertion token endpoint
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://passport-oauth-uat.apps.cloud.bns/oauth2/v1/token
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [AssertionTokenFlowService] Opaque token: eyJraWQiOiJmS0Y0alIxY0JIVmJWVVNrLTBCZGxOZnp6blBwT3k3d1FWNm1nX0VVSUNFIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJjaWYiOiI1MDYzMzE1OTgiLCJzdWIiOiI2MDVlYjBlNC0xYzlmLTRkZDQtYjRhNS00NGEyMWZmNDU5YmMiLCJhenAiOiI2MDVlYjBlNC0xYzlmLTRkZDQtYjRhNS00NGEyMWZmNDU5YmMiLCJzY29wZSI6ImNsaWVudC5hcHBsaWNhdGlvbi1kYXRhLmliLnNlcnZpY2UucmVhZCBjdXN0b21lcnMucHJvZmlsZS5iamZ0LmlucXVpcnkucmVhZCBub3RpZmljYXRpb24uc2VuZC1zbXMuYmpmdC5zZW5kLXNtcy53cml0ZSBiYWFzOmliOmtleW1hc3RlcjptYXAgZGlnaXRhbC5jYXJkLmliLmpvdXJuZXkucmVhZCBjbGllbnQuYXBwbGljYXRpb24tZGF0YS5pYi5zZXJ2aWNlLndyaXRlIGJhYXM6bXg6Y3VzdG9tZXJzOnJlYWQgY2FyZC5jcmVhdGUtZW5yb2xsbWVudC5iamZ0LmVucm9sbG1lbnQud3JpdGUgYmFhczpteDpzZW5kLWVtYWlsOndyaXRlIGRpZ2l0YWwuY2FyZC5pYi5qb3VybmV5LndyaXRlIGNhcmQuY2FyZC1ncm91cC1zdGF0dXMuYmpmdC5zdGF0dXMud3JpdGUiLCJpc3MiOiJodHRwczovL3Bhc3Nwb3J0LXUuc2NvaW50bmV0Lm5ldCIsImV4cCI6MTc1MzIxNDUxNiwiaWF0IjoxNzUzMjExNTE2LCJqdGkiOiI4NzlmOTZmMy01YjkwLTRkY2YtYjA4NC02YzIwY2Y2YTQzNjIiLCJwYXNzd29yZExhc3RDaGFuZ2VUaW1lIjoiMjAyNS0wMy0wNSAxODoyMToxMCIsImNsaWVudF9pZCI6IjYwNWViMGU0LTFjOWYtNGRkNC1iNGE1LTQ0YTIxZmY0NTliYyIsInNpZCI6IjhlMTA5MGU0LTFjOTUtNGQ4ZS04OTA2LTA3NzJjOGZlMjNhZiJ9.f2EOIrePseLGICp-d2SqnFHLRSjQ-2PRQAKKR1dNW8eRRyTnsnL3K2hStjVzbk-kuCYQhTif5g78dcrwNua1ybn8dwxWewhEAd3H7CLjuKg9e-SM5HQUiyVPNjfTYK2WNPDkuEDUAEdaMNwario7i_vwHGZVI9eWxZ_MCAbL0fjhVYMBkc84aynzHvRu5VA0QEBKH-c20x6dx-C2G93PvCS6Wz8XOUnVTN3XLEvf8ANhflYr1CRMHrYyi4CXoZFrj9O7jBZNaOukrIU2ZRe6JM0OP4CuNxsb7QPp-s4SZIEKjyyWbDu27Or2I-2rEHHntuwGBZ5fRoBGi58oOHlTbw
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow url included in whitelist http://baas-security-signatures.external.svc/v3/security/signatures/7ec2b0ea-dabd-40e3-9047-00dc9ff7522d/signers/0/signature
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [BaasSecuritySignaturesServiceValidation] validateSignature method response: {"statusCode":200,"message":"OK","timeStamp":"2025-07-22T13:11:56.54353928-06:00"}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [BaasSecuritySignaturesService] The OTP value is valid.
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [BaasSecuritySignaturesService] [ib-mx-security-signatures-lib] End validating OTP
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbSecuritySignatures] Received reponse: {"statusCode":200,"message":"OK","timeStamp":"2025-07-22T13:11:56.54353928-06:00"}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbBaasActivityLogConnector] Init call service GET: -> http://baas-customers-activity-reporting.external.svc/v1/customers/9d3089f0-825f-4f37-b0d8-e1634c80dd90/activities
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbBaasActivityLogConnector] Starts the construction of the activity log request object
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbBaasActivityLogConnector] Ends construction of activity log request object
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG Ellave is validated for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "SIEM_BUSINESS_LOG"
  ],
  "siem": true,
  "success": true
}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [CreateService] url create journey config Server>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "http://card-bcsmx-enrollment-api.external.svc/api/v1/card/enrollment-create"
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [CreateService] url create journey process >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "http://card-bcsmx-enrollment-api.external.svc/api/v1/card/enrollment-create"
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbCreateConnectorBcsService] url>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "http://card-bcsmx-enrollment-api.external.svc/api/v1/card/enrollment-create"
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbCreateConnectorBcsService] headerssss>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"Authorization":"Bearer undefined","channel-id":"362b2089-ff70-493f-9b47-4afc6437fd60","Content-Type":"application/json","x-b3-traceid":"41200de3d3e82a81","x-b3-spanid":"41200de3d3e82a81","x-channel-id":"Online","x-originating-appl-code":"BG3C","x-country-code":"MX"}
[Nest] 1  - 07/22/2025, 7:11:56 PM     LOG [IbCreateConnectorBcsService] requesstttt>>>>     {"card_info":{"card_id":"54a156e6-a009-4bfc-a6d1-63f7b6e03ae6","card_type":"CREDIT","card_holder_type":"PRIMARY_DIGITAL"}}
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [TokenService] Getting client credentials token from Cache
[Nest] 1  - 07/22/2025, 7:11:56 PM   DEBUG [TokenService] Getting client credentials token from Cache
[Nest] 1  - 07/22/2025, 7:11:57 PM     LOG [BaasSecuritySignaturesService] The activity registration operation 'VALIDATED_ELLAVE' has completed successfully
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbCreateConnectorBcsService] response.data>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"data":{"card_number":null,"card_number_key":"4079cb0d-339e-4aa5-8185-cae69b207a41"},"notifications":[{"category":null,"code":"I_CEN_BS_001","message":"Success operation","description":null,"action":null,"metadata":{},"uuid":null,"timestamp":"2025-07-22T19:11:58.271+00:00","severity":null}]}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbBaasActivityLogConnector] Init call service GET: -> http://baas-customers-activity-reporting.external.svc/v1/customers/9d3089f0-825f-4f37-b0d8-e1634c80dd90/activities
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbBaasActivityLogConnector] Starts the construction of the activity log request object
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbBaasActivityLogConnector] Ends construction of activity log request object
[Nest] 1  - 07/22/2025, 7:11:58 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:58 PM   DEBUG [TokenService] Getting client credentials token from Cache
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbCreateConnectorBcsService] Event : 452  : undefined :  undefined
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [CreateBcsService] got create from bcs by account 54a156e6-a009-4bfc-a6d1-63f7b6e03ae6 successful
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [CreateService] responseDigitalCard >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"data":{"card_number":null,"card_number_key":"4079cb0d-339e-4aa5-8185-cae69b207a41"},"notifications":[{"category":null,"code":"I_CEN_BS_001","message":"Success operation","description":null,"action":null,"metadata":{},"uuid":null,"timestamp":"2025-07-22T19:11:58.271+00:00","severity":null}]}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Successful card digital activating for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "SIEM_BUSINESS_LOG"
  ],
  "siem": true,
  "success": true
}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Starting saving digital card to personalData for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "SIEM_BUSINESS_LOG"
  ],
  "siem": true,
  "success": true
}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [PersonalDataService] Init call service POST: -> http://ib-client-application-data.ibclie.svc/ib-client-application-data/data
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [PersonalDataService] request save client data  in save Personal-data >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   {"request_id":"72025072219110093355","categories":[{"category_transaction_data":"PRODUCT_SUMMARY","transaction_data":{"accounts":[{"type":"TARJETA DE CREDITO","description":"VISA CLÁSICA","product_id":"9240","subproduct_id":"9100","account_id":"54a156e6-a009-4bfc-a6d1-63f7b6e03ae6","account_id_masked":"************7025","branch_id":"100001","date_opened":"2025-03-06"},{"type":"TARJETA DE CREDITO DIGITAL","description":"VISA CLÁSICA","product_id":"9240D","account_id":"4079cb0d-339e-4aa5-8185-cae69b207a41","branch_id":"100001","account_id_masked":null}]}}]}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [PersonalDataService] token  in save Personal-data >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "Bearer undefined"
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Digital card successfully saved in personalData for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "TEMP_DATA_TAG"
  ],
  "siem": true
}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbEventService] Sending emit message to kafka {"flow":"DC","request_id":"72025072219110093355","domain":"servicing","source":"ib-digital-card","date":"2025-07-22T19:11:58.492Z","state":"DC_CARD_CREATED","action":"Create card success"} {"source":"ib-digital-card","X-B3-TraceId":"000000000000000041200de3d3e82a83","X-B3-SpanId":"eebbd7191636ac18","X-B3-Sampled":"1","compliance":"false","request_id":"72025072219110093355"}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [SchemaRegistryService] Encoding message for model EventTracing
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbEventService] Getting request topic name for sending the message
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [EventTracingService] eventTracing service called
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [AuditEventService] Success call to eventTracingService service
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [EmailService] Sending email notification for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Starts building of email notification request object
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [PushService] Sending push notification for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Starts building of push notification request object
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbEventService] Sending message to kafka {"request_id":"72025072219110093355","additional_properties":{"state":"DETAILS_COMPLETED"}} {"source":"CREATED_CARD","X-B3-TraceId":"000000000000000041200de3d3e82a83","X-B3-SpanId":"eebbd7191636ac18","X-B3-Sampled":"1"}
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [SchemaRegistryService] Encoding message for model BusinessProcessDelivery
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [NotificationsServiceDelegate] BCS notifications services executions results: [{"result":{"status":"rejected","reason":{}}},{"result":{"status":"rejected","reason":{}}}]
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [IbEventService] Getting request topic name for sending the message
[Nest] 1  - 07/22/2025, 7:11:58 PM   DEBUG [ClientCredentialsFlowInterceptor] Client Credentials flow adding bearer token to request headers
[Nest] 1  - 07/22/2025, 7:11:58 PM   DEBUG [TokenService] Getting client credentials token from Cache
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [PersonalDataService] Success call to save -> http://ib-client-application-data.ibclie.svc/ib-client-application-data/data
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [SchemaRegistryService] Decoding message for model BusinessProcessDelivery
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG [CreateService] Delivery in digital card create requestId: 72025072219110093355 - finalState: DETAILS_COMPLETED - nextStep details-completed
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Digital card created successfully for requestId: 72025072219110093355
[Nest] 1  - 07/22/2025, 7:11:58 PM     LOG Object:
{
  "tags": [
    "SIEMEvent",
    "SIEM_BUSINESS_LOG"
  ],
  "siem": true,
  "success": true
}
{"level":30,"time":"2025-07-22T19:11:58.735Z","pid":1,"hostname":"ib-digital-card-journey-574d945d44-b2rsv","req":{"id":226722,"method":"POST","url":"/digital-card/v1/create","headers":{"host":"ib-digital-card-journey.ibprse.svc","x-b3-flags":"0","x-request-id":"d74ba5895bd37a5104cc788a0067f00b","x-real-ip":"127.0.0.1","x-forwarded-host":"ib-security-manager.ibwork.uat.npe-k8s.ib.bns","x-forwarded-port":"443","x-forwarded-proto":"https","x-forwarded-scheme":"https","x-scheme":"https","x-otp-token":"kG1/NDOl0Cm+lGGT0gmK72friL1cXPUeWp6vZBjGsrg=3oYCOQ==","x-auth-token":"8e1090e4-1c95-4d8e-8906-0772c8fe23af","x-originating-appl-code":"BJFT","x-channel-id":"Mobile","x-client-version":"14","x-device-model":"a123456789asdf21","x-client-type":"IOS","x-imei":"345621234567890986","x-os-type":"14","x-country-code":"MX","x-api-version":"1.0","x-user-context":"eyAic3ViLWNoYW5uZWwiIDogIk1vYmlsZSBJT1MiLCAib2ZmaWNlci1pZCIgOiAiNDU2IiwgInRyYW5zaXQtaWQiIDogIjU1NiIsIHRlbGxlci1pZCIgOiAiNzQ2NCIsICIgb3JpZ2luYXRpbmctaXAtYWRkcmVzcyI6IjEyMi4xMjIuMTIuMTEifQ==","operation-type":"MTB","authorization":"Bearer eyJraWQiOiIyZGZQTWttcUwwLWJXRENDVjAzOXRpNjljRWdubHMweS1DZkhydTFULXc4IiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJjaWYiOiI1MDYzMzE1OTgiLCJzdWIiOiJVc3I1MDYzMzE1OTgiLCJsYXN0QWNjZXNzVGltZSI6IjIwMjUtMDUtMjcgMTA6MDA6NDciLCJpc3MiOiJodHRwczovL3Bhc3Nwb3J0LW9hdXRoLWlzdC5hcHBzLmNsb3VkLmJucyIsInVzZXJOYW1lIjoiRGFydGgiLCJ1c2VyS2V5IjoiVXNyNTA2MzMxNTk4IiwicmZjIjoiTU9DQTczMDQyMyIsInBhc3N3b3JkTGFzdENoYW5nZVRpbWUiOiIyMDI1LTA1LTIxIDEwOjAwOjQ3IiwiY2xpZW50X2lkIjoiYXV0aGVudGljYXRlX214Iiwic2lkIjoiMDExM2ZjYTgtY2ZhMC00ODcyLThhMjEtZjM1OGQwMmYwYjZkIiwidXNlckxhc3ROYW1lIjoiVmFkZXIiLCJhenAiOiJhdXRoZW50aWNhdGVfbXgiLCJzY29wZSI6InRlc3Q6dGVzdDp0ZXN0OnRlc3QiLCJleHAiOjE3NDg0NDg2NDcsImlhdCI6MTc0ODQ0ODA0NywianRpIjoiMDEwYTYyMDItNmZmZC00NGEwLThiMWQtNTgwNjRjN2VlNzRkIn0.VhC3sMIrbjzQm1E8KSlhB9c_BQiyvxA2BAVvJL3WbRMeM87Dx1KXdrPAgpYESWRYyegHH4dpu7QAFhV2Uq2KABtLk7aJocofuff1ah5B9HLPX1fVHRgC8XubK3J7_NsuEDA4Cup0AYAmyPWUhkMJA1gZ9fnEgfIQjTh_CgVm1N18sZ4qTfjnp1uZRrdY3NE3b5_Cflgj0Wc6lGw_87SuvnU5bDXMB8gLLBfdE1dQg_mFpyjyqkOlEuXBar2FE4hlo0QtMX3M6LS2a5dp4q_4P_owA4bFkOapjjjFin289XxWu6Dw6aD4R_DL8eW7j-t5A3-bpIjizk_MLSfMjUdGMQ","user-agent":"PostmanRuntime/7.26.10","accept":"*/*","cache-control":"no-cache","postman-token":"63ccac4e-7aa8-4680-88a6-9231f6fb2eb9","accept-encoding":"gzip, deflate, br","x-ruxit-forwarded-for":"127.0.0.1","x-envoy-attempt-count":"1","cookie":"opaque_token=HdwwPWxApWIe2pN9glFTRljBR7Jy7iFLN-F1al0cFlk;request_id=72025072219110093355","content-length":"0","x-forwarded-client-cert":"By=spiffe://cluster.local/ns/ibprse/sa/ib-digital-card-journey;Hash=6088e620f0c29bb05eb49439640002fce540f7243ad3a9bcf5369e9cebaf71be;Subject=\"\";URI=spiffe://cluster.local/ns/ibwork/sa/ib-security-manager","x-b3-traceid":"41200de3d3e82a83","x-b3-spanid":"7bf8bde1ad1afcd0","x-b3-parentspanid":"2e35a5c77944ab78","x-b3-sampled":"0","x-dynatrace-application":"v=2;appId=;rid=124643404;rpid=-973667284"},"body":{}},"res":{"statusCode":302,"headers":{"x-powered-by":"Express","location":"details-completed"}},"responseTime":2881,"spanId":"eebbd7191636ac18","traceId":"000000000000000041200de3d3e82a83","msg":"request completed"}
