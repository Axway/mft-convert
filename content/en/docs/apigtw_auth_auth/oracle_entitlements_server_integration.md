{
"title": "Integrate with Oracle Entitlements Server 11g and 11gR2",
"linkTitle": "Integrate with Oracle Entitlements Server 11g and 11gR2",
"weight":"160",
"date": "2020-01-24",
"description": "Configure API Gateway to authorize authenticated users against Oracle Entitlements Server (OES) 11g and 11gR2."
}

When integrating with OES:

* API Gateway will authenticate a user against its local user repository.
* API Gateway will then delegate the authorization decision for the specified resource to OES.

The OES 11g Authorization filter is used to delegate the authorization decision to OES. This filter assumes that an authentication filter has been configured prior to it. Therefore, by the time the authorization filter executes, the `authentication.subject.id` message attribute is populated and its value is used as the subject in the authorization request to OES.

The following diagram shows the sequence of events that occurs when a client sends a message to API Gateway. The request sender is authenticated by API Gateway and is then authorized against Oracle Entitlements Server. If the user is permitted to access the requested resource, the request is routed to the Enterprise Application. Otherwise an appropriate fault message is returned to the client.

![Authorize and authenticate a user](/Images/IntegrationGuides/auth_auth/apigw_oes_11gR2_overview.png)

## Prerequisites

### API Gateway

You must have installed API Gateway and have received a valid license from Axway.

### OES user

You must create an OES user called `weblogic`. Refer to the OES documentation for instructions on how to add a user.

### API Gateway local user store

You must have added the `weblogic` user to the gateway local user store. The policy you will set up later requires an authenticated user's request to be authorized against OES. By adding the `weblogic` user to the local user store, the client can authenticate as this user. The user name will then be stored in the `authentication.subject.id` message attribute, which is then passed to the OES 11g Authorization filter and subsequently on to OES to make the authorization decision.

### OES client

