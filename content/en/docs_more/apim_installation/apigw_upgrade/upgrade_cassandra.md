{
    "title": "Upgrade Apache Cassandra",
    "linkTitle": "Upgrade Apache Cassandra",
    "weight": 30,
    "date": "2019-10-07",
    "description": "Upgrade Apache Cassandra from version 2.2.8 to version 2.2.12."
}

It is recommended that you upgrade your Cassandra version *before* you upgrade your API Gateway installation to version 7.7.

## Best practice

We recommend the following when upgrading your Apache Cassandra version:

* Upgrade your API Gateway installation after upgrading your Apache Cassandra version.
* In multi-datacenter clusters, upgrade every node in one datacenter before upgrading another datacenter. Upgrade and restart the nodes one at a time. Other nodes in the cluster continue to operate at the earlier version until all nodes are upgraded.
* When upgrading a cluster on a single-datacenter or multi-datacenter setup, you must avoid any schema changes until the entire cluster has been upgraded to the same version.
* Running `nodetool repair` on a Cassandra node will affect performance on a system running live traffic. It is recommended that you perform the Cassandra upgrade in the evening or during a maintenance window when the load is minimal.

## Cassandra upgrade steps – Single-node

The following steps give an example of how to upgrade Cassandra in a single-node setup.

### Install Cassandra 2.2.12

Follow these steps to install Apache Cassandra 2.2.12 using the API Gateway installer in default GUI mode.

1. Select the **Custom** option in the installer.
2. When prompted to select components to install, select only the **Cassandra** component.
3. When prompted for the Cassandra configuration, enter the following settings:

    | Installation Directory   | JRE Location |
    |--------------------------|--------------|
    | `/opt/db/cassandra-2212` | `/opt/jre`   |

4. Do not start Cassandra 2.2.12 when the installation completes. (Your earlier version of Cassandra should still be running.)

You can also install Cassandra in unattended mode, for example:

```
./APIGateway_7.7_Install_linux-x86-64_BN<n>.run --mode unattended --setup_type advanced --enable-components cassandra --disable-components apigateway,qstart,policystudio,analytics,configurationstudio,apitester,apimgmt,packagedeploytools --cassandraInstalldir /opt/db/cassandra-2212 --cassandraJDK /opt/jre --startCassandra 0
```

### Run nodetool drain on Cassandra 2.2.8

Run the following commands to drain the node and flush the memTables to the SSTables before copying data to the Cassandra 2.2.12 installation. Writes are not accepted while this is happening.

```
cd /opt/db/cassandra-228/cassandra/bin
./nodetool drain
```

### Stop Cassandra 2.2.8

Run the following commands to stop Cassandra.

```
ps –ef | grep cassandra
sudo kill -9 <cassandra_pid>
```

### Copy data from Cassandra 2.2.8 to Cassandra 2.2.12

Run the following command to copy the data folder and all subfolders from your Cassandra 2.2.8 installation to your new Cassandra 2.2.12 installation.

```
cp –R /opt/db/cassandra-228/cassandra/data /opt/db/cassandra-2212/cassandra
```

For example, after running this command, you should have the following directories:

* `/opt/db/cassandra-2212/cassandra/data/commitlog`
* `/opt/db/cassandra-2212/cassandra/data/data`
* `/opt/db/cassandra-2212/cassandra/data/saved_caches`

{{< alert title="Tip" color="primary" >}}By default, all Cassandra data is stored in the `data` directory. However, in a production environment you should store the commit log (for example, `/opt/db/cassandra-2212/cassandra/data/commitlog`) on a separate disk partition, or a separate physical device from the data file directories. You can change the default locations in `cassandra.yaml` (`commitlog_directory`, `data_file_directories`, and `saved_caches_directory` properties). For more information, see <http://docs.datastax.com/en/archived/cassandra/2.2/cassandra/configuration/configCassandra_yaml.html>.{{< /alert >}}

### Copy SSL certificates from Cassandra 2.2.8 to Cassandra 2.2.12

If you have SSL certificates in your Cassandra 2.2.8 installation, copy them to Cassandra 2.2.12. To copy the SSL certificates, copy the following files to `/opt/db/cassandra-2212/cassandra/conf/`:

* `/opt/db/cassandra-228/cassandra/conf/.truststore`
* `/opt/db/cassandra-228/cassandra/conf/.keystore`

### Update Cassandra 2.2.12 configuration files

Update your Cassandra 2.2.12 configuration files with the relevant settings from your Cassandra 2.2.8 installation. The following files must be updated:

