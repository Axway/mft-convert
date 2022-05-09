{
"title": "Transaction log settings in Policy Studio",
  "linkTitle": "Transaction log settings",
  "weight": "80",
  "date": "2019-10-14",
  "description": "Configure settings for API Gateway transaction audit, transaction access, and transaction event logging in Policy Studio."
}

## Transaction audit log settings

One of the most important features of a server-based product is its ability to maintain highly detailed and configurable logging. It is crucial that a record of each and every transaction is kept, and that these records can easily be queried by an administrator to carry out detailed transaction analysis. In recognition of this requirement, the API Gateway provides detailed logging to a number of possible locations.

You can configure the API Gateway so that it logs information about all requests. Such information includes the request itself, the time of the request, where the request was routed to, and the response that was returned to the client. The logging information can be written to the console, log file, local or remote syslog, or a database, depending on what is configured in the logging settings.

The API Gateway can also digitally sign the logging information it sends to the log files and the database. This means that the logging information can not be altered after it has been signed, thus enabling an irreversible audit trail to be created.

{{< alert title="Caution" color="warning" >}}The transaction audit log includes the complete contents of HTTP requests, including HTTP headers, body, and attachments. This may include sensitive information. You must ensure that appropriate safeguards are in place to protect this information in the different audit log locations.{{< /alert >}}

### Configure log output

To edit the default API Gateway logging settings in the Policy Studio tree, select the **Server Settings** node, and click **Logging > Transaction Audit Log**.

You can configure the API Gateway to log to the following locations described in this topic.

### Log to Text File

To configure the API Gateway to log in text format to a file, click the **Text File** tab, and select **Enable logging to file**. You can configure the following fields:

**File Name**: Enter the name of the text-based file that the API Gateway logs to. The default is `transactionLog`.

**File Extension**: Enter the file extension of the log file. Defaults to `.log`.

**Directory**: Enter the directory of the log file in this field. By default, all log files are stored in the `/logs/transaction` directory of your API Gateway installation.

**File Size (MB)**: Enter the maximum size that the log file grows to. When the file reaches the specified limit, a new log file is created. By default, the maximum file size is 1000 MB.

**Roll Log Daily**: Specify whether to roll over the log file at the start of each day. This is enabled by default.

**Number of Files**: Specify the number of log files that are stored. The default number is 20.

**Format**: You can specify the format of the logging output using the values entered here. You can use selectors to output logging information that is specific to the request. The default logging format is as follows:

```
${level} ${timestamp} ${id} ${text} ${filterType} ${filterName}
```

The available logging properties are described as follows:

