{
"title": "Performance tuning",
"linkTitle": "Performance tuning",
"weight":"118",
"date": "2019-10-14",
"description": "Optimize API Gateway performance using various configuration options."
}

General performance tuning options include tracing, monitoring, and logging. More advanced performance tuning options include database pooling, HTTP keep alive, chunked encoding, and client threads.

## General performance tuning

You can optimize your API Gateway performance by using Policy Studio to configure the general settings described in this section.

### Minimize tracing

The **Trace Log** is displayed in the **Logs** view in the API Gateway Manager web console. When tracing is running at a verbose level (for example, `DEBUG`), this means that the gateway is doing more work and is very dependent on disk input/output. You can set a less verbose trace level for a gateway instance or port interface (for example, `ERROR` or `FATAL`).

To set the tracing for the gateway instance, select **Environment Configuration > Server Settings > General** in the Policy Studio tree, and select the **Trace Level** (for example, `FATAL`)

![Minimize per-product instance tracing](/Images/APIGateway/admin_perf_tracing_instance.png)

You can also override the trace level for a gateway port interface, and set it to a quieter level. For example, in the Policy Studio tree, select **Environment Configuration > Listeners > API Gateway > Sample Services > Ports**. Right-click an interface in the list on the right, select **Edit**, and set the **Trace level** (for example, `FATAL`):

![Minimize per-product interface tracing](/Images/APIGateway/admin_perf_tracing_interface.png)

For more details, see [Configure API Gateway diagnostic trace](/docs/apim_administration/apigtw_admin/tracing).

### Disable real-time monitoring

Real-time monitoring is displayed in the **Monitoring**
view in the API Gateway Manager web console. This caches recent message transactions in the memory of API Gateway. You can remove this overhead by disabling real-time monitoring.

To disable in Policy Studio, select **Environment Configuration > Server Settings
> Monitoring > Real Time Monitoring**, and deselect **Enable Real Time Monitoring**:

![Disable real-time monitoring](/Images/APIGateway/admin_perf_realtime_monitor.png)