* `/opt/db/cassandra-2212/cassandra/conf/cassandra.yaml`
* `/opt/db/cassandra-2212/cassandra/bin/cassandra.in.sh`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra.rackdc.properties`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra-topology.properties`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra-env.sh` (This file only needs to be updated if you changed your JMX configuration after Cassandra 2.2.8 installation)

You can do a diff on the files to see a complete list of the differences. The following are the values in each file that must be updated:

`/opt/db/cassandra-2212/cassandra/conf/cassandra.yaml`:

```
rpc_address: <Set to the IP address of this Cassandra node>
listen_address: <Set to the IP address of this Cassandra node>
seed_provider:
    # Addresses of hosts that are deemed contact points.
    # Cassandra nodes use this list of hosts to find each other and learn
    # the topology of the ring.  You must change this if you are running
    # multiple nodes!
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
          # seeds is actually a comma-delimited list of addresses.
          # Ex: "<ip1>,<ip2>,<ip3>"
          - seeds: "<IP seed1>,<IP seed 2>"
server_encryption_options: <Set to same values as your 2.2.8 installation>
client_encryption_options: <Set to same values as your 2.2.8 installation>
```

`/opt/db/cassandra-2212/cassandra/bin/cassandra.in.sh`:

```
JAVA_HOME=<Set to the location of a 64-bit JRE, same value as your 2.2.8 installation>
```

`/opt/db/cassandra-2212/cassandra/conf/cassandra.rackdc.properties`:

Perform a diff on this file to identify the rack settings to update.

`/opt/db/cassandra-2212/cassandra/conf/cassandra-topology.properties`:

Perform a diff on this file to identify the rack settings to update.

`/opt/db/cassandra-2212/cassandra/conf/cassandra-env.sh`

If you changed your JMX configuration after Cassandra 2.2.8 installation, perform a diff on this file to identify the JMX settings to update.

### Start Cassandra 2.2.12

Run the following commands to start Cassandra.

```
cd /opt/db/cassandra-2212/cassandra/bin
./cassandra
```

### Run nodetool upgradesstables on Cassandra 2.2.12

Run the following commands to rewrite SSTables that are not on the current version and upgrade them to Cassandra version 2.2.12.

```
cd /opt/db/cassandra-2212/cassandra/bin
./nodetool upgradesstables
```

### Run nodetool repair on Cassandra 2.2.12

Run the following commands to repair the tables.

```
cd /opt/db/cassandra-2212/cassandra/bin
./nodetool repair
```

## Cassandra upgrade steps – Multi-node single datacenter

The following steps give an example of how to upgrade Cassandra in a single datacenter HA setup. In this example the datacenter has 2 API Gateways, 3 Cassandra nodes, and 2 groups.

The following steps give an example of how to upgrade a Cassandra cluster on a three-node HA setup with the following topology:

* Node 1 – Admin Node Manager, Cassandra 2.2.8
* Node 2 – Admin Node Manager, API Gateway, API Manager, Cassandra 2.2.8
* Node 3 – Node Manager, API Gateway, API Manager, Cassandra 2.2.8

There are two API Gateway groups, therefore two keyspaces. The groups are as follows:

* `Group1` has two API Manager-enabled API Gateway instances (one running on Node 2 and another on Node 3).
* `Group2` has two API Manager-enabled API Gateway instances (one running on Node 2 and another on Node 3).

![Multi-node HA topology](/Images/UpgradeGuide/cass_upgrade_topology2.png)

Cassandra is set up as follows:

* Cassandra read/write consistency is set to Local Quorum
* Cassandra authentication is set (user name and password set to `cassUser`/`cassPasswd`)
* SSL encryption is enabled on Cassandra

### Node 1

Perform the following steps on Node 1.

#### Install Cassandra 2.2.12 on Node 1

Follow these steps to install Apache Cassandra 2.2.12 using the API Gateway installer in default GUI mode.

1. Select the **Custom** option in the installer.
2. When prompted to select components to install, select only the **Cassandra** component.
3. When prompted for the Cassandra configuration, enter the following settings:

    | Installation Directory   | JRE Location |
    |--------------------------|--------------|
    | `/opt/db/cassandra-2212` | `/opt/jre`   |

4. Do not start Cassandra 2.2.12 when the installation completes. (Your earlier version of Cassandra should still be running.)

You can also install Cassandra in unattended mode, for example:

```
./APIGateway_7.7_Install_linux-x86-64_BN<n>.run --mode unattended --setup_type advanced --enable-components cassandra --disable-components apigateway,qstart,policystudio,analytics,configurationstudio,apitester,apimgmt,packagedeploytools --cassandraInstalldir /opt/db/cassandra-2212 --cassandraJDK /opt/jre --startCassandra 0
```

#### Run nodetool drain on Cassandra 2.2.8 on Node 1

Run the following commands to drain the node and flush the memTables to the SSTables before copying data to the Cassandra 2.2.12 installation. Writes are not accepted while this is happening.

```
cd /opt/db/cassandra-228/cassandra/bin
./nodetool drain
```

#### Stop Cassandra 2.2.8 on Node 1

Run the following commands to stop Cassandra.

```
ps –ef | grep cassandra
sudo kill -9 <cassandra_pid>
```

#### Copy data from Cassandra 2.2.8 to Cassandra 2.2.12 on Node 1

Run the following command to copy the data folder and all subfolders from your Cassandra 2.2.8 installation to your new Cassandra 2.2.12 installation.

```
cp –R /opt/db/cassandra-228/cassandra/data /opt/db/cassandra-2212/cassandra
```

For example, after running this command, you should have the following directories:

* `/opt/db/cassandra-2212/cassandra/data/commitlog`
* `/opt/db/cassandra-2212/cassandra/data/data`
* `/opt/db/cassandra-2212/cassandra/data/saved_caches`

{{< alert title="Tip" color="primary" >}}By default, all Cassandra data is stored in the `data` directory. However, in a production environment you should store the commit log (for example, `/opt/db/cassandra-2212/cassandra/data/commitlog`) on a separate disk partition, or a separate physical device from the data file directories. You can change the default locations in `cassandra.yaml` (`commitlog_directory`, `data_file_directories`, and `saved_caches_directory` properties). For more information, see <http://docs.datastax.com/en/archived/cassandra/2.2/cassandra/configuration/configCassandra_yaml.html>.{{< /alert >}}

#### Copy SSL certificates from Cassandra 2.2.8 to Cassandra 2.2.12 on Node 1

If you have SSL certificates in your Cassandra 2.2.8 installation, copy them to Cassandra 2.2.12. To copy the SSL certificates, copy the following files to `/opt/db/cassandra-2212/cassandra/conf/`:

* `/opt/db/cassandra-228/cassandra/conf/.truststore`
* `/opt/db/cassandra-228/cassandra/conf/.keystore`

#### Update Cassandra 2.2.12 configuration files on Node 1

Update your Cassandra 2.2.12 configuration files with the relevant settings from your Cassandra 2.2.8 installation. The following files must be updated:

* `/opt/db/cassandra-2212/cassandra/conf/cassandra.yaml`
* `/opt/db/cassandra-2212/cassandra/bin/cassandra.in.sh`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra.rackdc.properties`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra-topology.properties`
* `/opt/db/cassandra-2212/cassandra/conf/cassandra-env.sh` (This file only needs to be updated if you changed your JMX configuration after Cassandra 2.2.8 installation)

You can do a diff on the files to see a complete list of the differences. The following are the values in each file that must be updated:

`/opt/db/cassandra-2212/cassandra/conf/cassandra.yaml`:

```
rpc_address: <Set to the IP address of this Cassandra node>
listen_address: <Set to the IP address of this Cassandra node>
seed_provider:
    # Addresses of hosts that are deemed contact points.
    # Cassandra nodes use this list of hosts to find each other and learn
    # the topology of the ring.  You must change this if you are running
    # multiple nodes!
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
          # seeds is actually a comma-delimited list of addresses.
          # Ex: "<ip1>,<ip2>,<ip3>"
          - seeds: "<IP seed1>,<IP seed 2>"
