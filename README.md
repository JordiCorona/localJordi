app:
  dynamicLayoutAdatper:
    sdui:
      file:
        name:
          config: sdui/mx

management:
  endpoints:
    web:
      base-path: /
      exposure:
        include: health, info
  tracing:
    propagation:
      consume: b3_multi
      produce: b3_multi
      type: b3_multi
otel:
  logs:
    exporter: none
  metrics:
    exporter: none
  propagators: b3multi
  resource:
    attributes:
      service:
        name: ib-advisor-monitor-service
        namespace: ibwork
  traces:
    exporter: none

server:
  port: 8080
spring:
  application:
    name: ib-advisor-monitor-service
  jackson:
    property-naming-strategy: SNAKE_CASE
  security:
    oauth2:
      client:
        registration:
          passport-client:
            client-id: 838de6d9-ad37-48a1-b570-f5832d1486ef
            client-authentication-method: private_key_jwt
            authorization-grant-type: client_credentials
            client-name: passport-client
            provider: passport
        provider:
          passport:
            jwk-set-uri: https://passport-oauth-ist.apps.cloud.bns/oauth2/v1/certs
            token-uri: https://passport-oauth-ist.apps.cloud.bns/oauth2/v1/token

      resourceserver:
        jwt:
          issuer-uri: https://passport-oauth-ist.apps.cloud.bns
          jwk-set-uri: https://passport-oauth-ist.apps.cloud.bns/oauth2/v1/certs

security:
  client-credentials:
    passport-client:
      enabled: true
      secrets:
        key-store: ${PASSPORT_KEYSTORE_BASE64}
        key-store-password: ${PASSPORT_KEYSTORE_SECRET_BASE64}
        key-store-alias: ib-security-lib-ist
        key-store-type: JKS

pactbroker:
  url: https://pact-broker.ist.npe-k8s.ib.bns/

pact:
  verifier:
    publishResults: true
  provider:
    version: 1.0.22
    tag: production
    enabled: true
  consumer:
    version: 1.0.1
    tag: latest
    enabled: false

logging.level:
  com.scotiabank.ib.client.information.feign: INFO

feign:
  client:
    config:
      default:
        connectTimeout: 60000
        readTimeout: 60000
  proxy:
    url:
      sdui: http://localhost:1080
    endpoint:
      sdui: /sdui
