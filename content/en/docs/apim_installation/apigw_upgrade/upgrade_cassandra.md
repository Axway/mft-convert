{
"title": "Upgrade Apache Cassandra",
  "linkTitle": "Upgrade Apache Cassandra",
  "weight": 30,
  "date": "2019-10-07",
  "description": "Learn how to upgrade your Cassandra environment from version 2.2.8 or 2.2.12 to version 3.11.11 with no downtime."
}
The upgrade process differs according to your environment:

* For single-node environments, you can upgrade directly from version 2.2.8/2.2.12 to version 3.11.11.
* For multi-node or multi-DC environments, you must first upgrade each node and data center individually to version 2.2.19, then to the target version of 3.11.11.

{{< alert title="Note" color="primary" >}}It is possible to upgrade multi-node or multi-DC environments directly from 2.2.8/2.2.12 to version 3.11.11. However, this results in downtime of API Gateway and Cassandra during the upgrade process.{{< /alert >}}

## Before you start

* You must upgrade your API Gateway to the [August 2021](/docs/apim_relnotes/20210830_apimgr_relnotes/) release prior to upgrading your Cassandra environment to 3.11.11. This is because in this release we've implemented changes to the Cassandra client driver configuration to facilitate the upgrade process.
* In multi-datacenter clusters:

    * Upgrade every node in one datacenter before upgrading another datacenter.
    * Upgrade and restart the nodes one at a time.
    * Other nodes in the cluster continue to operate at the earlier version until all nodes are upgraded.
* When upgrading a cluster on a single-datacenter or multi-datacenter setup, you must avoid any schema changes until the entire cluster has been upgraded to the same version.
* Running `nodetool repair` on a Cassandra node affects performance on a system running live traffic. We recommend that you perform the Cassandra upgrade in the evening or during a maintenance window when the load is minimal.

## Update Cassandra's driver configuration in API Gateway

Before upgrading the Cassandra cluster, you must set Cassandra's driver protocol to version `V3` in each of your gateways:

1. Create a file called `jvm.xml` in the following location:

   ```
   INSTALL_DIR/apigateway/groups/GROUP_ID/INSTANCE_ID/conf
   ```
2. Edit the `jvm.xml` as follows:

   ```
   <ConfigurationFragment>
       <VMArg name="-DCASSANDRA_PROTOCOL_VERSION=3" />
   </ConfigurationFragment>
   ```
3. Restart API Gateway.

## Upgrade Cassandra in a multi-node single datacenter

The Cassandra upgrade in a multi-node environment follows a two-stage process:

1. Stage 1 - Upgrade Cassandra from version 2.2.8/2.2.12 to version 2.2.19.
2. Stage 2 - Upgrade Cassandra from version 2.2.19 to version 3.11.11.

### Stage 1 - Upgrade Cassandra 2.2.8/2.2.12 to 2.2.19

You must upgrade each node in your Cassandra cluster individually before moving to the next node in the cluster. The entire cluster must be upgraded before moving to Stage 2.

#### Complete your Cassandra 2.2.19 installation

