{
"title": "Configure LDAP directories",
"linkTitle": "Configure LDAP directories",
"weight": 6,
"date": "2019-10-17",
"description": "Configure a connection to an LDAP or LDAPS directory."
}

A filter that uses an LDAP directory to authenticate a user or retrieve attributes for a user must have an LDAP directory associated with it. You can use the **Configure LDAP Server**
dialog to configure connection details of the LDAP directory. Both LDAP and LDAPS (LDAP over SSL) are supported.

When a filter that uses an LDAP directory is run for the first time after a server restart, the server binds to the LDAP directory using the connection details configured on the **Configure LDAP Server**
dialog. Usually, the connection details include the user name and password of an administrator user who has read access to all users in the LDAP directory for whom you wish to retrieve attributes or authenticate.

## General configuration

Configure the following general LDAP connection settings:

**Name**:
Enter or select a name for the LDAP filter in the drop-down list.

**URL**:
Enter the URL location of the LDAP directory. The URL is a combination of the protocol (LDAP or LDAPS), the IP address of the host machine, and the port number for the LDAP service. By default, port `389`
is reserved for LDAP connections, while port `636`
is reserved for LDAPS connections. For example, the following are valid LDAP directory URLs:

```
ldap://192.168.0.45:389
ldaps://145.123.0.28:636
```

**Cache Timeout**:
Specifies the timeout for cached LDAP connections. Any cached connection that is not used in this time period is discarded. Defaults to 300000 milliseconds (5 minutes). A cache timeout of 0 means that the LDAP connection is cached indefinitely and never times out.

**Cache Size**:
Specifies the number of cached LDAP connections. Defaults to 8 connections. A cache size of 0 means that no caching is performed.

## Authentication configuration

If the configured LDAP directory requires clients to authenticate to it, you must select the appropriate authentication method in the **Authentication Type**
field. When the API Gateway connects to the LDAP directory, it is authenticated using the selected method. Choose one of the following authentication methods:

* None
* Simple
* Digest-MD5
* External

{{< alert title="Note" color="primary" >}}If any of the following authentication methods connect to the LDAP server over SSL, that server's SSL certificate must be imported into the API Gateway certificate store.{{< /alert >}}

### None

No authentication credentials need to be submitted to the LDAP server for this method. In other words, the client connects anonymously to the server. Typically, a client is only allowed to perform read operations when connected anonymously to the LDAP server. It is not necessary to enter any details for this authentication method.

### Simple

*Simple*
authentication involves sending a user name and corresponding password in clear text to the LDAP server. Because the password is passed in clear text to the LDAP server, it is recommended to connect to the server over an encrypted channel (for example, over SSL).

It is not necessary to specify a **Realm**
for the Simple authentication method. The realm is only used when a hash of the password is supplied (for Digest-MD5). However, in cases where the LDAP server contains multiple realms, and the specified user name is present in more than one of these realms, it is at the discretion of the specific LDAP server as to which user name *binds*
to it.

Select the **SSL Enabled**
checkbox to force the API Gateway to connect to the LDAP directory over SSL.

To successfully establish SSL connections with the LDAP server, you must import the server's certificate into the JRE truststore of API Gateway. The default truststore is located under `INSTALL-DIR/platform/jre/lib/security/cacerts`. To add the server certificate, use a keytool command like the following:

```
keytool -import -trustcacerts -keystore INSTALL-DIR/apigateway/platform/jre/lib/security/cacerts
-storepass TRUSTSTORE_PASSWORD -alias ldap_server -import -file CERTIFICATE_FILE
```

`TRUSTSTORE_PASSWORD` is the default `cacerts` password, which is available in the JRE documentation.

### Digest-MD5

With *Digest-MD5*
authentication, the server generates some data and sends it to the client. The client encrypts this data with its password according to the MD5 algorithm. The LDAP server then uses the client's stored password to decrypt the data and hence authenticate the user.

The **Realm**
field is optional, but may be necessary in cases where the LDAP server contains multiple realms. If a realm is specified, the LDAP server attempts to authenticate the user for the specified realm only.

### External

*External*
authentication enables you to use client certificate-based authentication when connecting to an LDAP directory. When this option is selected, you must select a client certificate from the API Gateway certificate store. The **SSL Enabled**
checkbox is selected automatically. Click the **Signing Key** button to select the client certificate to use to mutually authenticate to the LDAP server.

{{< alert title="Note" color="primary" >}}This means that you must specify the **URL**
field using LDAPS (for example, `ldaps://145.123.0.28:636`). The user name, password, and realm fields are not required for external authentication.{{< /alert >}}

## Test the LDAP connection

When you have specified all the LDAP connection details, click the **Test Connection**
button to verify that the connection to the LDAP directory is configured successfully. This enables you to detect any configuration errors at design time, rather than at runtime.

For LDAPS (LDAP over SSL) connections, the LDAP server's certificate must be imported into the Policy Studio JRE trusted store. Perform the following steps in Policy Studio:

1. Select the **Environment Configuration** > **Certificates and Keys** > **Certificates**
    node in the Policy Studio tree.
2. In the **Certificates**
    panel on the right, click **Create/Import**, and click **Import Certificate + Key**.
3. Browse to the LDAP server's certificate file, and click **Open**.
4. Click **Use Subject**
    on the right of the **Alias Name**
    field, and click **OK**.\
    The LDAP server's certificate is now imported into the **Certificate Store**, and must be added to the **Java keystore**.
5. In the **Certificates**
    panel, select the certificates that you wish the JRE to trust.
6. Click **Keystore**, and browse to the `cacerts`
    file in the following directory:

    ```
    INSTALL_DIR\policystudio\jre\lib\security
    ```

7. Select the `cacerts`
    file.
8. You are prompted for a password. (This is the default `cacerts` password which is available in the JRE documentation)
9. Click **OK**.
10. On the **Keystore** window, click **Add to keystore**.
11. Select the LDAP certificate imported previously.
12. Click **OK**.
13. Restart Policy Studio.
14. You can now click **Test Connection**
    to test the connection to the LDAP directory server over SSL.

## Additional JNDI properties

You can also specify optional JNDI properties as simple name-value pairs. Click the **Add**
button to specify properties in the dialog.
