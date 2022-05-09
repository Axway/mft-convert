---
title: Configure API Gateway with the metrics database
linkTitle: Configure API Gateway with the metrics database
weight: "3"
date: 2019-10-07
description: Configure an API Gateway instance and Node Manager to store metrics
  on historic traffic in a relational database.
---

 When API Gateway is configured to store metrics in a relational database, you can configure monitoring in API Gateway Analytics or API Manager to view data stored in the metrics database, or write custom SQL queries to retrieve metrics data as required.

{{< alert title="Note" color="primary" >}}This topic explains how to configure API Gateway with a metrics database. This topic assumes that you have already created your metrics database using the steps described in
[Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install/).{{< /alert >}}

## API Gateway metrics data streams

The following data streams are used to populate the metrics database:

* **Transaction and system data**: Transaction data includes clients, services, remote hosts, and protocols. System data includes CPU, memory and disk usage, and SLA breaches. The API Gateway writes this data to a transaction event log, with a new log file automatically created every 5 minutes. The Node Manager parses completed event logs and updates the metrics database.
* **Transaction audit log events**: These are written directly to the metrics database by the API Gateway instance.

## Connect to the API Gateway in Policy Studio

 To connect to the API Gateway in Policy Studio, perform the following steps:

1. Ensure the Admin Node Manager and API Gateway are running.
2. Create a new project or open an existing project based on a running API Gateway instance.

## Configure the metrics database connection

To configure the API Gateway connection to the metrics database, perform the following steps:

1. Expand the **Environment Configuration > External Connections > Database Connections** node in the Policy Studio tree.
2. Right-click the **Default Database Connection** tree node, and select **Edit**.
3. Configure the database connection to point to your metrics database.
4. Ensure that Policy Studio can make a network connection to the database server, and click the **Test Connection** button on the **Configure Database Connection** dialog to verify that the connection to the database is configured successfully. This enables you to detect any configuration errors at design time instead at runtime.

You can troubleshoot your database connection by viewing the contents of your server `.trc` file in the `INSTALL_DIR/apigateway/trace` directory. For more details, see [Configure API Gateway diagnostic trace](/docs/apim_administration/apigtw_admin/tracing/).

## Configure transaction audit logging to the metrics database

To configure the API Gateway instance to write transaction audit log data to the metrics database, perform the following steps:

1. In the Policy Studio tree, select the **Server Settings** node, and select ***Logging > Transaction Audit Log** in the window on the right.
2. Select the **Database** tab, and select **Enable logging to database**.
3. Select the **Default Database Connection** from the drop-down list if appropriate. Alternatively, select a database connection that you have configured. You must ensure that your database connection points to your metrics database.

{{< alert title="Tip" color="primary" >}}To write the content of message transactions to the database, you must also configure the **Log Message Payload** filter in your policies (for example, at the start and end of the policy).{{< /alert >}}

## Configure the API Gateway to write to the transaction event log

To configure the API Gateway instance to write transaction data to the transaction event log, perform the following steps:

1. In the Policy Studio tree, select the **Environment Configuration > Server Settings** node, and select **Logging > Transaction Event Log** in the window on the right.
2. Ensure **Writing to Transaction Event Log** is selected.
3. To enable monitoring of protocol and remote host merics, select the **Monitoring > Traffic Monitor** node, and ensure the following settings are selected.
    * Enable Traffic Monitor
    * Record inbound transactions
    * Record outbound transactions

## Deploy the updated configuration to the API Gateway

You must deploy these configuration changes to the API Gateway. Click the **Deploy** button in the toolbar, or press F6.

The API Gateway now sends transaction audit logging to the metrics database, and writes transaction data to the transaction event log. The final step is to configure the Node Manager to read the transaction event logs and write system and transaction metrics to the metrics database.

## Configure the Node Manager to process event logs and update the metrics database

 If you have not already done so, you must use the `managedomain`  tool to enable the Node Manager to process event logs from your API Gateway host, and to write metrics data to the metrics database.

All API Gateway instances running on the host node generate transaction event log files. These files are all written to the same folder, and are collectively processed and aggregated by the Node Manager on the host, and then written to the metrics database. The metrics database provides the data for the graphical charts in the monitoring views in API Gateway Analytics and API Manager.

{{< alert title="Note" color="primary" >}}The Node Manager on each host in the domain must be configured to write metrics data to the same database that API Gateway Analytics reads from. The API Gateway can write to the same database for transaction audit logging if required.{{< /alert >}}

### Use the managedomain interactive menu

You can enable metrics using the interactive `managedomain --menu` command. The following shows an example:

```
Select option:2
Select a host:
   1) LinuxMint01
   2) Enter host nameEnter selection from 1-2 [2]:1
Enter a new host name [LinuxMint01]:
Enter a new Node Manager name [Node Manager on LinuxMint01]:
Enter a new Node Manager port [8090]:
There is only one Node Manager in this domain so it must remain as an Admin Node Manager
Do you want to create an init.d script for this Node Manager [n]:
Do you want to reset the passphrase for the Node Manager on this host ? [n]:
Do you wish to edit metrics configuration (y or n) ? [n]:y
Do you wish to enable metrics (y or n) ? [y]:y
Enter metrics database URL [jdbc:mysql://127.0.0.1:3306/reports]:
Enter metrics database username [root]:
Enter metrics database plaintext password [*******]:
Testing Database connectivity for :jdbc:mysql://127.0.0.1:3306/reports, user :root
Metrics database connectivity succeeded
Metrics generation enabled. All other specified metrics settings updated.
Metrics settings updated successfully. Please reboot Node Manager on completion of this program.
Completed successfully.
Hit enter to continue...
```