For more details, see [Real-time monitoring metrics](/docs/apim_reference/monitor_traffic_events_metrics/#real-time-monitoring-metrics).

### Disable traffic monitoring

Traffic monitoring is displayed in the **Traffic** view in the API Gateway Manager web console. By default, the API Gateway stores recent HTTP traffic summaries to the API Gateway disk for use in API Gateway Manager. You can remove this overhead by disabling traffic monitoring.

To disable in Policy Studio, select **Environment Configuration > Server Settings > Monitoring > Traffic**, and deselect **Enable Traffic Monitor**:

![Disable traffic monitoring](/Images/APIGateway/admin_perf_traffic_monitor.png)

For more details, see [Traffic monitoring settings](/docs/apim_reference/monitor_traffic_events_metrics#traffic-monitoring-settings).

### Disable transaction logging

The **Transaction Log** is displayed in the **Logs** view in the API Gateway Manager web console. You should ensure that API Gateway is not sending transaction log messages or events to transaction log destinations. This is because the performance of API Gateway will be determined by the log destination.

To disable transaction logging in the API Gateway, you must disable all log destinations in Policy Studio. For example, select **Environment Configuration > Server Settings > Logging > Transaction Log**, and deselect **Enable logging to a file**. The following example shows disabling logging to file, you must perform this step in all tabs on this screen:

![Disable transaction logging](/Images/APIGateway/admin_perf_transaction_log.png)

For more details, see [Transaction audit log settings](/docs/apim_reference/log_global_settings#transaction-audit-log-settings).

### Disable access logging

You should also ensure that the API Gateway is not sending log messages to the access log. To disable access logging in the API Gateway, select **Environment Configuration > Server Settings > Logging > Transaction Access Log**, and deselect **Transaction Access Log Enabled**:

![Disable access logging](/Images/APIGateway/admin_perf_access_log.png)

For more details, see [Transaction access log settings](/docs/apim_reference/log_global_settings/#transaction-access-log-settings).

## Advanced performance tuning

You can also use the advanced configuration settings described in this section to optimize API Gateway performance.

### Configure spilling of data to disk

When stress testing with large messages (greater than 4 MB), the API Gateway spills data to disk instead of holding it in memory. By default, the `spilltodisk` option is triggered with payload sizes of 4 MB or more. For example, you can configure this in the `service.xml` file in the following directory by adding the `spilltodisk` option configured in bytes:

```
<install-dir>/apigateway/groups/<group-id>/<instance-id>/conf/service.xml
```

For example:

```
<NetService provider="NetService">
    <SystemSettings
            tracelevel="&server.tracelevel;"
            tracecomponent="&server.title;"
            title="&server.title;"
            homedir="$VINSTDIR"
            secret="&server.entitystore.secret;"
            servicename="&server.servicename;
            "spilltodisk="10485760"
             ...
```

This setting specifies the limit of what is considered a reasonably-sized incoming request message to hold in memory. After this limit is exceeded, to preserve memory, the system writes the content of the incoming request to disk when it arrives.

For example, if API Gateway receives a 200 MB message from an HTTP/1.1 server to forward to an HTTP/1.0 client, it may need to calculate the content length of that message before transmitting the first byte to the client. API Gateway can do this by holding it in memory, or storing it to disk. After a certain point, the benefits of holding it in memory are outweighed by the cost of the memory footprint, and API Gateway stores it on disk.

When the message is entirely received, API Gateway knows the content length, and can generate it before reading the data from the disk block-by-block, and forwarding to the client. The entire 200 MB does not need to reside in API Gateway at the same time.

{{< alert title="Note" color="primary" >}}The `spilltodisk` option is purely used to reduce pressure on virtual address space, and does not impact on monitoring or metrics. This option applies to HTTP and SMTP only.{{< /alert >}}

### Configure database pooling

The API Gateway uses Apache Commons Database Connection Pools (DBCP) for pooling database connections. For details, see <http://commons.apache.org/dbcp/>.

{{< alert title="Note" color="primary" >}}Axway recommends that if your policy interacts heavily with the database, the pool size for the database connection should be at least as big as the expected client population. This assumes the database can cope with this number of parallel connections.{{< /alert >}}

For example, if you are providing load from 100 parallel clients, the pool settings shown in the following example are recommended.

To configure database pooling in Policy Studio, select **Environment Configuration > External Connections > Database Connections > Add Database Connection**. For example:

![Configure database pooling](/Images/APIGateway/admin_perf_database_pool.png)

### Configure API Gateway for HTTP persistent connections

By default, API Gateway uses HTTP 1.0 for better interoperability. To enable HTTP persistent connections (`HTTP Keep-alive`), you must configure API Gateway to use HTTP 1.1.

In HTTP/1.1, the connection between a client and a server is maintained unless otherwise declared, so that further client requests can avoid the overhead of setting up a new connection. This may or may not model the client population of a particular scenario very well. If it is acceptable to reuse TCP connections (and SSL connections on top of these), ensure your client uses HTTP/1.1, and does not opt out of the HTTP persistent connection.

For the `sr` command, this means you should use the `-V1.1` and `-U1000` arguments to enable the connection be used a number of times before closing it.

For conformance with the HTTP/1.1 specification, the client must send a `Host` header in this configuration, so you must pass a further `-aHost:localhost` argument to `sr`. If the persistent connection is working correctly, `sr` reports a larger number of transactions to connections in its periodic output.

#### Configure HTTP 1.1 for outgoing connections

You can configure a remote host for a destination server supporting HTTP 1.1.

In the Policy Studio node tree, click **Environment Configuration > Listeners > API Gateway**. Select the remote host you want to configure, and click **Edit**, or add a new host, and select **Allow HTTP 1.1**.

The remote host only enforces settings to a specific configured server, not globally. You must edit the settings separately for each destination server you want to configure for HTTP 1.1.

If you have [set HTTP 1.1 to be used globally](/docs/apim_administration/apigtw_admin/admin_performance/#configure-http-11-globally), you can also use a remote host to turn it *off* for a specific server by deselecting **Allow HTTP 1.1**.

#### Configure HTTP 1.1 globally

You can enable HTTP 1.1 globally in the `service.xml` configuration file, which will then apply to all incoming connections and to outgoing connections to hosts for which there is no remote host. The HTTP 1.1 setting in a remote host will override this global setting.

Enable HTTP 1.1 globally by editing the following file:

```
INSTALL_DIR/apigateway/groups/<group>/<instance>/conf/service.xml
```

To change the default behavior of the HTTP 1.1 settings, set `allowHTTP11` to `true`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<NetService provider="NetService">
<!-- Configuration file for service. Note that if you wish for the user to enter a passphrase at     
start up then give the "secret" attribute a value "(prompt)", for example: secret="(prompt)" -->
<include file="serviceids.xml"/>
<include file="../../conf/group.xml"/>
<SystemSettings tracelevel="INFO" secret="${secret}"
        serviceID="${serviceID}" groupID="${groupID}"
        serviceName="${serviceName}"
        groupName="${groupName}"
        domainID="${domainID}" title="API Server"
        allowHTTP11="true"/>
<set property="headless" value="true"/>
<include file="$VDISTDIR/system/conf/platform.xml"/>
<include file="$VDISTDIR/system/conf/trace.xml"/>
<include file="$VDISTDIR/system/conf/libxml.xml"/>
<include file="$VDISTDIR/system/conf/jvm.xml"/>
<include file="$VDISTDIR/system/conf/nativeJAXP.xml"/>
<include file="esconnection.xml"/>
<include file="mgmt.xml"/>
<includes dir="extensions" pattern="*.xml"/>
</NetService>
```

Changing this setting enables HTTP 1.1 globally for this particular gateway instance. You must restart the instance before the setting takes effect.

### Configure chunked encoding

For interoperability reasons, the gateway normally does not use chunked encoding when talking to a remote server. Because of the HTTP protocol, API Gateway must send the `Content-Length`
header to the server, and so must precompute the exact size of the content to be transmitted. This may be expensive.

For example, when relaying data directly from client to server, or when the message exists as an abstract XML document in API Gateway, the size may not be immediately available. This means that the entire content from the client must be buffered, or the internal body structure must be serialized an extra time just to measure its size. By configuring a remote host for the destination server to allow HTTP 1.1, when a server is known to be advertising HTTP 1.1, chunked encoding can be used when transmitting to that server where appropriate.

For example, in the Policy Studio tree, select **Environment Configuration > Listeners > API Gateway**. Select the remote host Right-click the remote host, select **Edit**, and select **Allow HTTP 1.1**:

![Configure allow HTTP 1.1](/Images/APIGateway/admin_perf_allow_http11.png)

### Number of threads for concurrent connections

You may want to consider reducing the number of threads that API Gateway uses for processing incoming messages. Reducing the thread count may result in less resources being consumed. API Gateway handles less load because of the decreased thread count, depending on volume and number of concurrent requests.

For example, you can reduce the number of threads by adding a `maxThreads="64"` attribute in the `SystemSettings` element in the `service.xml` file for the gateway instance (for example, in `groups/group-2/conf/instance-1`). This setting also helps in configuring the gateway to back off from the target service (if the gateway can process more load than the target service).

By default, the maximum thread count for the gateway instance on Linux is 1024.

{{< alert title="Note" color="primary" >}}You must restart API Gateway for these settings to take effect.{{< /alert >}}

### Number of client threads on Linux

If there are more than 200 client threads connecting, you must change the following setting in the `venv` script in the `posix/lib` directory of your API Gateway installation:

```
limit=unlimited
```

For example, change this setting to the following:

```
limit=131072
```

This prevents the too many open files error that you might see in a 200 client thread test.

### Multiple connection filters

This applies if the performance test involves more than one connection filter, where the filter is routing to the same host and port with the same SSL credentials. You should create a policy containing the single connection filter, and delegate to this policy from where you normally do the routing, so you delegate to a single filter instead of a connection filter per policy.

Each connection processor caches connections independently. This is because two connection processors using different SSL certificates cannot pool their connections. They are not interchangeable from an authentication point of view. Therefore, when using multiple connection filters, there is potential to soak the target machine with too many connections.

This applies to both API Gateway **Connection** and **Connect to URL** filters in Policy Studio.
