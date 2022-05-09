---
title: API Gateway as an OAuth 2.0 server
linkTitle: API Gateway as an OAuth 2.0 server
weight: 30
date: 2019-11-18
description:  Learn about using API Gateway as an OAuth server.
---

OAuth 2.0 is an open standard for authorization that enables client applications to access server resources on behalf of a specific *resource owner*. OAuth also enables resource owners (end users) to authorize limited third-party access to their server resources without sharing their credentials. For example, a Gmail user could allow LinkedIn or Flickr to have access to their list of contacts without sharing their Gmail user name and password.

The API Gateway can be used as an *authorization server*
and as a *resource server*. An authorization server issues tokens to client applications on behalf of a resource owner for use in authenticating subsequent API calls to the resource server. The resource server hosts the protected resources, and can accept or respond to protected resource requests using access tokens.

{{< alert title="Note" color="primary" >}}This guide assumes that you are familiar with the terms and concepts described in the [OAuth 2.0 Authorization Framework](http://tools.ietf.org/html/rfc6749).{{< /alert >}}

## API Gateway OAuth concepts

The API Gateway uses the following definitions of basic OAuth 2.0 terms:

* Resource owner – An entity capable of granting access to a protected resource. When the resource owner is a person, it is referred to as an end user.
* Resource server – The server hosting the protected resources, and which is capable of accepting and responding to protected resource requests using access tokens. In this case, the API Gateway acts as a gateway implementing the resource server that sits in front of the protected resources.
* Client application – A client application making protected requests on behalf of the resource owner and with its authorization.
* Authorization server – The server issuing access tokens to the client application after successfully authenticating the resource owner and obtaining authorization. In this case, the API Gateway acts both as the authorization server and as the resource server.
* Scope – Used to control access to the resource owner's data when requested by a client application. You can validate the OAuth scopes in the incoming message against the scopes registered in the API Gateway. An example scope is `userinfo/readonly`.

## OAuth server example workflow

Assume that you are using an image printing website such as Canon to print some of your photos. You also have some photos on your Flickr account that you would like to print. However, you currently must download all these locally, and then upload them again to the printing website, which is inconvenient. You would like to be able to sign into Flickr from your Canon printing profile, and print your photos directly.

This problem can be solved using the example OAuth 2.0 web server flow shown in the following diagram:

![OAuth Workflow](/Images/OAuth/APIgw_Oauth_ex_client_workfl.png)

Out of band, the Canon printing client application preregisters with Flickr and obtains a client ID and secret. The client application registers a callback URL to receive the authorization code from Flickr when you, as resource owner, allow Canon to access the photos from Flickr. The printing application has also requested access to an API named `/flickr/photos`, which has an OAuth scope of `photos`.

The steps in the diagram are described as follows:

1. You are using a mobile phone and are signed into the Canon image printing website. You click to print Flickr photos. The Canon client application redirects you to the Flickr OAuth authorization server. You must already have a Flickr account.
2. You log in to your Flickr account, and the Flickr authorization server asks you "Do you want to allow the Canon printing application to access your photos?" You click **Yes**
    to authorize.
3. When successful, the printing application receives an authorization code at the callback URL that was preregistered out of band.

    {{< alert title="Note" color="primary" >}}You have not shared your Flickr user name and password with the printing application. At this point, you as resource owner are no longer involved in the process.{{< /alert >}}

4. The client application gets the authorization code, and must exchange this short-lived code for an access token. The client application sends another request to the authorization server, saying it has a code that proves the user has authorized it to access their photos, and now issues the access token to be sent on to the API (resource server). The authorization server verifies the authorization code and returns an access token.
5. The client application sends the access token to the API (resource server), and receives the photos as requested.

## API Gateway OAuth server features

API Gateway provides the following features to support OAuth 2.0:

* Web-based client application registration
* Generation of authorization codes, access tokens, and refresh tokens
* Support for the following OAuth authentication flows:
    * Authorization code grant (web server)
    * Implicit grant (user agent)
    * Resource owner password credentials
    * Client credentials grant
    * JWT
    * Refresh token
    * Revoke token
    * Token information service
    * SAML assertion

    For more information on the supported flows, see [OAuth 2.0 authentication flows](/docs/apim_policydev/apigw_oauth/oauth_flows).
* Sample client applications for all supported flows

The following diagram shows the roles of the API Gateway as an OAuth 2.0 resource server and authorization server:

![OAuth roles](/Images/OAuth/APIgw_OAuth_Roles1.png)
