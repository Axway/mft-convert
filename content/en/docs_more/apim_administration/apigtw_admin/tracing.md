{
"title": "Configure diagnostic trace",
  "linkTitle": "Configure diagnostic trace",
  "weight": "117",
  "date": "2019-10-14",
  "description": "Configure and view diagnostic trace and debugging information about API Gateway runtime execution."
}

By default, API Gateway produces diagnostic trace and debugging information to record details about its runtime execution. For example, this includes services starting or stopping, exceptions, and messages sent through the gateway. This information can then be used by administrators and developers for diagnostics and debugging purposes, and is useful when contacting [Axway Support](https://support.axway.com/).

You can view and search the contents of API Gateway tracing in the following locations:

* **Logs > Trace** view in API Gateway Manager
* A console window for the running server
* Trace files in the following locations:
    * Admin Node Manager: `INSTALL_DIR/trace`
    * API Gateway instance: `INSTALL_DIR/groups/<group-id>/<instance-id>/trace`
    * API Gateway Analytics: `INSTALL_DIR/trace`

You can view and search the contents of the gateway trace log, domain audit log, and transaction logs in the **Logs** view in API Gateway Manager.

This section explains how to configure the trace log only. For more details, see [Configure logging and events](/docs/apim_administration/apigtw_admin/logging).

For details on how to redact sensitive data from trace files (for example, user passwords or credit card details), see [Hide sensitive data in API Gateway Manager](/docs/apim_administration/apigtw_admin/admin_redactors/). The trace level you set impacts the redaction.

## View API Gateway trace files

Each time the gateway starts up, by default, it writes a trace file to the `trace` directory in your gateway installation (for example, `INSTALL_DIR/groups/group-2/server1/trace`).

The following example shows an extract from a default gateway trace file:

```
INFO    15/Jun/2012:09:54:01.047 [1b10] Realtime monitoring enabled
INFO    15/Jun/2012:09:54:01.060 [1b10] Storing metrics in database disabled
INFO    15/Jun/2012:09:54:03.229 [1b10] cert store configured
INFO    15/Jun/2012:09:54:03.248 [1b10] keypairs configured...
```

The trace file output takes the following format:

```
TraceLevel   Timestamp [thread-id] TraceMessage
```

For example, the first line in the above extract is described as follows:

`TraceLevel`: INFO

`Timestamp`: 15/Jun/2012:09:54:01.047 (day:hours:minutes:seconds:milliseconds)

`Thread-id`: [1b10]

`TraceMessage`: Realtime monitoring enabled

## Set API Gateway trace levels

The possible trace levels in order of least to most verbose output are as follows:

* `FATAL`
* `ERROR`
* `INFO`
* `DEBUG`
* `DATA`

`FATAL` is the least verbose and `DATA` the most verbose trace level. The default trace level is `INFO`.

### Set the trace level

You can set the trace level using the following different approaches:

**Startup trace**:

When Admin Node Manager is starting up, it gets its trace level from the `tracelevel` attribute of the `SystemSettings` element in `/system/conf/nodemanager`. You can set the trace level in this file if you need to diagnose boot up issues.

**Default Settings trace**:

When the gateway has started, it reads its trace level from the Default Settings for the gateway instance. To set this trace level in the Policy Studio, click the **Server Settings** node in the Policy Studio tree, then click the **General** option, then select a **Tracing level** from the drop-down list.

**Dynamic trace**:

You can also change dynamic gateway trace levels on-the-fly in API Gateway Manager. For more details, see [Configure logging and events](/docs/apim_administration/apigtw_admin/logging).

## Configure API Gateway trace files

By default, trace files are named `servername_timestamp.trc` (for example, `server1_20130118160212.trc`). You can configure the settings for trace file output in `INSTALL_DIR/system/conf/trace.xml`, which is included by `INSTALL_DIR/system/conf/nodemanager`. By default, `trace.xml` contains the following setting:

```
<FileRolloverTrace maxfiles="500" filename="%s_%Y%m%d%H%M%S.trc"/>
```

This setting means that API Gateway writes Node Manager trace output to `nodemanager;onhostname_timestamp .trc`
(for example, `nodemanager;on127.0.0.1_20130118160212.trc`) in the `trace`
directory of the API Gateway installation. And, the maximum number of files that the `trace`
directory can contain is `500`.

### Configure rollover settings

The `FileRolloverTrace` element can contain the following attributes:

`filename`

File name used for trace output. Defaults to the `tracecomponent` attribute read from the `SystemSettings` element.

`directory`

Directory where the trace file is written. Defaults to `INSTALL_DIR/trace` when not specified.

* If you change the trace directory, you will not be able to view the trace files in API Gateway Manager. For the recommended way to change the trace directory, see the following [Axway knowledge base article](https://support.axway.com/kb/179277).

`maxlen`

Maximum size of the trace file in bytes before it rolls over to a new file. Defaults to `16777216` (`16` MB).

`maxfiles`

Maximum number of files that the `trace` directory contains for this `filename`. Defaults to `500`.

`rollDaily`

Whether the trace file is rolled at the start of the day. Defaults to `true`.

The following setting shows example attributes:

```
<FileRolloverTrace maxfiles="5" maxlen="10485760" rollDaily="true
    directory="/mydir/log/trace" filename="myserver.trc"/>
```

This setting means that API Gateway writes trace output to `myserver.trc`
in the `/mydir/log/trace` directory, and rolls the trace files over at the start of each day. The maximum number of files that this directory can contain is `5`, and the maximum trace file size is 10 MB.

### Write output to syslog

On Linux, you can send API Gateway trace output to `syslog`. In your `INSTALL_DIR/system/conf/trace.xml` file, add a `SyslogTrace` element, and specify a `facility`. For example:

```
<SyslogTrace facility="local0"/>
```

## Run trace at DEBUG level

When troubleshooting, it can be useful to set to the trace level to `DEBUG` for more verbose output. When running a trace at `DEBUG` level, the gateway writes the status of every policy and filter that it processes into the trace file.

### Debug a filter

The trace output for a specific filter takes the following format:

```
Filter name {
     Trace for the filter is indented to identify output from the filter
} status, in x milliseconds
```

The status is `0`, `1`, or `2`, depending if the filter failed, succeeded, or aborted. For example, the result of an **WS-Security Username Token** filter running successfully is as follows:

```
DEBUG 12:43:59:093 [11a4] run filter [WS-Security Username Token] {
DEBUG 12:43:59:093 [11a4]   WsUsernameTokenFilter.invoke:Verify username and password
DEBUG 12:43:59:093 [11a4]   WsAuthN.getWSUsernameTokenDetails:
                            Get token from actor=current actor
DEBUG 12:43:59:093 [11a4]   Version handler -  creating a new ws block
DEBUG 12:43:59:108 [11a4]   Version handler - adding the ws element to the wsnodelist
DEBUG 12:43:59:108 [11a4]   Version handler -  number of ws blocks found:1
DEBUG 12:43:59:124 [11a4]   No timestamp passed in WS block, no need to check
                            timestamp
DEBUG 12:43:59:139 [11a4]   WsAuthN.getWSUsernameTokenDetails: Check <Created> element
                            in token. Value=2010-08-06T11:43:43Z
DEBUG 12:43:59:139 [11a4]   WS Nonce TimeStamp Max Size is 1000 and wsNonces cache 4
DEBUG 12:43:59:139 [11a4]   Add WS nonce for key [joe:2010-08-06T11:43:43Z].
                            New cache size [5].
DEBUG 12:43:59:155 [11a4]   WsBasicAuthN.getUsername:Getting username
DEBUG 12:43:59:171 [11a4]   WS-Security UsernameToken authN via CLEAR password
DEBUG 12:43:59:171 [11a4]   VordelRepository.checkCredentials:username=joe
DEBUG 12:43:59:186 [11a4] } = 1, in 62 milliseconds
```

### Debug a policy

The trace output for a policy shows it running with all its contained filters, and takes the following format:

```
policy Name {
     Filter 1 {
         Trace for the filter
     } status, in x milliseconds
```

```
     Filter 2 {
        Trace for the filter
    } status, in x milliseconds
}
```

For example, the following extract shows a policy called when running a simple service:

```
DEBUG ... run circuit "/axis/services/urn:cominfo"...
DEBUG ... run filter [Service Handler for 'ComInfoServiceService'] {
DEBUG ...   Set the service name to be ComInfoServiceService
DEBUG ...   Web Service context already set to ComInfoServiceService
DEBUG ...   close content stream
DEBUG ...   Calling the Operation Processor Chain [1. Request from Client]...
DEBUG ...   run filter [1. Request from Client] {
DEBUG ...      run filter [Before Operation-specific Policy] {
DEBUG ...         run circuit "WS-Security UsernameToken AuthN"...
DEBUG ...           run filter [WS-Security Username Token] {...
DEBUG ...           } = 1, in 62 milliseconds
DEBUG ...           ..."WS-Security UsernameToken AuthN" complete.
DEBUG ...       } = 1, in 74 milliseconds  ...
```

### Debug at startup

When running a startup trace with a `DEBUG` level set in the `SystemSettings`, API Gateway logs the configuration that it is loading. This can often help to debug any incorrectly configured items at start up, for example:

```
DEBUG 14:38:54:206 [1ee0] configure loadable module type RemoteHost, load order = 500000
DEBUG 14:38:54:206 [1ee0] RemoteHost {
DEBUG 14:38:54:206 [1ee0]     ESPK:1035
DEBUG 14:38:54:206 [1ee0]     ParentPK:113
DEBUG 14:38:54:206 [1ee0]     Key Fields:
DEBUG 14:38:54:206 [1ee0]         name:{csdwmp3308.wellsfargo.com}
DEBUG 14:38:54:206 [1ee0]         port:{7010}
DEBUG 14:38:54:221 [1ee0]     Fields:
DEBUG 14:38:54:221 [1ee0]         maxConnections:{128}
DEBUG 14:38:54:268 [1ee0]         turnMode:{off}
DEBUG 14:38:54:268 [1ee0]         inputBufSize:{8192}
DEBUG 14:38:54:268 [1ee0]         includeContentLengthRequest:{0}
DEBUG 14:38:54:268 [1ee0]         idletimeout:{15000}
DEBUG 14:38:54:268 [1ee0]         activetimeout:{30000}
DEBUG 14:38:54:268 [1ee0]         forceHTTP10:{0}
DEBUG 14:38:54:268 [1ee0]         turnProtocol:{http}
DEBUG 14:38:54:268 [1ee0]         includeContentLengthResponse:{0}
DEBUG 14:38:54:268 [1ee0]         addressCacheTime:{300000}
DEBUG 14:38:54:268 [1ee0]         outputBufSize:{8192}
DEBUG 14:38:54:268 [1ee0]         sessionCacheSize:{32}
DEBUG 14:38:54:268 [1ee0]         _version:{1}
DEBUG 14:38:54:268 [1ee0]         loadorder:{500000}
DEBUG 14:38:54:268 [1ee0]         class:{com.vordel.dwe.NativeModule}
DEBUG 14:38:54:268 [1ee0] }
```

For details on setting trace levels, and running a startup trace, see [Set API Gateway trace levels](/docs/apim_administration/apigtw_admin/tracing#set-api-gateway-trace-levels).

## Run trace at DATA level

When the trace level is set to `DATA`, the gateway writes the contents of the messages that it receives and sends to the trace file. This enables you to see what messages the gateway has received and sent (for example, to reassemble a received or sent message).

{{< alert title="Note" color="primary" >}}When the trace level is set to `DATA`, passwords provided during login are logged in plain text.{{< /alert >}}

### Search by thread ID

Every HTTP request handled by the gateway is processed in its own thread, and threads can be reused when an HTTP transaction is complete. You can see what has happened to a message in the gateway by following the trace by thread ID. Because multiple messages can be processed by the gateway at the same time, it is useful to eliminate threads that you are not interested in by searching for items that only match the thread you want.

You can do this using the search feature in the API Gateway Manager **Logs** view. Enter the thread you wish to search for in the **Only show lines with text** text box, and click **Refresh**. Alternatively, you can do this on the command line using vi by specifying the thread ID as a pattern to search for in the trace file. In this example, the thread ID is `145c`:

```
:g!/145c/d
```

The following example shows the `DATA` trace when a message is sent by API Gateway (message starts with `snd`):

```
DATA 17:45:35:718 [145c]  snd 1495:
    <POST /axis/services/urn:cominfo HTTP/1.1Connection:closeContent-Length:
    1295User-Agent:VordelSOAPAction:""Via:1.0 devsupport2 (Vordel)
    Host:devsupport2:7070Content-Type:text/xml<?xml version="1.0"
    encoding="UTF-8" standalone="no"?>
    <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <soap:Header>
       <wsse:Security xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/
          oasis-200401-wss-wssecurity-secext-1.0.xsd">
          <wsse:UsernameToken xmlns:wsu="http://docs.oasis-open.org/wss/
            2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd"
              wsu:Id="Id-00000128d05aca81-00000000009d04dc-10">
              <wsse:Username>joeuser</wsse:Username>
              <wsse:Nonce EncodingType="utf-8">
                  gmP9GCjoe+YIuK1einlENA==
               </wsse:Nonce>
              <wsse:Password Type="http://docs.oasis-open.org/wss/2004/01/
                 oasis-200401-wss-username-token-profile-1.0#PasswordText">
                  joepwd
              </wsse:Password>
              <wsu:Created>2010-05-25T16:45:30Z</wsu:Created>
          </wsse:UsernameToken>
       </wsse:Security>
      </soap:Header>
      <soap:Body>
         <ns1:getInfo xmlns:ns1="http://stock.samples">
              <symbol xsi:type="xsd:string">CSCO</symbol>
              <info xsi:type="xsd:string">address</info>
          </ns1:getInfo>
      </soap:Body>
    </soap:Envelope>
```

The following example shows the `DATA` trace when a message is received by API Gateway (message starts with `rcv`):

```
DATA 17:45:35:734 [145c]  rcv 557:<HTTP/1.0 200 OKSet-Cookie:8Set-Cookie2:
    8Content-Type:text/xml; charset=utf-8<?xml version="1.0" encoding="UTF-8"?>
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
       xmlns:xsd="http://www.w3.org/2001/XMLSchema"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
       <soapenv:Body>
         <ns1:getInfoResponse soapenv:encodingStyle="http://schemas.xmlsoap.org/
             soap/encoding/" xmlns:ns1="http://stock.samples">
             <getInfoReturn xsi:type="xsd:string">
               San Jose, CA
             </getInfoReturn>
         </ns1:getInfoResponse>
       </soapenv:Body>
    </soapenv:Envelope>
```

If you want to see what has been received by API Gateway on this thread, run the following command:

```
:g!/145c] rcv/d
```

All `snd` and `rcv` trace statements start and end with < and > respectively. If you are assembling a message by hand from the `DATA` trace, remember to remove characters. In addition, the sending and/or receiving of a single message may span multiple trace statements.

## Integrate trace output with Apache log4J

Apache log4j is included on API Gateway classpath. This is because some third-party products that API Gateway interoperates with require log4j. The configuration for log4j is found in the gateway `INSTALL_DIR/system/conf` directory in the `log4j2.yaml` file.

For example, to specify that the log4j appender sends output to the gateway trace file, add the following setting to your `log4j2.yaml` file:

```
Root:
  AppenderRef:
    - ref: STDOUT
    - ref: VordelTrace
  level: debug
```

## Environment variables

These variables override the `trace.xml` file settings, which enables the logging behavior to be defined at runtime.

```
APIGW_LOG_TRACE_TO_FILE=[true | false]
```

* true = Write trace files to disk
* false = Do not write trace files to disk

```
APIGW_LOG_TRACE_JSON_TO_STDOUT=[true | false]
```

* true = Output JSON formatted trace to `stdout`
* false = Do not output JSON formatted trace to `stdout`
