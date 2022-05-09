{
"title": "Revoke token flow",
"linkTitle": "Revoke token",
"weight": 80,
"date": "2019-11-18",
"description": "A revoke token request causes the removal of the client application permissions associated with the particular token to access the end-user's protected resources."
}

In some cases a user might wish to revoke access given to an application. An access token can be revoked by calling the API Gateway revoke service and providing the access token to be revoked.

![OAuth 2.0 Revoke Token](/Images/OAuth/APIgw_Revoke_token.png)

The endpoint for revoke token requests is as follows:

```
https://HOST:8089/api/oauth/revoke
```

The token to be revoked should be sent to the revoke token endpoint in an HTTP `POST`
with the following parameters:

| Parameter         | Description                                                                                      |
|-------------------|--------------------------------------------------------------------------------------------------|
| `token`           | Required. A token to be revoked (for example, `4eclEUX1N6oVIOoZBbaDTI977SV3T9KqJ3ayOvs4gqhGA4`). |
| `token_type_hint` | Optional. A hint specifying the token type. For example, `access_token` or `refresh_token`. |

The following is an example POST request:

```
POST /api/oauth/revoke HTTP/1.1
Content-Type:application/x-www-form-urlencoded; charset=UTF-8
Host:192.168.0.48:8080
Authorization:Basic U2FtcGxlQ29uZmlkZW50aWFsQXBwOjY4MDhkNGI2LWVmMDktNGIwZC04ZjI4LT
NiMDVkYTljNDhlYw==token=4eclEUX1N6oVIOoZBbaDTI977SV3T9KqJ3ayOvs4gqhGA4
&token_type_hint=refresh_token
```

## Run the sample client

The following Jython sample client creates a token revoke request to the authorization server:

```
INSTALL_DIR/samples/scripts/oauth/revoke_token.py
```

To run the sample, open a shell prompt at `INSTALL_DIR/samples/scripts`, and execute the following command:

```
run oauth/revoke_token.py
```

Paste the value associated with the token in the dialog:

![Revoking an OAuth 2.0 Access/Refresh](/Images/OAuth/oauth_revoke_token.png)

Select the type of token hint in the next dialog:

![Providing a token hint](/Images/OAuth/oauth_revoke_token_hint.png)

If you select `none`, no `token_type_hint` parameter is specified in the POST request. If you select `access_token`, the POST request contains `token_type_hint=access_token`. If you select `refresh_token`, the POST request contains `token_type_hint=refresh_token`.

When the authorization server receives the token revocation request, it first validates the client credentials and verifies whether the client is authorized to revoke the particular token based on the client identity.

{{< alert title="Note" color="primary" >}}
Only the client that was issued the token can revoke it.
{{< /alert >}}

The authorization server decides whether the token is an access token or a refresh token:

* If it is an access token, this token is revoked.
* If it is a refresh token, all access tokens issued for the refresh token are invalidated, and the refresh token is revoked.

## Response codes

The following HTTP status response codes are returned:

* HTTP 200 if the token was revoked successfully or if an invalid token was submitted.
* HTTP 401 if client authentication failed.
* HTTP 403 if the client is not authorized to revoke the token.

The following is an example response:

```
Token to be revoked:3eXnUZzkODNGb9D94Qk5XhiV4W4gu9muZ56VAYoZiot4WNhIZ72D3
Revoking token...............
Response from revoke token request is:200
Successfully revoked token
```
