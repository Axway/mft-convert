---
title: "Manage Apache Cassandra"
linkTitle: "Manage Apache Cassandra"
weight: 4
date: 2019-06-05
description: >
  Start or stop Cassandra manually or as a service.
---
On Linux, when you select to install Cassandra using the API Gateway installer as part of a Standard or Complete setup, Cassandra starts automatically. To start or stop Cassandra manually or as a service, perform the steps described in this page.

{{% alert title="Note" %}}
Before you start Cassandra, you must follow the best practices in [Apache Cassandra Best Practices](/docs/cass_admin/cassandra_bestpractices/) to achieve a stable Cassandra environment, and to prevent data integrity and performance issues.
{{% /alert %}}

## Manage Apache Cassandra

This section explains how to start and stop Cassandra.

### Start Apache Cassandra

To start Cassandra in the background, change to the bin directory and run the following command:

```
cd CASSANDRA_HOME/bin
./cassandra
```

To start Cassandra in the foreground, run the following command:

```
./cassandra -f
```

For more details, see [Running Cassandra in the Apache Cassandra Wiki](https://cwiki.apache.org/confluence/display/CASSANDRA2/RunningCassandra).

### Start Cassandra as a service

To install Cassandra as a service, you must install and configure the appropriate startup script for your system. For example, see the following example startup scripts:

* **CentOS**: [cassandra.centos.service](/samples/apimanagement/cassandra/cassandra.centos.service)
* **Debian**: [cassandra.debian.service](/samples/apimanagement/cassandra/cassandra.debian.service)

You must have root or sudo permissions to start Cassandra as a service. For example, typically the command to start Cassandra as a service is as follows:

```
sudo service cassandra start
```

For more details on how to configure services, see this [Knowledge Base article](https://support.axway.com/kb/178063/language/en) in the Axway Support site.

### Stop Cassandra

1. Find the Cassandra Java process ID (PID):

    ```
    ps auwx | grep cassandra
    ```

2. Run the following command:

    ```
    sudo kill *pid*
    ```

### Stop Cassandra as a service

You must have `root` or `sudo` permissions to stop the Cassandra service as follows:

```
sudo service cassandra stop
```

## Connect to API Gateway for the first time

Connecting to API Gateway depends on your operating system and installation setup type (Standard, Complete, or Custom).

### Standard or Complete setup

If you installed a default Standard or Complete setup (both include the QuickStart tutorial), Cassandra is installed on the same host, and listens on `localhost` by default. API Gateway runs on the same host and connects to Cassandra by default.

### Custom setup

If you installed a Custom setup and did not select the Quickstart tutorial, you must update the API Gateway Cassandra client configuration as follows:

1. Open the API Gateway group configuration in Policy Studio.
2. Select **Server Settings > Cassandra > Authentication**, and set the Cassandra username/password (default is `cassandra`/`cassandra`).
3. Select **Server Settings > Cassandra > Hosts**. Add an address for the Cassandra node. You can enter an IP address or host name.
4. Deploy the configuration to the API Gateway group.

For more details on configuring **Server Settings** in the Policy Studio client, see [Cassandra Settings](/docs/apim_reference/cassandra_settings/).

For details on updating the Cassandra server configuration, see [Configure a highly available Cassandra cluster](/docs/cass_admin/cassandra_config/).

## Further details

For more details on Apache Cassandra, see the following:

* [Apache Cassandra](http://cassandra.apache.org/) documentation.
* [Datastax](https://docs.datastax.com/en/cassandra-oss/3.x/index.html) documentation.
