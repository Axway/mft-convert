{
"title": "Configure client credentials",
"linkTitle": "Configure client credentials",
"weight": 2,
"date": "2019-10-17",
"description": "Client credentials enable you to globally configure client authentication settings for the following authentication options:"
}

Client credentials enable you to globally configure client authentication settings for the following authentication options:

* API keys as a client
* HTTP basic
* Kerberos
* OAuth 2.0 as a client

You can configure settings for client credentials under the **Environment Configuration > External Connections > Client Credentials**
node in the Policy Studio tree, which you can then specify at the filter level, for example, in the **Connection**
and **Connect To URL**
filters.

## Configure API key client credential profiles

API key client credential profiles enable you to globally configure authentication settings for API keys as a client. API keys are supplied by client users and applications calling REST APIs to allow the API service provider to track and control how the APIs are used (for example, to meter access and prevent abuse or malicious attack).

To configure API key client credential profiles, you must first configure a provider. The following Amazon Web Services provider configurations are included in an out-of-the-box installation of API Gateway:

* Amazon AWS V2 Signing
* Amazon AWS V4 Signing

### Add API keys

To add an API key for an existing API key provider, click an API key client credential node (for example, **Amazon AWS V2 Signing**), and click the **Add**
button on the **API Key Credential Profiles**
tab of the **API Key Credential Profile**
window. Complete the following fields on the **Add API Key**
dialog:

**Name**:
Enter a suitable name for this API key.

**API Key ID**:
Enter an identifier for this API key. This is the Amazon client ID associated with your Amazon account.

**API Key Secret**:
Enter a secret for this API key. This is the Amazon secret associated with your Amazon account.

To sort the list view of client credential profiles, click the column heading.

After you have configured your API key client credentials globally, you can select the client credential profile to use for authentication on the **Authentication**
tab of your filter (for example, in the **Connection**
and **Connect To URL**
filters).

### Add API key providers

To configure a new API key provider, right-click **API Keys**, and select **Add API Key Client Credential**. Complete the following fields on the **API Key Service Provider Configuration**
dialog:

**Name**:
Enter a suitable name for this API key service provider configuration.

**Sign the request using**:
Select an option to specify how the request is signed. The options are:

* Amazon Access Key Signing V2
* Amazon Access Key Signing V4
* No Signing

**Put the API key in**:
Select an option to specify where to put the API key in the request and enter a name in the **named**
field. The options are:

* Header – The parameter is added to the header of the request.
* Query String/Form Body – If the incoming request is a GET request, the parameter is added to the query string. If the incoming request is a POST request, the parameter is added to the content body.

Version 2 of the Amazon Signing API only supports the GET operation and only recognizes a limited set of query string parameters. Any unsupported query string parameters result in a `HTTP 400 Bad Request`
response from the V2 Amazon web service.

For example, to put the API key in the header of the request in a field named `AWSKeyId`, choose **Header**
and enter `AWSKeyId`.

{{< alert title="Tip" color="primary" >}}To change the configuration of an existing API key service provider, click the API key client credential node, and edit the settings on the **API Key Configuration**
tab of the **API Key Credential Profile**
window.{{< /alert >}}

## Configure HTTP basic/digest client credential profiles

A client can authenticate to the API service provider with a user name and password combination using HTTP basic authentication or HTTP digest authentication.

To add a HTTP basic/digest client credential profile, click the **HTTP Basic**
node, and click the **Add**
button on the **HTTP Basic/Digest Client Credentials**
window. Complete the following fields on the **Add HTTP Authentication Profile**
dialog:

**Profile Name**:
Enter a suitable name for this HTTP authentication profile.

**Choose Authentication Type**:
Select the **Basic**
or **Digest**
radio button to specify the type of HTTP authentication to use, and enter a user name and password in the **Username**
and **Password**
fields.

**Automatically send credentials**:
Select this option to automatically send credentials in the request. This is the equivalent of **Send token with first request**
in Kerberos. This option is selected by default.

After you have configured your HTTP client credentials globally, you can select the client credential profile to use for authentication on the **Authentication**
tab of your filter (for example, in the **Connection**
and **Connect To URL**
filters).

## Configure Kerberos client credential profiles

