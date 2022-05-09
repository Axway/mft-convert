{
"title": "Configure authentication repositories",
"linkTitle": "Configure authentication repositories",
"weight": 1,
"date": "2019-10-17",
"description": "Configure API Gateway to authenticate clients using user name and password combinations stored in an authentication repository."
}

API Gateway supports a wide range of common authentication schemes, including SSL, XML signatures, WS-Security Username tokens, and HTTP authentication. With SSL, the client authenticates to API Gateway using a client certificate. With XML signatures, the client is authenticated by validating the signature contained within the XML message. However, when API Gateway attempts to authenticate a client using a user name and password (for example, WS-Security Username tokens or HTTP authentication), it must compare the user name and password presented by the client to those stored in the *authentication repository*.

The authentication repository acts as a repository for *users*. Users serve many roles in API Gateway. For example, clients whose user name and password combinations are stored in the authentication repository can authenticate to API Gateway using that user name and password combination.

The authentication repository can be maintained in API Gateway's local configuration store, in an LDAP directory, or in a range of third-party Identity Management products and services. When a user has been successfully authenticated against one of these repositories, API Gateway can use any one of that user's stored attributes (for example, distinguished name (DName), email address, user name) to authorize that same user in a subsequent authorization filter.

For example, this *credential mapping* is useful in cases where your client-base uses user name and password combinations for authentication (*authentication attributes*), but their access rights must be looked up in an authorization server using the client's DName (*authorization attribute*). In this way, the client possesses a single *virtual identity* within API Gateway. The client can use one identity for authentication, and another for authorization, yet API Gateway sees both identities as representing the same client.

You can add a new repository under the **Environment Configuration > External Connections** node in Policy Studio node tree. Right-click the appropriate node (for example, **Database Repositories**), and select **Add a new Repository**. Similarly, you can edit an existing repository. Right-click the repository node (for example, the default **Local User Store**), and select **Edit Repository**. You can reuse the repositories added under the **External Connections** node in multiple filters.

## Configure Axway PassPort repositories

API Gateway can integrate with Axway PassPort to authenticate and authorize users for resources. To authenticate users against an Axway PassPort repository, right-click **Axway PassPort Repositories**, and select **Add a new Repository**.

Axway PassPort provides a central repository, identity broker, and security audit point for your Axway Business-to-Business Integration (B2Bi) or Managed File Transfer (MFT) solutions. Axway PassPort centralizes and simplifies provisioning and management for your entire online ecosystem, enabling secure collaboration between applications, divisions, customers, suppliers, and regulatory bodies.

### Configuration

Complete the following fields to configure an Axway PassPort repository:

* **Repository Name**: Enter a descriptive name for this repository.
* **Hostname**: Enter the host name or IP address of the server running PassPort.
* **Shared Secret**: Enter the PassPort shared secret. This is specified during PassPort installation.
* **CSD Name**: Enter the name of the Axway Component Security Descriptor (CSD) file to use when registering with PassPort. Defaults to `csd.xml`.

    {{< alert title="Note" color="primary" >}}The CSD file must be deployed in the API Gateway group's `conf` directory in your API Gateway installation (`INSTALL_DIR/apigateway/groups/GROUP_ID/conf`).{{< /alert >}}

* **PassPort Certificates—HTTPS**: Select the certificate used for PassPort SSL communication. To export this certificate from PassPort, perform the following steps:

    1. In the PassPort user interface, click **Administration** >**Server Security Settings**.
    2. Note the certificate used for **Default_HTTPS**.
    3. Click **Security** >**Certificates**.
    4. Select the certificate noted in step 2 (defaults to `CN=PassPortSecured,O=Axway,C=FR`), and click **Export Certificate**.
    5. In the **Export Certificate** dialog, select a **File Extension** of `.cer`.
    6. Click **OK**, and select a location to save the certificate.

    To import this certificate into the API Gateway, perform the following steps:

    1. In the API Gateway **Authentication Repository** dialog, click **Select**.
    2. In the **Select Certificate** dialog, click **Create/Import**.
    3. In the **Configure Certificate and Private Key** dialog, click **Import Certificate**.
    4. Select the certificate that was exported from PassPort.
    5. Give the certificate an **Alias Name** manually, or click **Use Subject**.
    6. Click **OK**.
    7. Select the certificate from the list, and click **OK**.

