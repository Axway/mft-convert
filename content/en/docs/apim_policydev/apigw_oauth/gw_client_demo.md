---
title: Set up API Gateway OAuth client
linkTitle: Set up API Gateway OAuth client
weight: 90
date: 2019-11-18
description: Learn about the API Gateway client demo, and how to deploy the OAuth client demo and import sample applications.
---

API Gateway includes a number of sample client applications and a web-based client demo. You can deploy the client demo using Policy Studio, and you can import the sample client applications manually. You can also migrate your existing client applications.

## Deploy the OAuth client demo

API Gateway ships with a preconfigured client demo that demonstrates the use of API Gateway and Google as OpenID providers, and API Gateway as a client. To deploy the client demo, perform the following steps:

1. Open Policy Studio and open or create a new project.
2. Select **File > Configure OAuth**.
3. If you do not have any Cassandra hosts configured, you must add a Cassandra host before you can continue:
    * Enter a name for the Cassandra server (for example, `local_cassandra`)
    * Enter the host name of the Cassandra host (for example, `localhost`)
    * Enter the port of the Cassandra host (for example, `9042`)
4. Click **Next**.
5. Select `clientdemo` as the OAuth deployment type. This deploys the client demo on port 8088.

    This option does not register the sample client applications in the Client Application Registry. You must import them manually as detailed in [Import sample client applications](#import-sample-client-applications).

6. Click **Finish**.
7. Click **Deploy** in the toolbar to deploy the updated configuration to API Gateway.

{{< alert title="Note" color="primary" >}}In earlier versions of API Gateway, OAuth was configured using the `deployOAuthConfig.py` script provided in `INSTALL_DIR/apigateway/samples/scripts/oauth` directory. This script is still supported for backwards compatibility, but we recommend that you use Policy Studio to configure OAuth in this version. For more information on running the `deployOAuthConfig.py` script, run the script with the `--help` option. Apache Cassandra must be installed and running, and the Cassandra hosts configured in Policy Studio before you run the `deployOAuthConfig.py` script.{{< /alert >}}

## Import sample client applications

The API Gateway ships with a number of preregistered sample client applications. For example, the default sample client applications include the following:

| Client ID               | Client secret                          |
|-------------------------|----------------------------------------|
| `SampleConfidentialApp` | `6808d4b6-ef09-4b0d-8f28-3b05da9c48ec` |
| `SamplePublicApp`       | `3b001542-e348-443b-9ca2-2f38bd3f3e84` |

{{< alert title="Note" color="primary" >}}The sample client applications are for demonstration purposes only and should be removed before moving the authorization server into production. {{< /alert >}}

To import the sample client applications into the Client Application Registry, perform the following steps:

1. Access the Client Application Registry web interface at the following URL:

    ```
    https://localhost:8089
    ```

2. Enter the Client Application Registry user name and password.
3. Click the **Import** button at the top right of the window.
4. Select the following sample file in the dialog:

    ```
    INSTALL_DIR/apigateway/samples/scripts/oauth/sampleapps.dat
    ```

5. You can also enter a **Decryption Secret** in the dialog. However, the `sampleapps.dat` file is in plain text format, and does not require a password.
6. Click **OK** to import the sample applications. The following figure shows these applications imported into the Client Application Registry:
    ![Client Application Registry HTML interface](/Images/OAuth/oauth_app_reg_ui.png)

Alternatively, you can use the following script to import the sample client application data without using the Client Application Registry web interface:

```
INSTALL_DIR/apigateway/samples/scripts/oauth/importSampleData.py
```

Edit this script to configure your user credentials and file location.

## API Gateway OAuth client demo

The API Gateway OAuth client demo shows a typical use case for OAuth 2.0 and OpenID Connect.

There are a number of actors involved in this demo:

* API Gateway acts as both a OpenID Connect relying party (RP) and an OpenID Connect identity provider (IdP). It also acts as a basic OAuth client application, an OAuth authorization server (AS) and an OAuth resource server (RS).
* Google is configured as an OAuth AS and RS and as an OpenID Connect IdP
* Salesforce is configured as an OAuth AS and RS but not as an OpenID Connect IdP.

To complete the configuration for Google and Salesforce you must register a client application with each provider, and update the relevant provider profiles with the received client ID and secrets. For more information, see [Configure OAuth client application credentials](/docs/apim_policydev/apigw_oauth/gw_oauth_client/#configure-oauth-client-application-credentials).

When you connect to the client demo (for example, at `https://localhost:8088`), a login page is displayed:

![Client demo login](/Images/OAuth/demo_login.png)

This page presents three login options:

1. Enter a user name and password and click **Sign In**. When you sign in with this option, API Gateway acts as a direct authentication server. This is regular form-based authentication backed by the relevant filter. The user credentials are checked against the local user store and a session is created for a valid user. After being authenticated the user can instantiate an OAuth 2.0 authorization flow to get an access token for one of the configured OAuth authentication servers.
2. **Sign in with API Gateway**. When you sign in with this option API Gateway acts as both the RP and the corresponding IdP. The IdP role is conceptually a separate API Gateway instance but for the purposes of the demo a single instance fulfills both roles. This option causes the API Gateway as RP to issue an authorization redirect through the user agent to the IdP. The request includes scopes for an ID token and a regular access token. After being authenticated the access token can be used to access a protected resource as well as the UserInfo endpoint.
3. **Sign in with Google**. When you sign in with this option the API Gateway acts as the RP with Google acting as the IdP. You must update the configuration in Policy Studio to add a valid set of Google OAuth credentials. This option redirects the user to Google for authentication and authorization. The authorization request asks for OpenID and access to the users calendar.

    {{< alert title="Note" color="primary" >}}The credentials you create with Google must have access to the Calendar and Google+ APIs.{{< /alert >}}

After successful authentication, you are presented with the following page:

![Client demo](/Images/OAuth/client_sample.png)

When using one of OpenID Connect options the user information presented on this page is acquired by accessing the UserInfo endpoint of the relevant IdP. The **Get Resource** button uses the received access token to access a protected resource (for example, the Google resource accessed is the user's Google calendar).

### Client policies

The majority of the work in this demo is carried out in the client policies. To support the different methods of authentication (form-based and OpenID Connect), the demo is configured to issue an anonymous session to start the process. This anonymous session is replaced with an updated user session after the user has been identified with either a successful form login or an ID token. For an example of the client policies used, see [Callback sample](/docs/apim_policydev/apigw_oauth/gw_oauth_client/#callback-sample).
