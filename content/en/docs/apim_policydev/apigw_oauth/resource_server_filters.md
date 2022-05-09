---
title: OAuth resource server filters
linkTitle: OAuth resource server filters
weight: 70
date: 2019-11-18T00:00:00.000Z
description: Filters you can use when API Gateway is acting as an OAuth resource server.
---

## Validate access token filter

The OAuth 2.0 **Validate Access Token** filter is used to validate a specified access token contained in persistent storage. OAuth access tokens are used to grant access to specific resources in an HTTP service for a specific period of time (for example, photos on a photo sharing website). This enables users to grant third-party applications access to their resources without sharing all of their data and access permissions.

Configure the following fields:

**Name**:
Enter a suitable name for this filter.

**Verify access token is in cache**:
Click the browse button to select the cache in which to verify the access token (for example, in the default **OAuth Access Token Store**). To add an access token store, right-click **Access Token Stores**, and select **Add Access Token Store**. You can store tokens in a cache, in a relational database, or in an Apache Cassandra database. For more details, see [Manage access tokens and authorization codes](/docs/apim_policydev/apigw_oauth/gw_oauth_authz_server/#manage-access-tokens-and-authorization-codes).

**Location of access token**:
Select one of the following:

* **In Authorization Header with prefix**:
    The access token is in the Authorization header with the selected prefix. Defaults to `Bearer`
    . This is the default option.
* **In query string/form body field named**:
    The access token is in the HTTP query string with the name specified in the text box.
* **In Attribute**:
    The access token is in the API Gateway message attribute specified in the text box.

**Validate Scopes**:
Select whether scopes match **Any** or **All** of the configured scopes in the table, and click **Add** to add an OAuth scope. The default scopes are found in `${http.request.uri}`. For example, the default scopes used in the OAuth demos are `resource.READ` and `resource.WRITE`.

### Validate access token response codes

The **Validate Access Token** filter performs a number of checks to determine if the token is valid. If any of the checks fail, the response can be examined to determine the reason for the failure.

The filter performs the following sequence of steps to determine if the token is valid:

1. Locate the token in the incoming request. The token can be in the Authorization header, in a query string, or in a message attribute.

    * If the filter is configured to find the token in a message attribute and no token is found, the following response is sent:

    ```
    HTTP/1.1 400 Bad Request
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="invalid_request",
    error_description="Unable to find token in the message."
    ```

    * If the filter is configured to find the token in the Authorization Bearer header and no token is found (or the Authorization header is not found or does not contain the Bearer header), the following response is sent:

    ```
    HTTP/1.1 400 Bad Request
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="invalid_request",
    error_description="Unable to find token in the message."
    ```

2. If the token is found in the incoming request, next verify that the token can be found in the API Gateway persistent storage mechanism. If it cannot be found, the following response is sent:

    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="invalid_token",
    error_description="Unable to find the access token in persistent storage."
    ```

3. If the token is found in persistent storage, next verify the authenticity of the token. This includes checking the token's expiry, client identifier, and required scopes.

    * Check if the token has expired. An expired token must not be able to allow access to a resource. If the token has expired, the following response is sent:

    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="invalid_token",
    error_description="The access token expired."
    ```

    * Check the client ID in the token and ensure it is the same as a client ID stored in the API Gateway client registry. (To use OAuth you need a client application and the client application must have OAuth credentials.) Check that the application is still enabled. If either checks fail, the following response is sent:

    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="invalid_token",
    error_description="The client app was not found or is disabled."
    ```

    * Validate the scopes in the token against the scopes configured in Policy Studio. In Policy Studio you can specify that scopes should match **Any** or **All** of the scopes listed in the table. If **All** is selected, the token scopes must match all of the scopes listed in Policy Studio. If **Any** is selected, the token scopes intersection with the scopes listed in Policy Studio must not be empty. If the scopes do not match, the following response is sent:

    ```
    HTTP/1.1 403 Forbidden
    WWW-Authenticate:Bearer realm="DefaultRealm",
    error="insufficient_scope",
    error_description="scope(s) associated with access token are not valid to access this resource.",
    scope="Scopes must match Any of these scopes:resource.WRITE"
    ```

    The message includes a further string listing the scopes required to access the resource.

4. If the token is authentic, allow access to the resource.
