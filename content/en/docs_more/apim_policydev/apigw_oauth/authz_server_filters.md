---
title: OAuth authorization server filters
linkTitle: OAuth authorization server filters
weight: 60
date: 2019-11-18
description: Filters you can use when API Gateway is acting as an OAuth authorization server.
---

## Authorization code flow filter

The OAuth 2.0 **Authorization Code Flow** filter is used to consume OAuth authorization requests. This filter supports the OAuth 2.0 authorization code (web server) flow, which is used by applications hosted on a secure server. A critical aspect of this flow is that the server must be able to protect the issued client application's secret. The web server flow is suitable for clients capable of interacting with the end user's user-agent (typically a web browser), and capable of receiving incoming requests from the authorization server (acting as an HTTP server). The authorization code flow is also known as the three-legged OAuth flow.

The OAuth 2.0 authorization code flow is as follows:

1. The web server redirects the user to the API Gateway acting as an authorization server to authenticate and authorize the server to access data on their behalf.
2. After the user approves access, the web server receives a callback with an authorization code.
3. After obtaining the authorization code, the web server passes back the authorization code to obtain an access token response.
4. After validating the authorization code, the API Gateway passes back a token response to the web server.
5. After the token is granted, the web server accesses their data.

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

The OAuth 2.0 **Authorization Code Flow** filter also supports the implicit grant (user agent) flow. This is used by client applications (consumers) residing on the user's device (for example, in a browser using JavaScript, or from a mobile device, or desktop application). These consumers cannot keep the client secret confidential (application password or private key).

For more details on these OAuth flows, see [Authorization code grant flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_auth_code) and [Implicit grant (or user agent) flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_user_agent).

### Authorization code flow validation settings

The settings on the **Validation/Templates** tab enable you to specify login and authorization forms to authenticate the resource owner.

Configure the following fields:

**Login Form**:
Enter the full path to the HTML form that the resource owner can use to log in. Defaults to the value `${environment.VDISTDIR}/samples/oauth/templates/login.html`.

**Authorization Form**:
Enter the full path to the HTML form that the resource owner can use to grant (allow or deny) client application access to the resources. Defaults to the value `${environment.VDISTDIR}/samples/oauth/templates/requestAccess.html`.

**Selector**:
Enter a selector for the message attribute that contains the `authentication.subject.id` of the current user if they have already been authenticated. Defaults to the `${authentication.subject.id}` message attribute.

{{< alert title="Note" color="primary" >}}Previous versions of API Gateway enabled you to call a policy to authorize the resource owner, and store the subject in a message attribute. This field is used to provide backward compatibility with configurations using that option. If an authenticated user is not found in the message, the filter automatically uses the internal flow and returns the specified login form.{{< /alert >}}

**Skip Authorization**:
Select this option to skip the authorization check and automatically accept the valid scopes in the request, or the scopes from the policy set in **Get scopes by calling the policy** on the **Access Token Details** tab.

### Authorization code settings

Configure the following fields on the **Authz Code Details** tab:

**Authorization Code will be stored here**:
Click the browse button to select where to cache the authorization code (for example, in the default **Authz Code Store**). To add an authorization code store, right-click **Authorization Code Stores**, and select **Add Authorization Code Store**. You can store codes in a cache, in a relational database, or in an Apache Cassandra database. For more details, see [Manage access tokens and authorization codes](/docs/apim_policydev/apigw_oauth/gw_oauth_authz_server/#manage-access-tokens-and-authorization-codes).

**Location of Access Code redirect page**:
Enter the full path to the HTML page used for the access code HTTP redirect. Defaults to the following:

```
${environment.VDISTDIR}/samples/oauth/templates/showAccessCode.html
```

`VDISTDIR` specifies the directory in which the API Gateway is installed.

**Length**:
Enter the number of characters in the authorization code. Defaults to `30`.

**Expiry (in secs)**:
Enter the number of seconds before the authorization code expires. Defaults to `600`
(10 minutes).

**Additional parameters to store for this Authorization Code**:
To store additional metadata with the authorization code, click **Add**, and enter the **Name** and **Value** in the dialog (for example, `Department`
and `Engineering`). When additional data is set, it is then available in the **Access Token using Authorization Code** filter when the authorization code is exchanged for an access token. You can also specify the fields in this table using selectors.

{{< alert title="Note" color="primary" >}}If you entered parameters for the authorization code and parameters for the access token, the data will be merged. For example, if you set `Name:John`and `Department:Engineering`as additional parameters for the authorization code, and set `Department:HR`as an additional parameter for the access token, the token is created with `Name:John`and `Department:HR`.{{< /alert >}}

### Authorization code flow access token settings

Configure the following fields on the **Access Token Details** tab:

**Access Token will be stored here**:
Click the browse button to select where to cache the access token (for example, in the default `OAuth Access Token Store`). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`
(one hour).

**Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Additional parameters to store for this Access Token**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`, `Engineering`).

