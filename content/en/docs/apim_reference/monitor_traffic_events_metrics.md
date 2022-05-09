{
"title": "Traffic monitoring and metrics settings in Policy Studio",
"linkTitle": "Traffic monitoring and metrics settings",
"weight":"100",
"date": "2019-10-14",
"description": "Configure settings for open traffic event logs, traffic monitoring, and real-time monitoring metrics in Policy Studio."
}

## Open traffic event log settings

The **Open Traffic Event Log** settings enable you to configure the open traffic event logs written by the API Gateway instances. For example, you can enable open traffic event logging, configure where the logs are stored, and whether or not transaction payloads are stored.

For more information on open logging, see [Configure open logging](/docs/apim_administration/apigtw_admin/admin_open_logging).

### Configure the open traffic event log

To configure the open traffic event log in the Policy Studio tree, select the **Server Settings** node, and click **Logging** > **Open Traffic Event Log**.

Configure the following fields:

**Enable Open Traffic Event Log**:

Enables writing to an open event log. This setting is disabled by default.

**Event Log Output**:

Choose one of the following options:

* `Use filesystem` – Select this option to write the log data to a file. If you select this option you must enter a directory in the **Event logs directory** field. This can be a location on the local drive, NFSv4, or SAN file system. This is the default.
* `Use console / traces` – Select this option to stream the traffic logs to `stdout` (console/trace files).

**Event logs directory**:

Specifies the directory where open traffic event logs are written. Defaults to `${environment.VDISTDIR}/logs`.

**Maximum disk space for logs (MB)**:

Specifies the maximum amount of disk space used for open traffic event logs. When the directory reaches the specified limit, the oldest log files are deleted. Defaults to `1024`
MB.

**Check disk space interval (secs)**:

Specifies how often the amount of available disk space used for event logs is checked. Defaults to `600` seconds. Enter `0` to disable disk space checks.

**Payload Storage**:

Choose one of the following options:

* `Do not store payloads` – Select this option to prevent received and sent payloads being stored. This is the default.
* `Use filesystem` – Select this option to store received and sent payloads on the file system. If you select this option you must enter a directory in the **Filesystem directory** field. This can be a location on the local drive, NFSv4, or SAN file system.

To confirm updates to these settings, click **Save** at the bottom right of the window. Click **Deploy** in the toolbar to deploy the updated configuration to the API Gateway.

## Traffic monitoring settings

The **Traffic Monitor** settings enable you to configure the traffic monitoring available in the web-based API Gateway Manager tool. For example, you can configure where the data is stored and what message transaction details are recorded in the message traffic log.

To access the **Traffic Monitor** settings, click the **Server Settings** node in the Policy Studio tree, and click **Monitoring** > **Traffic Monitor**. To confirm updates to these settings, click **Save** at the bottom right of the screen.

To access traffic monitoring in API Gateway Manager, go to `http://localhost:8090`, and click the **Traffic** button in the toolbar.

### Configuration

You can configure the following **Traffic Monitor** settings:

**Enable Traffic Monitor**:

Select whether to enable the web-based traffic monitoring in in API Gateway Manager. This is enabled by default.

**Transaction Store Location Settings**:

Enter the **Transaction Directory** that stores the traffic monitoring data files and database. You must enter either an absolute path, or a path relative to the API Gateway instance directory, which is the location where the server instance runs. For example:

```
INSTALL_DIR/apigateway/groups/GROUP/INSTANCE
```

The **Transaction Directory** defaults to `conf/opsdb.d`. If this directory or the database does not exist when the API Gateway starts up, they are recreated automatically.

**Persistence Settings**:

You can configure the following data persistence settings:

**Record inbound transactions**:

Select whether to record inbound message transactions received by the API Gateway. This is enabled by default.

**Record outbound transactions**:

Select whether to record outbound message transactions sent from the API Gateway to remote hosts. This is enabled by default.

**Record policy path**:

Select whether to record the policy path for the message transaction, which shows the filters that the message passes through. This is enabled by default.

**Record trace**:

Select whether to record the trace output for the message transaction. This is enabled by default.

**Record sent data for transactions**:

Select whether to record the sent payload data for the message transaction. This is enabled by default.

**Record received data for transactions**:

Select whether to record the received payload data for the message transaction. This is enabled by default.

These settings are global for all traffic passing through the API Gateway. You can override these persistence settings at the port level when configuring an HTTP or HTTPS interface.

