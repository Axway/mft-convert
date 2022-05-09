{
"title": "Configure SiteMinder/SOA Security Manager connections",
"linkTitle": "Configure SiteMinder/SOA Security Manager connections",
"weight": 10,
"date": "2019-10-17",
"description": "Create connections to CA SiteMinder and CA SOA Security Manager."
}

Before you start, you must register API Gateway as an agent in the CA SiteMinder Policy Server. For more details, see the [API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).

## Prerequisites

The following prerequisites apply to SiteMinder/SOA Security Manager connections.

### CA SiteMinder connections

Integration with CA SiteMinder requires CA SiteMinder SDK version 12.52-sp02 or later. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

1. Ensure that any SiteMinder binaries you may have previously added to API Gateway have been deleted.
2. Install CA SiteMinder SDK.
3. Copy the following `jar` files from the `java` directory of the CA SDK :
    * `cryptoj.jar`
    * `smagentapi.jar`
    * `smjavasdk2.jar`
4. Add the files to the following directory on API Gateway:

    ```
    INSTALL_DIR/apigateway/<platform>/lib
    ```

5. Restart API Gateway.

### CA SOA Security Manager connections

Integration with CA SOA Security Manager requires CA TransactionMinder SDK version 6.0 or later. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

For details on obtaining the required third-party binaries, see your CA product documentation.

#### Add third-party binaries to API Gateway

To add third-party binaries to API Gateway, perform the following steps:

1. Add the binary files as follows:
    * Add `.jar` files to the `INSTALL_DIR/apigateway/ext/lib`
        directory.
    * Add `.so` files to the `INSTALL_DIR/apigateway/<platform>/lib` directory.
2. Restart API Gateway.

#### Add third-party binaries to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies**
    in the Policy Studio main menu.
2. Click **Add**
    to select a JAR file to add to the list of dependencies.
3. Click **Apply**
    when finished. A copy of the JAR file is added to the `plugins`
    directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio with the `-clean` option. For example:

    ```
    cd INSTALL_DIR/policystudio/
    policystudio -clean
    ```

## Add a new connection

To add a new connection, in the node tree, click **Environment Configuration > External Connections**, right-click the **SiteMinder/SOA Security Manager Connection**
node, and select **Add a SiteMinder Connection** or **Add a SOA Security Manager Connection**.

To specify how API Gateway connects to SiteMinder, use the **SiteMinder Connection Details** dialog. To specify how API Gateway connects to CA SOA Security Manager, use the **SOA Security Manager Connection Details** dialog. The connection details to be configured are the same for both the SiteMinder and SOA Security Manager, with an additional setting for SOA Security Manager.

## SiteMinder and SOA Security Manager connection settings

This section describes settings that are common to both CA SiteMinder and CA SOA Security Manager connections.

**Agent Name**:
Enter the name of the agent to connect to SiteMinder or SOA Security Manager in the **Agent Name** field. This name *must* correspond to the name of an agent previously configured in the CA SiteMinder Policy Server.

**Agent Configuration Object**:
The name entered must match the name of the Agent Configuration Object (ACO) configured in the CA SiteMinder Policy Server. The only feature represented by the ACO parameters that API Gateway currently supports is the `PersistentIPCheck` setting. If you set the `PersistentIPCheck` ACO parameter to `yes`, API Gateway compares the IP address from the last request (stored in a persistent cookie) with the IP address in the current request to see if they match. If the IP addresses do not match, API Gateway rejects the request. This check is disabled if you set this parameter to `no`.

**SMHost.conf file created by smreghost**:
API Gateway host machine must be registered as an agent with SiteMinder or SOA Security Manager. To register the host, you must use the `smreghost`
tool on API Gateway machine. The tool creates a file called `SmHost.conf`, which you can then upload into API Gateway configuration using Policy Studio. For more details on creating the `SmHost.conf` file, see
[API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/)

To add the `SmHost.conf` file to API Gateway, ensure the `SmHost.conf` file is copied to the same machine running Policy Studio, click **Browse**, and select the the `SmHost.conf` file. You can select whether to use an `SmHost.conf` or `SmHost.cnf` file in the dialog. You can also enter the file name as an environment variable selector (for example, `${env.SMHOST}`).

After selecting the `SmHost.conf` configuration file, you can see the connection details in the text area.

## SOA Security Manager connection settings

This section describes details that are specific to CA SOA Security Manager connections only. In addition to the fields already described in the previous section, you must also configure the following field on the **SOA Security Manager Connection Details** dialog.

**XMLSDKAcceptSMSessionCookie**:
This setting controls whether the CA SOA Security Manager authentication filter accepts a single sign-on token for authentication purposes. The single sign-on token must reside in the HTTP header field named `SMSESSION` to authenticate using this mechanism. This token is created and updated when the CA SOA Security Manager authorization filter runs successfully.

When this check box is selected, the authentication filter allows authentication using a single sign-on token.

{{< alert title="Note" color="primary" >}}If no single sign-on token is present in the message, the authentication filter authenticates fully by gathering credentials from the request in whatever manner has been configured in the CA SOA Security Manager. When this check box is unselected, the authentication filter authenticates fully (it never allows authentication using a single sign-on token).{{< /alert >}}
