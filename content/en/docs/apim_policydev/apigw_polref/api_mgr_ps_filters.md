{
"title": "API management filters",
  "linkTitle": "API management filters",
  "weight": "200",
  "date": "2019-09-17",
  "description": "Use the API management filters within a policy to get information about API access, API proxies, client applications, application developers, and organizations from the API Manager Client Registry."
}
The Client Registry is a repository of organizations, API consumers, and client applications that consume the APIs registered in API Manager. The Client Registry also contains the authentication credentials of the client applications, and authorization and quota policies defined at the organization and application level. The Client Registry is stored in a Key Property Store (KPS) backed by an Apache Cassandra database.

Policy Studio includes API Management filters that provide read-only access to the Client Registry. These enable policy developers to develop policies in Policy Studio that leverage the information in the Client Registry.

## Read API access filter

You can use the **Read API Access** filter to get information from the Client Registry about a particular organization's, or a particular application's, access to an API.

This filter stores the information in a message attribute (for example, `apimgmt.apiaccess`). You can use this filter within an alert handling policy (or any other policy) to read an organization's or an application's API access easily.

Configure the following fields on the **General settings** tab:

**API ID selector**:

Enter a selector expression with the name of the message attribute that contains the API ID. ID is the key to the API proxy record in the KPS. The value of the selector is expanded at runtime. The default is `${apimgmt.apiproxy.id}`.

**Entity ID selector**:

Enter a selector expression with the name of the message attribute that contains the application or organization entity ID. ID is the key to the entity record in the KPS. The value of the selector is expanded at runtime. The default is `${apimgmt.entity.id}`.

**Type**:

Choose the type of the entity, `Application` or `Organization`.

**Name of attribute to set**:

Enter the name of the message attribute to set. The default is `apimgmt.apiaccess`.

## Read API proxy filter

You can use the **Read API Proxy** filter to get information from the Client Registry about an API proxy.

This filter stores the information in a message attribute (for example, `apimgmt.apiproxy`). You can use this filter within an alert handling policy (or any other policy) to get information about an API proxy easily.

Configure the following fields on the **General settings** tab:

**API Proxy ID selector**:

Enter a selector expression with the name of the message attribute that contains the API proxy ID. ID is the key to the API proxy record in the KPS. The value of the selector is expanded at runtime. The default is `${apimgmt.apiproxy.id}`.

**Name of attribute to set**:

Enter the name of the message attribute to set. The default is `apimgmt.apiproxy`.

## Read application credential filter

You can use the **Read Application Credential** filter to get information from the Client Registry about an application credential.

This filter stores the information in a message attribute (for example, `apimgmt.appcredential`). You can use this filter within an alert handling policy (or any other policy) to get information about an application credential easily.

The information is stored as a HashMap. To extract a value from the map, use a selector. For example:

```
${appcredential.get("appcredential.apikey.secret")}
```

Configure the following fields on the **General settings** tab:

**Credential Type**:

The type of application credential to read. Select one of the following values:

* `API Key`
* `OAuth Client`
* `External Client`

**Credential ID selector**:

Enter a selector expression with the name of the message attribute that contains the application credential ID (for example, `${alert.appcredential.apikey.id}`). ID is the key to the application credential record in the KPS. The value of the selector is expanded at runtime. The default is `${apimgmt.appcredential.id}`.

**Name of attribute to set**:

Enter the name of the message attribute to set (for example, `apimgmt.appcredential.apikey`).

## Read application developer filter

You can use the **Read Application Developer** filter to get information from the Client Registry about an API consumer.

This filter stores the information in a message attribute (for example, `apimgmt.appdeveloper`). You can use this filter within an alert handling policy (or any other policy) to get API Consumer information easily.

Configure the following fields on the **General settings** tab:

**Application Developer ID selector**: Enter a selector expression with the name of the message attribute that contains the API consumer ID. ID is the key to the application developer record in the KPS. The value of the selector is expanded at runtime. The default is `${apimgmt.appdeveloper.id}`.

**Name of attribute to set**: Enter the name of the message attribute to set. The default is `apimgmt.appdeveloper`.

## Read application filter

You can use the **Read Application** filter to get information from the Client Registry about an application.

This filter stores the information in a message attribute (for example, `apimgmt.application`). You can use this filter within an alert handling policy (or any other policy) to get information about an application easily.

Configure the following fields on the **General settings** tab:

**Application ID selector**:

Enter a selector expression with the name of the message attribute that contains the application ID. ID is the key to the application record in the KPS. The value of the selector is expanded at runtime. The default is `${authentication.application.id}`.

**Name of attribute to set**:

Enter the name of the message attribute to set. The default is `apimgmt.application`.

### Example alert handling policy

The following figure shows an example of an alert handling policy that uses the **Read Application** filter. This policy handles the alert generated when an application's access to an API is enabled. It uses the **Read Application** filter to get information about the application, which it then uses to populate an alert message.

![Example alert handling policy](/Images/docbook/images/api_mgmt/api_mgmt_alert_handling.png)

## Read organization filter

You can use the **Read Organization** filter to get information from the Client Registry about an organization.

This filter stores the information in a message attribute (for example, `apimgmt.organization`). You can use this filter within an alert handling policy (or any other policy) to get information about an organization easily.

Configure the following fields on the **General settings** tab:

**Organization ID selector**:

Enter a selector expression with the name of the message attribute that contains the organization ID. ID is the key to the organization record in the KPS.The value of the selector is expanded at runtime. The default is `${apimgmt.organization.id}`.

**Name of attribute to set**:

Enter the name of the message attribute to set. The default is `apimgmt.organization`.