Details of inbound and outbound transactions are also written to the transaction event log. If recording of inbound or outbound transactions is disabled in **Traffic Monitor** settings, transaction data will not be written to the event log. For more details, see [Transaction event log settings](/docs/apim_reference/log_global_settings/#transaction-event-log-settings).

**Transaction File Management Settings**:

You can use the following settings to configure transaction file management.

**Maximum transaction file size**:

Enter the maximum size of the transaction file, and select the units from the list. The default value is 8 MiB. When this limit is reached, a new file is created.

When using environment variables, for example, `${env.maxTransactionFile}`, this value must be expressed as bytes. For example, an 8MiB file size would need an env.maxTransactionFile = 8388608.

**Create new transaction file every day**:

Select the check box to force creation of a new transaction file every day at midnight, even if the maximum file size was not exceeded.

**Maximum number of transaction files**:

Enter the maximum number of transaction files to store on disk. The default value is 128. When this limit is reached, old files that have no open transactions are purged. The number of transaction files might be exceeded if the oldest file still has open transactions.

**Number of days after which transaction files will be purged**:

Enter the maximum number of days after which transaction files are purged. Files older than the specified number of days (based on their modification date) are purged. You can use a value of 0 to disable purging. The default value is 0.

## Real-time monitoring metrics

You can configure real-time monitoring metrics for a gateway instance. For example, this enables you to specify monitoring of messages at the level of API services, methods,clients, and remote hosts. This is important when managing APIs because of requirements to bill clients for their API usage.

When real-time monitoring is enabled, monitoring data is stored in API Gateway memory and displayed in the API Gateway Manager web console. API Gateway Manager uses the configured real-time monitoring metrics to display graphical reports in its **Dashboard** and **Monitoring** views. For more details on viewing real-time metrics, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service).

To configure real-time monitoring settings in the Policy Studio tree, select the **Server Settings** node, and click **Monitoring** > **Real Time Monitoring**.

### Enable monitoring

Configure the following general settings:

**Enable real-time monitoring**:

This enables real-time monitoring globally for the API Gateway instance in the API Gateway Manager web console. This setting must be enabled to display monitoring data in the **Dashboard**
and **Monitoring** views in API Gateway Manager, and is selected by default. To disable real-time monitoring, deselect this setting.

**System metrics update frequency (secs)**:

Specifies how often in seconds that system metrics are measured (for example, CPU, disk space, and memory usage). Defaults to `3` seconds.

### Configure real-time metrics

Configure the following settings in the **Real Time Monitoring Limits** section:

Real-time monitoring may have a negative impact on API Gateway performance. To optimize performance, disable monitoring for one or more metrics.

You should set the maximum services, methods, and remote hosts to values that will never be reached in normal operation. These settings protect the API Gateway by setting an upper limit on the amount of memory consumed by real-time monitoring.

**Service**:

Enables real-time monitoring of metrics data on the **API Services** tab. This is enabled by default.

**Maximum Services**:

Specifies the maximum number of API services that are monitored by the API Gateway. When the maximum is reached, the API Gateway stops collecting metrics for new services. Defaults to `10000`.

**Method**:

Enables real-time monitoring of metrics data on the **API Methods** tab. This is enabled by default.

To enable method monitoring, you must ensure that service monitoring is also enabled. Disabling service monitoring also disables method monitoring.

**Maximum Methods**:

Specifies the maximum number of API methods that are monitored by the API Gateway. When the maximum is reached, the API Gateway stops collecting metrics for new methods. Defaults to `100000`.

**Remote Host**:

Enables real-time monitoring of metrics data on the **Remote Hosts** tab. This is enabled by default.

**Maximum Remote Hosts**:

Specifies the maximum number of remote hosts that are monitored by the API Gateway. When the maximum is reached, the API Gateway stops collecting metrics for new remote hosts. Defaults to `10000`.

**Client**:

Enables real-time monitoring of metrics data on the **Clients** tab. This is enabled by default.

**Maximum Clients**:

Specifies the maximum number of clients that are monitored by the API Gateway. When the maximum is reached, the API Gateway stops collecting metrics for new clients. Defaults to `10000`.

The number of unique clients that communicate with an API Gateway is potentially unbounded. The maximum number of clients is therefore a soft limit. When this is reached, monitoring stops for the oldest client and begins for the newest client instead.

For the other maximum values (services, methods, and remote hosts), exceptions are thrown and logged when the limits are reached.

To confirm updates to these settings, click **Save** at the bottom right of the window. Click **Deploy** in the toolbar to deploy the updated configuration to the API Gateway.

You must restart the gateway instance after changing any of the maximum values (for example, **Maximum Services**, **Maximum Methods**, **Maximum Clients**, or **Maximum Remote Hosts**).
