{
"title": "Configure remote host settings",
  "linkTitle": "Configure remote host settings",
  "weight": 2,
  "date": "2019-10-17",
  "description": "Configure the way in which API Gateway connects to a specific external server or routing destination."
}
Use the **Remote Host Settings** to configure the way in which API Gateway connects to a specific external server or routing destination. For example, typical use cases for configuring remote hosts with API Gateway are as follows:

* Allowing API Gateway to send HTTP 1.1 requests to a destination server when that server supports HTTP 1.1.
* Resolving inconsistencies in the way the destination server supports HTTP.
* Mapping a host name to a specific IP address or addresses (for example, if a DNS server is unreliable or unavailable).
* Setting the timeout, session cache size, input/output buffer size, and other connection-specific settings for a destination server (for example, if the destination server is particularly slow, you can set a longer timeout).
* Stop accepting inbound connections on the HTTP interface when API Gateway loses connectivity to the remote host.
* Set timeouts for incoming connections, for example, to configure long polling.

You can add remote hosts *per-instance* to the API Gateway instance in the Policy Studio tree. For example, select **Environment Configuration** > **Listeners** > **API Gateway** >, right-click this instance, and select **Add Remote Host**. The tabs in the **Remote Host Settings** configuration window are described in the following sections.

You can use the **Remote Host Settings** to override incoming connection parameters (see [Configure an incoming remote host](#configure-an-incoming-remote-host)).

## General settings

You can configure the following settings on the **General** tab:

**Host alias** : The human readable alias name for the remote host (for example, `StockQuote Host`). This setting is required.

**Host name** : The host name or IP address of the remote host to connect to (for example `stockquote.com`). The host name is passed as the HTTP Host header value to the back-end even when using the Load Balancing feature. To pass the incoming host header from the client, see [Connect To URL filter](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter). If the host name entered in a **Static Router** filter matches this host name, the connection-specific settings configured on the **Remote Host** dialog are used when connecting to this host. This also includes any IP addresses listed on the **Addresses and Load Balancing** tab, which override the default network DNS server mappings, if configured.   This setting is required.

**Port** : The TCP port on the remote host to connect to. Defaults to `80`.

**Maximum connections** : The maximum number of connections to open to a remote host. If the maximum number of connections has already been established, the API Gateway instance waits for a connection to drop or become idle before making another request. The default value is `-1`, which allows unlimited connections. In the absence of a remote host, a global default value of `128` applies.

**Allow HTTP 1.1** : The API Gateway uses HTTP 1.0 by default to send requests to a remote host. This prevents any anomalies if the destination server does not fully support HTTP 1.1. If the API Gateway is routing on to a remote host that fully supports HTTP 1.1, you can use this setting to enable API Gateway to use HTTP 1.1.

**Include Content Length in Request** : When this option is selected, the API Gateway includes the `Content-Length` HTTP header in all requests to this remote host. This setting only applies to outgoing remote host connections.

**Include Content Length in Response** : When this option is selected, if the API Gateway sends a response to this remote host that contains a `Content-Length` HTTP header, it returns this length to the client. This setting only applies to incoming remote host connections.

**Send Server Name Indication TLS extension to server** : Adds a field to outbound TLS/SSL calls that shows the name that the client used to connect. This can be useful if the server handles several different domains, and needs to present different certificates depending on the name the client used to connect. For example, this is required by some cloud-based services such as Amazon CloudFront. This setting is not selected by default.

{{< alert title="Note" color="primary" >}}To send the SNI extension, you must ensure that the **Verify server's certificate matches requested hostname** setting is also selected. In addition, the **Port** setting must be the port that you are connecting to the server with (for example, `443` is the default port for SSL).{{< /alert >}}

**Verify server's certificate matches requested hostname** : Ensures that the certificate presented by the server matches the name of the remote host being connected to. This prevents host spoofing and man-in-the-middle attacks. This setting is selected by default.

## Address and load balancing settings

You can configure the following settings on the **Addresses and Load Balancing** tab:

**Addresses to use instead of DNS lookup**: You can add a list of IP addresses that the API Gateway uses instead of attempting a DNS lookup on the host name provided. This is useful in cases where a DNS server is not available or is unreliable. By default, connection attempts are made to the listed IP addresses on a round-robin basis.

For example, if a **Static Router** filter is configured to route to `www.webservice.com`, it first checks if any remote hosts have been configured with a **Host Name** entry matching `www.webservice.com`. If it finds a **Remote Host** with matching **Host Name** , it resolves the host name to the IP addresses listed here. In addition, it uses all the connection-specific settings configured on the **Remote Host** dialog when routing messages to these IP addresses. If it cannot find a matching host, the **Static Router** filter uses whatever DNS server has been configured for the network on which the API Gateway is running.

To add a list of IP addresses for a remote host, perform the following steps:

1. In the **Addresses to use instead of DNS lookup** box, select a priority group (for example, **Highest Priority**).
2. Click **Add**.
3. Enter an IP address or server name in the **Configure IP Address** dialog.
4. Click **OK**.
5. Repeat these steps to add more IP addresses as appropriate.

**Load balancing**: The **Load Balancing Algorithm** drop-down box enables you to specify whether load balancing is performed on a simple round-robin basis or weighted by response time. **Simple Round Robin** is the default algorithm. Connection attempts are made to the listed IP addresses on a round-robin basis in each priority group. The **Weighted by response time** algorithm compares the request/reply response times for the server address in each priority group. This is the simplest way of estimating the relative load of the address.

This algorithm works as follows:

1. The address with the least response time is selected to send the next message to.
2. If the address fails to send the message, it ignores that address for a period of time and selects another address in the same way.
3. If all addresses in a given group fail to accept a connection, addresses in the next group in ascending order of priority are used in the same way.
4. Only when all addresses in all priorities have failed to accept connections is delivery of the message abandoned, and an error raised.

The response times used by this algorithm decline over time. You can specify the rate of exponential decline by specifying a **Period to wait before response time is halved**. The default is 10,000 ms (10 sec). This enables addresses that were heavily loaded for a period of time to eventually resume accepting messages after the load subsides.

For example, server A takes 100 ms to reply, and the other servers in the same priority group reply in 25 ms. A **Period to wait before response time is halved** of 10,000 ms (10 sec) means that after 20 seconds server A is retried along with the other servers. In this case, the response time has been halved twice (100 ms / 2 / 2 = 25 ms).

## Advanced settings

The options available on the **Advanced** tab are used when creating sockets for connecting to the remote host. Default values are provided for all fields, which should only be modified under advice from the Axway Support.

You can configure the following configuration options on the **Advanced** tab:

**Connection Timeout** : If a connection to this remote host is not established within the time set in this field, the connection times out and the connection fails. Defaults to 30000 milliseconds (30 seconds).

**Active Timeout** : When the API Gateway receives a large HTTP request, it reads the request off the network when it becomes available. If the time between reading successive blocks of data exceeds the **Active Timeout**, the API Gateway closes the connection. This prevents a remote host from closing the connection while sending data. Defaults to 30000 milliseconds (30 seconds).

For example, the remote host's network connection is pulled out of the machine while sending data to the API Gateway. When the API Gateway has read all the available data off the network, it waits the **Active Timeout** period before closing the connection.

The **Active Timeout** value is also used as a wait time when the maximum number of connections for a remote host is reached. For example, when a remote host reaches the **Maximum connections** value, API Gateway waits the active timeout period before giving up on trying to make a new connection.

**Transaction Timeout (ms)** : A configurable transaction timeout that detects slow HTTP attacks (slow header write, slow body write, slow read) and rejects any transaction that keeps the worker threads occupied for an excessive amount of time.

Unlike other timeouts, this is not measured with respect to any network transaction, rather it is measured from the start of the API Gateway transaction until the timeout occurs. A rejected transaction will run to completion, though it will not return any response to the client, therefore, you must set this value longer than any legitimate traffic. The default value is 240,000 milliseconds (4 minutes).

**Max Received Bytes** : The maximum number of bytes received in a transaction. This is a configurable maximum length for the received data on transactions that API Gateway can handle. This setting limits the entire amount of data received over the link, regardless of whether it consists of body, headers, or request line. The default value is 20 MiB (20,971,520 bytes) and the maximum value is 16,384 PiB (18,446,744,073,709,551,615 bytes).

**Max Sent Bytes** : The maximum number of bytes sent in a transaction. This is a configurable maximum length for the transmitted data on transactions that API Gateway can handle. This setting limits the entire amount of data sent over the link, regardless of whether it consists of body, headers, or request line. The default value is 20 MiB (20,971,520 bytes) and the maximum value is 16,384 PiB (18,446,744,073,709,551,615 bytes).

**Idle Timeout** : The API Gateway supports HTTP 1.1 persistent connections. The **Idle Timeout** is the time that API Gateway waits after sending a message over a persistent connection to the remote host before it closes the connection. Defaults to 15000 milliseconds (15 seconds).

Typically, the remote host tells the API Gateway that it wants to use a persistent connection. The API Gateway acknowledges this, and keeps the connection open for a specified period of time after sending the message to the host. If the connection is not reused by within the **Idle Timeout** period, the API Gateway closes the connection.

**Input Buffer Size** : The maximum amount of memory allocated to each request. The default value is 8192 bytes.

**Output Buffer Size** : The maximum amount of memory allocated to each response. The default value is 8192 bytes.

**Cache addresses for (ms)** : The period of time to cache addressing information after it has been received from the naming service (for example, DNS). The default value is 300000 milliseconds.

**SSL Session Cache Size** : Specifies the size of the SSL session cache for connections to the remote host. This controls the number of idle SSL sessions that can be kept in memory. Defaults to `32`. If there are more than 32 simultaneous SSL sessions, this does not prevent another SSL connection from being established, but means that no more SSL sessions are cached. A cache size of `0` means no cache, and no outbound SSL connections are cached.

{{< alert title="Tip" color="primary" >}}You can use this setting to improve performance because it caches the slowest part of establishing the SSL connection. A new connection does not need to go through full authentication if it finds its target in the cache.{{< /alert >}}

At `DEBUG` level or higher, the API Gateway outputs trace when an entry goes into the cache, for example:

```
DEBUG   09:09:12:953 [0d50] cache SSL session 11AA3894 to support.acme.com:443
```

If the cache is full, the output is as follows:

```
DEBUG   09:09:12:953 [0d50] enough cached SSL sessions 11AA3894 to support.acme.com:443 already
```

**Input Encodings** : Click the browse button to specify the HTTP content encodings that the API Gateway can accept from peers. The available content encodings include `gzip` and `deflate`. By default, the content encodings configured the **Default Settings** are used. You can override this setting at the remote host and HTTP interface levels. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding/).

**Output Encodings** : Click the browse button to specify the HTTP content encodings that the API Gateway can apply to outgoing messages. The available content encodings include `gzip` and `deflate`. By default, the content encodings configured the **Default Settings** are used. You can override this setting at the remote host and HTTP interface levels. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding/).

