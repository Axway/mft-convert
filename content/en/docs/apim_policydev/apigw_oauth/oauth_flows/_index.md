---
title: API Gateway OAuth 2.0 authentication flows
linkTitle: OAuth 2.0 authentication flows
weight: 20
date: 2019-11-18
description: Learn about the supported OAuth 2.0 flows in detail, and how to run sample scripts demonstrating the flows.
---

API Gateway can use the OAuth 2.0 protocol for authentication and authorization. API Gateway can act as an OAuth 2.0 authorization server and supports several OAuth 2.0 flows that cover common web server, JavaScript, device, installed application, and server-to-server scenarios.

API Gateway includes sample Jython scripts for all supported OAuth flows. For example, to run a sample script for the implicit grant flow:

```
cd INSTALL_DIR/samples/scripts/oauth
sh run.sh oauth/implicit_grant.py
```

In addition to the following flows, API Gateway also supports:

* Refresh token flow – After the client application has been authorized for access, it can use a refresh token to get a new access token. This is only done after the consumer already has received an access token using the authorization code grant or resource owner password credentials flow.
* SAML assertion flow – The OAuth 2.0 **Access Token using SAML Assertion** filter enables an OAuth client to request an access token using a SAML assertion. This flow is used when a client wishes to utilize an existing trust relationship, expressed through the semantics of the SAML assertion, without a direct user approval step at the authorization server.
