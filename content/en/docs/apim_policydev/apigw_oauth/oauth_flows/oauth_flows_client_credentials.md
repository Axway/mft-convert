---
title: Client credentials grant flow
linkTitle: Client credentials grant
weight: 60
date: 2019-11-18
description: This user name and password flow is used when the client application needs to directly access its own resources on the resource server. Only the client application's credentials are used in this flow. The resource owner's credentials are not required.
---

The client credentials grant type must only be used by confidential clients.

The client can request an access token using only its client credentials (or other supported means of authentication) when the client is requesting access to the protected resources under its control. The client can also request access to those of another resource owner that has been previously arranged with the authorization server (the method of which is beyond the scope of the specification).

![Client Credentials Flow](/Images/OAuth/APIgw_Client_cred_grant_flow.png)

## Request an access token

The client token request should be sent in an HTTP `POST`to the token endpoint with the following parameters:

| Parameter    | Description                                             |
|--------------|---------------------------------------------------------|
| `grant_type` | Required. Must be set to `client_credentials`.          |
| `scope`      | Optional. The scope of the authorization.               |
| `format`     | Optional. Expected return format. The default is `json`. Possible values are:`urlencoded`, `json`, `xml`  |

The following is an example POST request:

```
POST /api/oauth/token HTTP/1.1
Content-Length:424
Content-Type:application/x-www-form-urlencoded; charset=UTF-8
Host:192.168.0.48:8080
Authorization:Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
grant_type=client_credentials
```

Comma (`,`) and plus (`+`) characters are treated as delimiters when specified in the `scope` parameter. For example, if you send the following client token request:

```
curl -ki https://localhost:8089/api/oauth/token --data-urlencode 'scope=resource.WRITE,resource.READ'
```

API Gateway returns an access token containing `"scope":"resource.WRITE resource.READ"`. The comma is interpreted as a space.

## Handle the response

The API Gateway authenticates the client against the Client Application Registry. An access token is sent back to the client on success. A refresh token is not included in this flow. An example valid response is as follows:

```
HTTP/1.1 200 OK
Cache-Control:no-store
Content-Type:application/json
Pragma:no-cache
{
    "access_token":“O91G451HZ0V83opz6udiSEjchPynd2Ss9......",
     "token_type":"Bearer",
     "expires_in":"3600"
}
```

## Run the sample client

The following Jython sample client sends a request to the authorization server using the client credentials flow:

```
INSTALL_DIR/samples/scripts/oauth/client_credentials.py
```

To run the sample, open a shell prompt at `INSTALL_DIR/samples/scripts`, and execute the following command:

```
run oauth/client_credentials.py
```

The script outputs the following:

```
Sending up access token request using grant_type set to client_credentials
Response from access token request:200
Parsing the json response
**********************ACCESS TOKEN RESPONSE***********************************
Access token received from authorization server OjtVvNusLg2ujy3a6IXHhavqdEPtK7qSmIj9fLl8qywPyX8bKEsjqF
Access token type received from authorization server Bearer
Access token expiry time:3599
******************************************************************************
Now we can try access the protected resource using the access token
Response from protected resource request is:200
<html>Congrats! You've hit an OAuth protected resource</html>
```
