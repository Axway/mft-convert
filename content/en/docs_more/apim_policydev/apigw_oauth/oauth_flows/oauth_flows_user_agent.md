---
title: Implicit grant (or user agent) flow
linkTitle: Implicit grant
weight: 30
date: 2019-11-18
description: The implicit grant (user agent) authentication flow is used by client applications (consumers) residing on the user's device.
---

This can be implemented in a browser using a scripting language such as JavaScript, or from a mobile device, or a desktop application. These consumers cannot keep the client secret confidential (application password or private key).

The user agent flow is as follows:

1. The web server redirects the user to the API Gateway acting as an authorization server to authenticate and authorize the server to access data on their behalf.
2. After the user approves access, the web server receives a callback with an access token in the fragment of the redirect URL.
3. After the token is granted, the application can access the protected data with the access token.

![OAuth 2.0 User Agent Flow](/Images/OAuth/APIgw_Oauth_implicit_grant_flow.png)

## Obtain an access token

This section details the steps for obtaining an access token.

### Web server redirects user to authorization endpoint

Redirect the user to the authorization endpoint with the following parameters:

| Parameter       | Description                                                                                                                                      |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| `response_type` | Required. Must be set to `token`.                                                                                                                |
| `client_id`     | Required. The client ID generated when the application was registered in the Client Application Registry.                                        |
| `redirect_uri`  | Optional. The location where the access token will be sent. This value must match one of the values provided in the Client Application Registry. |
| `scope`         | Optional. A space delimited list of scopes, which indicates the access to the resource owner's data requested by the application.                |
| `state`         | Optional. Any state the consumer wants reflected back to it after approval during the callback.                                                  |

The following is an example URL:

```
https://apigateway/oauth/authorize?client_id=SampleConfidentialApp&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%3A8090%2Fauth%2Fredirect.html&scope=https%3A%2F%2Flocalhost%3A8090%2Fauth%2Fuserinfo.email
```

{{< alert title="Note" color="primary" >}}During this step the resource owner user must approve access for the application (web server) to access their protected resources, as shown in the following example window.{{< /alert >}}

![OAuth 2.0 Implicit Grant Flow - Grant Access](/Images/OAuth/oauth_flow_allow_access.png)

### Web server receives callback with access token

The response to the above request is sent to the `redirect_uri`. If the user approves the access request, the response contains an access token and the state parameter (if included in the request). For example:

```
https://localhost/oauth_callback#access_token=19437jhj2781FQd44AzqT3Zg&token_type=Bearer&expires_in=3600
```

If the user does not approve the request, the response contains an error message.

After the request is verified, the API Gateway sends a response to the client. The following parameters are contained in the fragment of the redirect:

| Parameter      | Description                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `access_token` | The token that can be sent to the resource server to access the protected resources of the resource owner (user).                                   |
| `expires`      | The remaining lifetime on the access token.                                                                                                         |
| `type`         | Indicates the type of token returned. This field always has a value of `Bearer`.                                                                    |
| `state`        | Optional. If the client application sent a value for state in the original authorization request, the state parameter is populated with this value. |

### Web server uses access token to access protected resources

After the application obtains an access token, it can gain access to protected resources on the resource server by placing it in an `Authorization:Bearer`
HTTP header:

```
GET /oauth/protected HTTP/1.1
Authorization:Bearer O91G451HZ0V83opz6udiSEjchPynd2Ss9
Host:apigateway.com
```

For example, the `curl`command to call a protected resource with an access token is as follows:

```
curl -H "Authorization:Bearer O91G451HZ0V83opz6udiSEjchPynd2Ss9" https://apigateway.com/oauth/protected
```

## Run the sample client

The following Jython sample client creates and sends an authorization request for the implicit grant flow to the authorization server:

```
INSTALL_DIR/samples/scripts/oauth/implicit_grant.py
```

To run the sample, perform the following steps:

1. Open a shell prompt at the `INSTALL_DIR/samples/scripts` directory.
2. Execute the following command:

    ```
    run oauth/implicit_grant.py
    ```

    The script outputs the following:

    ```
    Go to the URL here:
    http://127.0.0.1:8080/api/oauth/authorize?client_id=SampleConfidentialApp&response_type=token&scope=https%3A%2F%2Flocalhost%3A8090%2Fauth%2Fuserinfo.email&redirect_uri=https%3A%2F%2Flocalhost%2Foauth_callback&state=1956901292
    Enter Access Token code in dialog
    ```

    After the resource owner has authorized and approved access to the application, the authorization server redirects to the redirection URI a fragment containing the access token. For example:

    ```
    https://localhost/oauth_callback#access_token=4owzGyokzLLQB5FH4tOMk7Eqf1wqYfENEDXZ1mGvN7u7a2Xexy2OU9&expires_in=3599
    &state=1956901292&token_type=Bearer
    ```

    In this example, the access token is:

    ```
    4owzGyokzLLQB5FH4tOMk7Eqf1wqYfENEDXZ1mGvN7u7a2Xexy2OU9
    ```

3. Enter this value into the **Enter Access Token from fragment** dialog.

    ![Entering OAuth 2.0 Access Token](/Images/OAuth/oauth_user_agent_token.png)

    The script attempts to access the protected resource using the access token. For example:

    ```
    **********************ACCESS TOKEN RESPONSE******************************
    Access token received from authorization server 4owzGyokzLLQB5FH4tOMk7Eqf1wqYfENEDXZ1mGvN7u7a2Xexy2OU9
    ******************************************************************************
    Now we can try access the protected resource using the access token
    Executing get request on the protected url
    Response from protected resource request is:200
    <html>Congrats! You've hit an OAuth protected resource</html>
    ```