**Include correlation ID in headers** : Specifies whether to insert the correlation ID in outbound messages. This means that an `X-CorrelationID` header is added to the outbound message. This is a transaction ID that is attached to each message transaction that passes through API Gateway, and which is used for traffic monitoring in the API Gateway Manager web console. You can use the correlation ID to search for messages in the web console, and you can also access its value from a policy using the `id` message attribute. This setting is selected by default.

## Configure watchdogs

You can configure an HTTP interface to shut down based on certain *conditions*. One such condition is dependent on the API Gateway being able to contact a particular back-end web service running on a remote host. To do this, you can configure an **HTTP Watchdog** for a remote host to poll the endpoint. If the endpoint cannot be reached, the HTTP interface is shut down.

To configure the API Gateway to shut down an HTTP interface based on the availability of a remote host, perform the following steps:

1. Configure an **HTTP Watchdog** for the remote host. See [Configure HTTP watchdog](#configure-http-watchdog).
2. Configure a **Requires Endpoint** condition on the HTTP interface. See [Configure conditions for an HTTP Interface](/docs/apim_policydev/apigw_gw_instances/general_services/#configure-conditions-for-an-http-interface).
3. When configuring this condition, select the remote host configured in step 1 (the host with the associated Watchdog).

{{< alert title="Note" color="primary" >}}When **Load Balancing** is configured as **Weighted by response time**, and remote host watchdogs are configured, the watch dog polling also contributes to the load balancing calculations.{{< /alert >}}

### Configure HTTP watchdog

An HTTP Watchdog can be added to a Remote Host configuration in order to periodically poll the Remote Host to check its availability. For example, if the Remote Host becomes unavailable for some reason, an HTTP interface can be brought down and can stop accepting requests. Once the Remote Host comes back online, the HTTP interface automatically starts up and starts accepting requests again.

To configure an HTTP Watchdog, right-click a previously configured Remote Host in the Policy Studio tree (for example, under **Environment Configuration** > **Listeners** > **API Gateway**). Then select **Watchdog**

> **Add**, and configure the following settings in the dialog.

**Valid HTTP Response Code Ranges** : You can use this section to specify the HTTP response codes that you can regard as proof that the Remote Host is available. For example, if a 200 OK HTTP response is received for the poll request, the Remote Host can be considered available.

To specify a range of HTTP status codes, click the **Add** button and enter the **Start** and **End** of the range of HTTP response codes in the fields provided. An exact response code can be specified by entering the response code in both fields (for example, `200`).

**HTTP Request for Polling** : The fields in this section enable you to configure the type and URI of the HTTP request to poll the Remote Host with. The default is the *Options* HTTP command with a URI of `*`, which is typically used to retrieve status information about the HTTP server. To use an alternative HTTP request to poll the Remote Host, select an HTTP request method from the **Method**, and specify the **URI** field.

**Remote Host Polling** : The settings in this section determine when and how the HTTP Watchdog polls the Remote Host. The **Poll Frequency** determines how often the Watchdog is to send the polling request to the Remote Host.

By default, the Watchdog uses real HTTP requests to the Remote Host to determine its availability. In other words, if the API Gateway is sending a batch of requests to the Remote Host, it uses the response codes from these requests to decide whether or not the Remote Host is up. Therefore, the Watchdog effectively "polls" the Remote Host by sending real HTTP requests to it.

To configure the Watchdog to send poll requests during periods when it is not sending requests to and receiving responses from the Remote Host, select **Poll if up**. In this case, the Watchdog uses real HTTP requests to poll the Remote Host as long as it sends them, but starts sending test poll requests when it is not sending HTTP requests to the Remote Host to test its availability.

As you increase the number of remote hosts configured with HTTP Watchdogs, it can negatively impact Policy Studio deployments time. You can decrease HTTP Watchdog polling frequency to mitigate this problem.

{{< alert title="Note" color="primary" >}}When a Remote Host is deemed to be down (an invalid HTTP response code was received) the Watchdog continues to poll it at the configured **Poll Frequency** until it comes back up again (until a valid HTTP response code is received).{{< /alert >}}

## Configure an incoming remote host

A remote host is normally used to configure specific connection features for the outward connection, that is, for the connection from API Gateway to the back-end service. However, you can also configure a remote host for an incoming connection, that is, for the connection from the client to API Gateway.

To configure an incoming remote host, configure the following settings on the **General** tab of the remote host settings:

1. Enter `incoming` in the **Port** field.
2. For an incoming connection, the port is referring to the remote address of the TCP connection. Incoming connections arrive from effectively arbitrary remote ports, so this acts as a wildcard for all incoming connections.
3. Enter the IP address of the host in the **Host name** field, rather than the DNS name.
4. A CIDR style netmask can be specified (for example, `192.168.0.0/24` matches any address in the `192.168.0.x` range). This works on a longest-match basis if more than one network specification matches the client.