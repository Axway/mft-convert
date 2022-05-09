{
"title": "Integrate with CA SiteMinder",
"linkTitle": "Integrate with CA SiteMinder",
"weight":"130",
"date": "2020-01-21",
"description": "Configure API Gateway to authenticate and authorize end users using CA SiteMinder."
}

CA SiteMinder is a centralized web access management system that provides user authentication and single sign-on, policy-based authorization, identity federation, and auditing of access to web applications and portals.

CA SiteMinder authenticates end users and authorizes them to access protected web resources. The gateway can request SiteMinder to authenticate end users using the user profiles stored in the SiteMinder server. SiteMinder decides whether the user should be authenticated, and the gateway then enforces this decision. API Gateway can also request SiteMinder to make authorization decisions on behalf of end users that have successfully authenticated to the gateway.

Integration with CA SiteMinder requires CA SiteMinder version 12.52. For more information on CA SiteMinder, go to the [CA Technologies website](https://www.broadcom.com/products/cyber-security/identity/siteminder).

## Flow description

SiteMinder authentication flow starts when an end user uses browser to attempt to access a resource protected with CA SiteMinder on the gateway. First, the gateway checks if the request from the user contains a valid session cookie for SiteMinder. If it does not find a valid session cookie the flow is as follows:

1. End user is prompted to provide the login credentials.
2. The gateway forwards the credentials to SiteMinder.
3. SiteMinder decides whether the user is authenticated and authorized for the requested resource. If the authentication is successful, SiteMinder returns a session cookie and a response to the gateway.
4. The gateway stores the session cookie from SiteMinder in the `siteminder.session` message attribute and creates a custom cookie.
5. The gateway returns a message with the cookie and a success response to the end user, and authorizes the user to access the requested resource.

On subsequent requests to access the protected resource, the end user sends the cookie to the gateway in the request message. API Gateway retrieves the cookie and validates it against SiteMinder. The end user stays authenticated for the entire lifetime of the session cookie. As long as the session cookie is valid, the gateway does not need to re-authenticate the end user against SiteMinder for every request. This increases throughput and performance considerably.

## Prerequisites

Before you start, you must have the following:

* API Gateway installed
* CA SiteMinder installed and configured

For more details on the installation and the initial configuration of CA SiteMinder, see the [CA SiteMinder documentation](https://support.ca.com/cadocs/0/CA%20SiteMinder%2012%2052-ENU/Bookshelfl). It is recommended you familiarize yourself with this documentation before you start integrating API Gateway with CA SiteMinder.

## Configuration process

The example policy is build in stages, starting with a simple authentication and authorization policy for SiteMinder that is then refined by adding further filters. It uses HTTP Basic to authenticate the end user, but you can replace it with another authentication mechanism, if required.

The following steps are required to integrate API Gateway with CA SiteMinder:

1. [Configure API Gateway as the SiteMinder agent](#configure-api-gateway-as-the-siteminder-agent)
2. [Configure SiteMinder connection](#configure-siteminder-connection)
3. [Configure SiteMinder authentication policy](#configure-siteminder-authentication-policy)
4. [Configure single sign-on](#configure-single-sign-on)

### Configure API Gateway as the SiteMinder agent

This section describes how to configure API Gateway to act as an agent for CA SiteMinder.

#### Add CA binaries to API Gateway

Integration with CA SiteMinder requires CA SiteMinder SDK version 12.52-sp02 or later. You must add the required third-party binaries to your API Gateway installation.

1. Ensure that any SiteMinder binaries you may have previously added to API Gateway have been deleted.
2. Install CA SiteMinder SDK.
3. Copy the following `jar` files from the `java` directory of the CA SDK:
    * `cryptoj.jar`
    * `smagentapi.jar`
    * `smjavasdk2.jar`
4. Add the files to the following directory on API Gateway:

    ```
    INSTALL_DIR/apigateway/<platform>/lib
    ```

5. Restart the gateway.

#### Register API Gateway as the SiteMinder agent

Before API Gateway can act as a Policy Enforcement Point (PEP) for SiteMinder, you must register the gateway as an agent for CA SiteMinder Policy Server. With the `smreghost` tool, you can register the agent from the command line, and create the `SmHost.conf` file.

The agent needs to be registered on the machine running the gateway instance. You must run the `smreghost` tool separately on each individual gateway instance. The `SmHost.conf` file is generated in the same directory as the `smreghost` tool.

Before you start, you need the following information on your CA SiteMinder Policy Server:

* IP address
* Login credentials (user name and password)
* Host Configuration Object name

To obtain this information, contact your SiteMinder administrator.

1. Go to `INSTALL_DIR/apigateway/<platform>/lib`.
2. Run the following:

    ```
    export LD_LIBRARY_PATH=INSTALL_DIR/apigateway/<platform>/lib
    ```

3. Run the `smreghost` tool as follows:

    ```
    smreghost -i <CA SiteMinder IP address> -u <user name> -p <password> -hc <Host Configuration Object name> –hn <hostname
    ```

    For example:

    ```
    smreghost -i 192.168.0.99 -u GatewayAgent -p XXXXXX -hc V6HostConfObject –hn apigateway.axway.int
    ```

    The hostname must be the fully qualified machine name of the host machine running API Gateway.

4. To enable debug output from the SiteMinder agent, add the following to `jvm.xml` if needed:

    ```
    <ConfigurationFragment>
    <VMArg name="-DSMJAVASDK_LOG_INFO=true"/>
    </ConfigurationFragment>
    ```

### Configure SiteMinder connection

This section describes how to configure API Gateway to connect to CA SiteMinder using Policy Studio

Before you start, you need the following information for your CA SiteMinder Policy Server:

* Web Agent name
* Agent Configuration Object name

To obtain this information, contact your SiteMinder administrator.

1. In the node tree, click **Environment Configuration > External Connections > SiteMinder/SOA Security Manager Connections**.
2. Select **Add a SiteMinder connection**.
3. Enter your agent name (`apigateway.axway.int`) and agent configuration object name (`V6HostConfObject`) you created in [Register API Gateway as the SiteMinder agent](#register-api-gateway-as-the-siteminder-agent).
4. Click **Browse**, select the `SmHost.conf` file for your agent, and click **OK**.

For more details on the fields and options in this configuration window, see [Configure SiteMinder/SOA Security Manager connections](/docs/apim_policydev/apigw_external_connections/connector_ca_connection/).

### Configure SiteMinder authentication policy

This section describes how to configure an authentication policy in Policy Studio for API Gateway to authenticate to CA SiteMinder.

#### Create an authentication repository

1. In the node tree, click **Environment Configuration > Authentication Repositories> SiteMinder Repositories**.
2. Select **Add a new Repository**, and enter a name for your repository (for example, `SiteMinder repository`).
3. Set **Agent Name** to the agent you registered (`GatewayAgent`), and click **OK**.

#### Configure routing to the protected resource

1. In the node tree, click **Policies**.
2. Add a new policy , and enter a name for it (`Route to protected resource`).
3. Open the **Routing** category in the palette, drag a **Connect to URL** filter onto the policy canvas, and enter a name for your filter (`Route to protected resource`).
4. Enter the URL for your protected resource, and click **Finish**.
5. Right click the filter, and select **Set as Start**.

The SiteMinder authentication and authorization policy calls to this protected routing policy using a **Policy Shortcut** filter.

#### Configure the SiteMinder authentication and authorization policy

This sections describes how to configure a policy to authenticate and authorize an end user existing in a CA SiteMinder repository.

To start, add a new policy named, for example, `SiteMinder`.

1. Open the **Authentication** category, and drag a **HTTP Basic** filter onto the policy canvas.
2. Set the following, and click **Finish**:
    * **Credential Format**: `User name`
    * **Repository name**: `<your SiteMinder repository>` (`SiteMinder repository`)
3. Open the **CA SiteMinder** category, and drag a **Authorization** filter onto the policy canvas.
4. Enter a name for the filter (for example, `Authorize user with SiteMinder`), and click **Finish**.
5. Open the **Utility** category, drag a **Policy Shortcut** filter onto the policy canvas.
6. Set **Policy Shortcut** to the routing policy you created (`Route to protected resource`), and click **Finish**.
7. Connect the filters with a success path.
8. Click on the **Add Relative Path** icon to create a new relative path (for example, `/siteminder`) that links to this policy.
9. Deploy the new configuration to API Gateway.

The policy looks like this:

![CA SSO authentication and authorization policy structure](/Images/IntegrationGuides/auth_auth/sm_policy.png)

The policy has the following flow:

* API Gateway authenticates the end user using HTTP Basic.
* API Gateway passes the end user’s credentials to SiteMinder.
* SiteMinder authenticates the end user, authorizes the end user for the particular resource, and sends a response to API Gateway.
* API Gateway routes the request to a policy shortcut calling the protected resource.

### Configure single sign-on

In a production deployment, it is not practical to authenticate and authorize each user for each request. Instead, you can configure API Gateway to store the SiteMinder session cookie for single sign-on (SSO).

After an end user is successfully authenticated, the gateway can create a custom cookie and place the SiteMinder session cookie validated for that end user as the cookie value. On later request, instead of re-authenticating the user every time, the gateway can check if the request contains a valid session cookie. If a valid session cookie is found, the end user does not have to be authenticate again. The gateway can also insert a SAML authorization assertion for downstream web services to consume.

This section expands the previously configured [SiteMinder authentication policy](#configure-siteminder-authentication-policy). To start, copy your SiteMinder authentication and authorization policy (`SiteMinder`), and rename it (for example, `SiteMinder Single Sign-On`.

#### Configure custom cookie creation

1. Open the **Conversion** category, and drag a **Create Cookie** filter onto the policy canvas.
2. Set the following:
    * **Cookie Name**: `smcookie`
    * **Cookie Value**: `${siteminder.session}`
    * **Path**: `/`
3. Set **Max-Age** to how long you want the cookie to remain valid, and click **Finish**.
4. Connect the **Authorization** filter (`Authorize user with SiteMinder`) to the **Create Cookie** filter with a success path.

    ![CA SSO policy with custom cookie creation](/Images/IntegrationGuides/auth_auth/sm_create_cookie.png)

The message attribute `${siteminder.session}` was specified in [Create an authentication repository](#create-an-authentication-repository). After the end user is successfully authenticated to SiteMinder, this is the message attribute that contains the end user’s SiteMinder session cookie.

#### Configure the cookie check

1. Open the **Attributes** category, and drag a **Get Cookie** filter onto the policy canvas.
2. Set the **Cookie Name** to `smcookie`.
3. Select **Fail if cookie not found in the message**, and click **Finish**.
4. Right-click the **Get Cookie** filter, and select **Set as Start**.
5. Connect the **Get Cookie** filter to the **HTTP Basic** filter with a failure path.

    ![CA SSO policy with the check for a vliad session cookie.](/Images/IntegrationGuides/auth_auth/sm_get_cookie.png)

If the request from then end user does not contain a valid SiteMinder cookie, the end user is prompted to provide login credentials.

#### Configure SiteMinder session validation

If API Gateway finds a valid session cookie from an incoming request, it uses the cookie value to validate the end user's SiteMinder session.

1. Open the **CA SiteMinder** category, and drag a **Session Validation** filter onto the policy canvas.
2. Set **Agent Name** to your registered SiteMinder agent (`GatewayAgent`).
3. Set **Selector Expression to retrieve session** to `${cookie.smcookie.value}`, and click **Finish**.
4. Connect the **Get Cookie** filter to the **Session Validation** filter with a success path.
5. Connect the **Session Validation** filter to the **Authorization** filter (`Authorize user with SiteMinder`) with a success path.
6. Connect the **Session Validation** filter to the **HTTP Basic** filter with a failure path.

![CA SSO policy with session validation](/Images/IntegrationGuides/auth_auth/sm_validate_session.png)

If the SiteMinder session cannot be validated, the end user is prompted to provide login credentials.

#### Deploy the policy

1. Click on the **Add Relative Path** icon to create a new relative path (for example, `/siteminder_sso`) that links to this policy.
2. Deploy the new configuration to API Gateway.

You have now configured a simple policy for single sign-on in SiteMinder authentication.

The policy has the following flow:

* API Gateway checks if the request contains a valid custom cookie. If a valid custom cookie is not found, the end user is prompted to provide login credentials.
* If a valid custom cookie is found, API Gateway retrieves the value of the cookie and uses that to check if the end user's session cookie in SiteMinder is still valid. If the session cookie is no longer valid, the end user is prompted to provide login credentials.
* SiteMinder authorizes the user to access the requested resource and sends response with the SiteMinder cookie back to API Gateway.
* API Gateway creates a custom cookie to hold the end user's SiteMinder session cookie.
* API Gateway calls to the protected resource using the routing policy.
* API Gateway sends the response along with the custom cookie to the end user.

This policy can be part of a larger policy, including features such as XML threat detection and conditional routing (not described in this guide). You can also authenticate the user with some other authentication method instead of HTTP Basic. In addition, you can add additional filters, such as messages in case of failures:

![CA SSO single sing-on policy with failure messages](/Images/IntegrationGuides/auth_auth/sm_messages.png)

#### Configure inserting a SAML token

You can extend the single sign-on to downstream web services, if required. After the end user is successfully authenticated and authorized, API Gateway adds a SAML authentication assertion to the response message for the web services to consume.

1. Open the **Authorization** category, and drag a **Insert SAML Authentication Assertion** filter onto the policy canvas.
2. On the **Assertion details** tab, select any issuer on the **Issuer Name** list, and set the expiry date for the SAML authentication assertion.
3. On the **Assertion Location** tab, make sure that **Add to WS-Security Block with SOAP Actor/Role** is selected and **SOAP Actor/Role** set to **Current Actor/Role Only**.
4. On the **Advanced** tab, select **Insert SAML Attribute Statement** and **Indent**, then click **Finish**.
5. Connect the filter between the **Create Cookie** and the **Policy Shortcut** filters with success paths.

![Adding Insert SAML Authorization Assertion to the policy](/Images/IntegrationGuides/auth_auth/sm_policy_SAML.png)

### Test the policy

This section describe how to test your SiteMinder policy using a standard browser.

1. Open a web browser in private (incognito) mode to ensure no user is yet logged in.
2. Enter the URL to call your policy:

    ```
    http://<ip address>:<port>/<path>
    ```

    For example:

    ```
    http://localhost:8080/siteminder\_sso
    ```

3. Enter the login credentials. API Gateway authenticates the user against SiteMinder and returns the response along with the SiteMinder session cookie.
4. Refresh the browser to access the protected resource. Because the custom cookie (`smcookie`) is available this time, API Gateway does not prompt for credentials. Instead of re-authentication, API Gateway validates the cookie against the SiteMinder.

The **Traffic Monitor** tab on the API Gateway Manager (`https://localhost:8090`) is an excellent place to view and troubleshoot the message flows. For more details, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service/#monitor-services-in-api-gateway-manager).
