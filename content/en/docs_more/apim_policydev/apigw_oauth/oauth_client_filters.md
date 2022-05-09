---
title: OAuth client filters
linkTitle: OAuth client filters
weight: 110
date: 2019-11-18
description: Filters you can use when API Gateway is acting as an OAuth client.
---

## Redirect resource owner to authorization server filter

The purpose of the **Redirect resource owner to Authz Server** filter is to redirect the resource owner's user agent to the OAuth authorization server. This filter can only be used in the authorization code flow.

Configure the following general settings for the **Redirect resource owner to Authz Server** filter:

**Name**:
Enter a suitable name for this filter.

**Choose OAuth Token Key**:
Enter the message attribute to be used as the key to look up the token. The token key must be set to the authentication value you require for the OAuth token. In the context of an OpenID Connect flow the OAuth token key can be a cookie value. Defaults to `${authentication.subject.id}`.

If no key is specified, this field is ignored. This is to accommodate anonymous use of OAuth whereby the user starting the process does not need to authenticate with API Gateway first.

**Override scopes setup for authz request**:
This field allows you to override the scopes assigned to a client profile. This could potentially be used in the OpenID Connect flow whereby you could specify scopes for the OpenID Connect flow, or if you already have a session you would not need to specify OpenID Connect scopes. (For an example, see the OAuth client demo.)

**Optionally select a client credential profile**:
Select this option and click the browse button to select an OAuth client credential profile. This can be used if no preceding filter has set the application profile on the message board, or to override the existing application profile.

**Configure OAuth State Map**:
This allows you to configure extra parameters in the OAuth state map (for example, to implement cross-site request forgery (CSRF) protection). By default the state map contains `client_id`
and `oauth.token.id`. Any extra parameters added here are added to the state map. The message attribute `oauth.state.map`
will be available at the callback endpoint when an authorization code is exchanged for a token.

Select one of the following options:

* **Add extra state properties**:
Click **Add** to store additional state properties, and enter the **Name** and **Value** in the dialog.
* **Retrieve properties from selector**:
Enter the attribute that stores the properties. Defaults to `oauth.state.map`.

## Retrieve OAuth client access token from token storage filter

You can use the **Retrieve OAuth Client Access Token from Token Storage** filter to retrieve a stored access token from a client access token store.

Tokens received from OAuth providers are stored in a **Client Access Token Store**. You can configure client access token stores under the **Environment Configuration > Libraries > OAuth2 Stores** node in the Policy Studio tree. Similar to an **Access Token Store**, this store can be backed by an API Gateway cache (default), a relational database, or an Apache Cassandra database. For more details on client access token stores, see [Manage client access tokens](/docs/apim_policydev/apigw_oauth/gw_oauth_client/#manage-client-access-tokens).

A configured token store is associated with an OAuth provider and is shared by all client applications registered with that provider.

These stored tokens can be retrieved by this filter by specifying the OAuth 2.0 provider profile (the client application registered with a provider) and the token key (for example, the authentication subject id of the current user). Stored tokens are indexed by the client ID of the the client application and the token key. If `authentication.subject.id` is not available, the client ID is used for both indexes. This is valid for grant types that treat the client application as the resource owner, that is, client credentials, JWT, and SAML (when no resource owner is specified).

If a valid token is found by this filter it is placed on the message board as `oauth.client.accesstoken`, and the filter passes. If the token is expired, or there is no token found, the filter fails (expired tokens are still placed on the message board). The fail path can be used to refresh an expired token or start the process of requesting a token. The client application is also placed on the message board, under the attribute name `oauth.client.application`, for use in subsequent filters.

Configure the following general settings for the **Retrieve OAuth Client Access Token from Token Storage** filter:

**Name**:
Enter a suitable name for this filter.

**Choose OAuth Token Key**:
Enter the message attribute to be used as the key to lookup the token. Defaults to `${authentication.subject.id}`.

**Choose profile to be used for token request**:
Click the browse button to select an OAuth 2.0 client credential profile.

## Get OAuth client access token filter {#getoauthtoken}

You can use the **Get OAuth Access Token** filter to request a token. This filter attempts to get the access token from persistent storage, and if a token is not available it sends an outbound token request.

Configure the following general settings for the **Get OAuth Access Token** filter:

**Name**:
Enter a suitable name for this filter.

**Choose OAuth Token Key**:
Enter the message attribute to be used as the key to look up the token. Defaults to `${authentication.subject.id}`.

If no key is specified in this field, the access token is not saved. This is to accommodate anonymous use of OAuth whereby the user starting the process does not need to authenticate with API Gateway first.

**Optionally select a client credential profile**:
Select this option and click the browse button to select an OAuth client credential profile. This can be used if no preceding filter has set the application profile on the message board, or to override the existing application profile.

### Retrieve token SSL and additional settings

You can configure SSL settings, such as trusted certificates, client certificates, SSL/TLS protocols, and ciphers on the **SSL** tab.

The **Settings** tab allows you to configure the following additional settings:

* **Retry**
* **Failure**
* **Proxy**
* **Redirect**
* **Headers**

By default, these sections are collapsed. Click a section to expand it.

For details on the fields on the **SSL** tab and the **Settings** tab, see [Connect to URL filter](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter).

## Save an OAuth client access token filter

You can use the **Save an OAuth Client Token** filter to save a token with a different token key.

This filter can fail if:

* The token has no token key
* The token has no application
* The token key is the same key that was already stored for the application
* There is a problem saving the token to persistent storage

Configure the following general settings for the **Save an OAuth Client Token** filter:

**Name**:
Enter a suitable name for this filter.

**Choose OAuth Token Key**:
Enter the message attribute to be used as the token key. Defaults to `${authentication.subject.id}`.

## Refresh an OAuth client access token filter

OAuth 2.0 client tokens are designed to be short lived and have an expiry time, however, tokens can be issued with refresh tokens. If a token has expired, and it has a refresh token, you can use the **Refresh an OAuth Client Access Token** filter to explicitly refresh the token. This filter looks up the token and checks for a refresh token. If it finds a refresh token, the filter sends an outbound refresh token request to the OAuth authorization server to obtain a new access token (and possibly a new refresh token).

Configure the following general settings for the **Refresh an OAuth Client Access Token** filter:

**Name**:
Enter a suitable name for this filter.

**Choose OAuth Token Key**:
Enter the message attribute to be used as the key to lookup the token. Defaults to `${authentication.subject.id}`.

**Optionally select a client credential profile**:
Select this option and click the browse button to select an OAuth client credential profile. This can be used if no preceding filter has set the application profile on the message board, or to override the existing application profile.

## Refresh token SSL and additional settings

You can configure SSL settings, such as trusted certificates, client certificates, SSL/TLS protocols, and ciphers on the **SSL**
tab.

The **Settings**
tab allows you to configure the following additional settings:

* **Retry**
* **Failure**
* **Proxy**
* **Redirect**
* **Headers**

By default, these sections are collapsed. Click a section to expand it.

For details on the fields on the **SSL** tab and the **Settings** tab, see [Connect to URL filter](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter).

## Delete an OAuth client access token filter

You can use the **Delete an OAuth Client Token** filter to explicitly delete an OAuth client access token, or refresh token, or both.

This filter requires the message attribute `oauth.client.application` to determine if the application has an access token, or refresh token, or both. The filter returns false if the required message attribute is not set or if there is a problem removing an access or refresh token from token storage.

Configure the following general setting for the **Delete an OAuth Client Token** filter:

**Name**:
Enter a suitable name for this filter.
