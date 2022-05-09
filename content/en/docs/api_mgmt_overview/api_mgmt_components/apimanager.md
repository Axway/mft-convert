{
"title": "Introduction to API Manager",
"linkTitle": "API Manager",
"weight":"15",
"date": "2020-04-14",
"description": "API Manager enables you to register APIs and manage their lifecycle from registration through publishing and retirement."
}

## API Manager features

The main API Manager features are:

* API registration - Adding APIs to the API Catalog. For details, see [API registration](#api-registration-and-lifecycle-management-in-api-manager).
* Partner organization management - API Manager includes partner-based management of API consumers that browse the API Catalog and client applications that use the APIs. Delegated partner administration enables partner organizations to manage their own API consumers, easing the management of large partners, or a large number of partners. A wide range of client application credentials are supported, including OAuth 2.0 and API keys.
* Policy management - API Manager enables you to apply authorization and quota policies to APIs at the partner and client application levels. Custom policies can also be developed in Policy Studio, and applied to APIs.
* API alerting - API Manager enables you to configure API, partner, policy and runtime events to generate alerts that trigger governance processes. For example, this includes sending an email notification or starting application workflows.
* API import and export - Registered APIs can be exported from API Manager and imported to another API Manager using a file-based package. This enables APIs to be promoted from a sandbox API group (where client applications are developed and tested) to the production API group. You can configure an API promotion policy to automate this process.

## API Manager tools

API Manager provides the following tools to enable you to virtualize and manage your APIs:

### API Manager web console

The API Manager web interface enables business or operational users (API owners) to easily register REST APIs and apply standard policies defined in the client registry to virtualize the APIs. It enables organizations and API consumers to consume APIs, browse the API Catalog, and monitor their API use. It also enables business or operational users (API administrators) to manage API clients and their consumption of APIs.

API Manager provides a role-based interface, in which API Manager users are assigned a role (for example, API developer, API consumer, API administrator, API owner, or organization administrator). The operations that a user can perform in API Manager depend on the role they are assigned. For example, a user assigned the API developer role can register and virtualize REST APIs.

API Manager is implemented as a web application that is hosted on the API Gateway. The default API Manager has Axway branding, and can be customized to use different branding. API Manager also has a management API that enables organizations to integrate with custom portals and other existing systems.

### API Gateway runtime

This is the runtime gateway that proxies the REST APIs registered in API Manager, and that enforces configured policies on client requests and responses. API Manager is a layered product running on API Gateway, and which provides all the underlying gateway capabilities. API Gateway is a prerequisite product for API Manager.

### API Catalog

This is the catalog of published APIs and their associated documentation that have been registered in API Manager. Client application developers can browse the API Catalog in API Manager and in API Portal. APIs can be tagged for classification and searching. The API Catalog is represented in Swagger format for tool integration.

### Client Registry

This is the repository of organizations and partners, API consumers, and client applications that consume the REST APIs. The Client Registry also contains the authentication credentials of the client applications, and authorization and quota policies defined at the organization and application level. The Client Registry is persisted in an Apache Cassandra backing store.

Policy Studio includes API management filters that provide read-only access to the Client Registry. These enable policy developers to develop policies that leverage the information in the Client Registry. Write access to the Client Registry must be performed using the API Portal API because data consistency checks are required.

### API Manager REST API

This [REST-based API](http://apidocs.axway.com/swagger-ui-NEW/index.html?productname=apimanager&productversion=7.7.0&filename=api-manager-V_1_3-oas3.json) provides the underlying capabilities supporting API Manager. This API enables the management of the data in the Client Registry and the browsing of registered APIs, with API documentation returned in Swagger format. The API Manager API enables the development of custom API consumer portals and integration with external partner management systems.

### API Manager CLI

Automation, easy integration of applications into existing processes, and simple management by users is an important aspect of the API management solution. The [API Manager CLI](/docs/api_mgmt_overview/api_mgmt_components/tools/#api-manager-cli) uses the REST API and allows the automatic registration of APIs by way of the command line, for example in the development environment and in the integration into a CI/CD pipeline.

## API management architecture

The following diagram shows a simplified API management architecture:

![API management simplified architecture](/Images/docbook/images/api_mgmt/api_mgmt_architecture_simple.png)

## API registration and lifecycle management in API Manager

API management focuses on registering existing REST APIs, and managing their consumption by customers and partners to support their business objectives. REST APIs are registered using the API Manager web console. REST APIs are managed directly by API Manager using authentication, authorization, and quota policies defined in the client registry. API administrators can use API Manager to manage API consumption, and API consumers can consume the virtualized APIs using API Manager, or using a customized self-service API Portal.

API Manager enables you to register APIs and manage their lifecycle from registration through publishing and retirement. Delegated API registration enables different teams of API owners to register and test their own APIs in isolation prior to publishing to other organizations in the API Catalog.

API management is performed by an *API owner* (a technical business or IT operational role). Registration of REST APIs in API Manager, and application of policies to those APIs, is a configuration task rather than a development task. It can be performed on a running API Gateway in a production environment. This approach enables you to manage and promote APIs more dynamically, more rapidly, and with less overhead than typical IT projects.

API Manager provides a web-based interface that enables API owners to register existing back-end REST APIs, apply standard policies, and virtualize them on API Gateway as public front-end APIs. The APIs are immediately available for management in API Manager, and for consumption in API Manager, or in a self-service API Portal.

In API Manager, the lifecycle of an API includes the following states:

1. Unpublished - The API is registered and tested in isolation in an API owner organization. The API is available to the API administrator and API owners who are members of that organization. The API can be edited, or be moved to the published state, or deleted. These actions can only be performed by the API owner or the API administrator.

    Unpublished APIs are displayed in the API Catalog view to users in the same organization. The users in this organization are the API owners and developers on the same team working on these APIs. However, an unpublished API is not displayed to users in other organizations. The API must first be published, and then that organization must be authorized to access the API. The API is then displayed in the API Catalog for users in that organization.

    All APIs (published and unpublished) are displayed in the API Catalog for the API administrator.
2. Published - When an API is ready to be consumed by other organizations, it is published in the API Catalog by the API owner. The API administrator must then approve the API as the final step to publish to other organizations in the API Catalog. When the API is published, the API administrator can authorize other organizations to access the API. This displays the API in API Manager and API Portal to API consumers who are members of the authorized organization.

    When an API is published, only the API administrator can make changes. The published API can only be deprecated or unpublished, and cannot be deleted. Unpublishing an API stops client applications in other organizations using the API. A published API cannot be edited, and must first be unpublished. However, the API administrator can edit the API documentation of a published API. This allows changes in the API documentation without impacting the API availability.
3. Deprecated - The published API in API Manager is flagged with a date when it will be unpublished in the API Catalog, and is no longer available to client applications in other organizations. The retirement date is displayed to API consumers in API Manager and API Portal. Retiring the API is achieved by unpublishing the API in the API Catalog. Only a published API can be deprecated and unpublished. When the API is unpublished, it is then available for API owners to edit.

    When an API is deprecated, it is still in the published state, and clients can continue to discover and use the API. This gives API consumers time to port their existing applications to adopt a newer version of the API. You can undeprecate an API by selecting the undeprecate option, which removes the retirement date flag in the API Catalog.

The following are the very high level tasks you can perform in API Manager.

### 1 - API registration

If the back-end API is an existing REST API, an API owner uses API Manager to register the APIs and apply standard policies. The registered APIs are virtualized by API Gateway, which protects the back-end services, and makes the APIs available for consumption. This describes the typical API management approach.

For more details, see the following:

* [Register REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/)
* [Virtualize REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/)

If the existing back-end API is not a REST API, or if custom policies are required, a policy developer uses Policy Studio to create a new API (for example, for SOAP to REST, or cloud-based applications). APIs developed in Policy Studio are then imported as back-end APIs in API Manager.

### 2 - API administration

The API administrator manages and monitors the APIs at runtime using API Manager. For example, this includes all organizations and users registered to log into API Manager, client applications and their authentication credentials, and authorization and quota policies. The API administrator manages who can consume, what they can consume, and how much can they consume. For example, which business partners are permitted to consume which APIs, and what are their quota levels.

For more details, see [Manage access to APIs](/docs/apim_administration/apimgr_admin/api_mgmt_admin/).

### 3 - API consumption

API consumers can self-register in API Manager or API Portal. They browse and consume the managed APIs provided by API Gateway, and use them to develop and test their applications. The organization administrators in named organizations manage the applications and API consumer users.

For example, API consumers might be internal developers or external business partners. They can log into API Manager or API Portal, and browse APIs and their associated documentation for consumption. They can then develop and test client applications that use these APIs. In this way, API Manager builds a community around the APIs, enabling organizations and consumers to register themselves, and to create and manage their own applications.

For more details, see [Consume APIs](/docs/apim_administration/apimgr_admin/api_mgmt_consume/).