A Kerberos client can authenticate to a Kerberos service by sending a Kerberos service ticket in the HTTP request to that service.

{{< alert title="Note" color="primary" >}}
You can also configure the API Gateway to authenticate to a Kerberos service by including the relevant Kerberos tokens inside the XML message. For more details, see
[Configure Kerberos services](/docs/apim_policydev/apigw_external_connections/kerberos_service/).
{{< /alert >}}

To add a credential profile for a Kerberos client, click **Kerberos**, and select **Add**
in the **Kerberos Client Credentials**
window.

Configure the following fields on the **Add Kerberos Profile**
dialog:

**Profile Name**:
Enter a suitable name for this Kerberos authentication profile.

**Kerberos Client**:
Click the **...** button, and select a previously configured Kerberos client from the list. To add a Kerberos client, right-click **Kerberos Clients**, and select **Add Kerberos Client**. You can also add Kerberos clients globally under **Environment Configuration > External Connections** in the node tree.

The selected Kerberos client has two roles. First, it must obtain a Kerberos Ticket Granting Ticket (TGT). Second, it uses this TGT to obtain a service ticket for the **Kerberos Service Principal**
selected in this profile.

For more details on configuring Kerberos clients, see [Configure Kerberos clients](#configure-kerberos-clients).

**Kerberos Service Principal**:
Click the **...** button, and select a previously configured Kerberos service principal from the list. To add a Kerberos principal, right-click **Kerberos Principals**, and select **Add Kerberos Principal**. You can also add Kerberos principals under **Environment Configuration > External Connections** in the node tree.

The Kerberos client must obtain a ticket from the Kerberos Ticket Granting Server (TGS) for the selected Kerberos service principal. The TGS grants a ticket for the principal, and the Kerberos client can then send the ticket to the Kerberos service. The principal in the ticket must match the principal of the Kerberos service for the client to be successfully authenticated. A service principal name (SPN) can be used to uniquely identify the service in the Kerberos realm.

For more details on configuring Kerberos principals, see [Configure Kerberos principals](#configure-kerberos-principals).

**Send token with first request**:
In some cases, a Kerberos client might not authenticate (send the `Authorization`
HTTP header) to the Kerberos service on first request. The Kerberos service then responds with an HTTP 401 response code, instructing the client to authenticate to the server by sending the `Authorization`
header. The Kerberos client sends a second request, this time with the `Authorization`
header containing the relevant Kerberos token. Select this option to *always*
send the `Authorization`
HTTP header containing the Kerberos service ticket on the first request to the Kerberos service. This option is selected by default.

**Send body only after establish context**:
Select this option if you want the Kerberos client to only send the message body after the context with the Kerberos service has been fully established (the client has mutually authenticated with the service).

**Pass when service returns 200 even if context not established**:
In rare cases, a Kerberos service might return a `200 OK`
response to the initial request from a Kerberos client even though the security context has not yet been fully established. This `200 OK`
response might not contain the `WWW-authenticate`
HTTP header. Select this option to send the request with the `Authorization`
header to the Kerberos service *even if* the context has not been established. The Kerberos service then decides whether to process the request depending on the status of the security context.

After you have configured your Kerberos client credentials profile globally, you can select the client credential profile to use for authentication on the **Authentication**
tab of your filter (for example, in the **Connection**
and **Connect To URL**
filters).

## Configure Kerberos clients

API Gateway can act as a Kerberos client. For this, it must authenticate to the Kerberos Key Distribution Center (KDC) as a specific Kerberos principal, and use the Ticket Granting Ticket (TGT) granted for it to obtain tickets from the Ticket Granting Service (TGS) to authenticate to Kerberos services.

You can configure Kerberos clients globally under **Environment Configuration > External Connections**
in the node tree. Right-click the **Kerberos Clients**
node in the tree, and select **Add a Kerberos Client**. The following sections describe how to configure the different fields int he dialog.

Once finished, you can select the configured Kerberos client when configuring other Kerberos-related filters. Ensure the check box **Enabled**
at the bottom of the window is selected.

### Kerberos endpoint settings

Configure the following fields on the **Kerberos Endpoint**
tab:

#### Ticket Granting Ticket Source

This option defines where to obtain the Kerberos client credentials and the session key used in communications with the TGS to request TGTs. A TGT can be retrieved from a cache created as part of a Java Authentication and Authorization Service (JAAS) login, from delegated credentials, or from the native GSS Library on Linux platforms.

{{< alert title="Note" color="primary" >}}Depending on the TGT source option option you select, the **Kerberos Principal**, **Password**, and **Keytab File**
fields below might be required or disabled.{{< /alert >}}

**Load via JAAS Login**:
By default, API Gateway performs a JAAS login to the Kerberos KDC. After the login, API Gateway caches the credentials, and they are used to acquire TGTs as needed.

The JAAS login acquires the credentials in one of the following ways:

* **Request from KDC** – Request a new TGT from the Kerberos KDC. The request is sent at server startup, server refresh (for example, when an update to configuration is deployed), and when the TGT expires.
* **Extract from Default System Ticket Cache** – If a TGT has already been obtained outside API Gateway and has been stored in the default system ticket cache, you can use this option to retrieve the TGT from the cache. On Windows 2000, the TGT is extracted from the cache using the Local Security Authority (LSA) API. On Linux, the assumed default location of the ticket cache is `/tmp/krb5cc_<uid>`, where `<uid>` is a numeric user identifier. If the ticket cache cannot be found in the default locations, or on a different Windows platform, API Gateway searches the cache in `{user.home}{file.separator}krb5cc_{user.name}`.

A system ticket cache can only hold the credentials of a single Kerberos client. To load credentials for more than one Kerberos client from system ticket caches, select **Extract from System Ticket Cache**
option.

* **Extract from System Ticket Cache** – Extract the TGT from an explicitly named system ticket cache instead of the default system ticket cache.To specify the cache, browse to the cache you want to use. You can populate ticket caches with client credentials using an external utility, such as `kinit`.

**Load from Delegated Credentials**:
The Kerberos client can use a delegated TGT that has been already retrieved from a **Kerberos Service** authentication filter. The TGT is extracted from message attributes (for example, `authentication.delegated.credentials`
and `authentication.delegated.credentials.client.name`) that have been set by the Kerberos Service filter.

If you select this option, it is not necessary to configure the **Kerberos Principal**
or **Secret Key**
fields.

**Load via Native GSS Library**:
Select this option to use the Native GSS-API to acquire the client's credentials. The Native GSS-API expects that the credentials already are in a system ticket cache that it can access.

The **Load via Kinit**
option determines the number of Kerberos clients you can use with API Gateway:

* If **Load via Kinit**
    *is* selected, API Gateway can support multiple Kerberos clients natively. API Gateway runs `kinit` and creates a ticket cache for each client in the `/conf/plugins/kerberos/cache` directory. The Native GSS-API can automatically acquire the client credentials from these caches.
* If **Load via Kinit** *is not* selected, API Gateway can support only one Kerberos client natively. The Kerberos client credentials must be in the default system ticket cache. API Gateway cannot support accessing credentials natively from the default system ticket cache and other system ticket caches.

{{< alert title="Note" color="primary" >}}To use the native GSS library and the `kinit`
tool, you must select to use the native GSS library on the instance-level API Gateway **Kerberos Configuration**
settings. For more details, see [Configure Kerberos settings](/docs/apim_policydev/apigw_poldev/security_server_settings/#configure-kerberos-settings). {{< /alert >}}

#### Kerberos Principal

A Kerberos principal is used to assign a unique identity for API Gateway to be used in the Kerberos environment.

To select which Kerberos principal to use, click the **...** button, and select a previously configured principal from the list. To add a Kerberos principal, right-click **Kerberos Principals**, and select **Add Kerberos Principal**. You can also add Kerberos principals under **Environment Configuration > External Connections** in the node tree.

For Kerberos constrained delegation (KCD), the Kerberos Principal selected here must be configured for credential delegation to a set of Kerberos services in the Kerberos KDC.

For more details, see [Configure Kerberos principals](#configure-kerberos-principals).

{{< alert title="Note" color="primary" >}}
The semantics of this field are slightly different depending on what TGT source you selected:

* If you selected to retrieve the TGT from the KDC, you are effectively asking the KDC to issue a TGT for the principal selected here.
* If you selected to retrieve the TGT from a system ticket cache, the principal selected here searches the cache to retrieve the TGT.
* If you selected to use the `kinit`
    utility, the name of the principal selected here is passed as an argument to `kinit`.
* If you selected to retrieve a TGT from delegated credentials, it is not necessary to specify any principal.
{{< /alert >}}

#### Secret Key

The Kerberos principal uses the secret key to communicate with the KDC's Authentication Service in order to acquire a TGT. The secret key can be generated from a password, or it can be acquired from the principal's keytab file. The options available depend on what you selected as the source of the TGT.

**Enter Password**:
You can only enter a password if you selected to request the TGT from the KDC. The password is used when generating the secret key. A secret key is not required if the TGT has been already retrieved either from a system ticket cache or from delegated credentials.

By default, the password entered here is stored in clear-text form in the underlying configuration data in API Gateway. If necessary, the password can be encrypted using a passphrase.

**Keytab**:
If you selected to request the TGT from the KDC, you can also extract the secret key for the principal from a keytab
file, which maps principal names to encryption keys. Using the `kinit`
tool also requires a keytab
file.

To load the principal-to-key mappings into the table in the dialog, select **Load Keytab**
and browse to the existing keytab file. To add a new keytab entry, select **Add Principal**. To delete a keytab entry, select the entry in the table and click **Delete Entry**. To export the entire contents of the keytab table in the dialog, click **Export Keytab**.

By default, the contents of the keytab table – derived from a keytab file or manually entered – stored in clear-text form in the underlying configuration data in API Gateway. If necessary, the contents of the keytab table can be encrypted using a passphrase.

When API Gateway starts, it writes the stored keytab contents to the `/conf/plugin/kerberos/keytabs/`
directory in your API Gateway installation. It is recommended to configure directory-based or file-based access control for this directory and its contents.

For more details on configuring the **Keytab Entry**
dialog, see the [Kerberos Keytab concepts](#kerberos-keytab-concepts).

### Kerberos Constrained Delegation settings

An end user application requesting a TGT for a back-end Kerberos service might be unable to authenticate to the Kerberos service with Kerberos credentials, for example, if accessing the service from outside the Kerberos domain. KCD enables the Kerberos principal you selected on the **Kerberos Endpoint** tab to act as a trusted Kerberos principal. The trusted Kerberos principal authenticates the end user application using some other authentication method than Kerberos credentials, then authenticates to the back-end Kerberos service as the end user application, and acquires a TGT in the name of the end user application.

To enable KCD, configure the following fields on the **Kerberos Constrained Delegation**
tab:

**Kerberos Principal to Impersonate**:

Select the end user principal being impersonated. The principal to impersonate is normally configured using a selector, so that many end user principals can be impersonated. A typical setup might be that the principal on the **Kerberos Endpoint** tab might be, for example, `TrustedAPIGateway@AXWAY.COM` and the Kerberos Principal to Impersonate is `${authentication.subject.id}@AXWAY.COM`.

If you don't set this field, the Kerberos Client does not attempt to impersonate other Kerberos principals in Kerberos Constrained Delegation.

**Select Cache for Impersonated Subjects**:

Select the cache used to store impersonated user credentials. These will be refreshed automatically so that expired credentials are not sent to the service-side.

You must define this field for KCD.

### Advanced settings

You can configure the following fields on the **Advanced**
tab:

**Mechanism**:
Select the mechanism used to establish a context between the Kerberos client and the Kerberos service. The Kerberos service must use the same mechanism.

**Mutual Authentication**:
Select this to carry out mutual authentication . This means that the service authenticates back to the client during the context setup. For SPNEGO mechanism, you must select this.

**Integrity**:
Select to enable data integrity for GSS operations.

**Confidentiality**:
Select to enables data confidentiality for GSS operations.

**Credential Delegation**:
Select this to delegate the request initiator's credentials to the acceptor during context setup. If you select this option, the acceptor can assume the initiator's identity and authenticate to other Kerberos services on behalf of the initiator.

**Anonymity**:
Select to prevent disclosing the Kerberos client's identity to the Kerberos service.

**Replay Detection**:
Select to enable replay detection for the per-message security services after context establishment.

**Sequence Checking**:
Select to switch on sequence checking for the per-message security services after context establishment.

**Synchronize to Avoid Replays Errors at Service**:

If a Kerberos client is running under stress and is attempting to send many requests to a Kerberos service within a very short (millisecond) time frame, the sequential Kerberos Authenticator
tokens the Kerberos client generates might contain identical values for the `ctime`
(the current time on the client's host) and `cusec`
(the microsecond portion of the client's timestamp) fields.

Since Kerberos service implementations often compare the `ctime` and `cusec` values on successive Kerberos Authenticator tokens to discover replay attacks, it is possible that the Kerberos service might reject Kerberos Authenticator requests in which the `ctime` and `cusec` fields have the same value.

To avoid this scenario, you can select this option to synchronize the creation of the Kerberos Authenticator requests using the **Pause Time**
value.

{{< alert title="Note" color="primary" >}}
On Linux, this setting is not required, so ensure it is deselected for better performance.{{< /alert >}}

**Pause Time**:
Specify the time interval (in milliseconds) to wait before generating the client-side Kerberos Authenticator tokens when synchronizing to avoid over-zealous replay detection on the Kerberos service. You can only set this value if you have also selected the **Synchronize to Avoid Replays Errors at Service**
option.

The default value of 15 milliseconds matches the clock resolution time of operating systems. For more details on the clock resolution on your target operating system, consult your operating system documentation.

**Refresh credential when remaining validity is X secs**:

Select this to enable refresh the Kerberos credentials shortly before they expire to avoid expiry failures on the service-side.

When API Gateway receives a request that uses a Kerberos client, API Gateway refreshes any cached Kerberos credentials used to process that request, if the time that this credential can be validly used is less than or equal to this number of seconds.

It is possible that a credential is only refreshed long after it has expired, as triggering the refresh requires a request that uses the expired credential.

## Configure Kerberos principals

A Kerberos principal represents a unique identity in a Kerberos system to which Kerberos can assign tickets to access Kerberos-aware services. Principal names are made up of several components separated with the `/`
separator. You can also specify a Kerberos realm as the last component of the name by using the `@`
character. If no realm is specified, the principal is assumed to belong to the default realm, as configured in the `krb5.conf`
file. For more details, see Kerberos configuration.

Typically, a principal name comprises three parts: the *primary*, the *instance*, and the *realm*. The format of a typical Kerberos v5 principal name is:

```
primary/instance@realm
```

* *Primary*: If the principal represents a user in the system, the primary is the user name of the user. Alternatively, for a host, the primary is specified as the `host`
    string.
* *Instance*: The instance can be used to further qualify the primary (for example, `user/admin@foo.abc.com`).
* *Realm*: This is your Kerberos realm, which is usually a domain name in upper case letters. For example, the `foo.abc.com`machine is in the `ABC.COM`
    Kerberos realm.

### Configuration

You can configure Kerberos principals globally under the **Environment Configuration** > **External Connections**
in the node tree. To configure a Kerberos principal, right-click the **Kerberos Principals**
node, and select **Add a Kerberos Principal**.

Configure the following fields on the **Kerberos Principal**
dialog:

**Name**:
Enter a friendly name for the Kerberos principal. This name is shown in the drop-down lists in other Kerberos-related configuration windows in the Policy Studio.

**Principal Name**:
Enter the name of the Kerberos principal in this field. The principal name consists of a number of components separated using the `/`
separator. Specify the realm here if the principal belongs to either a non-default realm or if a default realm is not specified in the `krb5.conf` file.

**Principal Type**:
Select the type of principal specified in the field above. The following table lists the available principal types.

| Principal name type          | Explanation                                          | OID                      |
|------------------------------|------------------------------------------------------|--------------------------|
| `NT_USER_NAME`               | The principal name identifies a named user on the local system  | `1.2.840.113554.1.2.1.1` |
| `KERBEROS_V5_PRINCIPAL_NAME` | The principal name represents a Kerberos version 5 principal.  | `1.2.840.113554.1.2.2.1` |
| `NT_EXPORT_NAME`             | The principal name represents an exported canonical byte representation of the name (for example, which can be used when searching for the principal in an Access Control List (ACL)). | `1.3.6.1.5.6.4`   |
| `NT_HOSTBASED_SERVICE`       | The principal name identifies a service associated with a specific host.   | `1.3.6.1.5.6.2`   |

The principal name types and their corresponding OIDs are defined in the General Security Services API (GSS-API).

To add new principal types, click **Add**. The name entered in the **Name**
field on the **Kerberos Principal Name OID**
must correspond to one of the constant fields defined in the `org.ietf.jgss.GSSName`
Java class. For other allowable name types, see the Javadocs for the GSSName
class.

Similarly, the corresponding OID for this name type must be entered in the **OID**
field of the dialog.

{{< alert title="Note" color="primary" >}}OIDs and principal type names must only be changed to reflect changes in the underlying GSS-API. Therefore, do not edit
the existing **Principal Types**
except under strict supervision by Axway Support.{{< /alert >}}

## Kerberos keytab concepts

The Kerberos keytab
file contains mappings between Kerberos principal names and DES-encrypted keys that are derived from the password used to log into the Kerberos Key Distribution Center (KDC). The purpose of the keytab file is to allow the user to access distinct Kerberos services without being prompted for a password at each service. Furthermore, it allows scripts and daemons to log in to Kerberos services without the need to store clear-text passwords or the need for human intervention.

{{< alert title="Note" color="primary" >}}Anyone with read access to the keytab file has full control of all keys contained in the file. For this reason, it is imperative that the keytab file is protected using very strict file-based access control.{{< /alert >}}

Each key entry in the Kerberos keytab file is identified by a Kerberos principal and an encryption type. For this reason, a keytab file can hold multiple keys for the same principal, each key belonging to a different encryption type. If the keytab file contains several keys for a principal, the Kerberos client or service uses the key with the strongest encryption type as agreed during the negotiation of previous messages with the KDC.

A keytab file can also contain keys for several different principals. In this case, at runtime, the Kerberos client or service only considers keys mapped to the Kerberos principal name you selected in the **Kerberos Principal**
drop-down list when configuring that Kerberos client or service.

The **Keytab** table in the **Secret Key**
section of the configuration window for a Kerberos client or Kerberos service is essentially a graphical interface to entries in a Kerberos keytab file. To generate a keytab entry, select **Keytab > Add Principal**. To remove an entry, select the entry and click **Delete Entry**.
You can configure Kerberos clients and services under **Environment Configuration > External Connections** in the node tree.

### Keytab configuration

Configure the following fields on the **Keytab Entry**
dialog:

**Kerberos Principal**:
Select an existing Kerberos principal from the drop-down list. To add a Kerberos principal, right-click **Kerberos Principals**, and select **Add Kerberos Principal**. You can also add Kerberos principals under **Environment Configuration > External Connections** in the node tree. For more details, see [Configure Kerberos principals](#configure-kerberos-principals).

**Password**:
Enter the password to seed the encryption algorithms for the encryption types.

**Key version number**:
Set the version number for the encryption key.

**Encryption Types**:
Select the encryption types. The encryption types determine the algorithms used to generate the encryption keys stored in the keytab file. If the keytab file contains multiple keys for a Kerberos principal, the encryption type is used to select an appropriate encryption key.

To ensure maximum interoperability between Kerberos clients and Kerberos services configured in API Gateway and different KDCs, all encryption types are selected by default. This way, the generated keytab entry contains a separate encryption key for each encryption type listed here, and each key is mapped to the selected Kerberos principal name.

You must ensure that the required encryption types exist in the keytab as defined in Kerberos system settings in the `krb5.conf` file. For a Kerberos client to request a Ticket Granting Ticket (TGT), it must have at least one key that matches one of the encryption types listed in the `default_tkt_enctypes`
setting in `krb5.conf`. A Kerberos service requires a key of a matching encryption type to be able to decrypt a TGT a Kerberos client presents.

For more details on the `krb5.conf` file, see [Configure Kerberos settings](/docs/apim_policydev/apigw_poldev/security_server_settings/#configure-kerberos-settings).

By default, for Windows 2003 Active Directory, TGT is encrypted using the `rc4-hmac`
encryption type. However, if the service user has enabled **Use DES encryption types for this account**, the `des-cbc-md5`
encryption type is used.
