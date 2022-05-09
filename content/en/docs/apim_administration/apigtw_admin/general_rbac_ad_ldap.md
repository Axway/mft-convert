{
"title": "Authentication and RBAC with Active Directory",
  "linkTitle": "Authentication and RBAC with Active Directory",
  "weight": "126",
  "date": "2019-10-14",
  "description": "Use LDAP to authenticate and perform Role-Based Access Control (RBAC) of API Gateway management services, and reconfigure API Gateway to use a Microsoft Active Directory LDAP repository."
}

This section uses the sample **Protect Management Interfaces (LDAP)** policy, meaning that the gateway uses an LDAP repository instead of the local Admin User store for authentication and RBAC of users attempting to access the gateway management services.

{{< alert title="Note" color="primary" >}}If you have multiple Admin Node Managers in your topology, you must ensure that you apply the configuration changes to each Admin Node Manager.{{< /alert >}}

## Create an Active Directory group

To create a new user group in Active Directory, perform the following example steps:

1. Click **Start** > **Administrative Tools** > **Active Directory Users and Computers**.
2. On the **Users** directory, right-click, and select **New** > **Group**.
3. Enter the Group name (for example, `APIGatewayAdministrator`).

You should add groups for the following default RBAC roles to give the LDAP users appropriate access to the API Gateway management services:

* `API Gateway Administrator`
* `API Gateway Operator`
* `Deployer`
* `KPS Administrator`
* `Policy Developer`

These RBAC roles are located in the `roles` section of the following file:

```
INSTALL_DIR\apigateway\conf\acl.json
```

![Create an Active Directory LDAP group](/Images/APIGateway/create_ldap_grp.png)

You can view the newly created groups using an LDAP browser.

## Create an Active Directory user

You will most likely be unable to create an `admin` user with a default password because the default password is not strong enough to be accepted by Active Directory. Using **Active Directory Users and Computers**, perform the following steps:

1. On the **Users** directory, right-click, and select **New** > **User**.
2. Enter a user name (for example, `admin`).

    ![Creating an Active Directory LDAP User](/Images/APIGateway/create_ldap_user.gif)

3. Click **Next**.
4. Enter a password (for example, `Axway123`).
5. Select **User cannot change password** and **Password never expires**.
6. Ensure **User must change password at next logon** is not selected.
7. Click **Next**.
8. Click **Finish**.

### Add the user to the group

To make the user a member of the group using **Active Directory Users and Computers** , perform the following steps:

1. Select the **APIGatewayAdministrator** group, right-click, and select **Properties**.
2. Click the **Members** tab.
3. Click **Add**.
4. Click **Advanced**.
5. Click **Find Now**.
6. Select the `admin` user.
7. Click **OK**.

You can view the newly created user using an LDAP browser.

{{< alert title="Note" color="primary" >}}The `memberOf` attribute points to the Active Directory group. The user has an instance of this attribute for each group they are a member of.{{< /alert >}}

## Create an LDAP connection

To create an new LDAP Connection, perform the following steps:

1. In Policy Studio, create a new project based on the Admin Node Manager configuration. For example:

    ```
    INSTALL_DIR\apigateway\conf\fed
    ```

2. In Policy Studio tree, select **Environment Configuration** > **External Connections** > **LDAP Connections**.
3. Right-click, and select **Create an LDAP Connection**.
4. Complete the fields in the dialog as appropriate. The specified **User Name** should be an LDAP administrator that has access to search the full directory for users. For example:

    ![Create an LDAP connection](/Images/APIGateway/create_ldap_cxn.gif)

5. Click **Test Connection** to ensure the connection details are correct.

## Create an LDAP repository

To create an new LDAP Repository, perform the following steps:

1. In the Policy Studio tree, select **Environment Configuration** > **External Connections** > **Authentication Repository Profiles** > **LDAP Repositories**.
2. Right-click, and select **Add a new Repository**.
3. Complete the following fields in the dialog:

