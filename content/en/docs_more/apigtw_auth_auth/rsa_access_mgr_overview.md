{
"title": "Integrate with RSA Access Manager",
"linkTitle": "Integrate with RSA Access Manager",
"weight":"140",
"date": "2020-01-20",
"description": "Configure API Gateway to authenticate and authorize end users using RSA Access Manager."
}

RSA Access Manager (formerly RSA ClearTrust) provides Identity Management and access control services for web applications. It centrally manages access to web applications, ensuring that only authorized users are allowed access to resources. You can configure API Gateway to act as a client to the RSA Access Manager, and leverage the user information stored in RSA Access Manager for user authentication and authorization.

API Gateway uses the **Access Manager** filter to query authorization information for a particular user on a given resource from RSA Access Manager and to make the authorization decision. If the user has been authorized for the resource in question, the request is allowed through to the service. Otherwise, the request is rejected. In addition, you can also configure an authentication filter to authenticate users against a RSA Access Manager authentication repository.

Integration with RSA Access Manager requires RSA Access Manager SDK version 6.2 or server installation libraries.

For more details on the product, see the [RSA Access Manager documentation](https://community.rsa.com/community/products/access-manager/server-62).

## RSA Access Manager server connections

API Gateway can connect to a number of Access Manager authorization servers or dispatcher servers using *connection sets* (connection groups). A connection set consists of a globally configured set of servers that API Gateway connects to. API Gateway round-robins between groups of external servers, providing a high degree of failover. If one of the servers becomes unavailable, API Gateway can use one of the other servers in the group.

You can deploy multiple Access Manager authorization servers for load-balancing purposes. In this case, API Gateway first connects to a dispatcher server that returns a list of active authorization servers. If the first dispatcher server in the connection group is not available, API Gateway attempts to connect to the dispatcher server with the next highest priority in the group. If a dispatcher server has not been deployed,API Gateway can connect directly to an authorization server. API Gateway then attempts to connect to one of the authorization servers.

API Gateway attempts to connect to the listed servers according to the priorities assigned to them. For example, a connection group could include two high-priority servers, one medium-priority server, and one low-priority server configured. If API Gateway can successfully connect to the two high-priority servers, it alternates requests only between these two servers, and the other servers in the group are not used. However, if *both* high-priority servers are unavailable, API Gateway attempts to connect to the medium-priority server. Only if the medium-priority server fails as well, API Gateway connects to the low-priority server.

## Flow description

1. An end user attempts to access a resource protected with RSA Access Manager on API Gateway.
2. API Gateway authenticates the end user against the RSA Access Manager authentication repository.
3. API Gateway requests RSA Access Manager to make the security decision for the user.
4. RSA Access Manager makes the security decision based on the user information, and returns the decision to API Gateway.
5. API Gateway enforces the security decision.
    * On success, API Gateway routes the message on to a configured target system.
    * On failure, API Gateway blocks the message and returns an error to the end user.

## Prerequisites

Before you start, you must have the following:

* API Gateway installed
* RSA Access Manager v6.2 installed and configured

## Configuration process

This guide provides a simple policy to demonstrate how API Gateway integrates with RSA Access Manager to authenticate and authorize users to specific resources. The example policy uses HTTP Basic to authenticate the end user, but you can replace it with another authentication mechanism, if required.

The following steps are required to integrate API Gateway with RSA Access Manager:

1. [Add RSA Access Manager binaries to API Gateway](#add-rsa-access-manager-binaries-to-api-gateway)
2. [Configure RSA Access Manager connection](#configure-rsa-access-manager-connection)
3. [Configure an RSA Access Manager authentication repository](#configure-an-rsa-access-manager-authentication-repository)
4. [Configure API Gateway policy](#configure-api-gateway-policy)

### Add RSA Access Manager binaries to API Gateway

You must copy RSA Access Manager libraries to API Gateway, so you must have RSA Access Manager installed on a server.

1. Copy the following files from the `lib` directory on your RSA Access Manager installation:
    * `axm-core-6.2.jar`
    * `cryptojce-6.1.jar`
    * `cryptojcommon-6.1.jar`
    * `jcm-6.1.jar`
2. Add the files to the `INSTALL_DIR/apigateway/ext/lib` directory on API Gateway:
3. Restart the gateway.

### Configure RSA Access Manager connection

This section describes how to configure a RSA Access Manager connection in Policy Studio.

The RSA Access Manager connection is configured as a connection set. A connection set consists of a globally configured set of servers that API Gateway connects to. You can reuse these global sets when configuring policies in Policy Studio.

{{< alert title="Note" color="primary" >}}All servers in the connection set must be of the same type (authorization servers or dispatcher servers). {{< /alert >}}

1. In the node tree, click **Environment Configuration > External Connections > Connection Sets**.
2. Select **RSA Access Manager Connection Sets**, and click **Add a Connection Set**.
3. Enter a name for the connection set (for example, `Authorization` or `Dispatch`), and click **Add** to add a server.
4. Enter the host name and port the server is listening on.
5. Select the security type for the server connection. The security type (**Clear**, **SSL (Anonymous)**, or **SSL Authentication**) you select must match the security requirement of the server.
6. If you selected **SSL Authentication**, click **Signing Key**, and select the certificate you want to use, then select **OK**.
7. To change the priority of a server in the set, select the server, and click **Up** or **Down**.
8. Repeat for all the servers you want to include in the connection set, and click **OK**.

To later view or edit your connection sets, click **Environment Configuration > External Connections > Connection Sets**, double-click **RSA Access Manager Connection Sets**, and select the connection you want.

For more details on the fields and options in this configuration window, see [Configure a connection group](/docs/apim_policydev/apigw_external_connections/common_connection_groups/#configure-a-connection-group).

### Configure an RSA Access Manager authentication repository

This section describes how to configure an RSA Access Manager authentication repository in Policy Studio.

1. In the node tree, click **Environment Configuration > External Connections > Authentication Repositories**.
2. Select **RSA Access Manager Repositories**, and click **Add a new Repository**.
3. Enter a name for the repository (for example, `RSA Access Manager Repository`).
4. Select which server type API Gateway connects to (**Authorization Server** or **Dispatch Server**).
5. Select the connection group that API Gateway connects to.
6. Select the authentication type you plan to use, and click **OK**.

For more details on the fields and options in this configuration window, see [RSA Access Manager repositories](/docs/apim_policydev/apigw_external_connections/common_user_store/#rsa-access-manager-repositories).

### Configure API Gateway policy

This section describes how to configure a policy for RSA Access Manager integration in Policy Studio.

The RSA Access Manager authentication repository is available from all authentication filters. Here, the example policy uses the **HTTP Basic** authentication filter to authenticate a client against a RSA Access Manager repository using a user name and password combination. You can configure a different authentication mechanism as required.

To start, add a new policy named, for example, `RSA Access Manager`.

1. Open the **Authentication** category in the palette, and drag a **HTTP Basic** filter onto the policy canvas.
2. Set the following, and click **Finish**:
    * **Credential Format**: `User Name`
    * **Allow client challenge**: Select this
    * **Repository Name**: The repository you configured (`RSA Access Manager Repository`)
3. Right click the filter, and select **Set as Start**.
4. Open the **Authorization** category in the palette, and drag an **Access Manager** filter onto the policy canvas.
5. Select which server type API Gateway connects to (**Authorization Server** or **Dispatch Server**), and select the connection group.
6. In **Server**, enter the name of the server that hosts the requested resource. The name entered must correspond to a preconfigured server name in RSA Access Manager.
7. In **Resource**, enter the name of the requested resource, and click **Finish**. The resource must be preconfigured in RSA Access Manager.
8. Connect the filters with a success path.
9. Click on the **Add Relative Path** icon to create a new relative path (for example, `/rsa`) that links to this policy.
10. Deploy the new configuration to API Gateway.

The policy looks like this:

![Policy](/Images/IntegrationGuides/auth_auth/rsa_policy.png)

The policy has the following flow:

* API Gateway authenticates the end user using HTTP Basic.
* API Gateway passes the end user’s credentials to RSA Access Manager.
* RSA Access Manager authenticates the end user, authorizes the end user for the particular resource, and sends a response to API Gateway.
* API Gateway relays the response to the end user.
