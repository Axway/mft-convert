{
"title": "Consume APIs in API Manager",
  "linkTitle": "Consume APIs",
  "weight": "80",
  "date": "2019-09-17",
  "description": "Consume APIs and manage client applications."
}

## Consume APIs

API consumers can consume the managed APIs exposed by the API Gateway.

### Browse and retrieve APIs

You can use the **API Catalog** view to browse and retrieve APIs in API Manager.

### Retrieve APIs using tags

When tags have been added in API Manager by the API administrator, you can use them to browse and retrieve APIs.

You can click the **Tags** button in the API Manager toolbar to select tags to filter, or you can filter tags manually by entering the `tag:` prefix followed by the tag value in the filter box (for example, `tag:Swagger`). You can also filter multiple tags by entering a comma-separated list without any spaces between values (for example, `tag:REST,R+D`).

## Manage client applications

You can use the **Clients** tab to manage client applications (for example, create, update, or remove client applications that invoke specific APIs). When an application is created, API administrators can also set authentication, quota, and sharing settings on the appropriate tab.

{{< alert title="Note" color="primary" >}}The API administrator must first specify the APIs that an organization is allowed to access before any of its client applications can have access to them. In API Manager, you can only add APIs to an application when they have been added to the organization. For more details, see [Manage access to APIs](/docs/apim_administration/apimgr_admin/api_mgmt_admin/).{{< /alert >}}

### Create an application

To create an application, perform the following steps:

1. Click **New application** in the toolbar, and configure the following general fields:
    * **Image**: Click to add a graphical image for the application (for example, .png, `.gif`, or `.jpeg` file).
    * **Application name**: Enter the name of the application. This field is required.
    * **Organization**: Enter the name of the organization that the application belongs to. This field is required. The choice of organization determines which APIs are available to the application.
    * **Enabled**: Select whether the application is enabled. Applications are enabled by default.
2. Configure the following additional attributes:
    * **Email**: Enter an email address for the application.
    * **Phone**: Enter a phone number for the application.
    * **Description**: Enter a short description of the application.
3. Click **Add API**
    to select the APIs and methods used by the application. You can add multiple APIs for an application.
4. Click **Create** in the toolbar.

### Edit an application

When applications have been created, you can click an application name in the **Managing applications** screen to edit its existing settings on the **Application** tab. API administrators can also configure additional settings on the following tabs.

#### Authentication

The following settings are available on the **Authentication** tab:

* **API Keys**: Click **New API Key** to create an API key for the application. API keys are enabled by default. Click **Show Secret** to obtain the associated secret key. You can also specify **JavaScript Origins** to allow the application to run on specific protocols or domains (for example, `https://my_test_url.example.com`) for Cross Origins Resource Sharing (CORS). You can enter one domain per line or `*` to allow all domains. For more details, see [Virtualize REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/).
* **OAuth Credentials**: Click **New client ID** to create a client ID for the application, and enter the following settings in the dialog:
    * **Application Type**: Applications set to **Confidential** must always send the generated secret along with their `OAuth-Authorization` request. Applications set to **Public** may ommit the secret, when not using the `client_credentials` grant type. Defaults to **Confidential**.
    * **Redirect URLs**: You can enter optional redirect URLs for the application (one URL per line). The application can then redirect users only to the specified URLs, which helps prevent attacks.
    * **X.509 Certificate**: You can paste the contents of a Base64-encoded public X.509 certificate for the application. This certificate is used to verify the signature of JWT tokens and SAML assertions used in the appropriate OAuth grant types.

    Newly created client IDs are enabled by default. You can click **Show Secret** to obtain the associated secret key. You can specify **JavaScript Origins** to allow the application to run on specific protocols or domains for CORS.
* **External Credentials**: Click **New client ID**, and enter the external client ID for the application. Client IDs are enabled by default. You can specify **JavaScript Origins** to allow the application to run on specific protocols or domains for CORS.
* **Application Scopes**: Click **Add scope**, and select one of the following scopes to manage application access to protected resources:
    * **resource.READ**: Read-only access to the resource.
    * **resource.WRITE**: Write access to the resource.
    * **openid**: OpenID Connect access to the resource.
    * **Add New Scope**: Enter a custom scope name to manage access to the resource.

    These settings are displayed only when **Enable application scopes** is selected in **Settings > API Manager settings > General settings**. Any new scopes added in an API Key or OAuth security device are also displayed.

#### Quota

The **Quota** tab enables API administrators to override the application-default quota and specify application-specific quota rules. For more details, see [Manage access to APIs](/docs/apim_administration/apimgr_admin/api_mgmt_admin/).

#### Sharing

The **Sharing** tab enables API administrators to manage access to the application for specified users. Click **Add User**, select an existing user name from the list, and select whether the user can **View** or **Manage** the application. The default is **View**.

You can add multiple existing users. For details on creating users, see [Manage access to APIs](/docs/apim_administration/apimgr_admin/api_mgmt_admin/). To remove user access to the application, select the user name, and click **Remove**.

## Manage the client application lifecycle

When you have created client applications, you can select them in the **Applications** view, click **Manage selected**, and chose one of the following options:

* **Delete selected item(s)**: Permanently deletes the selected applications from the client registry.
* **Disable**: Disables the selected applications in the client registry. Applications are enabled by default.
* **Enable**: Enables the selected applications that have previously been disabled in the client registry.
* **Export**: Exports a copy of the selected applications to your chosen directory. The APIs are exported in JSON format in a default `app-export.dat` file. You can specify the following options in the dialog:
    * Specify a different file name
    * Select whether to encrypt the application data
    * Add a password
    * Export API keys, OAuth credentials, and quota overrides

    You can then import this file into API Manager as required (for example, when promoting between environments).

You can click **Export all** in the menu bar at the top to export all client applications in the client registry. You can click **Import** to import previously exported applications in the selected `.dat` file.

{{< alert title="Note" color="primary" >}}Importing a `.dat` file, which contains a large number of applications, might take some time to complete. To avoid importing errors, consider increasing the default value of **Transaction Timeout** set in **General settings** within Policy Studio. For more information, see [General settings in Policy Studio](/docs/apim_reference/general_settings/#general-settings).{{< /alert >}}
