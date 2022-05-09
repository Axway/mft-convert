{
"title": "Operate and maintain API Gateway",
"linkTitle": "Operate and maintain API Gateway",
"weight":"40",
"date": "2019-10-14",
"description": "API Gateway operations, including starting and stopping, zero-downtime shutdown, and backup and disaster recovery."
}

## Start and stop the API Gateway

This section describes how to start and stop the Node Manager and the API Gateway instance on the command line. It also describes how to start Policy Studio.

You can also start and stop API Gateway instances using API Gateway Manager. For more details, see [Manage API Gateway instances](/docs/apim_administration/apigtw_admin/managetopology#manage-api-gateway-instances).

### Prerequisites

Before you can start API Gateway, you must first create a new domain that includes an API Gateway instance. If you installed the QuickStart tutorial, a sample API Gateway domain is automatically configured in your installation. Otherwise, you must create a new domain. For more details, see [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway).

If you are using Apache Cassandra, before starting API Gateway, you must first ensure that Cassandra is running. For details on installing and running Cassandra, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install/).

### Set passphrases

By default, data is stored unencrypted in the API Gateway configuration store. However, you can encrypt certain sensitive information such as passwords and private keys using a passphrase. When the passphrase has been set, this encrypts the API Gateway configuration data.

You must enter the passphrase when connecting to the configuration data (for example, using Policy Studio, or when the API Gateway starts up). For more details on configuring this passphrase, see [Configure an API Gateway encryption passphrase](/docs/apim_administration/apigtw_admin/general_passphrase/).

### Start the Node Manager

To start the Node Manager on Linux, complete the following steps:

1. Open a shell at the `/posix/bin` directory of your API Gateway installation.
2. Run the `nodemanager.sh` file, for example:

    ```
    ./nodemanager
    ```

3. If you are using an encryption passphrase, you are prompted for this passphrase.

### Start the API Gateway instance

To start the gateway instance and Policy Studio on Linux, perform the following steps:

1. Open a shell at the `/posix/bin` directory of your gateway installation.
2. Ensure that the `startinstance` file has execute permissions and run the `startinstance` command, for example:

    ```
    startinstance -n "my_server" -g "my_group"
    ```

3. If you are using an encryption passphrase, you are prompted for this passphrase.
4. When API Gateway has successfully started, run the`policystudio.sh` file in your Policy Studio installation directory. For example:

    ```
    cd /usr/home/policystudio
    ./policystudio
    ```

5. When Policy Studio is starting up, you are prompted for connection details for API Gateway.

#### Use `startinstance` without arguments

Enter the `startinstance` command without any arguments to display the servers registered on the machine. For example:

```
$ startinstance
usage:"startinstance [[-n instance-name -g group-name [instance-args]] | [directory-location [instance-args]]]"

The API Gateway instances listed below are available to run on this machine as follows:

startinstance -n "server1" -g "group1"
startinstance -n "server2" -g "group2"
```

If you have a single API Gateway instance on the host on which you run `startinstance`, that instance starts when you specify no arguments.

#### Startup options

You can specify the following optional instance arguments to the `startinstance` command:

| Option          | Description                                                |
|-----------------|------------------------------------------------------------|
| `-b <file>`       | Specifies the boot-time trace destination.                 |
| `-d`              | Runs as daemon/service.                                    |
| `-h <directory>`  | Specifies the service instance directory.                  |
| `-k`              | Kills the instance.                                       |
| `-P`              | Prompts for a passphrase at startup.                       |
| `-q`              | Runs in quiet mode (equivalent to `-Dquiet`).                |
| `-v`              | Changes to the installation instance directory on startup. |
| `-s`              | Tests if the service is running.                          |
| `-e`              | Specifies the end of arguments for `vshell`.                 |
| `-D prop[=value]` | Sets a configuration file property.                        |

The `-d`, `-s`, and `-k` options are designed for use with the service controller (for example, traditional SVR4 init, systemd, upstart, and so on). The `-d` option waits until the service is running before returning, and `-k` waits for the process to terminate. This means that when used in a script, the completion of the command indicates that the operation requested has completed.

If the service is running, `-s` will exit with a `0` status code, and with `1` otherwise. For example, you can use the following to print a message if the service is running:

```
startinstance -s -n InstanceName -g GroupName && echo Running
```

### Connect to API Gateway in Policy Studio

