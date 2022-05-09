---
title: API Manager as an OAuth 2.0 resource server
linkTitle: API Manager as an OAuth 2.0 resource server
weight: 80
date: 2019-11-28
description: Use API Manager as an OAuth resource server. Learn how to protect APIs with OAuth in API Manager, and how to manage OAuth scopes, OAuth authorizations, and client applications in API Manager.
---

## Protect APIs with OAuth

API Manager provides a web-based interface that enables API owners to register existing back-end REST APIs, apply standard policies, and virtualize them on API Gateway as public front-end APIs. When you virtualize a REST API, you can configure it with security devices, which provide prebuilt authentication and authorization mechanisms for the REST API. This enables you to control the authentication and authorization mechanisms that are supported for the API.

The security devices supported for the inbound request (between the client and API Gateway) include OAuth and OAuth (External). This enables you to protect the virtualized API using OAuth where the OAuth provider is API Gateway, or using OAuth with an external OAuth provider.

In addition to the prebuilt OAuth security devices, you can also create custom security profiles with multiple security devices, which can be applied as per-method overrides. This enables you to control the authentication and authorization mechanisms that are supported for the API not just at the API level, but also at the API method level.

For more information on virtualizing APIs in API Manager and protecting those APIs with OAuth, see [Virtualize REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/).

## Scopes in API Manager

API Manager supports an explicit API model with scopes assigned to APIs during API registration.

When a client application is authorized (granted access) to use an API, then all the API's scopes are associated with (available to) that application. When the client application makes an authorization request, it includes the scopes it is requesting in the request. In the authorization code flow, these scopes are displayed to the resource owner and the resource owner can select which scopes are granted to the client application.

### Enable global scopes in API Manager

To enable OAuth scopes at the level of the client application, in API Manager select **Settings > API Manager Settings > General settings > Enable application scopes**. This allows API administrators to create application-level scopes to permit access to OAuth resources that are not covered by API-level scopes. This setting can be used if you are using the API Gateway global scopes model. For more information, see [Scopes in API Gateway](/docs/apim_policydev/apigw_oauth/gw_oauth_resource_server/#scopes-in-api-gateway).

When you select the **Enable application scopes** setting, you can configure the scopes that a client application can access. When editing a client application in API Manager, select the **Authentication** tab. In the **Application Scopes** section you can specify scopes as free-form text or select a scope from a list of known configured scopes. You can also select a scope as a default scope for the client application.

## Authorization management in API Manager

During the OAuth authorization process, the OAuth authorization server asks a resource owner to authorize access to a given set of scopes requested by the client application. If the resource owner accepts the authorization request, the client application can interact with the OAuth authorization server to obtain an access token and subsequently access the resource owner's protected resources.

OAuth authorizations are stored in the Authorizations table in the OAuth KPS collection. This is a hidden KPS collection that is not visible in Policy Studio by default. For more information on viewing hidden KPS collections in Policy Studio, see the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).

There is only one authorization record per client application/resource owner. When a client application requests authorization and a record already exists for the client application/resource owner, the resource owner is only asked to grant access to any additional scopes requested, and not to the scopes previously authorized. If the resource owner grants access, the authorization record is updated to include the additional scopes.

There are several ways to view the OAuth authorizations:

* API Manager – See [View and revoke OAuth authorizations in API Manager](#Revoke).
* API Manager REST API – For more information, see the [Product APIs page](https://docs.axway.com/category/api) on the Axway Documentation portal.
* `kpsadmin` tool – For more information, see the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).

### View and revoke OAuth authorizations in API Manager{#Revoke}

API Manager enables you to view and revoke OAuth authorizations that have been granted to client applications by resource owners. This enables you to manage all client application authorizations to access OAuth-protected APIs. This also means that resource owners do not need to reauthorize application requests.

To view the OAuth authorizations in API Manager, select **Policy Management > OAuth Authorizations**.

![Manage OAuth authorizations](/Images/OAuth/oauth_authorizations.png)

For each client application, the following details are displayed:

* Application – The name of the client application
* Owner – The resource owner that granted the authorization
* Scopes – The scopes that the resource owner is allowing this client application to access

To revoke an authorization, select the check box next to the authorization, and select **Manage selected > Delete Selected item**.

When client applications are authorized to access OAuth-protected APIs, they are issued with an access token and optionally a refresh token. Revoking an OAuth authorization means that the access and refresh tokens that the client application has are no longer valid.

## Register and manage client applications in API Manager

In the API Manager web-based interface you can use the **Client Registry > Applications** tab to create and edit client applications and give them access to the APIs virtualized in API Manager. When an application is created, you can set authentication, quota, and sharing settings on the appropriate tab.

{{< alert title="Note" color="primary" >}}The API administrator must first specify the APIs that an organization is allowed to access before any of its client applications can have access to them. In API Manager, you can only add APIs to an application when they have been added to the organization.{{< /alert >}}

To register a new client application, click the **New application** button.

To edit an existing client application, click the application name in the list of applications. You can edit the following:

* On the **Application** tab, you can add APIs that the client application can access in the API ACCESS section.
* On the **Authentication** tab, you can add API keys, OAuth credentials, and external OAuth credentials for the application.

You can also add OAuth scopes at the client application level if you have enabled global scopes as described in [Enable global scopes in API Manager](#enable-global-scopes-in-api-manager).

* On the **Quota** tab, you can override the application-default quota and specify application-specific quota rules.
* On the **Sharing** tab, you can manage access to the application for specified users.

![Manage client applications in API Manager](/Images/OAuth/api_mgmt_app.png)

For more information on registering and managing client applications in API Manager, see [Manage client applications in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_consume/#manage-client-applications).