* Download [Cassandra 2.2.19](https://support.axway.com/en/downloads/download-details/id/1449277).
* Unzip the downloaded package, and copy the installation directory to the target Cassandra server node in an appropriate directory, for example, `/home/cassandra-2219/`.

Perform the following steps:

##### Step 1 - Backup your old Cassandra data

Perform the backup of your 2.2.8/2.2.12 data.

1. Use the `cqlsh` command to find your Cassandra keyspaces to back up.

   ```
   SELECT * from system.schema_keyspaces;
   ```

   In the following example, `xxx_group_2` is an API Management keyspace:

   ```
   keyspace_name                                  | durable_writes | replication
   -----------------------------------------------+----------------+-------------------------------------------------------------------------------------------
   system_auth                                    |           True | {'class': 'org.apache.cassandra.locator.NetworkTopologyStrategy', 'dc1': '3', 'dc2': '3'}
   xb9076241_7cf8_4963_a3ac_375c53f21b7d_group_2  |           True | {'class': 'org.apache.cassandra.locator.NetworkTopologyStrategy', 'dc1': '3', 'dc2': '3'}
   system_schema                                  |           True | {'class': 'org.apache.cassandra.locator.LocalStrategy'}
   system_distributed                             |           True | {'class': 'org.apache.cassandra.locator.SimpleStrategy', 'replication_factor': '3'}
   system                                         |           True | {'class': 'org.apache.cassandra.locator.LocalStrategy'}
   system_traces                                  |           True | {'class': 'org.apache.cassandra.locator.SimpleStrategy', 'replication_factor': '2'}
   ```
2. Copy the `apigw-backup-tool` folder, located at `install_dir/apigateway/tools/` to your Cassandra node.
3. Update the `/conf/apigw-backup-tool.ini` file to configure your backup. For more information, see [Apache Cassandra backup and restore](/docs/cass_admin/cassandra_bur/#update-your-configuration-file).
4. After the configuration is set and Cassandra is running, run the following command to validate the configuration:

   ```
   apigw-backup-tool validateConfig
   ```
5. While Cassandra is running, run the following command to create a backup in the `backup_root_dir` folder. The folder is configured in the `/conf/apigw-backup-tool.ini` file:

   ```
   apigw-backup-tool backup -k <keyspace name> -s <snapshot name>
   ```
6. Backup your Cassandra configuration.

   In addition to backing up your data in Apache Cassandra keyspaces, you must also back up your Apache Cassandra configuration and API Gateway configuration. You must back up the `CASSANDRA_HOME/conf` directory on the Cassandra node.

##### Step 2 - Update Cassandra configuration files

Compare the configuration in `CASSANDRA_HOME/conf` of your current Cassandra installation to the configuration in the new Cassandra 2.2.19 installation, and apply any custom changes to the relevant configuration file in your new Cassandra installation.

##### Step 3 - Copy SSL certificates

If you have SSL certificates in your old Cassandra installation, you must copy them to your new Cassandra installation.

Copy the following files to the `CASSANDRA_HOME/conf/` folder in your new installation:

* `CASSANDRA_HOME/conf/.truststore`
* `CASSANDRA_HOME/conf/.keystore`

##### Step 4 - Shutdown your old Cassandra installation

To shutdown your old Cassandra installation, follow these steps:

1. Run Cassandra's `nodetool drain` command in `CASSANDRA_HOME/bin` to flush any data in memory and write it to disk:

   ```
   $./nodetool drain
   ```
2. Kill the Cassandra process:

   ```
   $ps auwx | grep cassandra
   $sudo kill pid #Stop Cassandra
   ```

##### Step 5 - Copy data between Cassandra installations

Copy the `CASSANDRA_HOME/data` directory from your old Cassandra installation to the corresponding directory in your new installation (for example, `/home/cassandra-31111/cassandra/`).

The Cassandra 2.2.19 `data` directory should then have the following subdirectories:

```
commitlog
data
saved_caches
```

##### Step 6 - Start Cassandra 2.2.19

Run the following command from the `bin` directory of the Cassandra 2.2.19 installation to start the Cassandra instance:

```
$cd /home/cassandra-31111/cassandra/bin
$./cassandra
```

#### Repair and upgrade your tables

After you upgrade Cassandra in all nodes, run the following commands to repair and upgrade your tables:

1. Repair tables on your new installation:

   ```
   $cd /home/cassandra-2219/cassandra/bin
   $./nodetool repair
   ```
2. Rewrite SSTables that are not on the current version and upgrade them to the 2.2.19 version:

   ```
   $cd /home/cassandra-2219/cassandra/bin
   $./nodetool upgradesstables
   ```

### Stage 2 - Upgrade Cassandra 2.2.19 to 3.11.11

To start upgrading your old installation:

* Download [Cassandra 3.11.11](https://support.axway.com/en/downloads/download-details/id/1449279)
* Unzip the downloaded package, and copy the installation directory to the target Cassandra server node in an appropriate directory, for example, `/home/cassandra-31111/`.

To complete your installation, perform all steps from section [Complete your Cassandra 2.2.19 installation](#complete-your-cassandra-2219-installation) for each API Gateway node in the cluster, by replacing the 2.2.19 version with target version 3.11.11.

## Revert Cassandra's driver configuration in API Gateway

After you upgrade the Cassandra cluster, you must set Cassandra's driver protocol back to version `V4` in each of your gateways:

1. To reconfigure the protocol version on each instance of API Gateway, edit the `INSTALL_DIR/apigateway/groups/GROUP_ID/INSTANCE_ID/conf/jvm.xml` file, and remove the previously set JVM argument.

   ```
   <ConfigurationFragment>
       <VMArg name="-DCASSANDRA_PROTOCOL_VERSION=4" />
   </ConfigurationFragment>
   ```

   This reverts the gateway to use Cassandra's V4 protocol.
2. Restart API Gateway.

## Upgrade Cassandra in a multi-node multi-datacenter

To upgrade Apache Cassandra in a multi-node multi-datacenter environment, follow the same steps as for the [Multi-node single-datacenter](#upgrade-cassandra-in-a-multi-node-single-datacenter) procedure, which follows a two-stage process: upgrade Cassandra from version 2.2.8/2.2.12 to version 2.2.19, then upgrade from version 2.2.19 to version 3.11.11.

Attention to the following:

* Repeat the steps on each node in the cluster.
* Upgrade all of the nodes in one datacenter before moving to the next one.
* Only reconfigure the API Gateway to use Cassandra protocol `V4` after all datacenter have been upgraded.

## Upgrade multi-node multi-datacenter directly to 3.11.11

{{< alert title="Caution" color="warning" >}}Upgrading multi-node environments directly to version 3.11.11 result in downtime of API Gateway and Cassandra.{{< /alert >}}

To upgrade Apache Cassandra directly to version 3.11.11 in a multi-node or multi-datacenter environment, follow the same steps as section [Upgrade Cassandra in a multi-node single datacenter, Stage 2](#stage-2---upgrade-cassandra-2219-to-31111).

Attention to the following:

* Repeat the steps on each node in the cluster.
* Upgrade all of the nodes in one DC before moving to the next DC.
* Only restart the API Gateway once all nodes and DCs have been upgraded.

## Upgrade Cassandra in a single-node

In a single-node environment, you can upgrade directly to version 3.11.11.

Perform the following to get started:

1. Download [Cassandra 3.11.11](https://support.axway.com/en/downloads/download-details/id/1449279)
2. Unzip the downloaded package
3. Copy the installation directory to the target Cassandra server node in an appropriate directory, for example, `/home/cassandra-31111/`.
4. To complete your installation, perform all steps from section [Upgrade Cassandra in a multi-node single datacenter](#stage-1---upgrade-cassandra-2282212-to-2219) by replacing the 2.2.19 version with target version 3.11.11.

## Troubleshooting

For more information on problems you might encounter when running Cassandra with API Gateway, see [Cassandra troubleshooting](/docs/cass_admin/cassandra_troubleshooting).
