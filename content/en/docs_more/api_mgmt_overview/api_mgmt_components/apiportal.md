{
"title": "Introduction to API Portal",
"linkTitle": "API Portal",
"weight":"25",
"date": "2020-04-14",
"description": "API Portal is a self-service web-based portal that enables API consumers to consume APIs that you have exposed using API Manager."
}

API consumers can register and manage their user profile, register applications, manage application credentials, browse front-end APIs and supporting documentation, monitor application API usage, and access blogs, forums, and so on.

API Portal acts as a view to API Manager and communicates over its REST API. This includes all functionalities related to APIs. API manager handles the following operations:

* User login, logout, and user profiles
* API Catalog view
* Application management
* Monitoring

This means that there is a single place for API management - API manager - and API Portal automatically reflects all changes.
For example, if a new API is registered, it is automatically available in API Portal. If an application is created in API Portal, it is visible in the API manager and you might trigger an [alert](/docs/apim_administration/apimgr_admin/api_mgmt_alerts/) for this event.

This does not have to be a 1:1 relationship, as API portal can connect to several API managers at the same time. For example, you can connect API Portal to the development, pre-production, and production stage at the same time to get a complete overview of the available APIs. Visibility can be restricted accordingly.

API Portal is implemented as a standalone CMS-based portal that you can operate using Axway standard branding and functionality, or customize and extend to meet your specific needs and those of your target customers. You can deploy the web-based API Portal separately from the API Gateway and API Manager with a dedicated web interface to limit potential security breaches.

The following shows an example API Portal Catalog view.

![API-Portal Catalog view](/Images/api_mgmt_overview/api-portal-catalog-overview.png)

The following shows an example API Portal single API view.

![API-Portal API view](/Images/api_mgmt_overview/api-portal-catalog-detail.png)

## API Portal features

The following are the main features of API Portal. See [API Portal overview](/docs/apim_administration/apiportal_admin/apip_overview/) for more details of API Portal key capabilities.

### Developer self-registration and profile management

Client application developers can self-register and manage their profiles.

### Browse and test APIs in the API Catalog

The API Catalog contains the APIs that have been registered in API Manager and are available for use by client application developers. They can browse these APIs and their associated documentation, and invoke APIs using the built-in test capability.

### Create and manage applications

Application developers can register their applications that will use the APIs, and obtain API key or OAuth credentials for the application. They can also monitor their application's use of APIs using graphical data sourced from your API Manager metrics database.

### Content management, blogs, and discussion forums

API Portal runs on Joomla!™, an open source CMS platform for developing and deploying web sites. You can use the content management capabilities of Joomla! to store additional content, such as PDF documents and video, for display in API Portal. Joomla! also provides plugins for third-party blog and discussion forums.

### Customizable to provide a branded experience

You can deploy API Portal with no customization, using the out-of-the-box Axway branding, which is suitable for internal-facing API deployments. For external-facing API deployments, you can customize API Portal to provide a branded developer portal experience. You can customize API Portal using Joomla configuration screens (upgradeable), or by editing the API Portal PHP source code (not upgradeable).
