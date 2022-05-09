---
title: OAuth and OpenID Connect concepts
linkTitle: Concepts
weight: 10
date: 2019-11-18
description: Understand the main concepts involved in OAuth and OpenID Connect.
---

OAuth 2.0 is a *delegation protocol* that is useful for conveying *authorization decisions* across a network of web-enabled applications and APIs. OAuth 2.0 is not an authentication protocol; however, OpenID Connect can be used along with OAuth to create an authentication and identity protocol on top of this delegation and authorization protocol.

## OAuth 2.0

OAuth 2.0 is specified in the [OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749). OAuth can be used to provide:

* Delegated access
* Reduction of password sharing between users and third-parties
* Revocation of access

For example, when users share their credentials with a third-party application, the only way to revoke access from that application is for the user to change their password. However, this means that access from all other applications is also revoked. With OAuth, users can revoke access from specific applications without breaking other applications that should be allowed to continue to act on their behalf.

OAuth achieves this by introducing an authorization layer and separating the role of the client from that of the resource owner. OAuth defines four primary roles:

* Resource owner (RO): The entity that is in control of the data exposed by an API (for example, an end user).
* Client: The mobile application, web site, and so on, that wants to access data on behalf of the resource owner.
* Authorization server (AS): The Security Token Service (STS) or OAuth server that issues tokens.
* Resource server (RS): The service that exposes the data (for example, an API).

The client requests access to resources controlled by the resource owner and hosted by the resource server, and is issued a different set of credentials than those of the resource owner. Instead of using the resource owner's credentials to access protected resources, the client obtains an access token - a string denoting a specific scope, lifetime, and so on. Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner. The client uses the access token to access the protected resources hosted by the resource server.

OAuth defines two kinds of tokens:

* Access tokens: These tokens are presented by a client to the resource server (for example, an API), to get access to a protected resource.
* Refresh tokens: These are used by the client to get a new access token from the authorization server (for example, when the access token expires).

OAuth tokens can include a scope. Scopes are like permissions or rights that a resource owner delegates to a client, so that they can perform certain actions on their behalf. A client can request specific rights, but a user might only grant a subset, or might grant others that were not requested. The OAuth specification does not define specific scopes, meaning that you can use any string to represent an OAuth scope.

OAuth defines several different flows or message exchange patterns. The most commonly used is the authorization code (web server) flow. For more details on this flow and the other flows that API Gateway supports, see [OAuth 2.0 authentication flows](/docs/apim_policydev/apigw_oauth/oauth_flows).

## OpenID Connect 1.0

OpenID Connect is specified in the [OpenID Connect 1.0 specification](http://openid.net/specs/openid-connect-core-1_0.html). OpenID Connect builds on the OAuth protocol and defines an interoperable way to use OAuth 2.0 to perform user authentication. OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It enables clients to verify the identity of the user based on the authentication performed by an authorization server, as well as to obtain basic profile information about the user.

OpenID Connect defines the following roles:

* Relying party (RP): An OAuth client that supports OpenID Connect. The mobile application, web site, and so on, that wants to access data on behalf of the resource owner.
* OpenID provider (OP): An OAuth authorization server that is capable of authenticating the user and providing claims to a relying party about the authentication event and the user.

OpenID Connect defines a new kind of token, the ID token. The OpenID Connect ID token is a signed JSON Web Token (JWT) that is given to the client application alongside the regular OAuth access token. The ID token contains a set of *claims* about the authentication session, including an identifier for the user (`sub`), an identifier for issuer of the token (`iss`), and the identifier of the client for which this token was created (`aud`). Since the format of the ID token is known by the client, it can parse the content of the token directly.

In addition to the claims in the ID token, OpenID Connect defines a standard protected resource (the UserInfo endpoint) that contains claims about the current user. OpenID Connect defines a set of standardized OAuth scopes that map to claims (`profile`, `email`, `phone`, and `address`). If the end user authorizes the client to access these scopes, the OP releases the associated data (claims) to the client when the client calls the UserInfo endpoint. OpenID Connect also defines a special `openid` scope that switches the OAuth server into OpenID Connect mode.