### Use the managedomain command options

Alternatively, you can use `managedomain` command options to enable metrics when initializing a host, adding a host other than the Admin Node Manager, or editing a host.

The following example shows enabling metrics when initializing a host machine:

```
./managedomain --initialize --metrics_enabled y --metrics_dburl jdbc:mysql://127.0.0.1:3306/reports --metrics_dbuser root --metrics_dbpass MY_DB_PWD
```

The following example shows enabling metrics when adding a host machine other than the Admin Node Manager:

```
./managedomain --add --anm_host MY_HOSTNAME --nm_entitystore_passphrase MY_CONFIG_PWD --metrics_enabled y --metrics_dbuser root --metrics_dbpass MY_DB_PWD --metrics_dburl jdbc:mysql://1.2.3.4:3306/reports --nm_name MY_NODE_MNGR --port 8055
```

The following example shows enabling metrics when editing a host machine in the domain:

```
./managedomain --edit_host --nm_entitystore_passphrase bonjour --metrics_enabled y --metrics_dburl jdbc:mysql://127.0.0.1:3306/reports --metrics_dbuser root --metrics_dbpass MY_DB_PWD
```

The `managedomain` metrics options are described as follows:

| Option                  | Description                                                                                           |
|-------------------------|-------------------------------------------------------------------------------------------------------|
| `--nm_entitystore_pass` |  Specifies the encryption passphrase used to access the API Gateway instance configuration. If no passphrase has been set, omit this argument. |
| `--metrics_enabled`     | Specifies whether writing of metrics data is enabled. Enter `y`or `n`.  |
| `--metrics_dburl`       | Specifies the JDBC URL for the metrics database (for example, `jdbc:mysql://127.0.0.1:3306/reports`). |
| `--metrics_dbuser`      | Specifies the metrics database user (for example, `root` or metrics DB user name).                    |
| `--metrics_dbpass`      | Specifies the password for the metrics database user.                                                 |

{{< alert title="Note" color="primary" >}}When the `managedomain` command has finished, you must restart the Node Manager. {{< /alert >}}

For more details on `managedomain`, see the [API Gateway Administrator Guide](/docs/apim_administration/apigtw_admin/).

## Configure additional options for event log processing in the Node Manager

The parameters described in this section specify how transaction event logs are processed in the Node Manager. You can configure these optional settings by editing the Node Manager configuration using the `esexplorer` tool.

For example, perform the following steps:

1. Change to the following directory:

    ```
    INSTALL_DIR/apigateway/posix/bin
    ```

2. Enter the `esexplorer`command.
3. Select **Store > Connect**.
4. Browse to `INSTALL_DIR/apigateway/conf/fed/configs.xml`.
5. Select **System Components > Metrics Generation Configuration** in the tree on the left.
6. Configure the appropriate fields in the window on the right:
    * `sourceEventLogDir`: Specifies the folder in which the Node Manager looks for event log files. This should match the API Gateway transaction event log directory set in Policy Studio. Defaults to `${environment.VDISTDIR}/events`.
    * `retainProcessedEventLogs`: Specifies whether processed event logs should be deleted or retained in a separate directory. By default, event logs are deleted when their contents are written to the metrics database. Logs can be retained if they are needed for audit purposes or as input to a custom analytics process. Defaults to `false`.
    * `processedEventLogDir`: When `retainProcessedEventLogs`is `true`, specifies the directory to which event files are moved after being processed by the Node Manager. Defaults to `${environment.VDISTDIR}/events/processed`.
    * `dirSizeMb`: If `retainProcessedEventLogs`is `true`, specifies the maximum size of the `processedEventLogDir`. When the configured size is reached, the oldest log files in the directory are deleted. Defaults to 1024 MB.
    * `processCustomMessageAttributes`: Specifies whether message attributes contained in the transaction event log, are written to the database `transaction_data`table. Defaults to `true`.
    * `processCustomMetrics`: Specifies whether custom metrics generated by the API Gateway Java Metrics API and written to the transaction event log are written to the database. Defaults to `true`.

    {{< alert title="Note" color="primary" >}}When making changes using `esexplorer`
    , ensure that you open the latest configuration. For example, you could overwrite changes made using `managedomain`
    if an old version of the configuration was loaded into `esexplorer`
    and then updated.{{< /alert >}}

7. Stop and restart the Node Manager after editing its configuration using `esexplorer`.

## Further information

For details on how to view monitoring metrics in API Manager, see the [API Manager User Guide](/docs/apim_administration/apimgr_admin/).

For details on how to view monitoring metrics in API Gateway Analytics, see [Monitor traffic](/docs/apimanager_analytics/analytics_start/#monitor-traffic).
