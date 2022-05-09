{
"title": "Integrate with LDAP",
"linkTitle": "Integrate with LDAP",
"weight":"120",
"date": "2020-01-20",
"description": "Configure API Gateway to authenticate and authorize end users using an LDAP directory server."
}

API Gateway interacts with the following directory servers using the Lightweight Directory Access Protocol (LDAP):

* Apache Directory Server 2.0.0-M7
* IBM Security Directory Server 6.4.0
* Microsoft Active Directory 2012
* Open LDAP Directory Server 2.4.11
* Oracle Directory Server Enterprise Edition 11g
* Oracle Internet Directory 10.1.4.0.1
* Oracle Virtual Directory 11g

## Flow description

![Diagram illustrating the LDAP integration](/Images/IntegrationGuides/auth_auth/ldap_integration.png)

The integration flow is as follows:

* API Gateway authenticates requests by searching the LDAP server for the user. API Gateway sends a bind request to the LDAP server to authenticate the user's credentials. For example, API Gateway extracts the user name and password from an HTTP Basic request and binds to LDAP using these credentials to check if the user name and password are valid.
* API Gateway retrieves roles or attributes from LDAP by searching the LDAP server for entries and storing retrieved values on the whiteboard to be used by other filters.

When a request has been authenticated, API Gateway can insert a SAML token into the message to show that authentication has occurred. This SAML token can then be consumed by downstream applications to extract information about the original client that sent the request.

## Prerequisites

Before you start, you must have API Gateway and your chosen directory server installed and configured.

## Configuration process

The configuration process has the following steps:

