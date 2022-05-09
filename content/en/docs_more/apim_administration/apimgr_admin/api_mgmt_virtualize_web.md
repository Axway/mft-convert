{
"title": "Virtualize REST APIs in API Manager",
  "linkTitle": "Virtualize REST APIs",
  "weight": "60",
  "date": "2019-09-17",
  "description": "Virtualize a registered back-end API as a publicly exposed front-end API."
}
When you have registered a back-end REST API, you can then virtualize it as a publicly exposed front-end API. The **API Catalog**
stores information about the REST APIs that have been virtualized as front-end APIs. Virtualized REST APIs published in the **API Catalog** can be made available in API Manager for consumption by API consumers, and for administration by API administrators.

{{< alert title="Note" color="primary" >}}
You must first register a back-end REST API before you can virtualize a front-end REST API. For more details, see [Register REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/).
{{< /alert >}}

## Virtualized REST API security

When you virtualize a REST API, you can configure it with security devices, which provide prebuilt authentication and authorization mechanisms for the REST API. The following security devices are supported:

* [API Key](#api-key)
* [AWS Signing: Authorization Header](#aws-signing-authorization-header)
* [AWS Signing: Query String](#aws-signing-query-string)
* [HTTP Basic Authentication](#http-basic)
* [Invoke Policy (custom authentication)](#invoke-policy)
* [OAuth](#oauth)
* [OAuth (External)](#oauth-external)
* [Pass Through](#pass-through)
* [Two-way SSL](#two-way-ssl)

This enables you to control the authentication and authorization mechanisms that are supported for the API. For example, an API with higher security requirements can be more restrictive in the authentication mechanism that it supports. You can also configure custom profiles to suit your requirements. You can also configure virtualized REST APIs and API methods with custom policies if required (for example, for request, routing, and response processing).

## Virtualize a REST API as a front-end API

When you have first registered a back-end REST API in API Manager, you can then virtualize it as a publicly exposed front-end API. To virtualize a back-end REST API as a front-end API, perform the following steps:

1. Click the **API Registration > Frontend API** view in API Manager.
2. Click **New API > New API from backend API**.
3. Select an existing back-end API in the dialog (for example, **Petstore**), and click **Create**. This displays the following page:

![Imported API details in the web console](/Images/docbook/images/api_mgmt/api_mgmt_frontend_api_edit.png)

{{< alert title="Note" color="primary" >}}If the virtualized API is not a Swagger 2.0-compatible REST API, API Manager displays a message that Swagger download will not be available in the **API Catalog**.{{< /alert >}}

## Import a previously exported API

Alternatively, you can virtualize an existing API by importing a previously exported front-end API (for example, from another API Manager environment). For details on how to export APIs, see [Manage the front-end REST API lifecycle.](#manage-front-end-rest-api-lifecycle)

To import a previously exported API, perform the following steps:

1. Click the **API Registration > Frontend API** view in API Manager.
2. Click **New API > Import API collection**.
3. In the **Import from** dialog, complete the following:

   * **File**: Click to browse to the previously exported API (`.dat` file).
   * **Password**: Enter the password if required.
   * **Organization**: Select the organization from the list (for example, **API Development**).
4. Click **Import**.
5. Press **F5** to reload the API Manager web console.

## Configure inbound request settings

When you have virtualized a REST API to create a front-end API, you can edit and configure the inbound request settings between the client and the API Gateway (for example, for customized authentication, authorization, or monitoring). To configure inbound settings, perform the following steps in API Manager:

1. Select the **Inbound** tab.
2. You can edit the **Resource path** for the API in the text box on the right. Defaults to `/api`.
3. If query string-based version routing has been enabled in **Settings** > **API Manager settings**, you can remove the default version from the **Resource path** and enter an API version in the **Query string version routing** field instead (for example, `v1`). For details on how to enable query string version routing, see [Configure API routing based on version query string](/docs/apim_administration/apimgr_admin/api_mgmt_version_routing/).
4. Select a required security device from the **Inbound Security** list. This enables you to configure prebuilt inbound authentication and authorization mechanisms for the virtualized API. The most commonly used devices are **API Key** and **Pass Through**. The available options are detailed below.
5. Click the **Advanced** button on the right to configure settings such as monitoring, sharing resources across domains, and per-API method overrides.

The following shows an example:

![Frontend API: advanced inbound settings ](/Images/docbook/images/api_mgmt/api_mgmt_frontend_api_inbound_advanced.png)

### Inbound security settings

#### API Key

Configure the following to enable API key authentication:

**API key field name**: Enter a required name used to store the API key field in the inbound request. Defaults to `KeyId`.

**API key location**: Select the required location of the API key in the inbound request (`Request Headers` or `Query string/form body`). Defaults to `Request Headers`.

**Scopes must match**: Select whether the scopes configured in the next setting must match `Any` or `All` of the scopes registered for client applications.

**Scopes**: Enter the scopes configured for this API, which are used for client application authorization. These scopes define a set of authorizations (for example, all read methods) that can be assigned to specific client applications. You can enter a single scope (for example, `app.all`, `app.read`, or `app.write`) or a comma-separated list of scopes.

**Remove credentials on success**: Select whether to remove the user credentials from the message after successful authentication. This is selected by default. If you disable this option, an inbound request authorization header is forwarded to the back-end. To use another authentication and authorization mechanism to secure the connection to the back-end, you must remove the inbound authorization header using a custom routing policy.

#### AWS Signing (Authorization Header)

Configure the following to enable access to the API using an Amazon Web Services authorization header:

**Name**: Enter a required name for the device. Defaults to `AWS Signing Device (Authorization Header)`.

**Remove credentials on success**: Select whether to remove the user credentials from the message after successful authentication. This is selected by default. If you disable this option, an inbound request authorization header is forwarded to the back-end. To use another authentication and authorization mechanism to secure the connection to the back-end, you must remove the inbound authorization header using a custom routing policy.

#### AWS Signing (Query String)

Configure the following to enable access to the API using an Amazon Web Services query string:

**Name**: Enter a required name for the device. Defaults to `AWS Signing Device (Query String)`.

**API key field name**: Enter a required name of the query-string parameter used to store the API key field in the inbound request. Defaults to `AWSAccessKeyId`.

#### HTTP Basic

Configure the following to enable HTTP Basic authentication:

**Name**: Enter a required name for the device. Defaults to `HTTP Basic Device`.

**Realm**: Enter the realm required for HTTP basic authentication purposes (for example, `Flickr`). This enables clients to identify the zone that they are accessing. For example, the browser can then cache user credentials on a per-realm basis. The realm is required for all authentication schemes that issue an authentication challenge.

**Remove credentials on success**: Select whether to remove the user credentials from the message after successful authentication. This is selected by default. If you disable this option, an inbound request authorization header is forwarded to the back-end. To use another authentication and authorization mechanism to secure the connection to the back-end, you must remove the inbound authorization header using a custom routing policy.

{{< alert title="Note" color="primary" >}}When using HTTP basic authentication, the client application invoking the API must use the API key as username and the API secret as password, formatted as `Base64Encode("APIKey:APISecret")`.{{< /alert >}}

#### Invoke Policy

Configure the following to use a custom authentication policy:

**Name**: Enter the name of the custom security device. Defaults to `Invoke Policy`.

**Policy to invoke**: Select the authentication policy that this security device invokes. This lists policies already configured in Policy Studio in **Environment Configuration > Server Settings > API Manager > Inbound Security Policies**.

**Use client registry**: Select whether the `authentication.subject.id` identifier must match one of the application's external credentials in the Client Registry in API Manager. This setting is selected by default, and is required for API Manager to check if the organization has access to the API. If you do not select this setting, API Manager will not check the organization access.

**Traffic Monitor subject**: If **Use client registry** is not selected, the API has no client information, and API Manager does not check organization access to the API. In this case, you can enter the subject name to display in the Traffic tab in API Gateway Manager. Defaults to `${authentication.subject.id}`.

**Description**: Select where the [Markdown](http://daringfireball.net/projects/markdown/) description for this security device is located. Select one of the following:

* **Use original policy description**: Use the policy description specified when creating the policy in Policy Studio.
* **Use manual description**: Get the description from the contents of a field. Enter the description in the Manual Description field.
* **Use markdown file location**: Get the description from a file located on the server. Enter the path to this file in the Markdown file location field. For security reasons, this file must start with an environmentalized variable and cannot attempt directory traversal. For example, the following path is valid: `${env.DOCUMENTS}/markdown/api.md`. The following paths are invalid: `/opt/documents/api.md`, `${env.DOCUMENTS}/../markdown/api.md`.
* **Use external URL**: Get the description from an external URL. Enter the URL in the **External URL location** field.

{{< alert title="Note" color="primary" >}}Invoke Policy security devices generate an Authentication section in the API Catalog that displays the description entered when creating the security device.{{< /alert >}}

#### OAuth

Configure the following to enable OAuth authorization for access tokens issued by API Manager.

##### General OAuth settings

**Access token store**: Select a required OAuth access token store from the list. For details on how to add OAuth access token stores to this list, see [Configure API Manager settings in Policy Studio](/docs/apim_administration/apimgr_admin/api_mgmt_config_ps/).

**Scopes must match**: Select whether the OAuth scopes match `Any` or `All` of the OAuth scopes configured in the next field. OAuth scopes are used to control how access tokens are accepted.

**Scopes**: Enter a comma-separated list of scopes used to manage how access tokens are accepted. In addition, these tokens are used as default scopes for applications that use this API and do not send the `scope` parameter in the access token request. You can also configure additional default scopes for an application if enabled in **Settings > API Manager Settings > General settings > Enable application scopes**. For details, see [API Manager settings](/docs/apim_reference/api_mgmt_config_web/#api-manager-settings). Defaults to `resource.WRITE, resource.READ`.

**Remove credentials on success**: Select whether to remove the user credentials from the message after successful authentication. This is selected by default. If you disable this option, an inbound request authorization header is forwarded to the back-end. To use another authentication and authorization mechanism to secure the connection to the back-end, you must remove the inbound authorization header using a custom routing policy.

##### OAuth authorization settings

**Access token location**: Select the required location of the OAuth access token in the inbound request (`Request Header` or `Query string/form body`). Defaults to `Request Header`.

**Authorization header prefix**: Select the header prefix used to authorize the request (`Bearer` or `OAuth`). Defaults to `Bearer`.

##### Grant type implicit

**Enabled**: Select whether to enable this simplified authorization code flow optimized for browser-based clients. Enabling advertises this grant type in the API Catalog. It is the role of the OAuth authorization server to support it. Disabling excludes this grant type from the API Catalog.

**Login endpoint URL**: Enter the authorization endpoint where resource owners can interact with the OAuth service to authorize access for the client application. This is the URL where client applications will redirect end users. Defaults to `http://localhost:8089/api/oauth/authorize`.

**Login token name**: Enter the response parameter name that will contain the access token. Defaults to `access_token`.

##### Grant type authorization code

**Enabled**: Select whether the authorization code is obtained using an authorization server as an intermediary between the client and resource owner. Enabling advertises this grant type in the API Catalog. It is the role of the OAuth authorization server to support it. Disabling excludes this grant type from the API Catalog.

**Request endpoint URL**: Enter the authorization endpoint where resource owners can interact with the OAuth service to authorize access for client application. This is the URL where client application will redirect end users. Defaults to `http://localhost:8089/api/oauth/authorize`.

**Request client ID name**: Enter the name of the request parameter that will contain the client application ID. Defaults to `client_id`.

**Request client secret name**: Enter the name of the request parameter that will contain the client application secret. Defaults to `client_secret`.

**Token URL**: Enter the token endpoint URL where the client application will exchange an authorization code for an access token. Defaults to `http://localhost:8089/api/oauth/token`.

**Token name**: Enter the request parameter name that will contain the access code. Defaults to `access_token`.

#### OAuth (External)

Configure the following to enable OAuth authorization for access tokens issued by an external token provider.

##### General OAuth (external) settings

**Token information policy**: Select a required OAuth token information policy from the list. This is a custom policy used to obtain and extract token information from the external OAuth provider. For details on how to add OAuth token information policies to the list, see [Configure API Manager settings in Policy Studio](/docs/apim_administration/apimgr_admin/api_mgmt_config_ps/).

**Scopes must match**: Select whether the OAuth scopes match Any or All of the OAuth scopes configured in the next field. OAuth scopes are used to control how access tokens are accepted.

**Scopes**: Enter a comma-separated list of OAuth scopes used to manage how access tokens are accepted. Defaults to `resource.WRITE, resource.READ`.

**Remove credentials on success**: Select whether to remove the user credentials from the message after successful authentication. This is selected by default. If you disable this option, an inbound request authorization header is forwarded to the back-end. To use another authentication and authorization mechanism to secure the connection to the back-end, you must remove the inbound authorization header using a custom routing policy.

**Use client registry**: Select this option so that API Manager can use the external provided client ID (with either configured API keys, OAuth credentials, or external credentials) to identify the configured client application in API Manager client registry, which allows it to apply quotas and assign a subject identifier for traffic monitoring. If this is not selected, the API effectively is pass-through and the client registry does not apply API access or enforce quotas. This is not selected by default because external OAuth providers typically use their own client registries.

**Traffic monitor subject**: Enter the identifier name used for clients from the external OAuth provider, which is displayed on the Traffic tab in the API Gateway Manager console. This value can be a selector. Defaults to `${oauth.token.client_id}`.

**Extract token attributes**: Click the add button to specify OAuth token attributes to be extracted from the configured Token information policy and copied to the message whiteboard. For example, these can then be passed to request, response, or routing policies downstream. Specifying attributes ensures their values are retained on the whiteboard after invoking the policy. By default, the following attributes are extracted:

* `oauth.token.client_id`
* `oauth.token.scopes`
* `oauth.token.valid`

For authorization, grant type implicit, and grant type authorization code settings, see the [OAuth security device settings](#oauth).

For a complete example, see [Configure OAuth (External) security for a front-end API](/docs/apim_administration/apimgr_admin/example_oauth_external/).

#### Pass Through

Configure the following to enable pass-through authentication where the API Gateway passes the user credentials through to an authenticating server:

**Name**: Enter a required name for the device. Defaults to `Pass Through Device`.

**Subject ID**: Enter a required authentication subject ID. Defaults to `Pass Through`. This will be displayed in the Traffic view in the API Gateway Manager when the API is invoked using this device.

{{< alert title="Note" color="primary" >}}When you enable pass-through authentication, there is no client application context so application quotas cannot be enforced.{{< /alert >}}

#### Two-way SSL

To enable two-way (mutual) SSL authentication, the client must supply a certificate signed by the server CA.

Before you can configure an API to use two-way SSL you must first configure the API Manager traffic port (default 8065) to accept and trust client certificates issued by the server CA. API Manager ignores client certificates by default, but when you configure API Manager to accept them, clients have the option during the SSL handshake to send their client certificate, which is then used by API Manager to validate access.

For details on how to configure mutual authentication and accepting client certificates, see [Mutual Authentication settings](/docs/apim_policydev/apigw_gw_instances/general_services/#mutual-authentication-settings).

{{< alert title="Note" color="primary" >}}You must not _require_ client certificates, as this would mean that all clients for all APIs must provide a client certificate. Clients who do not provide a client certificate are rejected during the SSL handshake and are unable to access your APIs.{{< /alert >}}

By default, the provided client certificate must contain a Subject Common Name (CN) containing the API key generated by API Manager, or a configured external credential for a registered application. The CN value is evaluated at runtime using a selector (for example, `${certificate.subject.CN}`), and used to look up the Client Registry to retrieve the application.

Configure the following settings:

**Name**: Enter a required name for the device. Defaults to `Two-way SSL Device`.

**API key field**: Enter the name of the selector used to look up the KPS to retrieve and validate the API key and application details. Defaults to `${certificate.subject.CN}`.

### Advanced inbound settings

**Monitor API usage**: Select whether to enable monitoring metrics for the REST API in the **Monitoring > API Usage** view.

**Enable CORS from all domains**: Select whether to enable Cross Origin Resource Sharing (CORS) from all domains. When enabled, this means that requests to this API are allowed from all domains (which corresponds to a CORS setting of `*`). To add more advanced CORS configuration (for example, allowed or exposed headers), disable this setting, and add a specific CORS profile for this API. For more details, see [Configure CORS profiles](#configure-cors-profiles).

**PER-METHOD OVERRIDE**: You can click to override the REST API level settings for specified REST API methods. Click the add button, select an API method from the list, and override the following settings as required:

* **INBOUND SECURITY PROFILE**: Select a preconfigured security profile for the API method. For more details, see [Configure security profiles](#configure-security-profiles).
* **CORS PROFILE**: Select a preconfigured CORS profile for the API method.

## Configure outbound request settings

When you have virtualized a back-end REST API to create a front-end API, you can edit and configure the outbound request settings between the API Gateway and the back-end API. For example, this enables you to customize authentication, and request or response processing. The following page shows configuring an API key authentication profile:

![Front-end API outbound settings in the web console](/Images/docbook/images/api_mgmt/api_mgmt_frontend_api_outbound.png)

To configure outbound settings, perform the following steps in API Manager:

1. Select the **Outbound** tab.
2. Select an optional profile from the **Outbound authentication profile** list. This enables you to configure a pre-built authentication mechanism for outbound communication between the API Gateway and the virtualized API.
3. Click the **Advanced** button on the right to configure settings such as request or response processing, routing, and per-API method overrides. The following shows an example:

![Frontend API: advanced outbound settings ](/Images/docbook/images/api_mgmt/api_mgmt_frontend_api_outbound_advanced.png)

### Outbound authentication settings

#### No authentication

No authentication is performed between the API Gateway and the back-end API.

#### HTTP Basic authentication

Configure the following to enable HTTP Basic authentication:

**Name**: Enter a required name for the profile. Defaults to `HTTP Basic`.

**Username**: Enter the required username (API key) used to access the API.

**Password**: Enter the optional password (API secret) used to access the API.

#### HTTP Digest authentication

Configure the following to enable HTTP Digest authentication:

**Name**: Enter a required name for the profile. Defaults to `HTTP Digest`.

**Username**: Enter the required username (API key) used to access the API.

**Password**: Enter the optional password (API secret) used to access the API.

#### OAuth authentication

Configure the following to enable OAuth authentication:

**Provider Profile**: Select the OAuth service provider profile from the list.

**Token Key (Owner ID)**: Enter the message attribute to be used as the key to look up the token. The token key must be set to the authentication value you require for the OAuth token. Defaults to `${authentication.subject.id}`.

#### API Key authentication

Configure the following to enable API key authentication:

**Name**: Enter a required name for the profile. Defaults to `API key`.

**API key field name**: Enter a required name used to store the API key field in the outbound request (for example, `KeyId`).

**API key**: Enter the API key required to access the API (for example, `AIzaSyB6CzrBlkzuzDKJw0QaZhW9WwBV5IxXMS7`).

**Pass credentials as HTTP**: Select the required location of the API key in the outbound request (`Header`, `Query string`, or `Form`).

#### SSL authentication

To enable SSL authentication, the API Gateway must supply a certificate signed by certificate authority used by the API. You can configure the following settings:

**Name**: Enter a required name for the profile. Defaults to `SSL`.

**PFX/P12 Source**: Select whether to specify the certificate using a `.pfx` or `.p12` file, or using a URL.

**PFX/P12 File** or **PFX/P12 URL**: Browse to the PFX/P12 file, or enter the PFX/P12 URL.

**PFX/P12 Password**: Enter the password for the certificate.

**Trust all certificates in chain**: Select whether to trust all the CA certificates in the certificate chain. If this is not selected, only the top-level CA is trusted. This setting is selected by default.

### Advanced outbound settings

#### Global request policy

If a global policy has been configured, this field displays the read-only global request policy applied to all front-end API invocations (for example, a mandatory security, compliance, or governance policy). The global request policy is executed after inbound authentication, but before any configured request, routing, or response policies. No global policies are configured by default.

#### Request policy

Select an optional request processing policy for the API. For example, you could use this pre-configured policy to check the request message for additional authentication, authorization, or validation. No request policies are configured by default.

#### Default method routing

Select an optional routing policy for virtualized API method calls. For example, you could use this pre-configured policy to route to a backend JMS service. API method calls are routed to the API proxy in API Manager by default.

#### Response policy

Select an optional response processing policy for the API. For example, you could use this pre-configured policy to validate or transform outbound response messages. No response policies are configured by default.

#### Global response policy

If a global policy has been configured, this field displays the read-only global response policy applied to all front-end API invocations (for example, a mandatory security, compliance, or governance policy). The global response policy is executed last after any configured response policy. No global policies are configured by default.

#### Fault handler policy

If fault handler policies have been enabled and configured, you can select a fault handler policy for the API. The selected fault handler is executed when an error or exception occurs during the API Manager runtime API invocation. This setting defaults to the API Manager Default Fault Handler policy.

#### PER-METHOD OVERRIDE

You can click to override the REST API level settings for specified REST API methods. Click the add button, select an API method from the list, and override the following settings as required:

**REQUEST POLICY**: Select an optional request processing policy for the API method.

**RESPONSE POLICY**: Select an optional response processing policy for the API method.

**ROUTING**: Select an optional routing policy for the API method.

**FAULT HANDLER POLICY**: If fault handler policies have been enabled and configured, select an optional fault handler policy for the API method.

**EDIT API PROXY**: Click Edit to add parameters to the API method. To add parameters, click the add button, and configure the following settings:

* OUTBOUND PARAMETER: Enter the parameter name (for example, `customer_name`).
* PARAMETER TYPE: Enter the parameter type (for example, `query`, `path`, `form`, or `header`).
* DATA TYPE: Enter the parameter name (for example, `string`, `int`, and so on).
* REQUIRED: Select whether the parameter is required.
* OUTBOUND VALUE: Enter the parameter value (for example, `john doe` or `${params.path.id}`).
* EXCLUDE: Select whether to exclude the parameter.
* DEFAULT MAPPING: Select whether the parameter is mapped by default.

**AUTHENTICATION PROFILE**: Select an optional authentication profile for the API method.

## Configure API information

The **API** tab enables you to view and edit the API information to be displayed in the **API Catalog**. For example, this includes general settings such as the API name, version, graphic, documentation, and tags.

### Configure general API information

You can configure the following settings in the **GENERAL** section:

* **Image**: Click the **Add Image** box, and browse to the location of the graphic file for your API.
* **API Name**: Enter a value to edit your API name.
* **API Version**: Enter a value to edit your API version.

Other details such as the API state, owner, date are read-only.

### Configure API documentation

You can configure the following in **DOCUMENTATION** > **Description**:

* **Use original description**: Uses the description specified when the back-end API was registered.
* **Use manual description**: Click the **Edit** tab, and enter a description.
* **Use markdown file location**: Enter the location in **Markdown file location** (for example, `${environment.VINSTDIR}/../markdown/API/API.md`).
* **Use external URL**: Enter the location in **External URL location** (for example, `https://api.any.org/doc#myapi`).

The description is using the [Markdown](http://daringfireball.net/projects/markdown/) syntax.

### Configure API tags

The **TAGS** section enables you to add tags to categorize and help find your API in the **API Catalog**. Click the add button, and enter a tag name (for example, `Department`) and values (for example, `Engineering,Testing`). You can add multiple tags for your API. You can enter multiple tag values in a comma-separated list without any spaces between each value.

## Configure API method information

You can use the **Method** tab to configure the API method information to be displayed in the **API Catalog**. For example, this includes testing the method, configuring its documentation, and adding tags. For example, click **Try method** to invoke the method for test purposes. Authentication credentials are automatically formatted and passed in the test request. Other settings such as method parameters and content types are displayed as read-only.

You can configure API method documentation and tags in the same way that you configure API documentation and tags.

## Configure security profiles

You can use the **Security Profiles** tab to create custom security profiles with multiple security devices, which can then be applied as per-method override in the **Inbound** settings for the front-end API.

To create a security profile, perform the following steps:

1. Click the add button on the left.
2. In **GENERAL**, enter a **Name** for the security profile.
3. In **DEVICES**, click the add button, select a security device from the list, and configure its settings. For details on configuring each security device type, see [Inbound security settings](#inbound-security-settings).
4. Repeat to add multiple security devices if required.
5. You can click **Up** or **Down** to change the order in which security devices are invoked, or **Edit** to change any settings as required.

{{< alert title="Note" color="primary" >}}Multiple security devices are combined using an `OR` logic. This means that if authentication to the first security device fails, authentication to the second security device is attempted, and so on.{{< /alert >}}

For an end-to-end example of using security profiles, see [Configure API method-level authorization for client applications](/docs/apim_administration/apimgr_admin/api_mgmt_method_authz/).

## Configure authentication profiles

You can use the **Authentication Profiles** tab to create custom authentication profiles, which can then be applied to the front-end API in the **Outbound** settings.

To create an authentication profile, click the add button on the left, select an authentication profile from the list, and configure its settings. For details on configuring each authentication profile type, see [Outbound authentication settings](#outbound-authentication-settings).

## Configure CORS profiles

You can use the **CORS Profiles** tab to create profiles for Cross Origin Resource Sharing, which can then be applied to the front-end API in the **Inbound** settings. To create an authentication profile, click the add button on the left, and configure the following settings:

* **GENERAL**: Enter a descriptive **Name** for the profile.
* **ORIGINS**: Click the add button, and enter the list of domains allowed to access this API (for example, `http://test.mydomain.org`, or a wild-carded value such as `http://*.mydomain.org`). Defaults to `*` to allow all domains.
* **ALLOWED HEADERS**: Click the add button, and enter the list of HTTP headers that can be used when invoking this API. This list of headers is defined by the value of the `Access-Control-Request-Headers` CORS header.
* **EXPOSED HEADERS**: Click the add button, and enter the list of HTTP headers to be exposed to the client in response to an invocation of this API.

    This does not include simple headers such as `Cache-Control`, `Content-Language`, `Content-Type`, `Expires`, `Last-Modified`, and `Pragma`.
* **CREDENTIALS SUPPORT**: Select whether the API advertises that it supports user credentials. When selected, the `Access-Control-Allow-Credentials` CORS header is sent in the response, with a value of `true`. This setting is not selected by default.
* **PREFLIGHT RESULT CACHE**: Enter how long the results of a CORS preflight `OPTIONS` request can be stored in the client preflight result cache. When configured, the `Access-Control-Max-Age` CORS header is sent in the response.

For more details on using CORS, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/). This provides more background information and explains how to configure CORS for specific HTTP services and relative paths in Policy Studio. For example, this may be useful when using a third-party load balancer, and you need to configure a CORS profile for the default **API Portal** HTTP service in Policy Studio.

## Configure trusted certificates

You can use the **Trusted Certificates** tab to add X.509 certificates, which can be used for the **Outbound** and **Inbound** SSL settings. To add a new certificate, perform the following steps:

1. Click the add button on the left, and configure the following:

   * **Source**: Select the source of the certificate (**X.509 Certificate** file or **URL**).
   * **File** or **URL**: Browse to the certificate source file (PKCS12, PEM, DER file), and enter a password if required. Alternatively, enter the URL for the certificate.
   * **Use for outbound**: Select whether is certificate is used for outbound security between the API Gateway and the back-end API. This is selected by default.
   * **Use for inbound**: Select whether is certificate is used for inbound security between the client and the API Gateway. This is not selected by default.
2. If you selected a **URL** certificate source, enter your **User name** and **Password** if required.
3. Click **Import**.

## Manage front-end REST API lifecycle

When you have registered the back-end REST API, you can select it in the list of registered APIs, click **Manage selected**, and chose one of the following options:

* **Delete**: Deletes the selected REST API(s) from the **API Registration > Frontend API**
    view. You can delete APIs registered as back-end REST APIs in the **Backend API** view.
* **Publish**: Publishes the selected REST API to be consumed by API consumers. You can edit the **API Name**, and enter an optional **Virtual host** name. When published, the API can be assigned to any organization or application. The API is locked, and no further edits are allowed.
* **Unpublish**: Unpublishes the selected REST API. When unpublished, the API is only available to the API administrator and to API owners in their organization, and not to other organizations. The API can be edited, published, or deleted.
* **Deprecate**: Select whether to **Retire API at specific date**, and enter a **Retirement date**. When selected, the published API is displayed with a date when it will be retired (unpublished) from the API Catalog, and is no longer available to client applications. When deprecated, the API is still published and clients can continue to discover and use the API. Only a published API can be deprecated and unpublished.
* **Undeprecate**: Undeprecates the selected deprecated REST API.
* **Upgrade access to newer API**: Upgrades all organizations and applications that had access to the original API to a more recent version of the API (if one exists). You can also deprecate and retire the original API as options.
* **Grant Access**: Grants organizations access to the selected APIs. You can select whether to **Grant API access to** all organizations, specific organizations, or organizations with access to specific APIs. For more information see [Manage API access](#manage-api-access).
* **Export API collection**: Exports a copy of the selected front-end REST APIs to your chosen directory. The APIs are exported in JSON format in a `.dat` file, which combines the front-end API, back-end API, security profiles, and so on. You must specify the following in the dialog:

    * **Export file name**: Specify a file name to export (defaults to `api-export.dat`)
    * **Password**: Add a mandatory password for encryption

    You can then import this file into API Manager as required (for example, when promoting between environments). See [Import a previously exported API](#import-a-previously-exported-api) and [Promote managed APIs between environments](/docs/apim_administration/apimgr_admin/api_mgmt_promote/).

### Encryption of exported API collections

In earlier API Manager versions, you could export API collections as a plaintext file. Now, by default, you can no longer export API collections as plaintext. You must supply a password and the generated file is encrypted. If you wish to generate a plaintext export file, the administrator must add the following lines towards the start of `INSTALL_DIR/apigateway/webapps/apiportal/vordel/apiportal/app/app.config` (for example, before the `nodemanager` setting):

```
/* Flag to determine if API collections can be exported as clear text:
 - Set to false if API export as clear text is not allowed (exported file is always encrypted)
 - Set to true if API export as clear text is allowed (you can choose to encrypt the file or not)*/
allowAPIExportAsClearText: true,
```

If you change this setting, you must clear your browser cache so that the old setting is removed.

When `allowAPIExportAsClearText` is set to `true`, the **Export API** dialog also includes an **Encrypt** option, which enables you to select whether to encrypt the export file using the specified password. For example:

![Export API collection](/Images/docbook/images/api_mgmt/api_mgmt_export.png)

## Perform zero downtime updates to an API

Sometimes it is necessary to make non-breaking changes to an API. For example, when the front-end API remains unchanged but the back-end API must be updated, or when the front-end API has additional methods or parameters that are backwards compatible. In this case, you must perform a zero downtime update.

Consider the following example with only the v1.0.0 of an API published in API Manager:

![API v1.0.0 published in API Manager](/Images/APIGateway/only_v1.0_api.png)

To upgrade the access to a newer version of the API (v1.1.0) to all organizations and applications that had access to the old version, as well as to deprecate or retire the old API, perform a zero downtime update as follows:

1. Add the new API v1.1.0 to API Manager and publish it.

   ![Both old and new APIs published in API Manager](/Images/APIGateway/both_published.png)
2. Select the old v1.0.0 API, click **Manage selected**, and choose **Upgrade access to newer API**, which brings you to the following screen:

   ![Grant API access screen in API Manager](/Images/APIGateway/grant_api_access.png)

After this is done and the old API is unpublished, the new API will be invoked and traffic will flow through the new API.

## Manage API access

You can grant or revoke access to your APIs, as well as you can view a list of the organizations and applications that have access to your APIs. The scope of access is as follows:

* API Administrator (API Admin):

    * Grant access to any API belonging to any organization.
    * Dependency view for all published APIs from all organizations.
    * Revoke access to any API belonging to any organization.

* Organization Administrator (Org Admin):

    * Grant access to APIs that belong to an organization within the scope of the Org Admin.
    * Dependency view to APIs that belong to an organization within the scope of the Org Admin.
    * Revoke access to APIs that belong to an organization within the scope of the Org Admin.

### Grant access to an API

Both API Admins and Org Admins can grant API access to organizations. After access has been granted to an API, it can then be used in any application within the organization, but the Org Admin still needs access to all the organizations and proxies to be able to manage the API. For more information on how to enable API management access to Org Admins, see [Organization administrator](/docs/api_mgmt_overview/key_concepts/api_mgmt_orgs_roles/#organizationadministrator).

To grant access to an API, perform the following steps:

1. Click **Frontend API** view in API Manager, then select one or more APIs from the list.
2. Click the **Manage selected** drop-down list, then choose **Grant access**.
3. Choose an option in the **Grant API access dialog** dialog box, then click **OK**.
    ![Grant API access](/Images/docbook/images/api_mgmt/grant-api-access-example.png)

### List resources with access to the API

To view the list of organizations and applications, which can access an API, perform the following steps:

1. Click **Frontend API** view in API Manager, then select an API from the list.
2. In the details of the API, click the **API Access** tab.
    A table containing two tabs, **Organizations** and **Applications**, is shown.

    ![Dependency view by applications](/Images/docbook/images/api_mgmt/dependency-view-applications-tab.png)

You can view the usage of published front-end APIs that have been granted access to external organizations and their applications, as well as the access granted date.

### Revoke access to an API

You can revoke an API access from applications and organizations. When you revoke access at the organizational level, this automatically also remove the access from all applications belonging to that organization, which were using the API.

Note that you cannot revoke access to the original organization that API belongs to, no self-revoke access is allowed.

To revoke access to organizations or applications, perform the following steps:

1. Click **Frontend API** view in API Manager and select an API from the list.
2. In the details of the API, click the **API Access** tab.
3. From the **Organizations** tab, select the organizations you wish to revoke access from. The **Manage selected** drop-down list is enabled.
4. Click the **Manage selected** drop-down list, and select **Revoke access**.
5. Click **Revoke** in the **Revoke API Access** dialog box to confirm the operation.

    The organization is removed form the list.