server_encryption_options: <Set to same values as your 2.2.8 installation>
client_encryption_options: <Set to same values as your 2.2.8 installation>
```

`/opt/db/cassandra-2212/cassandra/bin/cassandra.in.sh`:

```
JAVA_HOME=<Set to the location of a 64-bit JRE, same value as your 2.2.8 installation>
```

`/opt/db/cassandra-2212/cassandra/conf/cassandra.rackdc.properties`:

Perform a diff on this file to identify the rack settings to update.

`/opt/db/cassandra-2212/cassandra/conf/cassandra-topology.properties`:

Perform a diff on this file to identify the rack settings to update.

`/opt/db/cassandra-2212/cassandra/conf/cassandra-env.sh`

If you changed your JMX configuration after Cassandra 2.2.8 installation, perform a diff on this file to identify the JMX settings to update.

#### Start Cassandra 2.2.12 on Node 1

Run the following commands to start Cassandra.

```
cd /opt/db/cassandra-2212/cassandra/bin
./cassandra
```

#### Run nodetool upgradesstables on Cassandra 2.2.12 on Node 1

Run the following commands to rewrite SSTables that are not on the current version and upgrade them to Cassandra version 2.2.12.

```
cd /opt/db/cassandra-2212/cassandra/bin
./nodetool upgradesstables
```

### Node 2

Repeat the same steps on Node 2.

### Node 3

Repeat the same steps on Node 3.

### Final step – Run nodetool repair on Cassandra 2.2.12 on each node

Run the following commands, one at a time on each node, to repair the tables.

```
cd /opt/db/cassandra-2212/cassandra/bin
./nodetool repair
```

## Cassandra upgrade – Multi-datacenter

To upgrade Apache Cassandra in a multi-DC setup, you can follow the same steps as for the multi-node single-DC setup:

* Repeat the steps on each node in the cluster
* Upgrade all of the nodes in one DC before moving to the next DC