* **PassPort Certificates—HTTPS Client Authentication (Optional)**: You can configure PassPort to use a different certificate for its client authentication protocol. To do this, repeat the steps for the HTTPS certificate, except when exporting from PassPort in step 2, make a note of the **Default_HTTPS_Client_Auth**
certificate.
* **Ports—HTTPS**: Enter the HTTPS port that PassPort is using. In PassPort, this is found under **Administration** > **Server Ports Configuration**. Defaults to `6453`.
* **Ports—HTTPS Client Authentication**: Enter the HTTPS client authentication port that PassPort is using. In PassPort, this is found under **Administration** > **Server Ports Configuration**. Defaults to `6666`.
* **Authentication—Domain**: Enter the PassPort domain that this repository is using for authentication and authorization. Defaults to `Synchrony`.

### Axway PassPort repository registration

The external connection to Axway PassPort requires that communication with the Axway PassPort server is performed over a secure connection using two-way SSL authentication. This means that the PassPort server must be able to identify and trust the client connection, and this trust is established by registration.

The first connection from the API Gateway to PassPort initiates registration. A public-private key pair is created and a Certificate Signing Request (CSR) is submitted to PassPort. This is where the **Shared Secret**
and **HTTPS**
port values are used. While the CSR is pending, the repository is unable to process any requests. However, registration is a once off event, and when complete, it does not need to be repeated.

When registration is complete, the key and signed certificate are stored in a Java Key Store file in the following directory of your API Gateway installation:

```
INSTALL_DIR/apigateway/groups/GROUP_ID/conf
```

#### Troubleshooting registration issues

In the unlikely event that automatic registration fails, you should check the following:

Ensure the time on the API Gateway is synchronized with the time on the PassPort machine. When PassPort processes the CSR, it sets the **Valid From** date to the current time. If the PassPort time is ahead of the API Gateway time, the API Gateway is unable to use the certificate because it is not yet valid. The error in the trace log is as follows:

```
java.security.cert.CertificateNotYetValidException:java.security.cert.CertificateNotYetValidException
```

By default, PassPort blocks for up to 2 seconds waiting for the CSR to be processed. You can configure this value in the PassPort administration user interface under **Administration** > **System Properties** (`am.registration.cert.signature.wait.time`). If the signing request takes longer than this, one of the following errors may be logged:

```
Authentication exception when authenticating system:
com.axway.passport.am.api.v2.service.external.PassportConnectionException:Registration is still in progress.
Certificate Signing Request has not yet been validated, please try again later.
[Status:Waiting Validation]
Authentication exception when authenticating system:
com.axway.passport.am.api.v2.service.external.PassportConnectionException:Registration is still in progress.
Certificate Signing Request has not yet been signed, please try again later.
[Status:Waiting Signing]
```

This is generally a transient error that may be generated when the initial registration is in progress. Resubmitting the request should succeed. If the error persists, check in the PassPort administration user interface for the reason why the signing request has been delayed.

If the registration request has been refused by the PassPort administrator, the following error is displayed:

```
Registration has been refused.
Please contact the PassPort Administrator for further information.
[Status:Validation Refused]
```

If CSR processing fails for some other reason, the following error is logged:

```
Registration has failed and API Gateway is unable to communicate with PassPort.
Please contact the PassPort Administrator or try manually re-triggering registration.
[Status:...]
```

To resolve this, contact the PassPort administrator to see why the signing request failed. To retry registration, you need to manually re-trigger registration, as explained in the next subsection.

#### Retrigger registration manually

When registration is complete, the API Gateway does not repeat the process, the generated Java Key Store (JKS) is used for all subsequent connection attempts. However, if for any reason the key pair in the JKS is no longer trusted by PassPort, or if registration is not being processed, you can trigger the registration procedure manually.

