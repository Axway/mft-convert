{
    "title": "Client authentication",
    "linkTitle": "Client authentication ",
    "weight": "310"
}{{< TransferCFT/suitevariablesTransferCFTName  >}} REST API supports *HTTP Basic* and *HTTP Bearer* as the authentication method. Confidentiality is ensured by the use of an HTTPS connection.

We recommended that you use the HTTP Bearer as opposed to Basic method for the following reasons:

- You user/password is not exposed.
- If the token is compromised, you can revoke the token using either the UI (My Access Token) or REST API. Note though that a token is just as sensitive as a user/password, you must store it in a protected manner.
- If am.type=passport and the PassPort server is down, you can still execute REST API requests.

Bearer authentication
---------------------

To use this type of authentication you must specify the HTTP Authorization header using the following syntax:

`Authorization: Bearer <token>`

****Example****

```
curl -X GET "https://localhost:1768/cft/api/v1/transfers" -H "accept: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6-B3zrHYjhAqX5UUYht2zkd5-iSBbdyUYuVpSTMhA"
```

To use the bearer method, you require an access token as described below.

> **Note**
>
> Note: REST API usage requires bearer authentication if SAML is enabled, am.type=saml.

### Generate an access token

In the {{< TransferCFT/axwayvariablesComponentLongName  >}} UI:

{{% TransferCFT/snippets/generate_token%}}

Basic authentication
--------------------

To use this type of authentication you must specify the HTTP Authorization header using the following syntax:

`Authorization: Basic <base64(user:password)>`

****Example****

```
curl -X GET "https://localhost:1768/cft/api/v1/transfers" -H "accept: application/json" -H "Authorization: Basic Z3Vlc3Q6Z3Vlc3QK"
```
{{% TransferCFT/snippets/bruteforce%}}
