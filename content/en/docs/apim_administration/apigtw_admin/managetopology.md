{
"title": "Manage domain topology in API Gateway Manager",
"linkTitle": "Manage domain topology in API Gateway Manager",
"weight":"35",
"date": "2019-10-14",
"description": "Use the topology view in API Gateway Manager to manage an existing API Gateway domain."
}

When using API Gateway Manager to manage an existing domain, you must ensure that the host was first registered in the domain using the `managedomain` script. For more details, see [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway).

The API Gateway Manager web console is available from the following URL:

```
https://HOST:8090
```

## Check the status of API Gateway groups and instances {#check-status}

You can check the status of the gateway groups and instances in the API Gateway Manager topology view.

The possible status of a gateway group in the topology view is:

* Consistent – A group displayed with a blue icon is consistent. The gateways in the group have the same configuration.
* Inconsistent – A group displayed with a black icon is inconsistent. The gateways in the group do not have the same configuration (for example, during the phased roll out of configuration in a production environment). The tooltip displays `<GroupName> configuration is inconsistent`.
* Unlocked – The gateways in the group are available for editing.
* Locked – The gateways in the group are locked by a specific user, and are not available for editing. The tooltip displays `<GroupName> [locked by <UserName>]`.

![Group configuration locked](/Images/APIGateway/topology_group_locked.png)

The possible status of a gateway instance in the topology view is:

* Running – A gateway displayed with a green icon is running.
* Not running – A gateway displayed with a red icon is not running. The tooltip displays `API Gateway cannot be reached`.

![API Gateway not running](/Images/APIGateway/topology_gateway_down.png)

* Unknown – A gateway displayed with a gray icon is not reachable. The Node Manager on the machine that the API Gateway is running on is down and the status cannot be determined. The tooltip displays `API Gateway status is unknown`.

![API Gateway unknown status](/Images/APIGateway/topology_gateway_unknown.png)

The possible status of a host (Node Manager) in the topology view is:

* Running – A host displayed with a blue icon is running.
* Not running – A host displayed with a red icon is not running. The tooltip displays `Node Manager cannot be reached`.

The topology view also displays the following messages and icons when there is a problem with the configuration:

Version mismatch message – A warning message is displayed if there is a mismatch in product versions between hosts in a topology. The host status continues to show a blue icon.

Warning icon – A warning icon is displayed if any of the following occur:

* Node Manager is down
* API Gateway instance is down
* Version mismatch between hosts

For example:

![Version mismatch warning](/Images/APIGateway/topology_version_mismatch_warning.png)

## Check the installed version number of API Gateway instances {#check-installed-version}

The installed product version number and any installed service packs or updates are displayed in the API Gateway Manager topology and grid views.

When no service pack or update is installed on a system, API Gateway Manager shows the product version (for example, `7.7`). If a service pack or update is installed API Gateway Manager shows the product version and the service pack or update number (for example, `7.7 SP1`).

The version information is the same for all processes running on a host as they all use the same physical installation. Version information is shown at the host and gateway level. Version information is not shown at the group level as groups can span multiple hosts.

To view the version information for a gateway instance in the topology groups view, click **View as > Topology** and **Show > Groups** and click a gateway instance:

![API Gateway instance version in topology groups view](/Images/APIGateway/topology_groups_instance_version.png)

The version information of the gateway instance is shown in the Version field (for example, `7.5.3 SP1`).

To view the version information for a host in the topology hosts view, click **View as > Topology** and **Show > Hosts** and click a host:

![Host version in topology hosts view](/Images/APIGateway/topology_hosts_host_version.png)

The version information of the host is shown in the Version field (for example, `7.5.3 SP1`).

To view the version information for a gateway instance in the grid groups view, click **View as > Grid** and **Show > Groups**:

![API Gateway instance version in grid groups view](/Images/APIGateway/grid_groups_instance_version.png)

The version information for each gateway instance in the group is shown in the Product Version column (for example, `7.5.3 SP1`).

To view the version information for a host in the grid hosts view, click **View as > Grid** and **Show > Hosts**:

![Host version in grid hosts view](/Images/APIGateway/grid_hosts_host_version.png)

The version information for each host is shown in the Product Version column (for example, `7.5.3 SP1`).

## Manage API Gateway groups

You can use the topology view in API Gateway Manager tool to create, delete, and lock API Gateway groups. The following example shows the options available at the group level:

![Manage groups in API Gateway Manager](/Images/APIGateway/admin_topology_group.png)

### Create an API Gateway group

To use the API Gateway Manager to create a gateway group, perform the following steps:

1. Click the **Menu** button in the topology view on the **Dashboard** tab.
2. Select **Create New Group**.
3. Enter a group name (for example, `Engineering`).
4. Click **OK**.

