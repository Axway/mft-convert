---
title: Authorization code grant (or web server) flow
linkTitle: Authorization code grant
weight: 20
date: 2019-11-18
description: The authorization code or web server flow is suitable for clients that can interact with the end-user's user-agent (typically a web browser), and that can receive incoming requests from the authorization server (can act as an HTTP server).
---
The web server authentication flow is used by applications that are hosted on a secure server. A critical aspect of the web server flow is that the server must be able to protect the issued client application's secret. The authorization code flow is also known as the *three-legged OAuth* flow.

The authorization code flow is as follows:

1. The web server redirects the user to the API Gateway acting as an authorization server to authenticate and authorize the server to access data on their behalf.
2. After the user approves access, the web server receives a callback with an authorization code.
3. After obtaining the authorization code, the web server passes back the authorization code to obtain an access token response.
4. After validating the authorization code, the API Gateway passes back a token response to the web server.
5. After the token is granted, the web server accesses the user's data.

![OAuth 2.0 Web Server Flow](/Images/OAuth/APIgw_Oauth_web_server_flow.png)

## Obtain an access token

This section details the steps for obtaining an access token.

### Web server redirects user to authorization endpoint

Redirect the user to the authorization endpoint with the following parameters:

| Parameter       | Description                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `response_type` | Required. Must be set to `code`.                                                                                                                                                                                                                                                                                               |
| `client_id`     | Required. The client ID generated when the application was registered in the Client Application Registry.                                                                                                                                                                                                                      |
| `redirect_uri`  | Optional. The location where the authorization code will be sent. This value must match one of the values provided in the Client Application Registry.                                                                                                                                                                |
| `scope`         | Optional. A space delimited list of scopes, which indicate the access to the resource owner's data requested by the application.                                                                                                                                                                                               |
| `state`         | Recommended. Any state the consumer wants reflected back to itself after approval during the callback. This value is used to prevent cross-site request forgery (CSRF) attacks, the consumer delivers this value to the authorization server when making an authorization request. For this reason, the value must be opaque, kept secret by the consumer, and known only to the consumer and the OAuth provider. |

The following is an example URL:

```
https://apigateway/oauth/authorize?client_id=SampleConfidentialApp&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%3A8090%2Fauth%2Fredirect.html&scope=https%3A%2F%2Flocalhost%3A8090%2Fauth%2Fuserinfo.email
```

{{< alert title="Note" color="primary" >}}During this step the resource owner user must approve access for the application web server to access their protected resources, as shown in the following example window.{{< /alert >}}

![OAuth 2.0 Authorization Code Grant Flow - Grant Access](/Images/OAuth/oauth_flow_allow_access.png)

### Web server receives callback with authorization code

The response to the above request is sent to the `redirect_uri`. If the user approves the access request, the response contains an authorization code and the `state`
parameter (if included in the request). If the user does not approve the request, the response contains an error message. All responses are returned to the web server on the query string. For example:

```
https://localhost/oauth_callback&code=9srN6sqmjrvG5bWvNB42PCGju0TFVV
```

### Web server exchanges authorization code for access token

After the web server receives the authorization code, it can exchange the authorization code for an access token and a refresh token. This request is an HTTPS `POST`, and includes the following parameters:

| Parameter       | Description                                                                                |
|-----------------|--------------------------------------------------------------------------------------------|
| `grant_type`    | Required. Must be set to `authorization_code`.                                             |
| `code`          | Required. The authorization code received in the redirect above.                           |
| `redirect_uri`  | Required. The redirect URL registered for the application during application registration. |
| `client_id`     | Optional. The `client_id` obtained during application registration.                        |
| `client_secret` | Optional. The `client_secret` obtained during application registration.                    |
| `format`        | Optional. Expected return format. The default is `json`. Possible values are:`urlencoded`, `json`, `xml`|

{{< alert title="Note" color="primary" >}}If the `client_id` and `client_secret` are not provided as parameters in the HTTP `POST`, they must be provided in the HTTP basic authentication header: `Authorization base64Encoded(client_id:client_secret)`.{{< /alert >}}

The following example HTTPS `POST` shows some parameters:

```
POST /api/oauth/token HTTP/1.1
Content-Type:application/x-www-form-urlencoded

client_id=SampleConfidentialApp&client_secret=6808d4b6-ef09-4b0d-8f28-3b05da9c48ec&code=9srN6sqmjrvG5bWvNB42PCGju0TFVV    &redirect_uri=http%3A%2F%2Flocalhost%3A8090%2Fauth%2Fredirect.html&grant_type=authorization_code&format=query
```

### Web server receives access token

After the request is verified, the API Gateway sends a response to the client. The following parameters are in the response body:

| Parameter       | Description                                                                                                       |
|-----------------|-------------------------------------------------------------------------------------------------------------------|
| `access_token`  | The token that can be sent to the resource server to access the protected resources of the resource owner (user). |
| `refresh_token` | A token that can be used to obtain a new access token.                                                            |
| `expires`       | The remaining lifetime on the access token.                                                                       |
| `type`          | Indicates the type of token returned. This field always has a value of `Bearer`.                                  |

The following is an example response:

```
HTTP/1.1 200 OK
Cache-Control:no-store
Content-Type:application/json
Pragma:no-cache{
    "access_token":“O91G451HZ0V83opz6udiSEjchPynd2Ss9......",
    "token_type":"Bearer",
    "expires_in":"3600" }
```

### Web server uses access token to access protected resources

After the web server obtains an access token, it can gain access to protected resources on the resource server by placing it in an `Authorization:Bearer` HTTP header:

```
GET /oauth/protected HTTP/1.1
Authorization:Bearer O91G451HZ0V83opz6udiSEjchPynd2Ss9
Host:apigateway.com
```

For example, the `curl` command to call a protected resource with an access token is as follows:

```
curl -H "Authorization:Bearer O91G451HZ0V83opz6udiSEjchPynd2Ss9" https://apigateway.com/oauth/protected
```

## Run the sample client

The following Jython sample client creates and sends an authorization request for the authorization grant flow to the authorization server:

```
INSTALL_DIR/samples/scripts/oauth/authorization_code.py
```

To run the sample, perform the following steps:

1. Open a shell prompt at the `INSTALL_DIR/samples/scripts` directory.
2. Execute the following command:

    ```
    run oauth/authorization_code.py
    ```

    The script outputs the following:

    ```
    Go to the URL here:
    http://127.0.0.1:8080/api/oauth/authorize?client_id=SampleConfidentialApp&response_type=code&scope=https%3A%2F%2Flocalhost%3A8090%2Fauth%2Fuserinfo.email&redirect_uri=https%3A%2F%2Flocalhost%2Foauth_callback
    Enter Authorization code in dialog
    ```

3. Copy the URL into a browser, and perform the following steps as prompted:

    * Provide login credentials to the authorization server. The default Client Application Registry user name is `regadmin`.
    * When prompted, grant access to the client application to access the protected resource.

    After the resource owner has authorized and approved access to the application, the authorization server redirects a fragment containing the authorization code to the redirection URI. For example:

    ```
    https://localhost/oauth_callback&code=AaI5Or3RYB2uOgiyqVsLs1ATIY0ll0
    ```

    In this example, the authorization code is:

    ```
    AaI5Or3RYB2uOgiyqVsLs1ATIY0ll0
    ```

4. Enter this value into the **Enter Authorization Code** dialog.

    ![Entering OAuth 2.0 Authorization Code](/Images/OAuth/oauth_web_server_authz_code.png)

    The script exchanges the authorization code for an access token, and then accesses the protected resource using the access token. For example:

    ```
    Enter Authorization code in dialog
    AuthZ code:AaI5Or3RYB2uOgiyqVsLs1ATIY0ll0
    Exchange authZ code for access token
    Sending up access token request using grant_type set to authorization_code
    Response from access token request:200
    Parsing the json response
    **********************ACCESS TOKEN RESPONSE***********************************
    Access token received from authorization server icPgKP2uVUD2thvAZ5ENhsQb66ffnZECXHyRQEz5zP8aGzcobLV3AR
    Access token type received from authorization server Bearer
    Access token expiry time:3599
    Refresh token:NpNbzIVVvj8MhMmcWx2zsawxxJ3YADfc0XIxlZvw0tIhh8
    ******************************************************************************
    Now we can try access the protected resource using the access token
    Executing get request on the protected url
    Response from protected resource request is:200
    <html>Congrats! You've hit an OAuth protected resource</html>
    ```
