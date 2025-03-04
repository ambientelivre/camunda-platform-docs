---
id: operate-configuration
title: Configuration
---

Operate is a Spring Boot application. This means every way to [configure](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)
a Spring Boot application can be applied.

By default, the configuration for Operate is stored in a YAML file (`application.yml`). All Operate-related settings are prefixed with `camunda.operate`. The following parts are configurable:

- [Webserver](#webserver)
  - [Security](#security)
- [Elasticsearch or OpenSearch](#elasticsearch-or-opensearch)
  - [Settings to connect](#settings-to-connect)
    - [Settings to connect to a secured Elasticsearch or OpenSearch instance](#settings-to-connect-to-a-secured-elasticsearch-or-opensearch-instance)
  - [Settings for shards and replicas](#settings-for-shards-and-replicas)
  - [A snippet from application.yml](#a-snippet-from-applicationyml)
- [Zeebe broker connection](#zeebe-broker-connection)
  - [Settings to connect](#settings-to-connect-1)
  - [A snippet from application.yml](#a-snippet-from-applicationyml-1)
- [Zeebe Elasticsearch or OpenSearch exporter](#zeebe-elasticsearch-or-opensearch-exporter)
  - [Settings to connect and import](#settings-to-connect-and-import)
  - [A snippet from application.yml:](#a-snippet-from-applicationyml-2)
- [Operation executor](#operation-executor)
  - [A snippet from application.yml](#a-snippet-from-applicationyml-3)
- [Monitoring Operate](#monitoring-operate)
  - [Versions before 0.25.0](#versions-before-0250)
- [Logging](#logging)
  - [JSON logging configuration](#json-logging-configuration)
  - [Change logging level at runtime](#change-logging-level-at-runtime)
    - [Set all Operate loggers to DEBUG](#set-all-operate-loggers-to-debug)
- [An example of application.yml file](#an-example-of-applicationyml-file)

## Webserver

Operate supports customizing the **context-path** using default Spring configuration.

Example for `application.yml`:
`server.servlet.context-path: /operate`

Example for environment variable:
`SERVER_SERVLET_CONTEXT_PATH=/operate`

The default context-path is `/`.

### Security

To change the values for http header for security reasons, you can use the configuration parameters:

| Name                                                                     | Description                                                                                                                                                  | Default value                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| camunda.operate.websecurity.contentSecurityPolicy                        | See [Spring description](https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/html/default-security-headers-2.html#webflux-headers-csp)  | base-uri 'self'; default-src 'self' 'unsafe-inline' 'unsafe-eval' cdn.jsdelivr.net;img-src \* data:; block-all-mixed-content; form-action 'self'; frame-ancestors 'none'; object-src 'none'; font-src 'self' fonts.camunda.io cdn.jsdelivr.net; sandbox allow-forms allow-scripts allow-same-origin allow-popups |
| camunda.operate.websecurity.httpStrictTransportSecurityMaxAgeInSeconds   | See [Spring description](https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/html/default-security-headers-2.html#webflux-headers-hsts) | 63,072,000 (two years)                                                                                                                                                                                                                                                                                           |
| camunda.operate.websecurity.httpStrictTransportSecurityIncludeSubDomains | See [Spring description](https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/html/default-security-headers-2.html#webflux-headers-hsts) | true                                                                                                                                                                                                                                                                                                             |

## Elasticsearch or OpenSearch

Operate stores and reads data from Elasticsearch or OpenSearch.

### Settings to connect

Operate supports [basic authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/setting-up-authentication.html) for Elasticsearch.

Set the appropriate username/password combination in the configuration to use it.

#### Settings to connect to a secured Elasticsearch or OpenSearch instance

To connect to a secured (https) Elasticsearch instance, you normally need to only set the URL protocol
part to `https` instead of `http`. A secured Elasticsearch instance also needs `username` and `password`.
The other SSL settings should only be used in case of connection problems; for example, in disabling
host verification.

:::note
You may need to import the certificate into JVM runtime.
:::

Either set `host` and `port` (deprecated), or `url` (recommended).

| Name                                              | Description                               | Default value         |
| ------------------------------------------------- | ----------------------------------------- | --------------------- |
| camunda.operate.elasticsearch.indexPrefix         | Prefix for index names                    | operate               |
| camunda.operate.elasticsearch.clusterName         | Cluster name of Elasticsearch             | elasticsearch         |
| camunda.operate.elasticsearch.url                 | URL of Elasticsearch REST API             | http://localhost:9200 |
| camunda.operate.elasticsearch.username            | Username to access Elasticsearch REST API | -                     |
| camunda.operate.elasticsearch.password            | Password to access Elasticsearch REST API | -                     |
| camunda.operate.elasticsearch.ssl.certificatePath | Path to certificate used by Elasticsearch | -                     |
| camunda.operate.elasticsearch.ssl.selfSigned      | Certificate was self-signed               | false                 |
| camunda.operate.elasticsearch.ssl.verifyHostname  | Should the hostname be validated          | false                 |

### Settings for shards and replicas

Operate creates the template with index settings named `operate-<version>_template` that Elasticsearch uses for all Operate indices. These settings can be changed.

The following configuration parameters define the settings:

| Name                                           | Description                                                  | Default value |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------- |
| camunda.operate.elasticsearch.numberOfShards   | How many shards Elasticsearch uses for all Operate indices   | 1             |
| camunda.operate.elasticsearch.numberOfReplicas | How many replicas Elasticsearch uses for all Operate indices | 0             |

These values are applied only on first startup of Operate or during version update. After the Operate
schema is created, settings may be adjusted directly in the Elasticsearch template, and the new settings are applied
to indices created after adjustment.

### A snippet from application.yml

```yaml
camunda.operate:
  elasticsearch:
    # Cluster name
    clusterName: elasticsearch
    # Url
    url: https://localhost:9200
    ssl:
      selfSigned: true
```

## Zeebe broker connection

Operate needs a connection to the Zeebe broker to start the import and execute user operations.

### Settings to connect

| Name                                  | Description                                                                                                        | Default value   |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------- |
| camunda.operate.zeebe.gatewayAddress  | Gateway address that points to Zeebe as hostname and port.                                                         | localhost:26500 |
| camunda.operate.zeebe.secure          | Connection should be secure via Transport Layer Security (TLS).                                                    | false           |
| camunda.operate.zeebe.certificatePath | Path to certificate used by Zeebe. This is necessary when the certificate isn't registered in the operating system | -               |

Additionally, visit [Zeebe Secure Client Communication](/docs/self-managed/zeebe-deployment/security/secure-client-communication/) for more details.

### A snippet from application.yml

```yaml
camunda.operate:
  zeebe:
    # Gateway host and port
    gatewayAddress: localhost:26500
```

## Zeebe Elasticsearch or OpenSearch exporter

For Elasticsearch, Operate imports data from indices created and filled in by the [Zeebe Elasticsearch exporter](../zeebe-deployment/exporters/elasticsearch-exporter.md).

For OpenSearch, Operate imports data from indices created and filled in by the [Zeebe OpenSearch exporter](../zeebe-deployment/exporters/opensearch-exporter.md).

Therefore, settings for this Elasticsearch or OpenSearch connection must be defined and must correspond to the settings on the Zeebe side.

### Settings to connect and import

See also [settings to connect to a secured Elasticsearch or OpenSearch instance](#settings-to-connect-to-a-secured-elasticsearch-or-opensearch-instance).

:::note
You may need to import the certificate keystore into the JVM runtime.

```yaml
# Kubernetes example:
zeebe:
  …
  javaOpts: >-
    …
    -Djavax.net.ssl.trustStore=/path/to/certificates/elasticsearch.jks
```

:::

| Name                                                   | Description                                                | Default value         |
| ------------------------------------------------------ | ---------------------------------------------------------- | --------------------- |
| camunda.operate.zeebeElasticsearch.clusterName         | Cluster name of Elasticsearch                              | elasticsearch         |
| camunda.operate.zeebeElasticsearch.url                 | URL of Zeebe Elasticsearch REST API                        | http://localhost:9200 |
| camunda.operate.zeebeElasticsearch.prefix              | Index prefix as configured in Zeebe Elasticsearch exporter | zeebe-record          |
| camunda.operate.zeebeElasticsearch.username            | Username to access Elasticsearch REST API                  | -                     |
| camunda.operate.zeebeElasticsearch.password            | Password to access Elasticsearch REST API                  | -                     |
| camunda.operate.zeebeElasticsearch.ssl.certificatePath | Path to certificate used by Elasticsearch                  | -                     |
| camunda.operate.zeebeElasticsearch.ssl.selfSigned      | Certificate was self-signed                                | false                 |
| camunda.operate.zeebeElasticsearch.ssl.verifyHostname  | Should the hostname be validated                           | false                 |

### A snippet from application.yml:

```yaml
camunda.operate:
  zeebeElasticsearch:
    # Cluster name
    clusterName: elasticsearch
    # Url
    url: https://localhost:9200
    # Index prefix, configured in Zeebe Elasticsearch exporter
    prefix: zeebe-record
```

## Operation executor

Operations are user operations, like cancellation of process instance(s) or updating the variable value.

Operations are executed in a multi-threaded manner.

| Name                                           | Description                      | Default value |
| ---------------------------------------------- | -------------------------------- | ------------- |
| camunda.operate.operationExecutor.threadsCount | How many threads should be used. | 3             |

### A snippet from application.yml

```yaml
camunda.operate:
  operationExecutor:
  	threadsCount: 3
```

## Monitoring Operate

Operate includes [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready) inside. This provides the number of monitoring possibilities.

Operate uses the following Actuator configuration by default:

```yaml
# Disable default health indicators
# https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-health-indicators
management.health.defaults.enabled: false
# enable Kubernetes health groups:
# https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes
management.health.probes.enabled: true
# enable several Actuator endpoints
management.endpoints.web.exposure.include: health, prometheus, loggers, usage-metrics, backup
```

With this configuration, the following endpoints are available for use out of the box:

`<server>:8080/actuator/prometheus` Prometheus metrics

`<server>:8080/actuator/health/liveness` Liveness probe

`<server>:8080/actuator/health/readiness` Readiness probe

This configuration may be overwritten by changing the corresponding configuration parameters values.

### Versions before 0.25.0

In versions before 0.25.0, management endpoints look different. Therefore, we recommend reconfiguring for next versions.

| Name      | Before 0.25.0    | Starting with 0.25.0       |
| --------- | ---------------- | -------------------------- |
| Readiness | /api/check       | /actuator/health/readiness |
| Liveness  | /actuator/health | /actuator/health/liveness  |

## Logging

Operate uses the Log4j2 framework for logging. In the distribution archive, as well as inside a Docker image, `config/log4j2.xml` logging configuration files are included and can be further adjusted to your needs:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN" monitorInterval="30">
  <Properties>
    <Property name="LOG_PATTERN">%clr{%d{yyyy-MM-dd HH:mm:ss.SSS}}{faint} %clr{%5p} %clr{${sys:PID}}{magenta} %clr{---}{faint} %clr{[%15.15t]}{faint} %clr{%-40.40c{1.}}{cyan} %clr{:}{faint} %m%n%xwEx</Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT" follow="true">
      <PatternLayout pattern="${LOG_PATTERN}"/>
    </Console>
	<Console name="Stackdriver" target="SYSTEM_OUT" follow="true">
      <StackdriverJSONLayout/>
    </Console>
  </Appenders>
  <Loggers>
    <Logger name="io.camunda.operate" level="info" />
    <Root level="info">
      <AppenderRef ref="${env:OPERATE_LOG_APPENDER:-Console}"/>
    </Root>
  </Loggers>
</Configuration>
```

By default, `ConsoleAppender` is used.

#### JSON logging configuration

You can choose to output logs in JSON format (Stackdriver compatible). To enable it, define
the environment variable `OPERATE_LOG_APPENDER` like this:

```sh
OPERATE_LOG_APPENDER=Stackdriver
```

### Change logging level at runtime

Operate supports the default scheme for changing logging levels as provided by [Spring Boot](https://docs.spring.io/spring-boot/docs/2.4.3/actuator-api/htmlsingle/#loggers).

The log level for Operate can be changed by following the [Setting a Log Level](https://docs.spring.io/spring-boot/docs/2.4.3/actuator-api/htmlsingle/#loggers-setting-level) section.

#### Set all Operate loggers to DEBUG

```shell
curl 'http://localhost:8080/actuator/loggers/io.camunda.operate' -i -X POST \
-H 'Content-Type: application/json' \
-d '{"configuredLevel":"debug"}'
```

## An example of application.yml file

The following snippet represents the default Operate configuration, which is shipped with the distribution. This can be found inside the `config` folder (`config/application.yml`) and can be used to adjust Operate to your needs.

```yaml
# Operate configuration file

camunda.operate:
  # Set operate userId, displayName and password.
  # If user with <userId> does not exists it will be created.
  # Default: demo/demo/demo
  userId: anUserId
  displayName: nameShownInWebpage
  password: aPassword
  roles:
    - OWNER
    - USER
  # ELS instance to store Operate data
  elasticsearch:
    # Cluster name
    clusterName: elasticsearch
    # Url
    url: http://localhost:9200
  # Zeebe instance
  zeebe:
    # Gateway address to zeebe
    gatewayAddress: localhost:26500
  # ELS instance to export Zeebe data to
  zeebeElasticsearch:
    # Cluster name
    clusterName: elasticsearch
    # url
    url: http://localhost:9200
    # Index prefix, configured in Zeebe Elasticsearch exporter
    prefix: zeebe-record
```