The simplest way to retrigger registration is to change the repository name and redeploy. However, if this is not an option, you can manually remove the keystores.

The format of the JKS file names is as follows:

```
keystore_REPOSITORYNAME_HOSTNAME_HTTPSCLIENTAUTHPORT.jks
```

All non-alphanumerics are replaced with an underscore (`_`).

For example, given the following repository details:

* **Repository Name**: `PassPort - local`
* **Hostname**: `passport-host`
* **HTTPS Client Authentication**: `6666`

The keystore name is `keystore_PassPort___local_passport_host_6666.jks`.

To trigger registration, perform the following steps:

1. Back up the JKS associated with the Axway PassPort Authentication Repository being reset.
2. Delete this JKS.
3. Restart the API Gateway instance. The deleted file is recreated and registration is initiated.

If registration has not been completed when the registration is being retriggered, you must delete the following additional temporary files to ensure clean registration:

```
keystore_REPOSITORYNAME_HOSTNAME_HTTPSCLIENTAUTHPORT.jks.csrid
keystore_REPOSITORYNAME_HOSTNAME_HTTPSCLIENTAUTHPORT.jks.pub
keystore_REPOSITORYNAME_HOSTNAME_HTTPSCLIENTAUTHPORT.jks.key
```

In addition, to avoid future confusion, it is good practice to ensure that all redundant certificates and signing requests are removed from PassPort using the PassPort administration user interface.

## CA SiteMinder repositories

If the user profiles are stored in an existing CA SiteMinder server, API Gateway can query SiteMinder to authenticate users.

To authenticate users against a SiteMinder repository, right-click **CA SiteMinder Repositories**, and select **Add a new Repository**. Complete the following fields on the Authentication Repository dialog:

* **Repository Name**: Enter a suitable name for this repository.
* **Agent Name**: Select a previously configured SiteMinder agent name from the list. To register a new agent, see [API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).
* **Resource**: Enter the name of the protected resource for which the user must be authenticated. Alternatively, you can enter a selector for a message attribute, which is looked up and expanded to a value at runtime. Message attribute selectors have the following format, `${message.attribute}`. For example, by default API Gateway specifies the original path the end user requested as the resource, `${http.request.uri}`.
* **Action**: The user must be authenticated for a specific action on the protected resource. By default, API Gateway takes this action from the HTTP verb used in the incoming request. You can use the following selector to get the HTTP verb, `${http.request.verb}`.

Alternatively, you can enter any user-specified value.

* **Single Sign-On Token**: By default, when an end user has been authenticated for a given resource, SiteMinder generates a *single sign-on token* (a session cookie). API Gateway stores this cookie in a user-specified message attribute. API Gateway returns the cookie to the end user along with the response. The end user can then pass this cookie with future requests to API Gateway. When API Gateway receives such a request, it can validate the session cookie using the **CA SiteMinder Session Validation** filter. The client stays authenticated for the entire lifetime of the session cookie. As long as the session cookie is valid, API Gateway does not need to re-authenticate the end user against SiteMinder for every request. This increases throughput and performance considerably.
* **Put Token in Message Attribute**: Enter the name of the message attribute where you wish to store the session cookie. By default, the cookie is stored in the `siteminder.session` attribute.

## Database repositories

API Gateway can store its authentication repository in an external database. This option makes sense if your organization already has a silo of user profiles stored in the database and you do not want to duplicate this store within API Gateway's local configuration storage.

To authenticate users against a database repository, right-click **Database Repositories**, and select **Add a new Repository**. Complete the following fields on the Authentication Repository dialog:

**Repository Name**:
Enter an appropriate name for the database in the **Repository Name**
field.

**Database**:
There are two basic configuration items required to retrieve a user's profile from the database:

* **Database Location**:
    To configure connection details for the database, click **Add**, and complete the Database Connection dialog. For details on configuring the fields on this dialog, see [Configure database connections](/docs/apim_policydev/apigw_external_connections/common_db_conf). To edit or remove previously configured database connections, select them from the drop-down list and click **Edit** or **Delete**.