**Generate Token Scopes**:
When requesting a token from the authorization server, you can specify a parameter for the OAuth scopes that you wish to access. When scopes are sent in the request, you can select whether the access token is generated only if the scopes in the request match all or any scopes registered for the application. Alternatively, for extra flexibility, you can get the scopes by calling out to a policy.

Select one of the following options to configure how access tokens are generated based on specified scopes:

* **Get scopes from a registered application**:
Select whether the scopes must match **Any** or **All** of the scopes registered for the application in the Client Application Registry. Defaults to **Any**. If no scopes are sent in the request, the token is generated with the scopes registered for the application.
* **Get scopes by calling policy**:
Select a preconfigured policy to get the scopes, and enter the attribute that stores the scopes in the **Scopes approved for token are stored in the attribute** field. Defaults to `scopes.for.token`. The configured filter requires the scopes as a set of strings on the message whiteboard.

### Authorization code flow advanced settings

The settings on the **Advanced** tab include monitoring settings and cookie settings.

**Enable monitoring**:
Select this option to enable real-time monitoring. If this is enabled you can view service usage in the web-based API Gateway Manager tool.

**Which attribute is used to identify the client**:
Enter the message attribute to use to identify authenticated clients. The default is `authentication.subject.id`, which stores the identifier of the authenticated user (for example, the user name or user's X.509 Distinguished Name).

**Composite Context**:
This setting enables you to select a service context as a composite context in which multiple service contexts are monitored during the processing of a message. This setting is not selected by default.

For example, the API Gateway receives a message and sends it to `serviceA` first, and then to `serviceB`. Monitoring is performed separately for each service by default. However, you can set a composite service context before `serviceA` and `serviceB` that includes both services. This composite service passes if both services complete successfully, and monitoring is also performed on the composite service context.

**Record Outbound Transactions**:
Select whether to record outbound message traffic. You can use this setting to override the **Record Outbound Transactions** setting in **Server Settings > Monitoring > Traffic Monitor**. This setting is selected by default.

**Resource Owner Cookie**:
Enter the name of the resource owner's cookie in the **Cookie Name** field. This is the cookie created to manage the session between the authorization server and the resource owner.

**Authorization Session Cookie**:
This cookie is needed to manage the session between the client application and the OAuth authorization server. Enter the following details for the session cookie:

* Cookie Name – Name of the session cookie
* Domain – Domain value for the `Set-Cookie` header
* Path – Path value for the `Set-Cookie` header
* Expires in – The length of time until the cookie expires
* Secure – Select the check box to add a `secure` flag to the `Set-Cookie` header
* HttpOnly – Select the check box to add a HttpOnly flag to the `Set-Cookie` header

## Access token using authorization code filter

The OAuth 2.0 **Access Token using Authorization Code** filter is used to get a new access token using the authorization code. This supports the OAuth 2.0 authorization code grant or web server authentication flow, which is used by applications that are hosted on a secure server. A critical aspect of this flow is that the server must be able to protect the issued client application's secret. For more details on this flow, see [Authorization code grant flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_auth_code).

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

### Authorization code application validation settings

Configure the following fields on this tab:

* **Use this store to validate the Authorization Code**:
Click the browse button to select the store in which to validate the authorization code (for example, in the default **Authz Code Store**). To add a store, right-click  **Authorization Code Stores**, and select **Add Authorization Code Store**. You can store codes in a cache, in a relational database, or in an Apache Cassandra database.

**Find client application information from message**:
Select one of the following:

* **In Authorization Header**:
This is the default setting.
* **In Form Body**:
The **Client Id** defaults to `client_id`, and **Client Secret** defaults to `client_secret`.

### Authorization code access token settings

Configure the following fields on the this tab:

**Access Token will be stored here**:
Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`(one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**:
Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.
Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200` (12 hours), and the length defaults to `46`.

* **Do not generate a refresh token**:
Select this option to generate a new access token only. The old refresh token passed in the request is removed.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department` and `Engineering`).

### Authorization code monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager.

**Enable monitoring**:
Select this option to enable real-time monitoring. If this is enabled you can view service usage in the web-based API Gateway Manager tool.

**Which attribute is used to identify the client**:
Enter the message attribute to use to identify authenticated clients. The default is `authentication.subject.id`, which stores the identifier of the authenticated user (for example, the user name or user's X.509 Distinguished Name).

**Composite Context**:
This setting enables you to select a service context as a composite context in which multiple service contexts are monitored during the processing of a message. This setting is not selected by default.

For example, the API Gateway receives a message and sends it to `serviceA` first, and then to `serviceB`. Monitoring is performed separately for each service by default. However, you can set a composite service context before `serviceA` and `serviceB`that includes both services. This composite service passes if both services complete successfully, and monitoring is also performed on the composite service context.

## Resource owner credentials filter

The OAuth 2.0 **Resource Owner Credentials** filter is used to directly obtain an access token and an optional refresh token. This supports the OAuth 2.0 resource owner password credentials flow, which can be used as a replacement for an existing login when the consumer client already has the user's credentials. For more details on this OAuth flow, see [Resource owner password credentials flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_resource_owner).

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

### Resource owner credentials application validation settings

Configure the following fields on this tab:

**Authenticate Resource Owner**:
Select one of the following:

* **Authenticate credentials using this repository**: Select one of the following from the list:
    * Simple Active Directory Repository
    * Local User Store
* **Call this policy**:
    Click the browse button to select a policy to authenticate the resource owner. You can use the **Policy will store subject in selector** text box to specify where the subject is stored. Defaults to the `${authentication.subject.id}`message attribute.

**Find client application information from message**:
Select one of the following:

* **In Authorization Header**: This is the default setting.
* **In Form Body**: The **Client Id** defaults to `client_id`, and **Client Secret** defaults to `client_secret`.

### Resource owner credentials access token settings

Configure the following fields on the this tab:

**Access Token will be stored here**:

Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`(one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**:
Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.
Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200` (12 hours), and the length defaults to `46`.
* **Do not generate a refresh token**:
Select this option to generate a new access token only. The old refresh token passed in the request is removed.
* **Preserve the existing refresh token**:
Select this option to generate a new access token and preserve the existing refresh token. The refresh token passed in the request is sent back with the access token response.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`and `Engineering`).

**Generate Token Scopes**:
When requesting a token from the authorization server, you can specify a parameter for the OAuth scopes that you wish to access. When scopes are sent in the request, you can select whether the access token is generated only if the scopes in the request match all or any scopes registered for the application. Alternatively, for extra flexibility, you can get the scopes by calling out to a policy.

Select one of the following options to configure how access tokens are generated based on specified scopes:

* **Get scopes from a registered application**:
Select whether the scopes must match **Any** or **All** of the scopes registered for the application in the Client Application Registry. Defaults to **Any**. If no scopes are sent in the request, the token is generated with the scopes registered for the application.
* **Get scopes by calling policy**:
Select a preconfigured policy to get the scopes, and enter the attribute that stores the scopes in the **Scopes approved for token are stored in the attribute** field. Defaults to `scopes.for.token`. The configured filter requires the scopes as a set of strings on the message whiteboard.

### Resource owner credentials monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

The traffic monitoring options enable you to view message traffic in API Gateway Manager.

**Record Outbound Transactions**:
Select whether to record outbound message traffic. You can use this setting to override the **Record Outbound Transactions** setting in **Server Settings > Monitoring > Traffic Monitor**. This setting is selected by default.

## Access token using client credentials filter

The OAuth 2.0 **Access Token using Client Credentials** filter enables an OAuth client to request an access token using only its client credentials. This supports the OAuth 2.0 client credentials flow, which is used when the client application needs to directly access its own resources on the resource server. Only the client application's credentials or public/private key pair are used in the flow. The resource owner's credentials are not required. For more details on this OAuth flow, see [Client credentials grant flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_client_credentials).

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

### Client credentials application validation settings

Configure the following fields on this tab:

**Find client application information from message**:
Select one of the following:

* **In Authorization Header**: This is the default setting.
* **In Form Body**: The **Client Id** defaults to `client_id`, and **Client Secret** defaults to `client_secret`.

### Client credentials access token settings

Configure the following fields on this tab:

**Access Token will be stored here**:
Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600` (one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**:
Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.
Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200` (12 hours), and the length defaults to `46`.
* **Do not generate a refresh token**:
Select this option to generate a new access token only. The old refresh token passed in the request is removed.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`and `Engineering`).

**Generate Token Scopes**:
When requesting a token from the authorization server, you can specify a parameter for the OAuth scopes that you wish to access. When scopes are sent in the request, you can select whether the access token is generated only if the scopes in the request match all or any scopes registered for the application. Alternatively, for extra flexibility, you can get the scopes by calling out to a policy.

Select one of the following options to configure how access tokens are generated based on specified scopes:

* **Get scopes from a registered application**:
Select whether the scopes must match **Any** or **All** of the scopes registered for the application in the Client Application Registry. Defaults to **Any**. If no scopes are sent in the request, the token is generated with the scopes registered for the application.
* **Get scopes by calling policy**:
Select a preconfigured policy to get the scopes, and enter the attribute that stores the scopes in the **Scopes approved for token are stored in the attribute** field. Defaults to `scopes.for.token`. The configured filter requires the scopes as a set of strings on the message whiteboard.

### Client credentials monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

## Access token using JWT filter

The OAuth 2.0 **Access Token using JWT** filter enables an OAuth client to request an access token using only a JSON Web Token (JWT). This supports the OAuth 2.0 JWT flow, which is used when the client application needs to directly access its own resources on the resource server. Only the client JWT token is used in this flow; the resource owner's credentials are not required. For more details on this OAuth flow, see [JWT flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_jwt).

A JWT is a JSON-based security token encoding that enables identity and security information to be shared across security domains. JWTs represent a set of claims as a JSON object. For more details, see: <https://tools.ietf.org/html/rfc7519.>

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

### JWT application validation settings

Configure the following fields on this tab:

**Audience (aud) must contain the following URI**:
Enter the JWT `aud`(intended audience). The JWT must contain an `aud`URI that identifies the authorization server, or service provider domain, as an intended audience. The authorization server must also verify that it is an intended audience for the JWT. Defaults to `http://apigateway/api/oauth/token`.

**Clock skew in seconds for JWT Claim**:
When creating the JWT, an OAuth client can set certain claims relating to time (for example, `iat`, `exp`, or `nbf`). This field allows you to enter a number of seconds to allow for clock skew when dealing with these claims.

If the `iat`claim is present, the OAuth token service asserts that the current time is greater than the issued at time. If the `exp`claim is present, the OAuth token service asserts that the current time is less than or equal to the expiry time (plus skew seconds if configured). If the `nbf`claim is present, the OAuth token service asserts that the current time is greater than or equal to expiry time (minus skew seconds if configured).

### JWT access token settings

Configure the following fields on this tab:

**Access Token will be stored here**:
Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`(one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**: Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.
Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200`(12 hours), and the length defaults to `46`.
* **Do not generate a refresh token**: Select this option to generate a new access token only. The old refresh token passed in the request is removed.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`and `Engineering`).

**Generate Token Scopes**:
When requesting a token from the authorization server, you can specify a parameter for the OAuth scopes that you wish to access. When scopes are sent in the request, you can select whether the access token is generated only if the scopes in the request match all or any scopes registered for the application. Alternatively, for extra flexibility, you can get the scopes by calling out to a policy.

Select one of the following options to configure how access tokens are generated based on specified scopes:

* **Get scopes from a registered application**: Select whether the scopes must match **Any** or **All** of the scopes registered for the application in the Client Application Registry. Defaults to **Any**. If no scopes are sent in the request, the token is generated with the scopes registered for the application.
* **Get scopes by calling policy**: Select a preconfigured policy to get the scopes, and enter the attribute that stores the scopes in the **Scopes approved for token are stored in the attribute** field. Defaults to `scopes.for.token`. The configured filter requires the scopes as a set of strings on the message whiteboard.

### JWT monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

## Access token using SAML assertion filter

The OAuth 2.0 **Access Token using SAML Assertion** filter enables an OAuth client to request an access token using a SAML assertion. This supports the OAuth 2.0 SAML flow, which is used when a client wishes to utilize an existing trust relationship, expressed through the semantics of the SAML assertion, without a direct user approval step at the authorization server. For more details on supported OAuth flows, see [OAuth 2.0 authentication flows](/docs/apim_policydev/apigw_oauth/oauth_flows/).

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes.

### SAML assertion validation settings

Configure the following fields on this tab:

**Audience and Recipient within SAML Assertion must contain the following URI**:
Enter a URI that must be contained in the SAML assertion's intended audience and recipient. The SAML assertion must contain a URI that identifies the authorization server as an intended audience, and that identifies the token endpoint URL of the authorization server as a recipient. Defaults to `http://apigateway/api/oauth/token`.

**Drift time (seconds)**:
Enter a drift time in seconds to allow for clock skew.

**Call the following policy to verify SAML Assertion signature**:
Click the browse button to select a policy to verify the SAML assertion signature.

To guarantee the integrity of an XML signature in a message, the **Access Token using SAML Assertion** filter should use the **XML Signature Verification** filter. When API Gateway receives the assertion, it converts the assertion into a W3C DOM document and stores this value in a message attribute named `oauth.saml.doc`. This message attribute is used by the **XML Signature Verification** filter. A sample SAML bearer policy flow is available after you set up API Gateway as an OAuth server.

### SAML assertion access token settings

Configure the following fields on this tab:

**Access Token will be stored here**:
Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`(one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**: Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.
Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200` (12 hours), and the length defaults to `46`.
* **Do not generate a refresh token**: Select this option to generate a new access token only. The old refresh token passed in the request is removed.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`and `Engineering`).

**Generate Token Scopes**:
When requesting a token from the authorization server, you can specify a parameter for the OAuth scopes that you wish to access. When scopes are sent in the request, you can select whether the access token is generated only if the scopes in the request match all or any scopes registered for the application. Alternatively, for extra flexibility, you can get the scopes by calling out to a policy.

Select one of the following options to configure how access tokens are generated based on specified scopes:\

* **Get scopes from a registered application**: Select whether the scopes must match **Any** or **All** of the scopes registered for the application in the Client Application Registry. Defaults to **Any**. If no scopes are sent in the request, the token is generated with the scopes registered for the application.
* **Get scopes by calling policy**: Select a preconfigured policy to get the scopes, and enter the attribute that stores the scopes in the **Scopes approved for token are stored in the attribute** field. Defaults to `scopes.for.token`. The configured filter requires the scopes as a set of strings on the message whiteboard.

### SAML assertion monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

## Access token information filter

The OAuth 2.0 **Access Token Information** filter is used to return a JSON description of the specified OAuth 2.0 access token. OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions.

An OAuth access token can be sent to the resource server to access the protected resources of the resource owner (user). This token is a string that denotes a specific scope, lifetime, and other access attributes. For details on this OAuth flow, see [Token information service flow](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_token_info).

### Token settings

Configure the following fields on the **Access Token Info Settings** tab:

**Token to verify can be found here**:
Click the browse button to select the location of the access token to verify (for example, in the default **OAuth Access Token Store**). To add a store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Where to get access token from**:
Select one of the following:

* **In Query String/Form Body**: This is the default setting. Defaults to the `access_token`
    parameter.
* **In a selector**: Defaults to the `${http.client.getCgiArgument('access_token')}`
    selector.

### Access token information monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

### Access token information advanced settings

The settings on the **Advanced** tab include the following:

**Return additional Access Token parameters**:
Click **Add** to return additional access token parameters, and enter the **Name** and **Value** in the dialog. For example, you could enter `Department` in **Name**, and the following selector in **Value**:

```
${accesstoken.getAdditionalInformation().get("Department")
```

## Revoke token filter

The OAuth 2.0 **Revoke a Token** filter is used to revoke a specified OAuth 2.0 access or refresh token. A revoke token request causes the removal of the client permissions associated with the specified token used to access the user's protected resources. For more details on this OAuth flow, see [Revoke token](/docs/apim_policydev/apigw_oauth/oauth_flows/oauth_flows_revoke_token).

OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions. OAuth refresh tokens are tokens issued by the authorization server to the client that can be used to obtain a new access token.

### Revoke token settings

Configure the following fields on this tab:

**Token to be revoked can be found here**:
Click the browse button to select the cache to revoke the token from (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Find client application information from message**:
Select one of the following:

* **In Authorization Header**: This is the default setting.
* **In Form Body**: The **Client Id** defaults to `client_id`, and **Client Secret** defaults to `client_secret`.

### Revoke token monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.

## Refresh access token filter

The OAuth 2.0 **Refresh Access Token** filter enables an OAuth client to get a new access token using a refresh token. This filter supports the OAuth 2.0 refresh token flow. After the client consumer has been authorized for access, they can use a refresh token to get a new access token (session ID). This is only done after the consumer already has received an access token using either the web server or user-agent flow. For more details on supported OAuth flows, see [OAuth 2.0 authentication flows](/docs/apim_policydev/apigw_oauth/oauth_flows/).

### Refresh access token application validation settings

Configure the following fields on this tab:

**Find client application information from message**:
Select one of the following:

* **In Authorization Header**: This is the default setting.
* **In Form Body**: The **Client Id** defaults to `client_id`, and **Client Secret** defaults to `client_secret`.

### Refresh access token settings

Configure the following fields on this tab:

**Access Token will be stored here**:
Click the browse button to select where to store the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database.

**Access Token Expiry (in secs)**:
Enter the number of seconds before the access token expires. Defaults to `3600`(one hour).

**Access Token Length**:
Enter the number of characters in the access token. Defaults to `54`.

**Access Token Type**:
Enter the access token type. This provides the client with information required to use the access token to make a protected resource request. The client cannot use an access token if it does not understand the token type. Defaults to `Bearer`.

**Refresh Token Details**:
Select one of the following options:

* **Generate a new refresh token**: Select this option to generate a new access token and refresh token pair. The old refresh token passed in the request is removed. This option is selected by default.

    Enter the number of seconds before the refresh token expires in the **Refresh Token Expiry (in secs)** field, and enter the number of characters in the refresh token in the **Refresh Token Length** field. The expiry defaults to `43200` (12 hours), and the length defaults to `46`.
* **Do not generate a refresh token**: Select this option to generate a new access token only. The old refresh token passed in the request is removed.

**Store additional meta data with the access token which can subsequently be retrieved**:
Click **Add** to store additional access token parameters, and enter the **Name** and **Value** in the dialog (for example, `Department`and `Engineering`).

### Refresh access token monitoring settings

The real-time monitoring options enable you to view service usage in API Gateway Manager. See [monitoring settings](#authorization-code-monitoring-settings) for details of the common fields.