You must have installed the OES client (security module) on the machine running the gateway. The OES client has its own installer, which is available from [www.oracle.com](http://www.oracle.com./).

{{< alert title="Note" color="primary" >}}In the following integration steps, this version of the OES client was used: Oracle Entitlements Server Security Module 11g - 11.1.2.0.0.{{< /alert >}}

The OES client installer requires that a JRE is available on the target machine. In the absence of a preferred JVM on the target machine, API Gateway ships with a JRE that can be used.   On UNIX, the JRE is located in `INSTALL_DIR/apigateway/platform/jre`.

Start the OES client installer from the command line and pass the JRE location using the `jreLoc` argument as follows:

```
./runInstaller –jreLoc INSTALL_DIR/apigateway/platform/jre
```

### OES 11g or 11gR2

You must have installed, configured, and started OES 11g or 11gR2. For example, you can start it using the following commands on a UNIX-based system:

```
cd ~/Middleware/user_projects/domains/oes_domain
./startWebLogic.sh
```

{{< alert title="Note" color="primary" >}} This command assumes a WebLogic domain of `oes_domain` has already been configured.{{< /alert >}}

### cURL testing utility

To test the integration steps, the cURL testing utility is used to POST requests to the gateway. It is available from the [cURL Releases and Downloads](http://curl.haxx.se/download.html).

Alternatively, you can use any client capable of sending HTTP POST requests with HTTP basic authentication.

## Configure Oracle Entitlements Server

This section describes how to configure Oracle Entitlements Server to work with API Gateway.

### Create a resource and authorization policy in OES

The following steps describe how to create a resource and an authorization policy for that resource in Oracle Entitlements Server. For more detailed instructions about each of these steps, refer to the OES documentation.

You can use the OES Authorization Policy Manager web-based administration interface to configure the resource. The web interface is available at the following URL, where `OES_HOST` refers to the IP or host name of the machine on which OES is running:

```
http://OES_HOST:7001/apm
```

Log in using your WebLogic credentials and complete the following steps:

#### Create the application

To add a new application in OES, perform these steps:

1. Open the **Authorization Management** tab.
2. Right-click on the **Applications** node in the tree and select **New**:
3. Enter a name and a description for the new application, for example, `MyApplication` and `First App`.
4. Click **Save** at the top right-hand corner of the page.

#### Create the security module

The next step is to create a new security module:

1. Open the **System Configuration** tab.
2. Double-click the **Security Modules** node in the tree.
3. Click **New** at the top of the **Security Modules** table.
4. Enter `MySM` as the name of the new security module.
5. Click **Add** at the top of the **Bound to Applications** table.
6. Leave the **Search** field blank and click the search button to the right of the field.
7. Select `MyApplication` from the search results and click **Add**.

The `MyApplication` should now be displayed in the **Bound to Applications** table as follows:

![MyApplication](/Images/IntegrationGuides/auth_auth/oracle_entitlements_server_11gR2_05.png)

#### Create the resource type

You will now configure the resource type for the resource that users will be authorized for:

1. Open the **Authorization Management** tab again.
2. Expand the **Applications** node and then expand the newly created **MyApplication** node.
3. Double-click the **Resource Types** node.
4. Click **New** above the table showing the existing **Resource Types**.
5. Enter `MyResourceType` in the **Name** field.
6. Add an action for this resource by clicking **New** above the **Actions** table.  Add `POST` as an action.
7. Click **Save** to save the new **Resource Type**

#### Create the resource

The next step is to add the **Resource**:

1. Expand the **Default Policy Domain** and then the **Resource Catalog** nodes.
2. Double-click the **Resources** node.
3. Click **New** above the table listing all existing **Resources**.
4. Select `MyResourceType` as the **Resource Type**.
5. Enter `MyResource` as the **Resource name**.
6. Click **Save** to save the **Resource**

#### Configure the authorization policy

Now you must create the authorization policy that will determine whether to permit or deny access to the resource:

1. Double-click the **Authorization Policies** node beneath the **Default Policy Domain** node in the tree.
2. Click **New** above the table showing the existing **Authorization Policies**.
3. Enter `MyPolicy` as the name of the new policy.
4. Chose to "Permit" access to the resource target using the corresponding **Effect** check box.
5. Configure **Principals** (users, roles, or both) that can access the resource by clicking the "plus" button to the right of the **Principals** table.
6. Select the **Users** tab in the **Search Principal** window:
7. Select the default `weblogic` user from the table and click **Add Selected** to add the `weblogic` user to the **Selected Principals** table.

    ![Search Principal window](/Images/IntegrationGuides/auth_auth/oracle_entitlements_server_11gR2_08.png)

8. Click **Add Principals** at the bottom of the window.
9. Next, you must specify a resource target that this policy will act on. Click the "plus" button to the right of the **Targets** table.
10. Click the **Resources** tab on the **Search Targets** window.
11. Select `MyResourceType` in the **Resource Type** drop-down and click **Search**.
12. Select `MyResource` from the table and click **Add Selected** to add the resource to the **Selected Targets** table.
13. Click **Add Targets** at the bottom of the page.

### Set up the OES client

The OES client distributes policies to individual security modules that protect applications and services. Policy data is distributed in a controlled manner or in a non-controlled manner. The distribution mode is defined in the `jps-config.xml` configuration file for each security module. The specified distribution mode applies to all application policy objects bound to that security module. Consult with the OES administrator to find out the distribution mode. For the purposes of this section, the _controlled_ distribution mode is used.

#### Controlled mode

Complete the following steps to configure the OES client in controlled mode:

1. Open a command prompt and change directory to your OES client installation directory (this is referred to as `OES_CLIENT_HOME` throughout the remainder of this section).
2. Set the `JAVA_HOME` environment variable. For example:

    ```
    export JAVA_HOME=/home/oesuser/Oracle/Middleware/jdk160_29
    ```

3. Edit the following file:

    ```
    OES_CLIENT_HOME/oessm/SMConfigTool/smconfig.java.controlled.prp
    ```

4. Ensure that the following values are set:
    * `oracle.security.jps.runtime.pd.client.policyDistributionMode`: Accept the default value `controlled-push` as the distribution mode.
    * `oracle.security.jps.runtime.pd.client.RegistrationServerHost`: Enter the address of the Oracle Entitlements Server Administration Server.
    * `oracle.security.jps.runtime.pd.client.RegistrationServerPort` : Enter the SSL port number of the Oracle Entitlements Server Administration Server. You can find the SSL port number from the WebLogic Administration console. By default, `7002` is used.
5. On UNIX-based systems, run the `config.sh` script located in the  `OES_CLIENT_HOME/oessm/bin` directory.

    ```
    ./config.sh –smConfigId MySM -prpFileName OES_CLIENT_HOME/oessm/SMConfigTool/smconfig.java.controlled.prp
    ```

6. When prompted, specify the following:
    * OES user name (Administration Server's user name)
    * OES password (Administration Server's password)
    * New key store password for enrollment

    The following shows some sample output:

    ```
    ./config.sh -smConfigId MySM -prpFileName
    /home/oesuser/Oracle/Middleware/oes_client/oessm/SMConfigTool/smconfig.java.controlled.prp
    Configuring for Controlled Policy Distribution Mode
    Security Module configuration is created at: /home/oesuser/Oracle/Middleware/oes_client/oes_sm_instances/MySM
    Enter password for key stores: ******
    Enter password for key stores again: ******
    Passwords are saved in credential store.
    Keystores are initialized successfully.
    Please enter a value for OES Admin Server User name:weblogic
    Please enter a value for OES Admin Server Password:
    Enrollment is proceeded successfully.
    ```

7. Ensure that the security module has been configured correctly by checking that the `OES_CLIENT_HOME/oes_sm_instances/MySM` directory has been created.
8. Depending on your OES client setup, the registration process might not have generated a `cwallet.sso` file in your `OES_CLIENT_HOME/oes_sm_instances/MySM/config` directory. If there is no `cwallet.sso` file present in the `config` directory, you can copy the one generated in the `config/enroll` directory to the `config` directory using the following commands:

    ```
    cd oes_sm_instances/MySM/config
    [oesuser@oeseval config]$ ls
    enroll  jps-config.xml  system-jazn-data.xml
    # cp enroll/cwallet.sso ./
    # ls
    cwallet.sso  enroll  jps-config.xml  system-jazn-data.xml
    ```

### Non-controlled mode

Alternatively, you can use the non-controlled or controlled pull distribution mode. Consult the Oracle documentation for configuring these modes.

### Distribute the OES policy

When the OES client has registered with OES, you can distribute the policy for that application so that clients making authorization requests for this resource will be subject to the policy enforcement rules.

Follow these steps in the OES Authorization Policy Manager web-based administration interface:

1. Double-click the **MyApplication** node in the tree on the **Authorization Management** tab.
2. Open the **Policy Distribution** tab for the application configuration.
3. Expand the `MySM` entry in the table.

    You should see an entry representing the recently registered OES client instance, as shown in the following screenshot:

    ![MyApplication](/Images/IntegrationGuides/auth_auth/oracle_entitlements_server_11gR2_09.png)

4. Select the **MySM** application and click **Distribute** to push the authorization policy configured for this application.
5. Click **Refresh** to update the **Synced** status. You should see a green tick to indicate a successful distribution.

{{< alert title="Note" color="primary" >}}You might have to restart WebLogic for your newly registered security module to be displayed in the list on the **Policy Distribution** tab.{{< /alert >}}

## Configure API Gateway

This section describes how to configure API Gateway to work with Oracle Entitlements Server.

### Modify the API Gateway classpath

API Gateway's classpath must be extended to include the OES client JAR. To achieve this, create a `jvm.xml` file at the following location:

```
INSTALL_DIR/apigateway/conf/jvm.xml
```

Edit this `jvm.xml` so that its contents are as follows, providing values for `OES_CLIENT_HOME` and `SM_NAME` that are based on the location where the OES client was installed and the SM name used when enrolling the OES client (`MySM`):

```xml
<ConfigurationFragment>
<!-- Change these ENV VARS to match the location where the OEM Client has
been installed and configured -->
<Environment name="OES_CLIENT_HOME" value="/home/oes/Oracle/Middleware/oes_client" />
<Environment name="SM_NAME" value="MySM" />
<Environment name="INSTANCE_HOME"
value="$OES_CLIENT_HOME/oes_sm_instances/$SM_NAME" />

<!-- Add OES Client JAR to the classpath -->
<ClassPath name="$OES_CLIENT_HOME/modules/oracle.oes.sm_11.1.1/oes-client.jar" />

<!-- Add OES JARs to the classpath -->
<ClassPath name="[PATH_TO_OES_JARS]/api.jar"/>
<ClassPath name="[PATH_TO_OES_JARS]/asi_classes.jar"/>

<VMArg name="-Doracle.security.jps.config=$INSTANCE_HOME/config/jps-config.
xml"/>
<!-- Optional argument to add enhanced logging (via log4j) for the OES Client -->
<VMArg name="-Djava.util.logging.config.file=$INSTANCE_HOME/logging.properties"/>
</ConfigurationFragment>
```

The following is an example `jvm.xml` file for Windows:

```xml
<ConfigurationFragment>
  <!-- Environment variables -->
  <!-- change these to match the location where the OEM Client has been installed and configured -->
<Environment name="OES_CLIENT_HOME" value="C:\Oracle\product\11.1.1\as_1" />
<Environment name="SM_NAME" value="MySSM" />
<Environment name="INSTANCE_HOME" value="$OES_CLIENT_HOME/oes_sm_instances/$SM_NAME" />
<!-- Add OES Client to classpath -->
<ClassPath name="$OES_CLIENT_HOME/modules/oracle.oes.sm_11.1.1/oes-client.jar" />
<VMArg name="-Doracle.security.jps.config=$INSTANCE_HOME/config/jps-config.xml"/>  
</ConfigurationFragment>
```

### Start API Gateway

Start API Gateway so that it runs with the OES client classpath and the associated environment settings. For more information, see [Start API Gateway](/docs/apim_installation/apigtw_install/install_gateway/#start-api-gateway).

Command example:

```
startinstance -n "APIGateway1" -g "Group1"
```

### Configure API Gateway to delegate authorization to OES

This section explains how to configure API Gateway to delegate authorization decisions to Oracle Entitlements Server.

The resulting policy created in API Gateway will appear as follows:

![Policy](/Images/IntegrationGuides/auth_auth/oracle_entitlements_server_11gR2_10.png)

#### Configure the authentication filter

In this example, it is assumed that the user profile to be authorized through OES is also stored in the local user store of API Gateway. API Gateway can also delegate authentication decisions to other systems (for example, LDAP directories, databases, and other third-party identity management systems, including CA SiteMinder, Oracle Access Manager, RSA Access Manager, and so on). For simplicity, API Gateway's local user store is used in this example.

Configure the authentication filter as follows:

1. In Policy Studio, create a new policy called `OES Authorization`.
2. Drag a **HTTP Basic** filter from the **Authentication** category in the palette onto the canvas and configure it as follows:
    * **Name**: Enter `HTTP Basic Authentication` in the field provided.
    * **Credential Format**: Select `User Name` from the drop-down list.
    * **Allow Client Challenge**: Select the **Allow client challenge** check box.
    * **Repository Name**: Select `Local User Store` from the drop-down list.
3. Click **OK**.
4. To set this authentication filter to be the starting filter of the policy, right-click the filter in the canvas and select **Set as Start**.

#### Configure the OES 11g authorization filter

Configure the OES 11g authorization filter as follows:

1. From the **Oracle Entitlements Server** category in the palette on the right of Policy Studio, drag the **11g Authorization** filter onto the canvas, and configure it as follows:
    * **Resource**: Enter a formatted string representing the resource that you created in OES and for which API Gateway will ask OES for authorization decisions. The resource you created earlier in OES can be represented with the string `MyApplication/MyResourceType/MyResource`.
    * **Action**: The rules created in the OES policy can permit/deny access to a resource based on the requested action, for example, POST, GET, DELETE, and so on. In this example, you will be POST-ing the request to the resource, so you must enter `POST` in the **Action** field. Remember, you configured POST access to the this resource earlier when configuring the OES policy.
    * You can optionally configure environment context attributes. However, for the purposes of this integration example it is not necessary to configure this section.
2. Click **OK**.
3. Set the success path from the **HTTP Basic Authentication** filter to point at the newly created OES 11g authorization filter.

#### Add the success message filter

Display a success message after successfully authorizing the user by adding a **Set Message** filter.

1. Drag a **Set Message** filter from the **Conversion** category onto the canvas and configure it as follows:
    * **Name**: Enter `Set Success Message` in the text field.
    * **Content-type**: Enter `text/plain` as the content-type of the message to return to the client.
    * **Message Body**: Enter the following message to return to the client: `User '${authentication.subject.id}' was authorized successfully!`
2. Click **OK**.
3. Set the success path of the 11g authorization filter to the **Set Success Message** filter.

#### Add the failure message filter

If OES returns false for the authorization request you should return an appropriate error message to the client.

Display a failure message after an unsuccessful authorization event by adding another **Set Message** filter:

1. Drag a **Set Message** filter from the **Conversion** category onto the canvas, and configure it as follows:
    * **Name**: Enter `Set Failure Message` in the text field.
    * **Content-type**: Enter `text/plain` as the content-type of the message to return to the client.
    * **Message Body**: Enter the following message to return the client: `The user '${authentication.subject.id}' was NOT authorized to access the resource!`
2. Click **OK**.
3. Set the failure path of the 11g authorization filter to the **Set Failure Message** filter.

#### Add a relative path for the OES authorization policy

In order for API Gateway to invoke this policy for certain requests you must create a relative path and map it to the policy. All requests received on this path are automatically forwarded to the policy for processing.

To add a relative path for this policy click **Add Relative Path** in the toolbar beneath the policy canvas.

Enter the path on which to receive requests for this policy in the field provided in the Resolve path to Policies dialog:

![Policies dialog](/Images/IntegrationGuides/auth_auth/oracle_entitlements_server_11gR2_16.png)

For example, if you enter a relative path of `/oes`, you can see that this path is automatically mapped to the `OES Authorization` policy created earlier in this section.

### Deploy the policy

To push the configuration changes to the live API Gateway instance you must deploy the new policy. You can do this by pressing the **F6** button.

## Test the integration

Having completed the integration steps, you can now test the setup using the cURL testing utility.

{{< alert title="Note" color="primary" >}}You must have added a `weblogic` user to the API Gateway local user store as outlined in [Prerequisites](#prerequisites).{{< /alert >}}

Assuming you are running API Gateway on a machine called `apigateway` on the default port of 8080, you can send a POST request to the authorization policy on API Gateway using HTTP basic authentication with the following command:

```
curl  --user weblogic:weblogic --data "test=data" http://apigateway:8080/oes
User 'weblogic’ was authorized successfully!
```

You can see that the success message has been returned by API Gateway meaning that the `weblogic` user has been successfully authorized by OES to access the resource. A quick look at the API Gateway’s trace output shows the OES 11g Authorization filter making the successful authorization request for the `MyApplication/MyResourceType/MyResource` resource:

```
DEBUG   23/Oct/2012:15:28:45.183 [42687940] run circuit "OES Authorization"...
DEBUG   23/Oct/2012:15:28:45.183 [42687940] run filter [HTTP Basic Authentication] {
DEBUG   23/Oct/2012:15:28:45.183 [42687940]     VordelRepository.checkCredentials: username=weblogic
DEBUG   23/Oct/2012:15:28:45.183 [42687940]     Attempt to map incoming format Username to repository authZ format Username
DEBUG   23/Oct/2012:15:28:45.183 [42687940] } = 1, filter [HTTP Basic Authentication]
DEBUG   23/Oct/2012:15:28:45.183 [42687940] Filter [HTTP Basic Authentication] completes in 0 milliseconds.
DEBUG   23/Oct/2012:15:28:45.183 [42687940] run filter [11g Authorization] {
DEBUG   23/Oct/2012:15:28:45.183 [42687940]     creating subject from 'weblogic'
DEBUG   23/Oct/2012:15:28:45.183 [42687940]     checking 'POST' to resource: MyApplication/MyResourceType/MyResource
DEBUG   23/Oct/2012:15:28:45.185 [42687940]     Request: {weblogic, POST, MyApplication/MyResourceType/MyResource}
Result: true
DEBUG   23/Oct/2012:15:28:45.185 [42687940]     result from OES: true
DEBUG   23/Oct/2012:15:28:45.185 [42687940]     no obligations in response
DEBUG   23/Oct/2012:15:28:45.185 [42687940] } = 1, filter [11g Authorization]
DEBUG   23/Oct/2012:15:28:45.185 [42687940] Filter [11g Authorization] completes in 2 milliseconds.
```

Now let's see what happens when you authenticate with a user that is present in the API Gateway's local user store, but that has not been configured with authorization rights in OES. To demonstrate this, you can send up a request using the credentials of the default API Gateway `admin` user:

```
curl  --user admin:changeme --data "test=data" http://apigateway:8080/oes
User 'admin’ was NOT authorized to access the resource!
```

If you take a quick look at the API Gateway's trace output again, you can see that the 11g Authorization filter has blocked the authorization request:

```
DEBUG   23/Oct/2012:15:29:22.678 [41f67940] run circuit "OES Authorization"...
DEBUG   23/Oct/2012:15:29:22.678 [41f67940] run filter [HTTP Basic Authentication] {
DEBUG   23/Oct/2012:15:29:22.678 [41f67940]     VordelRepository.checkCredentials: username=admin
DEBUG   23/Oct/2012:15:29:22.678 [41f67940]     Attempt to map incoming format Username to repository authZ format Username
DEBUG   23/Oct/2012:15:29:22.678 [41f67940] } = 1, filter [HTTP Basic Authentication]
DEBUG   23/Oct/2012:15:29:22.678 [41f67940] Filter [HTTP Basic Authentication] completes in 0 milliseconds.
DEBUG   23/Oct/2012:15:29:22.678 [41f67940] run filter [11g Authorization] {
DEBUG   23/Oct/2012:15:29:22.678 [41f67940]     creating subject from 'admin'
DEBUG   23/Oct/2012:15:29:22.678 [41f67940]     checking 'POST' to resource: MyApplication/MyResourceType/MyResource
DEBUG   23/Oct/2012:15:29:22.679 [41f67940]     Request: {admin, POST, MyApplication/MyResourceType/MyResource}
Result: false
DEBUG   23/Oct/2012:15:29:22.679 [41f67940]     result from OES: false
ERROR   23/Oct/2012:15:29:22.679 [41f67940]     Failed to authZ to OES
DEBUG   23/Oct/2012:15:29:22.680 [41f67940] } = 0, filter [11g Authorization]
DEBUG   23/Oct/2012:15:29:22.680 [41f67940] Filter [11g Authorization] completes in 2 milliseconds.
```