When starting Policy Studio, you are prompted for details on how to connect to the Admin Node Manager (for example, the server session, host, port, user name, and password). The default connection URL is:

```
https://<HOST>:8090/api
```

`HOST` is the IP address or host name of the machine on which the API Gateway runs.

### Stop API Gateway

To stop the API Gateway instance, you must specify the group and instance name to the `startinstance` command along with the `-k` option. For example:

```
./startinstance -g "my_group" -n "my_server" -k
```

### Stop the Node Manager

To stop the Node Manager, you must specify the `nodemanager` command along with the `-k` option. For example:

```
./nodemanager -k
```

## Shut down an API Gateway using zero downtime shutdown {#zero-downtime-shutdown}

Perform a zero downtime shutdown of a gateway in a multi-node API Gateway environment with a load balancer.

When you need to shut down a gateway for any reason (for example, during an upgrade), zero downtime shutdown enables you to indicate this to the load balancer for a set amount of time before the shutdown begins, avoiding traffic loss.

To perform a zero downtime shutdown, follow these steps:

1. Enable zero downtime shutdown in Policy Studio, and set the delay before shutdown. For more information, see [Zero downtime settings](/docs/apim_reference/general_settings/#zero-downtime-settings).
2. Configure your load balancer to ping the Health Check LB policy periodically to determine if each API Gateway is healthy. This is available on the following default URL:

    ```
    http://APIGATEWAY_HOST:8080/healthchecklb
    ```

3. Initiate shutdown of an API Gateway using the command line or API Gateway Manager.
4. When shutdown is initiated on the API Gateway:
    * The Health Check LB policy returns a `503 Service Unavailable` response. This indicates to the load balancer that the API Gateway is not available for traffic and the load balancer stops routing to it.
    * After the specified delay before shutdown (for example, 10 seconds), the API Gateway is shut down.

## API Gateway backup and disaster recovery

You must ensure that your API Gateway system can recover from any natural disasters (for example, floods, hurricanes, or earthquakes) and human-induced disasters (for example, failures, fires, or explosions).

Many organizations have a mirrored backup and disaster recovery site with full capacity to recover from any major incidents. These systems are typically kept in a separate physical location on cold stand-by until they need to be brought into action. In this case, the backup and disaster site must be a mirrored image of production environment that replicates all resources and assets (for example, LDAP and databases), and with the same number of gateway instances. You should ensure that any required third-party systems are included in your backup and recovery solution.

Example approaches to keeping production and backup environments in sync include making backups to disk or tape, and sending these off-site at regular intervals, or cloud-based solutions that replicate on-site systems, and back up to off-site datacenters.

For details on backing up and restoring an Admin Node Manager in a highly available environment, see [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr).

### Components that must be backed up

Whichever backup strategy you choose, in a production environment you must ensure that the gateway installations on all gateway host nodes are backed up on a regular basis. For example, this includes hosts that run the following components:

* API Gateway instance
* Admin Node Manager
* Node Manager
* API Manager
* API Gateway Analytics

You must also back up all databases and third-party systems used with the API Gateway. For example, this includes the following:

* Databases used by API Gateway and API Manager to store metrics (for example, MySQL, Oracle, MS SQL, or DB2)
* Apache Cassandra database used by API Gateway and API Manager for internal data storage.
* Shared disks used by embedded ActiveMQ to store JMS messages
* Any databases or third-party systems that the API Gateway connects to in External Connections

{{< alert title="Note" color="primary" >}}
You do not need to back up Policy Studio or Configuration Studio because these tools run in a temporary workspace when required. However, if you have modified any third-party dependencies on the **Preferences** page (for example, to connect to a specific database), you must also add the relevant `.jar` on the **Runtime Dependencies** page in your disaster recovery environment.{{< /alert >}}

### Back up API Gateway

Before starting the back up, you can remove the contents of the `apigateway/conf/opsdb.d` directory. This contains transient monitoring data, which can be quite large in some cases, and does not need to be backed up.

To back up the gateway installation, you must back up files that have changed in the following directory:

```
<install-dir>/apigateway
```

This backs up all Admin Node Manager, Node Manager, API Manager, and API Gateway instances in that installation.

For example, the following directories include API Gateway configuration, and will typically need to be backed up on a regular basis:

```
<install-dir>/apigateway/conf
<install-dir>/apigateway/groups
<install-dir>/apigateway/ext
<install-dir>/apigateway/system/conf/nodemanager.xml
```

### Back up before applying a service pack or installing an update

Updates or service packs require a full backup of the API Gateway installation, to enable you to restore the previous installation.

For example, the following directories should always be backed up before applying a service pack or installing an update:

```
<install-dir>/apigateway/conf
<install-dir>/apigateway/groups
<install-dir>/apigateway/samples
<install-dir>/apigateway/skel
<install-dir>/apigateway/system
<install-dir>/apigateway/platform (Win32 for Windows)
<install-dir>/apigateway/tools
<install-dir>/apigateway/upgrade
<install-dir>/apigateway/webapps
```

### Back up API Gateway Analytics

Similarly, to back up an API Gateway Analytics installation, you must back up files that have changed in the following directory:

```
<install-dir>\analytics
```

For example, the following directories include API Gateway Analytics configuration, and will typically need to be backed up on a regular basis:

```
<install-dir>\analytics\conf
<install-dir>\analytics\ext
```

This backs up the API Gateway Analytics installation only. You must also back up the metrics database separately.

### Back up databases and third-party systems

You must back up all databases and third-party systems used with API Gateway. For example, you can back a MySQL database by creating a dump file of the tables in use:

```
mysqldump -u root temp_backup > db_tables.dump
```

For more details, see the user documentation for your third-party system.

### Disaster recovery plan and tests

To ensure that your backup and disaster recovery processes are successful, you should conduct full restoration tests on a regular basis. You must ensure that you can restore all required files as quickly and easily as possible.

To ensure this, your backup and disaster recovery plan should include key metrics for recovery points and recovery times for your real-world business processes (for example, creating a purchase order, booking a reservation, and so on).

### Example of creating an API Gateway disaster recovery site

This simple example shows how to create a disaster recovery site from a backup of an API Gateway production deployment. It assumes that both the disaster recovery site and the primary production site have the same version of API Gateway installed. In this scenario, the disaster recovery site is a cold standby, and the configuration from production is replicated using a backup of production configuration.

#### Back up the production environment

To back up the production environment, perform the following steps.

1. Browse to the directory where the API Gateway is installed (for example, `/opt/apigateway`).
2. Tar or zip the following:
    * `apigateway/groups`
    * `apigateway/conf`
    * `apigateway/system/conf/nodemanager.xml`
    * `apigateway/ext`

If you want the API Gateway and Node Manager to start up automatically on the new host, you should also include `/etc/init.d/vshell-*`.

This includes separate startup scripts files for the Node Manager and API Gateway instances if an `init.d` script was created using `managedomain` during initial setup.

You can create these at any time using `managedomain`, and choosing option `2`, Edit a host, for a Node Manager, or option `10`, Add script or service for an existing local API Gateway. For more details, see [Managedomain command reference](/docs/apim_reference/managedomain_ref/).

For example, the following command creates a `.tar` file running from the root directory:

```
tar -cvf apigateway_backup.tar /opt/apigateway/conf /opt/apigateway/groups /opt/apigateway/system/conf/nodemanager.xml
```

The following example creates a `.tar` file containing the startup scripts running from the root directory:

```
tar -cvf startup_scripts.tar /etc/init.d/vshell-*
```

#### Copy to the disaster recovery site

To replicate to the disaster recovery site:

1. Ensure that the files are tarred before copying because this preserves the permissions and ownership of the files.
2. Copy the created `.tar` files from the primary production machine to the cold standby machine.
3. Extract the files so that they are extracted in the same directories, overwriting existing files if necessary. The following example extracts the files from the root directory:

    ```
    tar -C / -xvf apigateway_backup.tar
    ```

4. The following is optional:

    ```
    tar -C / -xvf startup_scripts.tar
    ```

5. When all the files are copied over and extracted successfully, you should be able to start the Admin Node Manager and API Gateway instances the same way as in primary production site running the same topology and configurations.

This example assumes that backups are collected on regular basis from the master node in the production site.

### Further information

For details on how to back up and restore an Admin Node Manager for signing SSL certificates in an API Gateway domain, see [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr).

For details on how to back and restore internal data stored in Apache Cassandra (for example, API Gateway KPS data or API Manager data), see [Apache Cassandra backup and restore](/docs/cass_admin/cassandra_bur/).