* `level`: The log level (`fatal` , `fail`, `success`).
* `timestamp`: The time that the message was processed in user-readable form. For more details, see **Date format** in [General settings](/docs/apim_reference/general_settings/#general-settings).
* `id`: The unique transaction ID assigned to the message.
* `text`: The text of the log message that was configured in the filter itself. In the case of the **Log Message Payload** filter, the `${payload}` selector contains the message that was sent by the client.
* `filterName`: The name of the filter that generated the log message.
* `filterType`: The type of the filter that logged the message.
* `ip`: The IP address of the client that sent the request.

**Signing Key**: To sign the log file, select a **Signing Key** from the Certificates Store that is used in the signing process. By signing the log files, you can verify their integrity at a later stage.

To confirm updates to these settings, click **Save** at the bottom right of the screen.

### Log to XML File

To configure the API Gateway to log to an XML file, click the **XML File** tab, and select **Enable logging to XML file**. The log entries are written as the values of XML elements in this file. You can view historical XML log files (not the current file) as HTML for convenience by opening the XML file in your default browser. The `/logs/xsl/MessageLog.xsl` stylesheet is used to render the XML log entries in a more user-friendly HTML format.

You can configure the following fields on the **XML File** tab:

**File Name**: Enter the name of the text-based file that the API Gateway logs to. By default, the log file is called `axway`.

**File Extension**: Enter the file extension of the log file in this field. By default, the log file is given the `.log` extension.

**Directory**: Enter the directory of the log file in this field. By default, all log files are stored in the `/logs/transaction` directory of your API Gateway installation.

**File Size**: Enter the maximum size that the log file grows to. When the file reaches the specified limit, a new log file is created.By default, the maximum file size is 1000 kilobytes.

**Roll Log Daily**: Specify whether to roll over the log file at the start of each day. This is enabled by default.

**Number of Files**: Specify the number of log files that are persisted. The default number is 20.

**Signing Key**: To sign the log file, select a **Signing Key** from the Certificates Store that will be used in the signing process. By signing the log files, you can verify their integrity at a later stage.

### Log to Database

Using this option, you can configure the API Gateway to log messages to an Oracle, SQL Server, or MySQL relational database.

Before configuring the API Gateway to log to a database, you must first create the database tables that the API Gateway writes to. For details on setting up tables for supported databases, see the [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/).

When you have set up the logging database tables, you can configure the API Gateway to log to the database. Click the **Database** tab, and select **Enable logging to database**. You can configure the following fields on the **Database** tab:

**Connection**: Select an existing database from the **Connection** drop-down list. To add a database connection, click the **External Connections** button on the left, right-click the **Database connections** tree node, and select **Add a Database Connection**.

**Signing Key**: You can sign log messages stored in the database to ensure that they are not tampered with. Click **Signing Key** to open the list of certificates in the Certificate Store, and select the key to use to sign log messages.

### Log to Local Syslog

To configure the API Gateway to send logging information to the local UNIX syslog, click the **Local Syslog** tab, and select the **Enable logging to local UNIX Syslog** checkbox. You can configure the following fields:

**Select Syslog server**: Select the local syslog facility that the API Gateway should log to. The default is `LOCAL0`.

**Format**: You can specify the format of the log message using the values (including selectors) entered in this field.

### Log to Remote Syslog

To configure the API Gateway to send logging information to a remote syslog, click the **Remote Syslog** tab, and select the **Enable logging to Remote Syslog** checkbox. You can configure the following fields:

**Syslog Server**: Select a previously configured **Syslog Server** from the list. For details on how to configure Syslog Server, see [Configure external connections](/docs/apim_policydev/apigw_external_connections/).

**Format**: You can specify the format of the log message using the values (including properties) entered in this field. For details on the properties that are available, see [Log to Text File](#log-to-text-file).

### Log to System Console

To configure the API Gateway to send logging information to the system console, click the **System Console** tab, and select the **Enable logging to system console**.

For details on how to use the **Format** field to configure the format of the log message, see [Log to Text File](#log-to-text-file).

## Transaction access log settings

The access log records a summary of the request and response messages that pass through the API Gateway. By default, the API Gateway records this in the `access.log`
file in the `log` directory. This file rolls over with a version number added for each new version of the file (for example, `access.log.0`, `access.log.1`, and so on).

The transaction access log file format is based on that used by Apache HTTP Server. This means that the log file can be consumed by third-party Web analytics tools such as Webtrends to generate charts and statistics.

### Access log format

The syntax used to specify the access log file is based on the syntax of available patterns used by the access log files in Apache HTTP Server. For example, the default pattern used by API Gateway is as follows:

```
%h %l %u %t "%r" %s %b
```

The following extract from the `access.log` file illustrates the log format resulting from the default access log patern:

```
s1.axway.com - lisa [09/05/2012:18:24:48 00] "POST / HTTP/1.0" 200 429
s2.axway.com - dave [09/05/2012:18:25:26 00] "POST / HTTP/1.0" 200 727
s3.axway.com - fred [09/05/2012:18:27:12 00] "POST / HTTP/1.0" 200 596
................
................
```

### Supported log format strings

API Gateway supports the following subset of the Apache HTTP Server log format strings:

| Log format string | Description                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| **`%a`**          | Remote IP address.                                                                                                |
| **`%A`**          | Local IP address.                                                                                                 |
| **`%b`**          | Bytes sent, excluding HTTP headers, in Common Log Format (for example, `-` instead of `0` if no bytes were sent). |
| **`%B`**          | Bytes sent, excluding HTTP headers.                                                                               |
| **`%D`**          | Time taken to process the request, in milliseconds.                                                               |
| **`%h`**          | Remote host name.                                                                                                 |
| **`%H`**          | Request protocol.                                                                                                 |
| **`%I`**          | Current request thread name (can compare later with stack traces).                                                |
| **`%l`**          | Remote logical user name (always `-`).                                                                            |
| **`%m`**          | Request method.                                                                                                   |
| **`%p`**          | Local port.                                                                                                       |
| **`%q`**          | Query string (prepended with `?` if it exists, otherwise an empty string).                                        |
| **`%r`**          | First line of the request that originated at the client.                                                          |
| **`%s`**          | HTTP status code returned to the client in the response.                                                          |
| **`%t`**          | Date and time of the request in Common Log Format.                                                                |
| **`%{format}t`**  | Date and time, in any format supported by `SimpleDateFormat`.                                                     |
| **`%T`**          | Time taken to process the request, in seconds.                                                                    |
| **`%u`**          | Remote user that was authenticated.                                                                               |
| **`%U`**          | Requested URL path.                                                                                               |
| **`%v`**          | Local server name.                                                                                                |
| **`%{xxx}i`**     | Incoming request header, where `xxx` is the header name.                                                          |
| **`%{xxx}o`**     | Outgoing request header, where `xxx` is the header name.                                                          |
| **`%{xxx}c`**     | Cookie value, where `xxx` is the cookie name.                                                                     |
| **`%{xxx}r`**     | API Gateway message attribute, where `xxx` is the attribute name.                                                 |

### Aliases for commonly used patterns

In addition, you can specify one of the following aliases for commonly used patterns:

* **`common`**: `%h %l %u %t "%r" %s %b`
* **`combined`**: `%h %l %u %t "%r" %s %b "%{Referer}i" "%{User-Agent}i"`

For more details on Apache HTTP Server access log formats, see <http://httpd.apache.org/docs/current/logs.html>.

### Configure the access log

To configure the access log in the Policy Studio tree, select the **Server Settings** node, and click **Logging** >**Transaction Access Log**. To confirm updates to these settings, click **Save** at the bottom right of the window.

You can configure the following fields to enable the server to write an access log to file:

**Writing to Transaction Access Log**: Select whether to configure the API Gateway instance to start writing event data to the transaction access log. This setting is disabled by default.

**File name**: Enter the name of the access log file. When the file rolls over (because the maximum file size has been reached, or because the date has changed), a suitable increment is appended to the file name. Defaults to `access`.

**File extension**: Enter the file extension for the log file. Defaults to `.log`.

**Directory**: Enter the directory for the access log file. Defaults to the `logs/access` directory of your product installation.

**File size (MB)**: Specify the maximum size that the log file is allowed reach before it rolls over to a new file. Defaults to `1000` MB.

**Roll log daily**: Select whether to roll over the log file at the start of each day. This is enabled by default.

**Number of log files**: Specify the number of log files that are stored. Defaults to `20`.

**Format**: Enter the access log file format. This is based on the syntax used in Apache HTTP Server access log files, for example:

```
%h %l %u %t "%r" %s %b
```

For more details, see [Supported log format strings](#supported-log-format-strings).

{{< alert title="Note" color="primary" >}}These settings configure the access log at the API Gateway level. You must also configure the access log at the service level on a specific relative path. {{< /alert >}}

For example, in the Policy Studio tree, select the relative path, right-click it in the **Resolvers** pane, and select **Edit**. Then click the **Logging Settings** tab, and select **Include in server access log records**.

### Redact sensitive details from the access log

The default syntax for the access log is as follows:

```
%h %l %u %t "%r" %s %b
```

The `%r` format string results in the entire HTTP request line being added to the access log file, including the query string. For example:

```
127.0.0.1 - - [02/07/2014:12:39:29 00] "POST /healthcheck?name=value HTTP/1.0" 200 19
```

The query string may contain sensitive information (for example, credit card number, or social security number). If you do not wish the query string to be included in the access log, it is recommended that you use the following format instead:

```
%h %l %u %t "%m %U% %H" %s %b
```

For example, this results in the following output instead:

```
127.0.0.1 - - [02/07/2014:12:39:29 00] "POST /healthcheck HTTP/1.0" 200 19
```

The `"%m %U %H"` options log the method, path, and HTTP version. This results in the same output as `%r`, but without the query string.

To confirm updates to these settings, click **Save** at the bottom right of the screen. Click **Deploy** in the toolbar to deploy the updated configuration to the API Gateway.

## Transaction event log settings

The **Transaction Event Log** provides a summary of each gateway message transaction, which is written to a log file, and used to generate metrics for historic traffic (for example, in API Gateway Analytics or the **Monitoring** view in API Manager). In a distributed system with multiple gateway instances running, the events data is written to separate transaction event log files for each API Gateway instance.

The event log file data is processed by the local Node Manager every 5 minutes, aggregated into the appropriate metrics data, and then written to a database. API Manager can use the data from the database to display metrics in the system. Event log file data is written in JSON format, which also enables it to be integrated with third-party logging tools such as Splunk.

{{< alert title="Note" color="primary" >}}
Node Manager processing of event log data is not enabled by default. You must enable the Node Manager to write metrics to the database. For more details, see [Configure API Gateway with a metrics database](/docs/apim_administration/apigtw_admin/monitor_service#configure-api-gateway-with-the-metrics-database).
{{< /alert >}}

### Transaction event log formats

Event log files are located in the `events` directory of your API Gateway installation by default. For example:

```
INSTALL-DIR/apigateway/events/group-2_instance-1.log
```

When each event log file has been processed (every 5 minutes), it can be moved to a `processed` directory. For example:

```
INSTALL-DIR/apigateway/events/processed
```

By default, files are deleted after being processed.

Entries in the transaction event log file are generated for different event types (for example, `header`, `system`, `transaction`, `alert`, and `custom`).

**Event log header entries**:

Event log `header` entries contain details about the creation of the log file. For example, this includes when the log file is created, and on which host, domain, group, instance, and so on.

The fields in the header entries include the following:

| Field           | Description                                                                                            |
| --------------- | ------------------------------------------------------------------------------------------------------ |
| type            | `header`. Entries of type header identify the API Gateway group and instance, one record per log file. |
| logCreationTime | Time the event log file was created.                                                                   |
| hostname        | Name of the host the API Gateway process is running on.                                                |
| domainId        | Topology ID of the API Gateway system.                                                                 |
| groupId         | API Gateway group ID.                                                                                  |
| groupName       | API Gateway group name.                                                                                |
| serviceId       | API Gateway instance ID.                                                                               |
| serviceName     | API Gateway instance name.                                                                             |
| version         | API Gateway version.                                                                                   |

The following example shows the JSON format used for `header` events:

```
{
   "type":"header",
   "logCreationTime":"2015-01-23 12:25:00.120",
   "hostname":"user1-PC",
   "domainId":"cfbe55d1-be45-4968-8b4b-f06a4db858b8",
   "groupId":"group-2",
   "groupName":"QuickStart Group",
   "serviceId":"instance-1",
   "serviceName":"QuickStart Server",
   "version":"7.7"
}
...
```

**Event log system entries**:

Event log `system` entries contain details about the API Gateway system. For example, this includes details such as the amounts of disk space, memory, and CPU.

The fields in the system entries include the following:

| Field       | Description                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------- |
| type        | `system`. Entries of type system contain CPU and memory information, written once every minute.    |
| time        | Timestamp of the event (in milliseconds since the epoch, taken from `System.currentTimeMillis()`). |
| diskUsed    | Percentage of disk used (disk on which the API Gateway instance is running: `$VINSTDIR`).          |
| instCpu     | Percentage of current process CPU usage (total usage divided by the number of cores).              |
| sysCpu      | Percentage of the global CPU usage on the system.                                                  |
| instMem     | Current process resident memory size (in KB).                                                      |
| sysMem      | System memory used (in KB).                                                                        |
| sysMemTotal | System total memory size (in KB).                                                                  |

The following example shows the JSON format used for `system` events:

```
{
   "type":"system",
   "time":1422015900120,
   "diskUsed":30,
   "instCpu":1,
   "sysCpu":5,
   "instMem":533436,
   "sysMem":4641996,
   "sysMemTotal":16759240
}
...
```

**Event log transaction entries**:

Event log `transaction` entries contain details about a specific message transaction. For example, this includes details such as the protocol, method, bytes sent and received, IP addresses, ports, service name, and so on.

The fields in the transaction entries include the following:

**type**: `transaction` - Entries of type transaction describe a transaction and the transaction legs. A transaction entry is recorded for each API call.

**time**: Timestamp of the event (in milliseconds since the epoch, taken from `System.currentTimeMillis()`).

**path**: Resource path representing the transaction.

**protocol**: Inbound protocol.

**protocolSrc**: Local inbound protocol port (or path).

**duration**: Execution time of the transaction (in ms).

**status**: Transaction result status.

**serviceContexts**: OAuth, web service, and service context elements. Each service context contains the following fields:

* **monitor**: Indicates if metrics monitoring is enabled for this service.
* **client**: Identity of the client.
* **org**: Authentication organization name.
* **app**: Authentication application name.
* **method**: Protocol method used.
* **status**: Execution status of this service context.
* **duration**: Processing time of this service context.

**customMsgAtts**: Custom message attributes that have been added to the event log (see [Configure the transaction event log](#configure-the-transaction-event-log) for details of how to configure these attributes globally).

**correlationId**: Transaction correlation ID.

**legs**: Legs processed during the transaction. Each leg contains the following fields:

* **status**: HTTP status code returned.
* **statustext**: HTTP status message returned.
* **method**: HTTP method used.
* **Vhost**: Virtualized API's host.
* **wafStatus**: Threat protection profile status.
* **bytesSent**: Number of bytes sent.
* **bytesReceived**: Number of bytes received.
* **remoteName**: Name representing remote host of the transaction.
* **remoteAddr**: Remote host address of transaction.
* **localAddr**: Local host of transaction.
* **remotePort**: Remote port of transaction.
* **localPort**: Local port of transaction.
* **Sslsubject**: Subject name of peer certificate used to establish SSL connection.
* **leg**: Transaction element number.
* **timestamp**: Event timestamp.
* **duration**: Time elapsed between request creation and response processing (in ms).
* **serviceName**: API Gateway instance name.
* **subject**: Authenticated user (content of attribute `authentication.subject.id`).
* **operation**: SOAP request method used.
* **type**: Protocol used.
* **finalStatus**: Status text of the transaction element (global policy) execution. The value is only written to the record with leg number `0` after all policies have been executed. Initialized to "null" for all records. Possible values: "Pass", "Fail", or "Error".

The following example shows the JSON format used for an HTTP `transaction` event with a service context and inbound and outbound transaction legs:

```
{
   "type":"transaction",
   "time":1425291330502,
   "path":"/stockquote.asmx",
   "protocol":"http",
   "protocolSrc":"8080",
   "duration":1842,
   "status":"success",
   "serviceContexts":[
   {
      "service":"StockQuote",
      "monitor":true,
      "client":null,
      "org":null,
      "app":null,
      "method":"GetQuote",
      "status":"success",
      "duration":1824}],
   "customMsgAtts":{},
   "correlationId":"4038f4540400788ebe4f84ca",
   "legs":[
     {
        "uri":"/stockquote.asmx",
        "status":200,
        "statustext":"OK",
        "method":"POST",
        "vhost":null,
        "wafStatus":0,
        "bytesSent":1278,
        "bytesReceived":612,
        "remoteName":"127.0.0.1",
        "remoteAddr":"127.0.0.1",
        "localAddr":"127.0.0.1",
        "remotePort":"49104",
        "localPort":"8080",
        "sslsubject":null,
        "leg":0,
        "timestamp":1425291328660,
        "duration":1843,
        "serviceName":"StockQuote",
        "subject":null,
        "operation":"GetQuote",
        "type":"http",
        "finalStatus":"Pass"
     },
     {
      "uri":"/stockquote.asmx",
      "status":200,
      "statustext":"OK",
      "method":"POST",
      "vhost":null,
      "wafStatus":0,
      "bytesSent":736,
      "bytesReceived":1202,
      "remoteName":"www.webservicex.net",
      "remoteAddr":"173.201.44.188",
      "localAddr":"10.142.10.142",
      "remotePort":"80",
      "localPort":"49438",
      "sslsubject":null,
      "leg":1,
      "timestamp":1425291329916,
      "duration":566,
      "serviceName":"StockQuote",
      "subject":null,
      "operation":"GetQuote",
      "type":"http",
      "finalStatus":null
     }
   ]
}
...
```

#### Inbound and outbound transaction legs

In this example, the `legs` data is based on traffic monitoring. `Leg 0` is always the inbound transaction, and its `duration` value is the overall transaction duration observed by API Gateway. Subsequent `legs` are outbound calls, so their `duration` value represents the back-end transactions observed by API Gateway.

The back-end duration for `leg 1` to `leg n`, typically from a [Connect To URL](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter) filter, only includes the time taken for API Gateway to receive the response headers, and not the entire body. For this reason, API Gateway cannot accurately measure the time from the back-end `leg` `durations`, because the reply bodies can be received in the background.

At this point, **Connect to URL** has an HTTP status and a filter return value so that it can move onto the next filter in the the circuit chain. The next filter, though, might need to wait until the entire body has been received before it can proceed.

The service context is an abstract concept, and the `duration` at this level measures time spent in that context only. The service context might be set in an arbitrary place in a policy, so this information is typically not as useful as the `leg` data—unless in composite services scenarios.

The top-level transaction `duration` is obtained separately, but should be similar to the `leg 0` value.

For more information about transactions and legs, see [Introduction to transactions and legs in API Gateway](/docs/apim_administration/apigtw_admin/admin_open_logging/#introduction-to-transactions-and-legs).

### Event log alert entries

Event log `alert` entries contain details about a specific system alert. For example, this includes details such as the protocol, method, bytes sent and received, IP addresses, ports, service name, and so on.

The fields in the alert entries include the following:

**type**: `alert` - Entries of type alert describe API Gateway alerts.

**time**: Time the alert event was created.

**alertType**: Type of the alert:

* AlertMessage
* SlaBreachAlertMessage
* SlaClearAlertMessage

**level**: Integer representing the level of the alert:

* 1 (ERROR)
* 2 (WARNING)
* 3 (INFO)

**id**: Unique ID of the alert record.

**srcId**: API Gateway instance ID.

**msgId**: ID of the message processed during the alert.

**defaultMsg**: Custom message configured in the alert.

**clientIP**: Client IP address in use when alert was triggered.

**policy**: Policy name.

**filter**: Alert filter name.

The following example shows the JSON format used for an HTTP `transaction` event with a service context and inbound and outbound transaction legs:

```
{
   "type": "alert",
   "time": 1431948768350,
   "alertType": "AlertMessage",
   "level": 1,
   "id": "6985e4fe:14d66cc3493:-8000",
   "srcId": "user1-LinuxDev1:instance-1",
   "msgId": "Id-e0cd59550500ad6f16e3ce38",
   "defaultMsg": "This is an alert",
   "clientIP": "127.0.0.1",
   "policy": "My Policy Name",
   "filter": "Alert Filter Name"
}
```

For details on `custom` event entries, see the [API Gateway Javadoc](https://support.axway.com/htmldoc/1444954). The `com.vordel.reporting.rtm.api.MetricGroup class` includes details on the Java API and the resulting metric event in the transaction event log.

### Configure the transaction event log

To configure the transaction event log in the Policy Studio tree, select the **Server Settings** node, and click **Logging** > **Transaction Event Log**.

Configure the following fields to enable the API Gateway instance to write a transaction event log to a file:

**Writing to Transaction Event Log**: Enables writing to an event log for all message transactions received by the API Gateway. This setting is enabled by default, and is required for API Gateway Analytics and API Manager metrics. For example, you could deselect this setting to optimize performance.

**Write transaction event logs to directory**: Specifies the directory where transaction event logs are written. Defaults to `${environment.VDISTDIR}/events`.

* If transaction event logs are being used to populate the metrics database, you must also update the `sourceEventLogDir` property in the Node Manager configuration if you change this directory.

**System event frequency (secs)**: Specifies how often in seconds that a system entry is written to each event log file. Defaults to `60` seconds. For more details, see [Event log system entries](#event-log-system-entries).

**Maximum disk space for event logs (MB)**: Specifies the maximum amount of disk space used for event logs. When the directory reaches the specified limit, the oldest log files are deleted. Defaults to `1024` MB.

**Check disk space interval (secs)**: Specifies how often the amount of available disk space used for event logs is checked. Defaults to `600` seconds.

**Select the message attributes to be stored in transaction events**: Enables you to specify custom message attributes to write to the transaction event logs (for example, the HTTP request URI). To specify an attribute, click **Add**, and enter the attribute name in the dialog.

To confirm updates to these settings, click **Save** at the bottom right of the window. Click **Deploy** in the toolbar to deploy the updated configuration to the API Gateway.