* **Database Query**:
    **Database Query** retrieves the profile of a specific user from the database to enable API Gateway to authenticate the user. after a successfull authentication, you can select an attribute of this user to use for the authorization filter later in the policy. **Database Query** can be an SQL statement, stored procedure, or function call. For details on how to configure **Database Query**, see [Configure database queries](/docs/apim_policydev/apigw_external_connections/common_db_conf/#configure-database-queries).

**Format Password Received From Client**:
If the user sends up a clear-text password to API Gateway, but that user's password is stored in a hashed format in the database, API Gateway must hash the password before performing the authentication step.

* **Hash Client Password**:
    Depending on whether you wish to hash the user's submitted password, select the appropriate option.
* **Hash Format**:
    If you have selected to hash the client's password, you must specify the format of the hashed password. The most typical formats have been listed, but you can also enter another format. Formats should be entered in terms of message attribute selectors. The following formats are available from the **Hash Format** list.

    ```
    ${authentication.subject.id}:${authentication.subject.realm}:${authentication.subject.password}
    ${authentication.subject.password}
    ```

    The first option combines the username, authentication realm, and password respectively. This combination is then hashed. The second option simply creates a hash of the user's password.
* **Hash Algorithm**:
    Select either **MD5** or **SHA1** as the digest algorithm to use when creating the hash.

**Query Result Processing**:
You can use these options to provide API Gateway with some meta information about the result **Database Query** returns. You can identify the name of the database table column or row that contains the user's password, as well as the name of the column or row that contains the attribute that is to be used for the authorization filter.

* **Password Column**:
    Specify the name of the database table column that contains the user's password. The contents of this column are compared to the password the user submits.
* **Password Type**:
    Depending on how the user's password has been stored in the database, select either **Clear Password** or **Digest Password**.
* **Authorization Attribute Column**:
    When **Database Query** is run, all of the user's attributes are returned. Only the user's user name and password are used for the authentication event. In addition, you can use one of the other user's attributes for authorization at a later stage in the policy. The additional authorization attribute must be either a user name or an X.509 DName. Enter the name of the column containing the user name or the DName here, but only if this value is required for authorization purposes.
* **Authorization Attribute Format**:
    API Gateway's authorization filters are all based on a user name or DName. All authorization filters evaluate if a user identified by a user name or DName is allowed to access a specific resource. Select the appropriate format from the drop-down list depending on what type of user credential is stored in the database table column entered above.

## Entrust GetAccess repositories

Entrust GetAccess provides Identity Management and access control services for web resources. It centrally manages access to web applications, enabling users to benefit from a single sign-on capability when accessing the applications that they are authorized to use.

You can configure API Gateway to connect to a group of GetAccess servers in a round-robin fashion. This provides the necessary failover capability if one or more GetAccess servers are not available. When API Gateway successfully authenticates to a GetAccess server, it obtains authorization information about the end user from the GetAccess SAML PDP. The authorization details are returned in a SAML authorization assertion thatAPI Gateway then validates to determine whether the request should be allowed.

To authenticate users against an Entrust GetAccess repository, right-click **Entrust GetAccess Repositories**, and select **Add a new Repository**. Configure the following fields on the **Authentication Repository** dialog:

**Repository Name**:
Enter an appropriate name for this repository.

**Request**:
Configure the following request settings:

* **URL Group**:
    Select a URL group from the drop-down list. This group consists of a number of GetAccess Servers to which API Gateway round-robins connection attempts. To add URL groups, in the node tree, click **Environment Configuration > External Connections > URL Connection Sets**, right-click **Entrust GetAccess URL Sets**, and select **Add a URL Set**. For more details on adding and editing URL groups, see Configure URL groups.
* **WS-Trust Attribute Field Name**:
    Specify the field name for the `Id` field in the WS-Trust request. The default is `Id`.

**Response**:
Configure the following response settings:

* **SOAP Actor/Role**:
    To add the SAML authorization assertion to the response message, select a SOAP actor/role to indicate the WS-Security block where the assertion is added. If you leave this field blank, the assertion is not added to the message.
* **Drift Time**:
    The specified time is used to allow for the possible difference between the time on the GetAccess SAML PDP and the time on the machine hosting API Gateway. This comes into effect when validating the SAML authorization assertion.

## Local repositories

The authentication repository
can be maintained in the same database that API Gateway uses to store all its configuration information. To edit the default user store, select **Local Repositories > Local User Store > Edit Repository**. Alternatively, to create a new user store, select **Local Repositories > Add a new Repository**.

You can enter an appropriate name for the repository in the **Repository Name** field. The **Authorization Attribute Format** field enables administrators to specify whether to use the client's **X.509 Distinguished Name** or **User Name**
in subsequent authorization filters. If **User Name** is selected, the user name the client uses to authenticate to API Gateway is used in any configured authorization filters. If **X.509 Distinguished Name** is selected, the X.509 DName API Gateway has stored for that user is used for subsequent authorization.

For example, if the administrator selects **User Name** from the **Authorization Attribute Format** list, `admin` (the **User Name** field) is used for authorization. Alternatively, if the administrator selects **X.509 Distinguished Name**, the X.509 DName is used for authorization (for example, `O=Company, OU=comp, EMAIL=emp@company.com, CN=emp`).

For more information on adding and configuring users to the authentication repository, see [Manage API Gateway users](/docs/apim_administration/apigtw_admin/manage_user_access/).

## LDAP repositories

In cases where an organization stores user profiles in an LDAP directory, it does not make sense to re-enter these profiles into the default API Gateway user store. Instead, API Gateway can leverage an existing LDAP directory by querying it for user profile data. If a user's profile can be retrieved, and you can bind to the LDAP directory as that user, the user is authenticated.

### Authentication with LDAP

When a filter is configured to authenticate a user against an LDAP repository using a user name and password combination, the flow is as follows:

1. A pooled LDAP connection to the repository selected in the **LDAP Directory** field is retrieved.
2. A search filter is run using the retrieved connection (for example, `(&(objectClass={User})(sAMAccountName={c05vc}))`). Attributes configured in the **Login Authentication Attribute** and **Authorization Attribute**
    fields are retrieved in this search.

    For example, if you select `Distinguished Name` from the drop-down list, the user's DName is retrieved from the LDAP directory. This uniquely identifies the user in the LDAP directory, and is used to bind to the directory so the user's password can be verified. The attribute specified in **Login Authentication Attribute** is used when you bind as any user. The value of the attribute specified in **Authorization Attribute** is stored in `authentication.subject.id`, and can be used by subsequent filters in the policy (for example, Authorization filters that authorize the authenticated user).
3. If no results are returned from the search, the user is not found in the directory. It is important that the administrator user configured on the **Configure LDAP Server** window has the ability to see the user that you are attempting to authenticate.
4. If multiple users are returned from the search, an attempt is made to bind to the directory using each **Login Authentication Attribute** value retrieved from the search, together with the password from the message.
5. If more than one user is authenticated correctly, an error is returned because you only want to authenticate a single user.
6. If no user is authenticated, an error is returned.
7. If a single user's **Login Authentication Attribute** value and password binds successfully to the directory, authentication has succeeded.
8. Any successful bind is immediately closed.

### Create an LDAP repository

To create a new LDAP repository, right-click **LDAP Repositories**, and select **Add a new Repository**. The details entered on the Authentication Repository dialog depend on the type of LDAP directory that you are using. Policy Studio has default entries for some of the more common LDAP directories, which are available from the drop-down lists. However, you can also connect to alternative LDAP directories.

The following subsections demonstrate how to configure this window for typical user searches on three common LDAP servers:

* Oracle Directory Server
* Microsoft Active Directory Server
* IBM Directory Server

#### Oracle Directory Server

To configure the Authentication Repository dialog for Oracle Directory Server (formerly iPlanet and Sun Directory Server), use the following settings:

* **Repository Name**: Enter a suitable name for this user store.
* **Directory Name**: Click **Add/Edit** to add details of your Oracle Directory Server.

The **User Search Conditions** section instructs API Gateway to search the LDAP tree according to the following conditions:

* **Base Criteria**: Enter where API Gateway should begin searching the LDAP directory (for example, `cn=Users, dc=qa, dc=vordel, dc=com`).
* **User Class**: Enter or select the name given by the particular LDAP directory to the *User* class. For Oracle Directory Server, select `'inetorgperson' LDAP Class`.
* **User Search Attribute**: The value entered depends on the type of LDAP directory to which you are connecting. When a user is stored in an LDAP directory, a number of user *attributes* are stored with that user. One of these attributes corresponds to the user name presented by the client for authentication. However, different LDAP directories use different names for that user attribute. For Oracle Directory Server, select `cn` from the drop-down list.
* **Allow Blank Passwords**: Select this to allow the use of blank passwords.

In the next section, you must specify the following:

* **Login Authentication Attribute**: In an LDAP directory tree, there must be one user attribute that uniquely distinguishes any one user from all the others. In Oracle Directory Server, the Distinguished name is referred to as the *entrydn* or Entry Domain Name. Select `Entry Domain Name` to uniquely identify the client authenticating to API Gateway.
* **Authorization Attribute**: When the client has been successfully authenticated, you can use any one of that user's stored attributes in a subsequent authorization filter. In this case, you want to use the user's Entry Domain Name (DName) for an authorization filter, so enter `entrydn` in the text box. However, you can enter any user attribute as long as the subsequent authorization filter supports it. The value of the LDAP attribute specified is stored in the `authentication.subject.id` message attribute.
* **Authorization Attribute Format**: Because any user attribute can be specified in the **Authorization Attribute** above, you must inform API Gateway of the type of this attribute. API Gateway uses this information internally in subsequent authorization filters. Select `X.509 Distinguished Name` from the drop-down list.

#### Microsoft Active Directory Server

This subsection describes how to configure the Authentication Repository
dialog for Microsoft Active Directory Server. The values enter here differ from those entered when interfacing to the Oracle Directory Server:

* **Repository Name**: Enter a suitable name for this search.
* **LDAP Directory**: Click **Add/Edit** to add details of your Active Directory Server.

The **User Search Conditions** instruct API Gateway to search the LDAP tree according to certain criteria. The values specified are different from those selected for Oracle Directory Server, because MS Active Directory Server uses different attributes and classes to Oracle Directory Server:

* **Base Criteria**: The base criteria specify the base object under which to search for the user's profile (for example, `cn=Users, dc=qa, dc=vordel, dc=com`).
* **User Class**: In Active Directory Server, the user class is called *User*, so select `'User' LDAP Class`.
* **User Search Attribute**: This specifies the name of the user attribute whose value corresponds to the user name entered by the client during a successful authentication process. With Active Directory Server, this attribute is called *givenName*, which represents the name of the user. Enter `givenName` in this field.
* **Login Authentication Attribute**: Enter the name of the user attribute that uniquely identifies the user in the LDAP directory. This attribute is the DName and is called *distinguishedName* in Active Directory Server. Select `Distinguished Name` from the drop-down list to uniquely identify the user. API Gateway authenticates the user name and password presented by the client against the values stored for the user identified in this field.
* **Authorization Attribute**: When the client has been successfully authenticated, API Gateway can use any of that user's stored attributes in subsequent authorization filters. Because most authorization filters require a DName, enter `Distinguished Name` in the text box. However, any user attribute could be entered here, as long as the subsequent authorization filter supports it.
* **Authorization Attribute Format**: API Gateway needs to know the format of the **Authorization Attribute**. Select `X.509 Distinguished Name` from the drop-down list.

#### IBM Directory Server

The configuration details for IBM Directory Server provide an example of a directory server that does not return a full DName as the result of a standard LDAP user search. Instead, it returns a *contextualized DName*, which is relative to the specified **Base Criteria**. In such cases, API Gateway can build up the full DName by combining the **Base Criteria** and the returned name. The following example shows how this works in practice.

If `C=IE` is specified as the **Base Criteria**, the IBM Directory Server returns `CN=niall, OU=Dev`, instead of the full DName, which is `C=IE, CN=niall, OU=Dev`. To enable API Gateway to do this, leave the **Login Authentication Attribute** field blank. API Gateway then automatically concatenates the specified **Base Criteria** (`C=IE`) with the contextualized DName returned from the directory server (`CN=niall, OU=Dev`) to obtain the fully qualified DName (`C=IE, CN=niall, OU=Dev`).

You can also leave the **Authorization Attribute** field blank to enable API Gateway to automatically use the fully qualified DName for subsequent authorization filters. You should select `X.509 Distinguished Name` from the **Authorization Attribute Format** drop-down list.

## Oracle Access Manager repositories

You can authorize an authenticated user for a particular resource against an Oracle Access Manager (OAM) repository. After successful authentication, OAM issues a Single Sign On (SSO) token, which can then be used instead of the user name and password.

To authenticate users against an OAM repository, right-click **Oracle Access Manager Repositories**, and select **Add a new Repository**. Configure the following fields on the Authentication Repository dialog:

* **Repository Name**: Enter an appropriate name for this repository.
* **Resource Request**: Configure the following settings for the resource request:
    * **Resource Type**: Enter the type of the resource for which you are requesting access. For example, for access to a Web-based URL, enter `http`.
    * **Resource Name**: Enter the name of the resource for which the user is requesting access. By default, this field is set to `//hostname${http.request.uri}`, which contains the     original path requested by the client.
    * **Operation**: In most access management products, users are authorized for a limited set of actions on the requested resource. For example, users with management roles may be     permitted to write (`HTTP POST`) to a certain web service, but users with junior roles might only have read access (`HTTP GET`) to the same service. Use this field to specify the     operation to which you want to grant the user access on the specified resource. By default, this is set to the `http.request.verb` message attribute, which contains the HTTP verb used     by the client to send the message to API Gateway (for example, `HTTP POST`).
    * **Include query string**: Select whether the query string parameters are used by the OAM server to determine the policy that protects this resource. This setting is optional if the     policies configured do not rely on the query string parameters.
    * **Client location**: If the client location must be passed to OAM for it to make its decision, you can enter a valid DNS name or IP address to specify this location.
    * **Optional Parameters**: You can add optional additional parameters to be used in the authentication decision. The available optional parameters include the following:
        * `ip`:IP address, in dotted decimal notation, of the client accessing the resource.
        * `operation`: Operation attempted on the resource (for HTTP resources, one of `GET`, `POST`, `PUT`, `HEAD`, `DELETE`, `TRACE`, `OPTIONS`, `CONNECT`, or `OTHER`).
        * `resource`: The requested resource identifier (for HTTP resources, the full URL).
        * `targethost`: The host (`host:port`) to which resource request is sent.

        One or more of these optional parameters may be required by certain authentication schemes, modules, or plugins configured in the OAM server. To determine which parameters to add,     see your OAM server configuration and documentation.
    * **Single Sign On**: Configure the following settings for single sign on:
    * **Create SSO Token**: Select whether to create an SSO token. This is selected by default.
    * **Store SSO Token in User Attribute**: Enter the name of the message attribute that contains the user's SSO token. This attribute is populated when authenticating to OAM using the HTTP Basic or HTTP Digest filter. By default, the SSO token is stored in the `oracle.sso.token` message attribute.
    * **Add SSO Token to User Attributes**: Select whether to add the SSO Token to user message attributes. This is selected by default.
* **OAM Access Server SDK Directory**: Enter the path to your OAM Access Server SDK directory. For more details on the OAM Access Server SDK, see your OAM documentation.

## Oracle Entitlements Server 10g repositories

You can authenticate and authorize a user for a particular resource against an Oracle Entitlements Server (OES) 10g repository.

For example, API Gateway can extract credentials from the message sent by the client, and delegate authentication to OES 10g. When the client has been authenticated, API Gateway queries OES 10g to see if the client is permitted to access the web service resource. When authentication and authorization have passed, the message is trusted and forwarded to the target web service.

To authenticate and authorize users against an OES 10g repository, right-click **Oracle Entitlements Server 10g Repositories**, and select **Add a new Repository**. Configure the following fields on the Authentication Repository dialog:

* **Repository Name**: Enter an appropriate name for this repository.
* **Oracle SSM Settings**: Click **Configure** to launch the **Oracle Security Service Module Settings** dialog. For details on configuring these settings, see [Configure Oracle Security Service Module settings (10g)](/docs/apim_policydev/apigw_poldev/security_server_settings/#configure-oracle-security-service-module-settings-10g).

## RADIUS repositories

{{< alert title="Note" color="primary" >}}This feature has been deprecated and will be removed in a future release.{{< /alert >}}

You can configure API Gateway to authenticate users in a Remote Authentication Dial In User Service (RADIUS) repository. RADIUS is a client-server network protocol that provides centralized authentication and authorization for clients connecting to remote services.

To authenticate users against a RADIUS repository, perform the following steps:

1. Right-click **RADIUS Repositories**, and select **Add a new Repository**.
2. In the Authentication Repository dialog, enter the RADIUS **Repository Name**.
3. On the **Client** tab, select the RADIUS clients that you wish to authenticate to the repository. For details on how to add clients to this list, see Configure RADIUS clients.
4. On the **Attributes** tab, click **Add** to add a RADIUS attribute. This is a name-value pair used to determine how access is granted. Examples include `User-Name`, `User-Password`, `NAS-IP-Address`, or `NAS-Port`).
5. In the RADIUS Attributes dialog, enter a **Name** for the attribute (for example, `User-Name`). You can select standard RADIUS attributes from the drop-down list, or enter a custom attribute.
6. Enter a **Value** for the attribute, and click **OK**.
7. Click **OK** in the Authentication Repository dialog when you have finished adding attributes.

## RSA Access Manager repositories

RSA Access Manager (formerly known as RSA ClearTrust) provides Identity Management and access control services for web applications. It centrally manages access to web applications, ensuring that only authorized users are allowed access to resources. Integration with RSA Access Manager requires RSA Access Manager SDK version 6.2, or server installation libraries.

To authenticate users against an RSA Access Manager repository, right-click **RSA Access Manager Repositories**, and select **Add a new Repository**. Configure the following fields on the Authentication Repository dialog:

* **Repository Name**: Enter an appropriate name for this repository.
* **Connection Details**: API Gateway can connect to a group of Access Manager *authorization servers* or *dispatcher servers*. When multiple Access Manager authorization servers are deployed for load-balancing purposes, API Gateway first connects to a dispatcher server, which returns a list of active authorization servers. An attempt is made to connect to one of these authorization servers using round-robin DNS. If the first dispatcher server in the connection group is not available, API Gateway attempts to connect to the dispatcher server with the next highest priority in the group, and so on.

    If a dispatcher server has not been deployed, API Gateway can connect directly to an authorization server. If the authorization server with the highest priority in the connection group is not available, API Gateway attempts to connect to the authorization server with the next highest priority, and so on. You can select the type of the connection group using the **Authorization Server** or **Dispatcher Server** radio button. All servers in the group must be of the same type.
* **Connection Group**: Select the **Connection Group** to use for authenticating clients. You can add connection groups under the **Environment Configuration > External Connections** node in Policy Studio. Expand the **Connection Sets** node, right-click **RSA Access Manager Connection Sets**, and select **Add a Connection Set**. For more details on adding and editing connection groups, see Configure connection groups.
* **Authentication Type**: Select one of the following authentication types for the connection:
    * HTTP Basic
    * Windows NT
    * RSA SecureID
    * LDAP
    * Certificate Distinguished Name

## Sun Access Manager repositories

{{< alert title="Note" color="primary" >}}This feature has been deprecated and will be removed in a future release. See [Oracle Access Manager repositories](#oracle-access-manager-repositories) instead.{{< /alert >}}

## Tivoli repositories

API Gateway can integrate with Tivoli Access Manager to authenticate users. To authenticate users against a Tivoli repository, right-click **Tivoli Repositories**, and select **Add a new Repository**.

For more details on how to configure API Gateway to communicate with a Tivoli server, see the
[API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).
