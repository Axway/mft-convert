---
title: Resource owner password credentials flow
linkTitle: Resource owner password credentials
weight: 50
date: 2019-11-18
description:  The resource owner password credentials grant type is suitable in cases where the resource owner has a trust relationship with the client (for example, the device operating system or a highly privileged application).
---

The resource owner password credentials flow is also known as the username-password authentication flow. This flow can be used as a replacement for an existing login when the consumer already has the user's credentials.

The authorization server should take special care when enabling this grant type, and only allow it when other flows are not viable.

This grant type is suitable for clients capable of obtaining the resource owner's credentials (username and password, typically using an interactive form). It is also used to migrate existing clients using direct authentication schemes such as HTTP basic or digest authentication to OAuth by converting the stored credentials to an access token.

![Resource Owner Password Credentials Flow](/Images/OAuth/APIgw_Resc_own_pwd_cred_flow.png)

## Request an access token

The client token request should be sent in an HTTP `POST`to the token endpoint with the following parameters:

| Parameter    | Description                                                                   |
|--------------|-------------------------------------------------------------------------------|
| `grant_type` | Required. Must be set to `password`.                                          |
| `username`   | Required. The resource owner's user name.                                     |
| `password`   | Required. The resource owner's password.                                      |
| `scope`      | Optional. The scope of the authorization.                                     |
| `format`     | Optional. Expected return format. The default is `json`. Possible values are `urlencoded`,                                                           `json`, and `xml`. |

The following is an example HTTP `POST`request:

```
POST /api/oauth/token HTTP/1.1
Content-Length:424
Content-Type:application/x-www-form-urlencoded; charset=UTF-8
Host:192.168.0.48:8080
Authorization:Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
grant_type=password&username=johndoe&password=A3ddj3w
```

Comma (`,`) and plus (`+`) characters are treated as delimiters when specified in the `scope` parameter. For example, if you send the following client token request:

```
curl -ki https://localhost:8089/api/oauth/token --data-urlencode 'scope=resource.WRITE,resource.READ'
```

API Gateway returns an access token containing `"scope":"resource.WRITE resource.READ"`. The comma is interpreted as a space.

## Handle the response

The API Gateway validates the resource owner's credentials and authenticates the client against the Client Application Registry. An access token, and optional refresh token, is sent back to the client on success. For example, a valid response is as follows:

```
HTTP/1.1 200 OK
Cache-Control:no-store
Content-Type:application/json
Pragma:no-cache
{
"access_token":“O91G451HZ0V83opz6udiSEjchPynd2Ss9......",
"token_type":"Bearer",
"expires_in":"3600",
"refresh_token":"8722gffy2229220002iuueee7GP..........."
}
```

## Run the sample client

The following Jython sample client sends a request to the authorization server using the resource owner password credentials flow:

```
INSTALL_DIR/samples/scripts/oauth/resourceowner_password_credentials.py
```

To run the sample, open a shell prompt at `INSTALL_DIR/samples/scripts`, and execute the following command:

```
run oauth/resourceowner_password_credentials.py
```

The script outputs the following:

```
Sending up access token request using grant_type set to password
Response from access token request:200
Parsing the json response
**********************ACCESS TOKEN RESPONSE***********************************
Access token received from authorization server lrGHhFhFwSmycXStIza1jjvXlSaac9JNIgviF7oPiV8OnxlSIsrxVA
Access token type received from authorization server Bearer
Access token expiry time:3600
******************************************************************************
Now we can try access the protected resource using the access token
Executing get request on the protected url
Response from protected resource request is:200
<html>Congrats! You've hit an OAuth protected resource</html>
```
