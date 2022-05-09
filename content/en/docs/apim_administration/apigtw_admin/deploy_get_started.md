{
"title": "Manage API Gateway deployments",
"linkTitle": "Manage deployments",
"weight":"90",
"date": "2019-10-18",
"description": "Use Policy Studio or API Gateway Manager to deploy API Gateway configuration, and perform a zero downtime policy deployment in a multi-node API Gateway environment with a load balancer."
}

You can use Policy Studio to deploy configuration to API Gateway instances running in groups in an API Gateway domain. Policy Studio enables you to edit the gateway configuration and then deploy it to the server instance, where it can be reloaded later. You can deploy modified configuration to multiple gateway instances in a group managed by an Admin Node Manager.

API Gateway Manager also enables you to deploy configuration packages to the API Gateway instances running in groups in a domain. In this way, Policy Studio and API Gateway Manager enable policy developers and administrators to centrally manage the policies that are enforced at all nodes throughout the network.

In addition, Policy Studio enables you to compare and merge differences between versions of the same policy. Policies can be merged, and deployed to any running instance that is managed by Policy Studio. One of the most powerful uses of this centralized management capability is in transitioning from a staging environment to a production environment. For example, policies can be developed and tested on the staging environment, and when ready, they can be deployed to all instances deployed in the production environment.

## Deploy API Gateway configuration

You can edit API Gateway configuration in a Policy Studio project, and deploy to specified API Gateway instances running in an API Gateway group. You can deploy projects based on existing configuration, configuration packages, factory configuration, or a running API Gateway instance.

Policy Studio also enables you to create configuration packages (`.fed`, `.tar.gz`, `.pol`, or `.env` files), and to deploy projects based on configuration packages to API Gateway instances.

You can also deploy API Gateway configuration packages in the API Gateway Manager web console. Alternatively, you can use the `managedomain` script to create and deploy deployment packages (`.fed` files) on the command line.

### Deploy configuration in Policy Studio

You can deploy updates to a currently loaded configuration when editing the configuration in Policy Studio. To deploy a currently loaded configuration, perform the following steps:

1. Click the **Deploy** button on the right in the toolbar.
2. In the **Open Connection** dialog, in the **Saved Session**s section, select the server session to use from the list. You can edit a session name by entering a new name and clicking **Save**. You can also click the appropriate button to **Add**, **Clone**, or **Remove** saved sessions.
3. In the **Connection Details** section, configure the following:

    * **Host**: Enter the server host to connect to. The default is `localhost`.
    * **Port**: Enter the port to connect on. The default Admin Node Manager port is `8090`.
    * **User name**: The deployment service is protected by HTTP basic authentication. Enter the administrator user name to use to authenticate to the server.
    * **Password**: You can edit API Gateway configuration in a Policy Studio project, and deploy to specified API Gateway instances running in an API Gateway group. You can deploy projects based on existing configuration, configuration packages, factory configuration, or a running API Gateway instance.
4. Click **Advanced** to enter the **URL** of the deployment service exposed by the server. This setting is optional. The default Admin Node Manager URL is `https://localhost:8090/api`.
5. Click **Next** to configure deployment options.

    If an advisory warning has been configured, you must click **Next** again.

6. In the **Select the servers(s) you wish to deploy to** section, select an API Gateway group from the **Group** list, and select the server instance(s) in the box below.

    If the server uses a different API Gateway encryption passphrase for its environment, click **Advanced**, select **The target server uses a different passphrase**, and enter the **Passphrase** used by the target server.

7. Click **Next**, and wait for the deployment to complete.
8. Click **Finish**.

{{< alert title="Note" color="primary" >}}You must connect to the Admin Node Manager server to deploy API Gateway configuration or manage multiple API Gateway instances in your network.{{< /alert >}}

### View deployment results in Policy Studio

When you click **Deploy**, the **Deployment Results** screen is displayed, and deployment to each server occurs sequentially. Feedback is provided using icons in the **Task** column, and text in the **Status** column. When the configuration has deployed, click **Finish**.

**Cancel deployments**:

You can cancel deployments by clicking the **Cancel** button. Feedback is provided in the **Status** column. You cannot cancel a deployment when it has started. The wizard performs the cancellation at the end of the current deployment, with all remaining deployments being canceled.

**Deployment errors**:

Client-side and server-side errors can occur. Client-side errors are displayed in the **System Trace** in the **Console** view. If any server-side deployment errors occur during the deployment process, you can review these in the **Deployment Error Log**
view. This is displayed at the bottom of the screen when you click **Finish**, and lists any errors that occur for each instance. The corresponding Console **Deployment Log** is also available in the **Console** view.

**Redeploy**:

When you have deployed a configuration to one or more instances, you can click back through the wizard to change your selections and redeploy, without needing to exit and relaunch the wizard.

### API Gateway configuration packages

You can deploy configuration based on API Gateway configuration packages in Policy Studio and in the API Gateway Manager web console. API Gateway includes the following types of configuration package:

* A *deployment package* can be a `.fed` (XML) or `.tar.gz` (YAML) file that contains all API Gateway configuration. This includes policies, listeners, external connections, users, certificates, and environment settings.
* A *policy package* is a `.pol` file that contains policies, listeners, external connections, and environment settings.
* An *environment package* is an `.env` file that contains users, certificates, and environment settings. The content of the `.fed` file is equivalent to the combined contents of the `.pol` and `.env` files.
* A *package property* is a name-value pair that applies to a specific configuration package (`.fed`, `.pol`, or `.env`). Specifying a property associates metadata with the configuration in that package. For example, the **Name** property with a value of `Default Factory Configuration` is associated with a default installation.

### Create a configuration package in Policy Studio

You can create an API Gateway configuration package for a currently loaded project configuration. To create a package (`.fed`, `.pol`, or `.env`), perform the following steps:

1. In the main menu, select **File > Save Package** followed by the appropriate option:
    * **Deployment Package**
        * `.fed` (XML)
        * `.tar.gz` (YAML)
    * **Policy Package** (`.pol`)
    * **Environment Package** (`.env`)
2. Enter a file name, and click **Save**.

#### Configure package properties in Policy Studio

You can view or modify API Gateway configuration package properties for a currently loaded project configuration. To view and modify configuration properties, perform the following steps:

1. In the Policy Studio tree, and select **Environment Configuration > Package Properties > Policy** or **Environment**.
2. If you wish to create any additional properties (for example, **Department**), click the green (+) button on the right, and enter a property value (for example, `Engineering`).
3. If you wish to remove a property, click the red (x) button on the right of the property.
4. Click **Save** at the top right of the screen.

### Deploy packages in Policy Studio

You can deploy the configuration as normal using the **Deploy** button in the toolbar. For more details, see [Deploy configuration in Policy Studio](#deploy-configuration-in-policy-studio).

### Deploy packages in API Gateway Manager

You can also use the API Gateway Manager web console to deploy configuration packages to a group of API Gateway instances. This functionality is available on the default **Dashboard**
tab.

For more details, see [Manage domain topology in API Gateway Manager](/docs/apim_administration/apigtw_admin/managetopology).

### Deploy packages on the command line

You can create and deploy a deployment package (`.fed`) using the `managedomain --menu` command in the following directory:

```
INSTALL_DIR/apigateway/posix/bin
```

The deployment options for the `managedomain --menu` command are as follows:

```
18) Deploy to a group
19) List deployment information
20) Create deployment archive
21) Download deployment archive
22) Update deployment archive properties
```

For more details, see [Managedomain command reference](/docs/apim_reference/managedomain_ref/).

### Compare and merge configurations in Policy Studio

You can compare and merge differences between the currently loaded API Gateway configuration with a configuration stored in a deployment package (`.fed` file). Click the **Compare**
button on the Policy Studio toolbar to select a `.fed` file to compare the current configuration against. The results are displayed in the **Compare/Merge** tab.

For example, you can view the differences made to particular fields in an Authentication filter that occurs in both configurations. When a difference is located, you can merge the differences, and thereby update the fields in the Authentication filter in the current configuration with the field values for the same Authentication filter in the deployment package.

## Perform zero downtime deployment

When you deploy configuration (for example, a `.fed` file) to a group of API Gateways, the configuration is deployed sequentially to each API Gateway in the group. While the configuration is being deployed to a given API Gateway there is a service interruption during which that API Gateway cannot process traffic.

Zero downtime deployment enables you to orchestrate deployment to a load-balanced set of API Gateways, ensuring that a subset can always process traffic.

The following diagram illustrates the process:

![ZDD flows](/Images/APIGateway/ZDD_process.png)

1. A user initiates deployment of new configuration from Policy Studio.
2. The Admin Node Manager starts deployment in `API Gateway 1`.
3. `API Gateway 1` starts responding to a ping from the load balancer with `503 Service Unavailable`. The load balancer stops routing traffic to `API Gateway 1`.
4. `API Gateway 1` performs the deployment.
5. `API Gateway 1` starts responding to the load balancer ping with `200 OK`. The load balancer starts routing traffic to `API Gateway 1` again.
6. `API Gateway 1` informs the Admin Node Manager that deployment is complete.
7. The Admin Node Manager repeats step 2 through step 6 for `API Gateway 2`.

### Deploy configuration using zero downtime deployment

To perform a zero downtime policy deployment, follow these steps:

1. Enable zero downtime deployment in Policy Studio, and set the delays before and after deployment. For more information, see [Zero downtime settings](/docs/apim_reference/general_settings/#zero-downtime-settings).
2. Configure your load balancer to ping the Health Check LB policy periodically to determine if each API Gateway is healthy. This is available on the following default URL:

    ```
    http://APIGATEWAY_HOST:8080/healthchecklb
    ```

3. Initiate deployment to a group of API Gateways using API Gateway Manager, Policy Studio, or `managedomain`. The configuration is deployed sequentially to each API Gateway in the group.
4. When deployment is initiated on each API Gateway:
    * The Health Check LB policy returns a `503 Service Unavailable` response. This indicates to the load balancer that this API Gateway is not available for traffic and the load balancer stops routing to it.
    * After the specified delay before deployment (for example, 10 seconds), the configuration is deployed to the API Gateway.
    * When the deployment is complete, the Health Check LB policy returns a `200 OK` response. This indicates to the load balancer that this API Gateway is available for traffic again.
    * After the specified delay after deployment (for example, 10 seconds), a response is sent to the deployment request. Deployment can now be initiated to the next API Gateway in the group.