### Delete an API Gateway group

To delete a group, perform the following steps:

1. Ensure that the API Gateway instances in the group have been stopped.
2. Hover over the group in the topology view, and click the edit button on the right.
3. Select **Delete Group**.
4. Click **OK**.

### Lock an API Gateway group

To lock a group and prevent other users from editing its configuration, perform the following steps:

1. Hover over the group in the topology view, and click the edit button on the right.
2. Select **Lock Group**.
3. Click **OK**.

Similarly, to unlock an API Gateway group, select **Unlock Group**. Only administrator users can unlock groups locked by other users.

## Manage API Gateway instances

You can use the topology view in API Gateway Manager to create, delete, start, and stop API Gateway instances.

### Create an API Gateway instance

To use the API Gateway Manager to create an API Gateway instance, perform the following steps:

1. Hover over the API Gateway group in the topology view, and click the edit button on the right.
2. Select **New API Gateway**.
3. Configure the following fields:
    * **Name**: API Gateway instance name (for example, `Server2`).
    * **Management Port**: Local management port (for example, `8086`).
    * **Services Port**: External traffic port (for example, `8081`).
    * **Host**: Host address (for example, `127.0.0.1`).
    * **Domain CA Passphrase**: The passphrase for the domain SSL certificate authority.
4. Click **OK**.

### Start an API Gateway instance

To start an API Gateway instance, perform the following steps:

1. Ensure that the API Gateway instance has been stopped.
2. Hover over the API Gateway instance in the topology view, and click the edit button on the right.
3. Select **Start**.
4. Click **OK**.

The following example shows how to start a stopped API Gateway instance:

![Start API Gateway instance in API Gateway Manager](/Images/APIGateway/admin_topology_instance.png)

### Stop an API Gateway instance

To stop an API Gateway instance, perform the following steps:

1. Ensure that the API Gateway instance has been started.
2. Hover over the API Gateway instance in the topology view, and click the edit button on the right.
3. Select **Stop**.
4. Click **OK**.

### Edit an API Gateway tag

API Gateway tags are name-value pairs that you can add to API Gateway instances, which then enable you to filter for API Gateway instances by tag in the API Gateway Manager**Dashboard**. To add an API Gateway tag, perform the following steps:

1. Hover over the API Gateway instance in the topology view, and click the edit button on the right.
2. Select **Edit Tags**.
3. In the dialog, click the green plus icon to add a tag.
4. Enter a **Tag** name (for example, `Department`), and a **Value** (for example, `Engineering`).
5. Click **Apply**.

To view the newly created tag in the in the API Gateway Manager topology view, click **View as** > **Grid**. To filter this view, enter a tag name or value in the **Filter**
field.

## Deploy API Gateway configuration

You can use the API Gateway Manager web console to deploy API Gateway configuration packages to a group of API Gateway instances. This includes the following configuration packages:

* deployment package (`.fed`)
* policy package (`.pol`)
* environment package (`.env`)

### Deploy a deployment package

To deploy an existing deployment package to a group of API Gateways, perform the following steps:

1. In the **TOPOLOGY** view, right-click the API Gateway group to which to deploy the package, and select **Deploy Configuration**.
2. Select **I wish to deploy configuration contained in a single Deployment Package**.
3. Click **Browse for .fed**, and select the `.fed` file in the dialog.
4. Click **Next**.
5. Select **Deploy** in the wizard, and the deployment package is deployed to the API Gateway group.
6. Click **Finish**.

### Deploy policy and environment packages

To deploy existing policy and environment packages to a group of API Gateways, perform the following steps:

1. In the **TOPOLOGY** view, right-click the API Gateway group to which to deploy the packages, and select **Deploy Configuration**.
2. Select **I wish to deploy configuration contained in Policy Package and Environment Package**.
3. Click **Browse for .pol**, and select the `.pol` file in the dialog.
4. Click **Browse for .env**, and select the `.env` file in the dialog.
5. Click **Next**.
6. Select **Deploy** in the wizard, and the packages are deployed to the API Gateway group.
7. Click **Finish**.

### Deploy a YAML deployment package

To deploy an existing YAML deployment package to a group of API Gateways, perform the following steps:

1. In the **TOPOLOGY** view, right-click the API Gateway group to which to deploy the package, and select **Deploy Configuration**.
2. Select **I wish to deploy YAML configuration contained in a single Deployment Archive**.
3. Click **Browse for .tar.gz**, and select the `.tar.gz` file in the dialog.
4. Click **Next**.
5. Select **Deploy** in the wizard, and the deployment package is deployed to the API Gateway group.
6. Click **Finish**.