1. [Check the details for the directory server](#check-the-details-for-the-directory-server)
2. [Configure an LDAP connection](#configure-an-ldap-connection)
3. [Configure an LDAP authentication repository](#configure-an-ldap-authentication-repository)
4. [Configure API Gateway policy](#configure-api-gateway-policy)
5. [Secure the connection to the directory server](#secure-the-connection-to-the-directory-server)

### Check the details for the directory server

Before you can configure API Gateway to connect to your directory server, you must check that you have the following connection details and the user search conditions for the directory server.

#### Connection details

**LDAP URL**:

The LDAP URL containing the host name and port that your directory server is listening on. For example:

* `ldap://192.168.0.129:10389`
* `ldaps://192.168.0.129:10636`

**User name**:

The distinguished name (DN) of the user that API Gateway uses when connecting to the directory server. The format may vary depending on your directory server. For example:

* `uid=admin,ou=system`
* `cn=root`
* `CN=Administrator,CN=users,DC=axway,DC=com`  
* `cn=admin,o=Axway,I=Dublin4,st=Dublin,C=IE`

**Password**:

The password of the user API Gateway uses. For example:

* `secret`

Ensure you have these details at hand when you start configuring the connection between the gateway and the directory server.

#### Check the user search conditions

API Gateway searches the directory server based on the details you define when configuring the LDAP authentication repository for the gateway.

1. Connect and log in to the directory server using an LDAP browser.
2. Decide how you want to search the repository and note down the following details:

**Base Criteria**:

The root DN to use when running queries against the directory server. For example:

* `ou=system`
* `CN=LOCALHOST`
* `CN=users,DC=axwayqa,DC=com`
* `ou=R&D,o=Axway,I=Dublin4,st=Dublin,C=IE`  

**User Class**:

The object class searched in the directory server. Each object in an LDAP directory has at least one object class associated with it. For example:

* `inetOrgPerson`
* `User`
* `Person`

**User Search Attribute**:

The attribute that contains the user name. For example:

* `uid`
* `cn`

The format of the setting values may vary depending on your directory server.

When searching the directory server, API Gateway generates a search based on the User Class and User Search Attribute values:

```
(&(objectclass=<User Class>)(<User Search Attribute>=<value>))
```

For example:

```
(&(objectclass=inetOrgPerson)(uid=admin))
```

This example searches the repository as follows:

* Search for an object of type `inetOrgPerson` where the attribute `uid` has the value `admin`. Start from under the value entered for Base Criteria.
* If the user is found, return the DN.

API Gateway binds to the directory server using the returned DN and the password extracted from the request. A successful bind indicates that the user name and password are valid and the user has been authenticated.

### Configure an LDAP connection

API Gateway binds to the directory server using the connection details and user credentials specified in the LDAP connection. Usually, the connection details include the user name and password of an administrator user who has read access to all users in the LDAP server you want to authenticate or retrieve attributes for.

Follow these steps to configure the connection between the gateway and your directory server in Policy Studio:

1. In the node tree, click Environment Configuration > External Connections.
2. Right-click LDAP Connections, and click Add a LDAP Connection.
3. Enter a name for your connection (for example, LDAP Connection), and set the following values:
    * **URL**: `<LDAP url>`
    * **Type**: `Simple`
    * **User Name**: `<user name for API Gateway>`
    * **Password**: `<password for API Gateway>`
4. Click **Test Connection** to verify that the connection to the directory server is configured successfully.
5. Click **OK** to save the entry in **LDAP Connections**.

### Configure an LDAP authentication repository

API Gateway requires an authentication repository to authenticate a user using the user name and password. Then, it compares the credentials the user presents to those stored in the authentication repository. If the gateway can retrieve a user's profile and bind to the directory server as that user, the user is authenticated.

You can leverage your existing directory server and configure the gateway to query it for user profile data.

Follow these steps to configure integration between the gateway and your directory server in Policy Studio.

1. In the node tree, click **Environment Configuration > External Connections > Authentication Repositories**.
2. Right-click **LDAP Repositories**, and click **Add a new Repository**.
3. Enter a name for your repository (for example, LDAP Repository), and set the following values:
    * **LDAP Directory**: The LDAP connection you created (`LDAP Connection`).
    * **Base Criteria**: `<Base Criteria of your directory server>`.
    * **User Class**: `<User Class of your directory server>`.
    * **User Search Attribute**: `<User Search Attribute of your directory server>`.
    * **Login Authentication Attribute**: This setting is optional. If left blank, API Gateway uses a default name as the authentication attribute. If not blank, the specified     attribute is retrieved in the initial search for the user.
    * **Authorization Attribute**: Enter the attribute stored LDAP for the user you want to use for authorization.
    * **Authorization Attribute Format**: Depending on the format of your selected authorization attribute, select either `User Name` or `X.509 Distinguished Name`.
4. Click **OK** to save the configuration.

### Configure API Gateway policy

This section describes the steps required to configure integration between API Gateway and your directory server in Policy Studio.

#### Create an authentication policy

You must configure an authentication policy to set the gateway to authenticate against your directory server. This example uses the **HTTP Basic** authentication filter with a user name and password combination, but you can configure a different authentication mechanism as required.

1. Add a new policy named, for example, `LDAP Authentication`.
2. Open the **Authentication** category in the filter palette, and drag an **HTTP Basic** filter onto the policy canvas.
3. Set the following, and click **Finish**:
    * **Credential Format**: `User Name`
    * **Repository Name**: The LDAP repository you configured (`LDAP Repository`)
4. For more details on the fields and options in this configuration window, see .
5. Click on the **Add Relative Path** icon to create a new relative path (for example, `/ldap`) that links to this policy, and deploy the policy to API Gateway.

#### Test the policy

To test that the policy works and trace the operation in the log files, add a **Reflect** and a **Trace** filter to the policy. These filters are not required in the production environment.

1. Open the **Utility** category in the palette, drag a **Reflect** filter onto the policy canvas, and click **Finish**.
2. Drag a **Trace** filter onto the policy canvas, and click **Finish**.
3. Connect the filters with success paths.

    ![Diagram showing the three filters connected with success paths](/Images/IntegrationGuides/auth_auth/ldap_policy_test.png)

4. Deploy the updated configuration to API Gateway.
5. Test the policy (for example, using the `sr` command). See the for more information on testing tools.

#### Retrieve attributes from the directory server

You can enhance your LDAP policy to retrieve attributes for the user after a successful authentication. The gateway stores the credentials of the user in the `authentication.subject.id` message attribute. Typically, this contains the Distinguished Name (DN) or user name of the authenticated user. The gateway extracts the name from the message attribute and uses the name to query the directory server.

1. Open the **Attributes** category in the filter palette, and drag a **Retrieve from Directory Server** filter onto the policy canvas.
2. In **LDAP Directory**, select the LDAP connection you configured (`LDAP connection`).
3. Select **From Selector Expression**, and enter `${authentication.subject.id}` to obtain the value of this message attribute at runtime.
4. In **Base Criteria**, enter the Base Criteria of your directory server.
5. In **Search Filter**, enter the following:

    ```
    (&(objectclass=<User Class>)(<User Search Attribute>=<value>))
    ```

    For example:

    ```
    (&(objectclass=inetOrgPerson)(CN=Administrator))
    ```

6. In **Attribute Name** table, list the attributes you want to retrieve from the user profile. If no attributes are explicitly listed, API Gateway extracts all user attributes. The retrieved attributes are set to `attribute.lookup.list` for that user.
7. For example, a user `CN=Administrator` could have an attribute `memberOf` with the value `CN=Group Policy Creator Owners`. If you choose to retrieve the Attribute Name `memberOf`, API Gateway returns the value `CN=Group Policy Creator Owners`.
8. Click **Finish**.
9. Connect the filters with success paths.

    ![Connect the filters with success paths](/Images/IntegrationGuides/auth_auth/ldap_policy_retrieve.png)

#### Validate a retrieved attribute

After retrieving user attributes, API Gateway can validate a retrieved attribute value using an **Evaluate Selector** filter. Based on whether the filter fails or passes, API Gateway can then make a decision in the policy, such as allow the user to access a particular resource.

In this example, API Gateway checks if the user `CN=Administrator` belongs to the group "Policy Creator Owners".

1. Open the **Utility** category in the palette, and drag an **Evaluate Selector** filter onto the policy canvas.
2. In the **Expression** field, enter the selector expression to evaluate, and click **Finish**:

    ```
    ${<user>[<place in attribute.lookup.list>].<Attribute Name>.contains("<attribute value>")}
    ```

    For example:

    ```
    ${Administrator[0].memberOf.contains("CN=Group Policy Creator Owners,CN=Users,DC=axwayqa,DC=com")}
    ```

    The selector expression reads as follows:

    * Check if the retrieved attribute `memberOf` in the `attribute.lookup.list` for the user `Administrator` contains the value `CN=Group Policy Creator Owners,CN=Users,   C=axwayqa,   DC=com`.
    * If this matches, the filter passes.

3. Connect the filters with success paths. For testing, connect the **Evaluate Selector** filter between the **Trace** and **Reflect Message** filters.

    ![Diagram showing the filters connected with success paths](/Images/IntegrationGuides/auth_auth/ldap_policy_evaluate.png)

This example prints the following in the trace file:

```
DEBUG 14/02/2013 16:54:21.205 }
DEBUG   14/02/2013 16:54:21.205  user {
DEBUG   14/02/2013 16:54:21.205  Value: [CN=Administrator: null:null:
{memberof=memberOf: CN=Group Policy Creator Owners,CN=Users,DC=axwayqa,DC=com,
CN=Domain Admins,CN=Users,DC=axwayqa,DC=com, CN=Enterprise Admins,CN=Users,DC=axwayqa,DC=com,
CN=Schema Admins,CN=Users,DC=axway,DC=com, CN=Administrators,CN=Builtin,DC=axwayqa,DC=com}]
```

#### Insert a SAML token

You can extend the authentication to downstream web services, if required. After the end user is successfully authenticated and the attributes retrieved, API Gateway adds a SAML authentication assertion to the response message for the web services to consume.

1. Open the **Authentication** category in the palette, and drag an **Insert SAML Authentication Assertion** filter onto the policy canvas.
2. On the **Assertion details** tab, select any issuer on the **Issuer Name** list, and set the expiry date for the SAML authentication assertion.
3. On the **Assertion Location** tab, make sure that **Add to WS-Security Block with SOAP Actor/Role** is selected and **SOAP Actor/Role** set to **Current Actor/Role Only**.
4. On the **Advanced** tab, select **Insert SAML Attribute Statement** and **Indent**, then click **Finish**.
5. Connect the filters with success paths.

    ![Connect the filters with success paths](/Images/IntegrationGuides/auth_auth/ldap_policy_final.png)

For more details on how to configure an insert SAML Authentication Assertion filter, see [Insert SAML authentication assertion filter](/docs/apim_policydev/apigw_polref/authn_common/#insert-saml-authentication-assertion-filter)

### Secure the connection to the directory server

For security, you can use an SSL connection between the gateway and your directory server. This section describes how to configure this in Policy Studio.

API Gateway and Policy Studio require the CA certificate of your directory server. You must import the CA certificate into the API Gateway and Policy Studio Java keystores.

#### Add the LDAP server certificate to the API Gateway certificate store

1. In the node tree, click **Environment Configuration > Certificates and Keys > Certificates**.
2. Click **Create/Import > Import Certificate**, and select the CA certificate of your directory server.
3. In **Alias Name**, give the certificate a name or click **Use Subject** to use the subject name , then click **OK**.

#### Add the LDAP server certificate to the API Gateway Java keystore

1. In the node tree, click **Environment Configuration > Certificates and Keys > Certificates**.
2. Click **Keystore**, click the browse button next to the **Keystore** field, and browse to the **keystore** file:

    ```
    INSTALL_DIR/apigateway/posix/jre/lib/security/cacerts
    ```

3. Click **Open**, and enter the keystore password.
4. Click **Add to keystore**.
5. Select the CA certificate of your directory server, and click **OK**.
6. Give a name to the certificate, or use the default name, and click **OK**.
7. Click **OK** to save the configuration, and deploy the updated configuration to API Gateway.

#### Add the LDAP server certificate to the Policy Studio Java keystore

1. In the node tree, click **Environment Configuration > Certificates and Keys > Certificates**.
2. Click **Keystore**, click the browse button next to the **Keystore** field, and browse to the keystore file:

    ```
    INSTALL_DIR/policystudio/posix/jre/lib/security/cacerts
    ```

3. Click **Open**, and enter the keystore password.
4. Click **Add to keystore**.
5. Select the CA certificate of your directory server, and click **OK**.
6. Give a name to the certificate, or use the default name, and click **OK**.
7. Click **OK** to save the configuration, and restart Policy Studio.

#### Configure the LDAP connection over SSL

1. In the node tree, click **Environment Configuration > External Connections > LDAP Connections**.
2. Right-click the appropriate directory server connection, and click **Edit**.
3. In the **URL** field, enter the LDAPS host name and port. For example:

    ```
    ldaps://ldap_host:636
    ```

4. Select the **SSL Enabled** check box, and click **Test Connection**.
5. After a successful connection, click **OK**, and deploy the deploy the updated configuration to API Gateway.
