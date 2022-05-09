{
"title": "Configure API Manager settings in Policy Studio",
  "linkTitle": "Configure settings in Policy Studio",
  "weight": "20",
  "date": "2019-09-17",
  "description": "Configure settings that apply to API Manager and the underlying API Gateway in Policy Studio."
}
Policy Studio enables you to configure a range of settings that apply to API Manager and the underlying API Gateway. This topic describes how to create a Policy Studio project with API Manager configuration, and how to configure each of the API Manager settings.

## Create a Policy Studio project with API Manager configuration

To create a Policy Studio project with API Manager configuration, perform the following steps:

1. Ensure that your API Gateway installation has already been configured for API Manager. For more details, see [Enable API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_config/#enable-api-manager).
2. Create a project from one of the following:

   * API Gateway instance
   * API Gateway configuration directory
   * `.fed`, `.pol`, or `.env` file

## Configure API Manager server settings

In the Policy Studio tree, select **Server Settings** > **API Manager** to configure the settings described in this section.

The following settings enable you to change the API Manager settings:

Default Administrator Name
: Enter the default API administrator user name. This sets the default administrator user name for logging in to API Manager. The default value is `apiadmin`.

Default Administrator Password
: Enter the default API administrator password. This sets the default administrator password for logging in to API Manager.

Default Administrator Email
: Enter the default API administrator email. This sets the default administrator email for API Manager. The default value is `apiadmin@localhost`.

Administrator Distinguished Name (DN)
: Enter the API administrator DN.

Community Organization Name
: Enter the organization name for the 'Community' organization. The default is `Community`.

{{< alert title="Note" color="primary" >}}The default API administrator user name and password set in Policy Studio are used only when configuring the initial connection to Apache Cassandra. After Cassandra has been configured (for example, after you have deployed this configuration to API Gateway) changing the credentials in Policy Studio has no effect, and you must use API Manager to change the administrator credentials. For more information, see [Account settings](/docs/apim_reference/api_mgmt_config_web/#account-settings).{{< /alert >}}

### Alerts

The **Alerts** settings enable you to configure runtime alerts, which call specified policies to handle the alert event. For example, the policy might send an email to an interested party, or forward the alert to an external notification system. Sample policies are provided as a starting point for custom development.

You can enable or disable alerts in the API Manager web interface. You can change the policy that is executed when an alert is generated on this screen. For more details, see [Configure API management alerts](/docs/apim_administration/apimgr_admin/api_mgmt_alerts/).

### API Listeners

The **API Listeners** settings enable you to configure API Gateway listeners to service API Manager-registered APIs. Defaults to `Portal Listener`.

This window only displays listeners that do not have a relative path resolver on the `/` relative path. For more details on API Gateway listeners, relative paths, and resolvers, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/).

### API Promotion

The **API Promotion** settings enable you to configure an optional policy that is invoked when APIs registered in API Manager are promoted between environments (for example, from a test or sandbox environment to a live production environment).

To select a promotion policy, click the browse button on the right, and select a policy that you have already created. By default, no API promotion policy is selected. For more details, see [Promote managed APIs](/docs/apim_administration/apimgr_admin/api_mgmt_promote/).

### API Connectors

The **API Connectors** settings enable you to configure client authentication profiles to use with specific API connectors and plugins. For example, this includes connecting to Cloud APIs such as Salesforce.com and Google. A preconfigured plugin for Salesforce.com APIs is provided by default.

For more details, see [Configure cloud application connectors](/docs/apim_administration/apimgr_admin/api_mgmt_connector/).

### Identity Provider

The **Identity Provider** settings enable you to integrate API Manager with a wide range of external user repositories. For example, this includes third-party identity providers such as Apache Directory, OpenLDAP, Microsoft Active Directory, and so on. To enable integration, select **Use external identity provider**, and configure the following set of custom policies:

**Account authentication policy**
: Click the browse button, and select the required authentication policy that is invoked whenever a user tries to log in to API Manager. This setting is mandatory.

**Account information policy**
: Click the browse button, and select the required information policy that is invoked on first login to seed the user profile in API Manager. This setting is mandatory.

**Account creation success (optional)**
: Click the browse button, and select an optional policy that is invoked when a new user has been registered with API Manager.

**Account creation failure (optional)**
: Click the browse button, and select an optional policy that is invoked when an attempt to register a new account with API Manager has failed.

API Manager provides sample external identity provider configuration. For more details, see [Configure external LDAP identity providers](/docs/apim_administration/apimgr_admin/api_mgmt_config_external/).

The **Identity Provider** settings are used only to configure integration of API Manager with external user repositories. All other API Manager data is stored using a Key Property Store (KPS) in an Apache Cassandra cluster.

### Monitoring

The **Monitoring** settings allow you to configure monitoring metrics in API Manager:

**Enable monitoring**
: Select whether to enable monitoring metrics displayed on the **Monitoring** tab in API Manager. Monitoring is enabled by default.

**Use the following database**
: Click the browse button to configure the connection to the database that stores the monitoring metrics.

For more details on monitoring, see [Monitor APIs and applications in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_monitor/).

### OAuth Outbound Credentials

The **OAuth Outbound Credentials** setting enables you to configure optional client credentials for use with OAuth outbound authentication. These enable clients to request an OAuth access token using only their client credentials with the authorization specified in the header. By default, no credentials are configured.

### OAuth Token Information Policies

The **OAuth Token Information Policies** setting enables you to configure optional policies used by external OAuth security devices in API Manager. These include custom policies used to obtain and extract token information from external OAuth providers. By default, no policies are configured.

### OAuth Token Stores

The **OAuth Token Stores** settings enable you to configure OAuth token stores for the OAuth security devices used by API Manager-registered APIs. Click **Add** to configure an OAuth access token store. To add a store, right-click **Access Token Stores**, and select **Add Access Token Store**. Defaults to **OAuth Access Token Cache**.

### Quota Settings

The **Quota Settings** enable you to configure how quota information is stored. Quotas enable you to manage the maximum message traffic rate that can be sent by applications to APIs. For more details on configure quotas in API Manager, see [Manage access to APIs](/docs/apim_administration/apimgr_admin/api_mgmt_admin/).

You can configure the following settings in Policy Studio:

**Send warning if API usage reaches**
: Enter the **% of System Quota** and **% of Application Quota** that must be reached before warnings are sent to the API consumer or client application as response headers. Both API usage values default to `80` per cent.

When API usage reaches the defined % value, the warning is sent with the following header:

```
"X-Rate-Limit":"[\{\"window\":<remaining-time>,\"type\":\"throttle\",\"remaining\":<remaining-calls>}]"
```

When API usage exceeds the defined % value, the warning is sent with the following header:

```
"HTTP-Status":"429 - Too many requests"

"Retry-After":"<value in seconds when the quota windows opens again for traffic>", for example: "Retry-After": "28"
```

To notify an API administrator or trigger other internal processes you can use the corresponding quota alerts. For more details, see [Alerts](#alerts).

**Where to store quota data**
: Select **In external storage** or **In memory only**. This setting defaults to **In external storage**, and to keep the quota in memory only if the time window is below 30 seconds. In this case, if the API administrator configures a quota in API Manager with a time window below 30 seconds, the data is stored in memory instead of in external storage. Alternatively, to never use external storage, select **In memory only** to store data in memory in all cases.

If you select **In external storage**, you must specify an external storage mechanism:

* **Automatic (adapt to KPS storage configuration)**: The data is stored externally as configured in the Key Property Store (KPS). This is the default option.
* **Use database**: To store your data in a relational database, select this option, and specify the database connection that you want to use in **Environment Configuration** > **External Connections** > **Database Connections**.
* **Use Cassandra**: To store your data in an Apache Cassandra database, select this option.
* **Cassandra consistency levels**: When **Use Cassandra** is selected, you can configure **Read** and **Write** consistency levels for the Cassandra database. These settings control how up-to-date and synchronized a row of data is on all of its replicas. For high availability, you must ensure that the Cassandra read and write consistency levels are both set to `QUORUM`.

For more details on consistency levels, see [How is the consistency level configured](http://docs.datastax.com/en/archived/cassandra/2.2/cassandra/dml/dmlConfigConsistency.html).

{{< alert title="Note" color="primary" >}}Quota data is not shared for those quotas created in API Manager with a time window less than the value configured in Policy Studio, irrespective of the storage selected. This could impact on throttling in an HA environment, where multiple API Gateways are servicing requests and contributing to total message counts.{{< /alert >}}

### Inbound Security Policies

The **Inbound Security Policies** settings enable you to configure the custom security policies that can be applied to APIs registered in API Manager. These policies enable you to perform custom policy-based authentication on front-end APIs.

API Manager provides a number of built-in authentication policies to secure APIs (for example, API keys, OAuth 2.0, and SSL), which you can select when creating front-end APIs. You can extend the built-in authentication policies with custom authentication policies that have been developed in Policy Studio. For example, a custom policy could use CA SiteMinder to authenticate client application requests to APIs. In addition, custom authentication policies can specify a message that is displayed in the API Catalog informing application developers of the authentication mechanism to use when accessing the API.

To configure your custom inbound security policies, click **Add**, and select the appropriate policies in the dialog. The configured polices are added to the list.

{{< alert title="Note" color="primary" >}}Inbound security policies must set the `authentication.subject.id` message attribute to match the client ID set in the external credentials of the application, and return true for successful authentication.{{< /alert >}}

For details on applying inbound security policies to front-end APIs, see [Virtualize REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/).

### Global Request Policies

The **Global Request Policies** settings enable you to configure optional global enforcement policies for virtualized APIs in API Manager (for example, security, compliance, or governance policies executed as part of every API call).

To configure global request policies, click **Add**, and select policies in the dialog. By default, no global policies are configured.

When global request policies have been configured in Policy Studio, the API administrator can select a global request policy in API Manager on the **API Manager** **settings** page. The selected global request policy is executed after inbound authentication but before any request, routing, or response policies configured for the front-end API. For more details, see [Enforce API Manager global policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#enforce-api-manager-global-policies).

### Global Response Policies

The **Global Response Policies** settings enable you to configure optional global enforcement policies for virtualized APIs in API Manager (for example, security, compliance, or governance policies executed as part of every API call).

To configure global response policies, click **Add**, and select policies in the dialog. By default, no global response policies are configured.

When global response policies have been configured in Policy Studio, the API administrator can select a global response policy in API Manager on the **API Manager** **settings** page. The selected global response policy is executed last after any response policy configured for the front-end API. For more details, see [Enforce API Manager global policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#enforce-api-manager-global-policies).

### Request Policies

The **Request Policies** settings enable you to configure optional request processing policies for virtualized APIs in API Manager. For example, you could use the configured policies to check request messages for authentication or authorization.

To configure request policies, click **Add**, and select policies in the dialog. By default, no request policies are configured.

When request policies have been configured in Policy Studio, you can then apply them in API Manager on the **Frontend API > Outbound > Advanced** page. The selected request policy is executed after inbound authentication and any global request policy, but before any routing or response policies configured for the front-end API.

### Response Policies

The **Response Policies** settings enable you to configure optional response processing policies for virtualized APIs in API Manager. For example, you could use the configured policies to validate or transform outbound response messages.

To configure response policies, click **Add**, and select policies in the dialog. By default, no response policies are configured.

When response policies have been configured in Policy Studio, you can then apply them in API Manager on the **Frontend API > Outbound > Advanced** page. The selected response policy is executed after any routing policy configured for the front-end API, but before any global response policy.

### Routing Policies

The **Routing Policies** settings enable you to configure custom routing policies for virtualized APIs in API Manager. For example, you could use the configured policies to route to a back-end JMS service.

To configure routing policies, click **Add**, and select policies in the dialog. By default, no routing policies are configured, and the default URL-based routing policy is used. For more details, see [API Manager custom policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/).

When routing policies have been configured in Policy Studio, you can then apply them in API Manager on the **Frontend API > Outbound > Advanced** page. The selected routing policy is executed after any request policy and before any response policy configured for the front-end API.

### Fault Handler Policies

The **Fault Handler Policies** settings enable you to configure optional fault handler policies that are applied to front-end API invocations in API Manager. The configured fault handler is executed when an error or exception occurs during the API Manager runtime API invocation.

To configure fault handler policies, click **Add**, and select policies in the dialog. The API Manager **Default Fault Handler** policy is configured by default.

When fault handler policies are configured, an API administrator can select a global fault handler policy for all front-end APIs on the **API Manager** **settings** page in API Manager. API developers can also select fault handler policies for specific front-end APIs and API methods on the **Frontend API > Outbound > Advanced** page.

For more details, see [Add API Manager fault handler policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#add-api-manager-fault-handler-policies).

### SMTP Server

Under **SMTP Server** settings, to send emails (for example, for user registration or client application approval), you must configure an SMTP server for API Manager in the Policy Studio. The default setting is `Portal SMTP`
server on `localhost`.

You must ensure that API Manager is configured with the SMTP server used by your organization to generate emails for user registration or client application approval.

For example, to configure your SMTP server, perform the following steps:

1. Click the browse button on the on the right of the **SMTP Server**
   field.
2. Right-click **Portal SMTP**, and select **Edit**.
3. Complete the SMTP settings in the dialog. The following example settings use the Gmail SMTP server:

   * **Name**: Name for your SMTP server (for example, `Acme Portal SMTP Server`).
   * **SMTP Server Hostname**: Hostname of your SMTP server (for example, `smtp.gmail.com`).
   * **Port**: SMTP server port number (for example, `465`).
   * **Connection Security**: Select the type of connection security to use for SMTP. The options are `NONE`, `SSL`, or `TLS`. The default is `NONE`.
   * **User Name**: Your email user name (for example, `joe.bloggs@gmail.com`).
   * **Password**: Your email password.

### Cookie SameSite Attribute

Select the value of the **SameSite** attribute of the `Set-Cookie` header. Possible values for the flag are:

* **Strict**: Prevent the cookie from being sent by the browser to the target site in all cross-site browsing contexts, even when following a regular link. This is the default value.
* **Lax**: Provides a reasonable balance between security and usability for websites that want to maintain user’s logged-in session after the user arrives from an external link
* **None**: No protection. The browser attaches the cookies in all cross-site browsing contexts.