* **Repository Name**: Enter an appropriate name for the repository.
* **LDAP Directory**: Use the LDAP directory created in [Create an LDAP connection](#create-an-ldap-connection).
* **Base Criteria**: Enter the LDAP node that contains the users.
* **User Search Attribute**: Enter `cn`. This is the username entered at login time (in this case, `admin`).
* **Authorization Attribute**: Enter `distinguishedName`. This is the username entered at login time (`admin`). The `authentication.subject.id` message attribute is set to the value of this LDAP attribute (see example below). The `authentication.subject.id` is used as the base criteria in the filter that loads the LDAP groups (the user’s roles). This enables you to narrow the search to a particular user node in the LDAP tree. For more details, see the **Retrieve Attributes from Directory Server** filter in [Configure a test policy for LDAP authentication and RBAC](#config-test-policy).

An example value of the `authentication.subject.id` message attribute is as follows:

```
CN=admin, CN=Users,DC=kerberos3,DC=qa,DC=vordel,DC=comn
```

![Creating an LDAP Repository](/Images/APIGateway/create_ldap_repo.gif)

### Connect to other LDAP repositories

This topic uses Microsoft Active Directory as an example LDAP repository. Other LDAP repositories such as Oracle Directory Server (formerly iPlanet and Sun Directory Server) and OpenLDAP are also supported.

For an example of querying an Oracle Directory Server repository, see the **Retrieve Attributes from Directory Server** filter in [Configure a test policy for LDAP authentication and RBAC](#config-test-policy). For details on using OpenLDAP, see [Authentication and RBAC with OpenLDAP](/docs/apim_administration/apigtw_admin/general_rbac_openldap).

## Configure a test policy for LDAP authentication and RBAC {#config-test-policy}

To avoid locking yourself out of Policy Studio, you can configure an example test policy for LDAP authentication and RBAC, which is invoked when a test URI is called on the server (and not a management services URI). Policy Studio provides an example policy named **Protect Management Interfaces (LDAP)** when the Admin Node Manager configuration is loaded.

You must edit this policy to change it from using the sample LDAP connection to the one that you created in [Create an LDAP connection](#create-an-ldap-connection). You must also change it from using the sample authentication repository to the authentication repository that you created in [Create an LDAP repository](#create-an-ldap-repository). Then in [Use the LDAP policy to protect management services](#use-the-ldap-policy-to-protect-management-services), you must hook up this new LDAP policy to the `/` path.

### Configure the example test policy

Perform the following steps:

1. In Policy Studio, create a new project based on the Admin Node Manager configuration. For example:

    ```
    INSTALL_DIR\apigateway\conf\fed
    ```

2. For the example policy, select **Policies** > **Management Services** > **Sample LDAP Policies** > **Protect Management Interfaces (LDAP)** when the Admin Node Manager configuration is loaded. This policy is summarized at a high-level as follows:

    * **Scripting Language** filter returns `true` if the Node Manager is the Admin Node Manager. This enables subsequent HTTP authentication and RBAC, and updates the HTTP headers based on the user role. Otherwise, this filter calls the **Call Internal Service (no RBAC)** filter without updating the HTTP headers.
    * **HTTP Basic Authentication** filter verifies the user name and password against the LDAP repository configured in [Create an LDAP repository](#create-an-ldap-repository).
    * **Retrieve Attributes from Directory Server** filter finds the LDAP groups that the user belongs to using the LDAP directory connection configured in [Create an LDAP connection](#create-an-ldap-connection).
    * **Management Services RBAC** filter reads the user roles from the configured message attribute (`authentication.subject.role`). This returns `true` if one of the roles has access to the management service currently being invoked, as defined in the `acl.json` file. Otherwise, this returns `false` and the **Return HTTP Error 403:Access Denied (Forbidden)** policy is called because the user does not have the correct role.

3. You must edit some HTTP-based filters and change them from using the **Sample Active Directory Repository** to using the repository that you created in [Create an LDAP repository](#create-an-ldap-repository). This repository is referenced in the following filters in the example policy:

    * **Authenticate login attempt**
    * **HTTP Basic**

    You can view all referenced filters by selecting **Environment Configuration** > **External Connections** > **LDAP Repositories** > **Sample Active Directory Repository** > **Show all References**.

4. You must edit the LDAP-based filters and change them from using the **Sample Active Directory Connection** to using the LDAP connection that you created in [Create an LDAP connection](#create-an-ldap-connection). This repository is referenced in the following components in the example policy:

    * **Read Roles from Directory Server**
    * **Re-Read Roles from Directory Server**
    * **Sample Active Directory Repository**

5. Similarly, you can view all referenced components by selecting **Environment Configuration** > **External Connections** > **LDAP Repositories** > **Sample Active Directory Connection** > **Show all References**.

### Test the policy configuration

To test this policy configuration, perform the following steps:

1. Update the `acl.json` file with the new LDAP group, for example:

    ```
    "CN=APIGatewayAdministrator,CN=Users,DC=kerberos3,DC=qa,DC=vordel,DC=com" :[
    "emc", "mgmt", "mgmt_modify", "dashboard", "dashboard_modify", "deploy" "config",
    "monitoring", "events", "traffic_monitor", "settings", settings_modify", "logs" ]
    ```

2. Update the `adminUsers.json` file with the new role, for example:

    ```
    {
    "name" :"CN=APIGatewayAdministrator,CN=Users,DC=kerberos3,DC=qa,DC=vordel,
        DC=com","id" :"role-8"
    }
    ```

3. And increase the number of roles, for example:

    ```
    "uniqueIdCounters" :{"Role" :9,"User" :2},
    ```

4. In the Policy Studio tree, select **Environment Configuration** > **Listeners** > **Node Manager** > **Add HTTP Services**, and enter a service name (for example, `LDAP Test`).
5. Right-click the HTTP service, and select **Add Interface** > **HTTP**.
6. Enter an available port to test the created policy (for example, `8888`), and click **OK**.
7. Right-click the HTTP service, and select **Add Relative Path**.
8. Enter a relative path (for example, `/test`).
9. Set the **Path Specify Policy**    to the **Protect Management Interfaces (LDAP)** policy, and click **OK**.
10. Close the connection to the Admin Node Manager file, and restart the Admin Node Manager so it loads the updated configuration.
11. Use an API test client such as Postman or `curl` to call `http://localhost:8888/test`.
12. Enter the HTTP Basic credentials (for example, username `admin` and password `Axway123`). If authentication is passed, the Admin Node Manager should return an HTTP 404 code (not found)

{{< alert title="Note" color="primary" >}}Do not use the **Admin Users** tab in the API Gateway Manager to manage user roles because these are managed in LDAP.{{< /alert >}}

## Use the LDAP policy to protect management services

If the authentication and RBAC filters pass, you can now use this policy to protect the management interfaces. To ensure that you do not lock yourself out of the server, perform the following steps:

1. Make a copy of the `/apigateway/conf/fed` directory contents from the server installation and put it into a directory accessible from Policy Studio.
2. In Policy Studio, select **File** > **New project**, enter a name, and click **Next**.
3. Select **From existing configuration**, then click **Next**.
4. Browse to the copy of `/conf/fed`, and click **Finish** to make a new project out of this config.

    Policy Studio then opens the project files located in `/apiprojects/YOUR_PROJECT_NAME` for editing. This means that no change is applied to the original files, located in `/conf/fed`, and they can be used as a backup.
5. Click **Environment Configuration** > **Listeners** > **Node Manager** > **Management Services** node, select the `/` and the `/configuration/deployments` relative paths, and set the **Path Specify Policy** to the **Protect Management Interfaces (LDAP)** policy.
6. Remove the previously created `LDAP Test` HTTP Services, and close the connection to the file.
7. Copy all of the `.XML` and `.MD5` files from `/apiprojects/YOUR_PROJECT_NAME` directory back to the Admin Node Manager’s `/conf` directory.

    This overwrites the files that were previously in the `/conf` directory.
8. Restart the Admin Node Manager process.
9. Start Policy Studio and connect to the Admin Node Manager using `admin` and password `Axway123` (the LDAP user credentials). You should now be able to edit the gateway configurations as usual.

## Add an LDAP user with limited access to management services

You can add an LDAP user with limited access to management services. For example, assume there is already a user named `Fred` defined in Active Directory. `Fred` has the following DName:

```
CN=Fred,CN=Users,DC=kerberos3,DC=qa,DC=vordel,DC=com
```

`Fred` belongs to an existing LDAP group called `TraceAnalyzers`. He can also belong to other LDAP groups that have no meaning for RBAC in the API Gateway. The `TraceAnalyzers`
LDAP group has the following DName:

```
CN=TraceAnalyzers,CN=Users,DC=kerberos3,DC=qa,DC=vordel,DC=com
```

The user `Fred` should be able to read server trace files in a browser. No other access to management services should be given to `Fred`.

### Add limited access rights

You must perform the following steps to allow `Fred` to view the trace files:

1. Add the following entry in the `roles` section in the `acl.json` file:

    ```
    "CN=TraceAnalyzers,CN=Users,DC=kerberos3,DC=qa,DC=vordel,DC=com" :
    [ "emc", "mgmt", "logs" ]
    ```

2. Update the `adminUsers.json` file with the new role as follows:

    ```
    {
    "name" :"CN=TraceAnalyzers,CN=Users,DC=kerberos3,DC=qa,DC=vordel,
        DC=com","id" :"role-8"
    }]
    ```

    And increase the number of roles, for example:

    ```
    "uniqueIdCounters" : {
    "Role" :9,
    "User" :2
    },
    ```

3. Restart the Admin Node Manager so that the `acl.json` and `adminUsers.json` file updates are picked up.
4. Enter the following URL in your browser:

    ```
    http://localhost:8090/
    ```

5. Enter user credentials for `Fred` when prompted in the browser.
6. The API Gateway Manager displays a **Logs** tab enabling access to the trace files that `Fred` can view.

{{< alert title="Note" color="primary" >}}`Fred` is not allowed to access the server APIs used by the Policy Studio. If an attempt is made to connect to the server using the Policy Studio with his credentials, an `Access denied` error is displayed. {{< /alert >}}

No other configuration is required to give user `Fred` the above access to the management services. Other users in the same LDAP group can also view trace files without further configuration changes because the LDAP group is already defined in the `acl.json` file.
