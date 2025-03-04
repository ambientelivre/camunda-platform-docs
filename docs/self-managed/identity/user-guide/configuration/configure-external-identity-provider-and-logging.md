---
id: configure-external-identity-provider-and-logging
title: "Configuring an external identity provider and logging"
sidebar_label: "Configuring an external identity provider and logging"
description: "In this guide we will demonstrate how to configure an external identity provider like OpenID Connect, SAML, LDAP, or Active Directory, as well as logging."
---

:::info
The Identity UI does not offer support for configuring external identity providers. You can configure an external
identity provider directly in Keycloak Administrator Console.
:::

To configure an external identity provider like OpenID Connect, SAML, LDAP, or Active Directory, take the following steps:

1. Log in to the Keycloak Administrator Console. Open the URL you have configured for Keycloak in your browser.
   :::tip
   When using the example
   [Docker Compose](/self-managed/platform-deployment/docker.md#docker-compose) setup, Keycloak
   is available at [http://localhost:18080/](http://localhost:18080/).
   :::
2. Click **Administrator Console** and log in using the Keycloak administrator credentials. The default administrator username is `admin`. When deploying Camunda Platform 8 with [Helm charts](/self-managed/platform-deployment/helm-kubernetes/overview.md),
   you can extract the password as described in
   [secrets extraction](/self-managed/platform-deployment/helm-kubernetes/upgrade.md#secrets-extraction).
   Using the example [Docker Compose](/self-managed/platform-deployment/docker.md#docker-compose)
   setup, the password is set via `KEYCLOAK_ADMIN_PASSWORD` environment variable and is `admin` per default.
3. Select the realm you are using with Camunda Platform 8. By default, this is **Camunda-platform**.
   ![keycloak-realm-select](../img/keycloak-realm-select.png)
4. Add an identity provider using one of the following methods:
   1. To add an OpenID Connect or SAML provider, select **Identity Providers** in the main menu, click **Add provider...**, and fill in all required configuration settings.
      ![keycloak-add-identity-provider](../img/keycloak-add-identity-provider.png)
   2. To connect to your LDAP, Active Directory, or Kerberos server, select **User Federation** in the main menu, click **Add provider...**, and fill in all required configuration settings.
      ![keycloak-add-user-federation](../img/keycloak-add-user-federation.png)

:::tip
Keycloak supports a wide variety of authentication options, such as mapping external user groups, roles, or scopes to internal roles, and configuring the login screen and flow when multiple providers are added. Visit the Keycloak documentation for details on [adding a provider](https://www.keycloak.org/docs/16.1/server_admin/index.html#adding-a-provider),
[configuring authentication](https://www.keycloak.org/docs/16.1/server_admin/index.html#configuring-authentication), and
[integrating identity providers](https://www.keycloak.org/docs/16.1/server_admin/index.html#_identity_broker).
:::

## Configuring logging

The Identity component uses the [Log4j2](https://logging.apache.org/log4j/2.x/) framework to control
the log level and log format.

By default, Identity logs at the `info` and produces a log line with following format:

```
2022-06-27 15:05:21.442 INFO 34490 --- [main] i.c.i.Application: Started Application in 3.659 seconds (JVM running for 4.185)
```

Identity also provides support for configuring the log level and log pattern should it be required by
exposing two environmental variables, these are:

| Environment variable   | Accepted values                                                                                                                           |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `IDENTITY_LOG_LEVEL`   | OFF, FATAL, ERROR, WARN, INFO, DEBUG, TRACE, ALL                                                                                          |
| `IDENTITY_LOG_PATTERN` | _See the [Log4j2 pattern layout docs](https://logging.apache.org/log4j/2.x/manual/layouts.html#PatternLayout) for possible placeholders._ |
