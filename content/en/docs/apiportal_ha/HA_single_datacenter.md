{
    "title":"Configure single datacenter high availability",
    "linkTitle":"Configure single datacenter HA",
    "weight":"1",
    "date":"2019-08-09",
    "description":"Configure API Portal for high availability (HA) in a single datacenter."
}

## Configure API Portal

This section describes how to deploy API Portal high availability (HA) in a single datacenter.

### Configure the database cluster

You must configure a relational database management system (RDBMS) to store API Portal data.

You must install and configure the database on each database node before installing and configuring API Portal.

The database cluster has the following requirements in a production environment:

* The database must be supported by API Portal. For more information on supported databases, see [Software requirements](/docs/apim_installation/apiportal_install/install_software_prereqs/#software-requirements).
* For HA, it is best to have at least three database nodes in each datacenter (for example, to prevent data corruption due to split-brain syndrome). Install the database on each node.
* Choose a unique name for the database cluster.
* Do not start any of the databases until the database cluster is fully configured.
* You must synchronize time on all servers.
* To avoid firewall issues, you must open the RDBMS ports needed to allow bidirectional communication among the database nodes.

For more details on installing and configuring your database, see [MySQL documentation](https://dev.mysql.com/doc/refman/5.6/en/) or [MariaDB documentation](https://mariadb.com/kb/en/mariadb/documentation/).

#### Fine-tune database settings

You can use advanced settings to fine-tune the database and its performance:

* To automatically set increment IDs in the database and synchronize the IDs between the API Portal instances, check the settings for `auto_increment_increment` and `auto_increment_offset`.
* To avoid timeout issues, check the `master-connect-retry` setting.
* To avoid filling disk space with database replication logs, adjust the `expire_log_days` and `max_binlog_size` as needed.

For more details on the database settings, see [MySQL documentation](https://dev.mysql.com/doc/refman/5.6/en/) or [MariaDB documentation](https://mariadb.com/kb/en/mariadb/documentation/).

### Configure the shared file storage

You must set up a shared file system between API Portal instances to keep them synchronized.

### Configure API Portal

After setting up the database cluster, install API Portal. For HA, you must have at least two API Portal instances.

#### Install API Portal for HA as a software installation

For HA, install API Portal on the first node, and then install it on each of the other nodes.

1. Install API Portal on the first node as detailed in [Install API Portal as software installation](/docs/apim_installation/apiportal_install/install_software/#install-api-portal-software).

    * When prompted for database connection settings during the API Portal software installation, enter the access host of the database you created for this node.
    * When asked if this is going to be HA setup with database replication, enter `y`.

2. Install the EasyBlog and EasyDiscuss components on the first node as detailed in [Install Joomla! components](/docs/apim_installation/apiportal_install/install_software/#install-joomla-components).

    {{< alert title="Note" color="primary" >}}When you change the Joomla! admininstrator password on the first node, the password is changed for all nodes. {{< /alert >}}

3. Install API Portal on each of the other nodes as detailed in [Install API Portal as software installation](/docs/apim_installation/apiportal_install/install_software/#install-api-portal-software). For each node:

    * When prompted for database connection settings, use the same database server or cluster you used for the first API Portal instance.
    * The installation checks the database connection and if it is successful for any node other than the first node, it asks you to confirm that this is a HA setup. Enter `y`.

4. Install the EasyBlog and EasyDiscuss components on each other node as detailed in [Install Joomla! components](/docs/apim_installation/apiportal_install/install_software/#install-joomla-components).

#### Upgrade API Portal for HA as a software installation

For HA upgrade, upgrade API Portal and perform the post-upgrade steps on each node as detailed in [Upgrade API Portal as software installation](/docs/apim_installation/apiportal_install/upgrade_automatic/).

### Configure load balancer

Configuring the load balancer may vary depending on the load balancer in question. Set up and configure the load balancer as instructed in the documentation of your load balancer.

You must situate the load balancer between the two API Portal instances.

## Failover scenarios

This topic describes the expected behavior in case of a failover in API Portal high-availability (HA) deployment in a single datacenter.

### One API Portal instance is down

In this scenario, one of the API Portal instances in the datacenter goes down:

![Illustration of API Portal HA configuration with one instance down.](/Images/APIPortal/API_Portal_HA_failover_instance.png)

The following applies on this scenario:

* The API Portal instance that is down can no longer handle requests.
* All requests are handled by the remaining API Portal instances.
* You must restart the API Portal instance that is down.

### One database node is down

In this scenario, one database node in a database cluster goes down:

![Illlustration of API Portal HA configuration with one database node down](/Images/APIPortal/API_Portal_HA_failover_db.png)

When a database node has been down and absent from a cluster for a time, you must repair the node after re-integrating it into the cluster. By design, the database node eventually becomes consistent with the other nodes.

The following applies in this scenario:

* The database cluster tolerates the loss of a node in the cluster, and ensures 100% data consistency.
* You must restart the database node that is down, and connect it to the cluster to synchronize and start operation.

### The whole datacenter is down

In this scenario, the whole datacenter goes down:

![Illustration of API Portal HA setup when the datacenter is down](/Images/APIPortal/API_Portal_HA_failover_dc.png)

The following applies in this scenario:

* No requests can be handled.
* Load balancer must properly handle failures.
* No data can be deployed until the system is back to normal.

#### Restart the datacenter

Restart the datacenter in the following sequence:

1. Restart the database nodes and reconfigure the cluster.
2. When all database nodes are running and synchronized, restart the API Portal instances.
3. Synchronize the shared file system on the API Portal instances.
