# application-test.yml
server:
  port: 0 # Puerto aleatorio para tests

spring:
  application:
    name: ib-advisor-monitor-service-test
  jackson:
    property-naming-strategy: SNAKE_CASE
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: http://dummy
          jwk-set-uri: http://dummy

logging:
  level:
    root: WARN
    com.scotiabank.ib.advisor.monitor: DEBUG

# Deshabilitamos tracing y OTEL en tests
otel:
  logs:
    exporter: none
  metrics:
    exporter: none
  traces:
    exporter: none

# Feign fake para que no intente llamadas reales
feign:
  client:
    config:
      default:
        connectTimeout: 2000
        readTimeout: 2000
  proxy:
    url:
      sdui: http://localhost:9999
    endpoint:
      sdui: /sdui

# Pact deshabilitado en tests
pact:
  provider:
    enabled: false
  consumer:
    enabled: false
