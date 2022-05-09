{
"title": "Configure HTTP services",
  "linkTitle": "Configure HTTP services",
  "weight": 1,
  "date": "2019-10-17",
  "description": "Configure HTTP services groups,  HTTP/S interfaces, packet sniffers, and Management services."
}
API Gateway uses HTTP *services* to handle traffic from various HTTP-based sources. The available HTTP services are as follows:

* **HTTP interfaces**: *HTTP interfaces* define the ports and IP addresses on which API Gateway listens for incoming requests. You can also add *HTTPS interfaces* to specify SSL certificates to authenticate to clients, and certificates considered trusted for establishing SSL connections with clients.
* **Relative path**: You can configure *relative paths* so that when a request is received on a specific path, API Gateway can map it to a specific policy, or chain of policies.
* **Static content provider**: You can use a *static content provider* to serve static HTTP content from a particular directory. In this case, API Gateway is effectively acting as a web server.
* **Servlet applications**: API Gateway can act as a servlet container, which enables you to drop servlets into the HTTP services configuration. This should only be used by developers with very specific requirements and under strict advice from Axway Support.
* **Packet sniffer**: You can add a *packet sniffer* to intercept network packets from the client, assemble them into complete HTTP messages, and log these messages to an audit trail. Because the packet sniffer operates at the network layer (unlike an HTTP-based traffic monitor at the application layer), the packets are intercepted transparently to the client. This means that the packet sniffer is a *passive service*, which is typically used for management and monitoring instead of general policy enforcement.

## HTTP services groups

An *HTTP services group* is a container around one or more HTTP services. Usually, an HTTP services group is configured for a particular type of HTTP service. For example, you could have an **HTTP Interfaces** group that contains the configured HTTP interfaces, and another **Static Providers** group to manage static content providers. While organizing HTTP services by type eases the task of managing services, API Gateway is flexible enough to enable administrators to organize services into groups according to whatever scheme best suits their requirements.

This section describes a scenario where HTTP services groups can prove useful, and how to use separate services groups to process, for example, SSL traffic on a different channel.

### HTTP interfaces and relative paths

An HTTP services group must have at least one HTTP interface together with at least one relative path. The HTTP interface determines which TCP port API Gateway listens on, and the relative path maps a request received on a particular path (request URI) to specific policies. You can add several HTTP interfaces to a group, in which case requests received on any one of the opened ports are processed in the same manner. For example, `http://<HOST>:8080/test` and `http://<HOST>:8081/test` requests can both be processed by the same policy (mapped to the `/test` relative path).

You can also add multiple relative paths to a HTTP services group, where each path is bound to a specific policy or chain of policies. For example, if a request is made to `http://<HOST>:8080/a`, it is processed by Policy A. If a request is made to `http://<HOST>:8080/b`, it is handled by Policy B. Requests made to the other interface are also processed by the same policy, meaning that a request made to `http://<HOST>:8081/b` is also handled by Policy B.

This means that relative paths configured under a HTTP services group are bound to all HTTP interfaces configured for that group. If you have two interfaces listening on ports `8080` and `8081`, API Gateway handles requests to `http://<HOST>:8080/a` and `http://<HOST>:8081/a` identically.

{{< alert title="Note" color="primary" >}}The maximum length for a custom URL used in the HTTP connection is limited by the "Maximum Memory per Request" field when configuring a port in the HTTP Services interface of Policy Studio.{{< /alert >}}

### Example configuration

In this example configuration, a SSL validation policy is added to process requests to `http://<HOST>:443/a`, while the existing Schema Validation policy continues to handle requests for `http://<HOST>:8080/a`. To distinguish between receiving requests on the two different ports, a new `SSL HTTP Services Group` is added alongside the existing `HTTP Services Group`. The new group opens a single HTTPS interface that listens on the SSL port `443`, and is configured with a relative path of `/a` to handle requests on this path:

| Service Group             | HTTP Port | Relative Path | Policy                   |
| ------------------------- | --------- | ------------- | ------------------------ |
| `HTTP Services Group`     | `8080`    | `/a`          | Schema Validation Policy |
| `SSL HTTP Services Group` | `443`     | `/a`          | SSL Validation Policy    |

Using HTTP services groups in this way, you can configure API Gateway to dispatch requests received on the same path (for example, `/a`) to different policies depending on the port where the request was accepted.

### Default HTTP services groups

By default, API Gateway ships with preconfigured HTTP services groups (for example, **Default Services** and **Sample Services**). These groups contain some generic default policies you can use with an out-of-the-box installation of API Gateway.

In addition to the preconfigured services groups, you can add new HTTP services groups to dispatch requests to different policies based on the port on which the requests are received.

For details on the default service group used by the Admin Node Manager and API Gateway Analytics, see [Management services](#management-services).

### Add an HTTP services group

1. In Policy Studio, click **Environment Configuration > Listeners**.
2. Right-click **API Gateway**, and select **Add HTTP Services**.
3. Enter a name for the group.
4. By default, Cross Origin Resource Sharing (CORS) is disabled because no profile is selected. To enable CORS for the HTTP services group, on the **CORS** tab, select a preconfigured CORS profile. If no profiles have already been configured, right-click **CORS Profiles**, and select **Add a CORS Profile**. To edit an existing profile, right-click the profile and select **Edit**.

When you have created an HTTP services group, you can configure it with the HTTP services described in this topic.

## HTTP and HTTPS interfaces

An HTTP interface defines the address and port that API Gateway listens on. There are two types of interface: HTTP and HTTPS. The HTTP interface handles standard, non-authenticated HTTP requests, while the HTTPS interface can accept mutually authenticated SSL connections.

Before you can configure an interface, you must first configure a HTTP services group.

### Add HTTP or HTTPS interface

To create an HTTP interface, in Policy Studio tree, click **Environment Configuration > Listeners > API Gateway**, select the HTTP services group (for example, **Default Services**), right-click the **Ports** node, and select **Add HTTP**or **Add HTTPS**.

#### Configure Network settings

The following fields on the **Network** tab are common to both HTTP and HTTPS interfaces and must be configured:

* **Port**: The port number that API Gateway listens on for incoming HTTP requests.
* **Address**: The IP address or host of the network interface on which instance listens.
* For example, you can configure the instance to listen on port `80` on the external IP address of a machine, while having a web server running on the same port but on the internal IP address of the same machine. By entering `*`, the instance listens on all interfaces available on the machine hosting API Gateway.
* **Protocol**: Select the Internet Protocol version (IPv) that this interface uses. You can select `IPv4`, `IPv6`, or both of these protocol versions. The default is `IPv4`.
* **Trace level**: The level of trace output. The possible values in order of least verbose to most verbose output are:

    * `FATAL`
    * `ERROR`
    * `INFO`
    * `DEBUG`
    * `DATA`

    The default trace level is read from the system settings.
* **Enable interface**: This setting is enabled by default. If you want to disable the HTTP interface, deselect this setting.

#### Configure Traffic Monitor settings

The fields on the **Traffic Monitor** tab are common to both HTTP and HTTPS interfaces. To override the system-level settings at HTTP or HTTPS interface level, select **Override settings for this port**, and configure the relevant options.

#### Configure Advanced settings

The following fields on the **Advanced** tab are common to both HTTP and HTTPS interfaces and must be configured:

* **Backlog**: When API Gateway is busy handling concurrent requests, the operating system can accept additional incoming connections. In such cases, a backlog of connections can build up while the operating system waits for the instance to finish handling current requests. The specified value is the maximum number of connections API Gateway instance allows the operating system to accept and queue up until the instance is ready to read them. The larger the backlog, the larger the memory usage. The smaller the backlog, the greater the potential for dropped connections.
* **Idle Timeout**: API Gateway supports the use of HTTP 1.1 persistent connections. Typically, a client informs API Gateway that it wants to use a persistent connection. API Gateway acknowledges this and keeps the connection open for a certain time after sending the response to the client. If the client does not reuse the connection by sending up another request within the timeout period, API Gateway closes the connection. The **Idle Timeout** value is the time (in milliseconds) that API Gateway waits for a new request after sending a response over a persistent connection before closing the connection. The default value is 60000 milliseconds (60 seconds). You can override this setting in the **Remote Host Settings** (for more details, see [Configure an incoming remote host](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/#configure-an-incoming-remote-host)).
* **Active Timeout**: When API Gateway receives a large HTTP request, it reads the request off the network as it becomes available. If the time between reading successive blocks of data exceeds the **Active Timeout** value, API Gateway closes the connection. For example, if the client loses network connection while sending the data, instead of being tied to the transaction for a long time, API Gateway first reads all the available data off the network and then waits the **Active Timeout** period before closing the connection. The default value is 60000 milliseconds (60 seconds). You can override this setting in the **Remote Host Settings** (for more details, see [Configure an incoming remote host](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/#configure-an-incoming-remote-host)).
* **Maximum Memory per Request**: The maximum amount of memory in bytes that API Gateway allocates to each request.
* **Input Encodings**: The HTTP content encodings that API Gateway can accept from peers. By default, the content encodings configured in the **[General Settings](/docs/apim_reference/general_settings/#general-settings)** are used. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding/). You can override this setting in the **Remote Host Settings** (for more details, see [Configure an incoming remote host](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/#configure-an-incoming-remote-host)).
* **Output Encodings**: The HTTP content encodings that API Gateway can apply to outgoing messages. By default, the content encodings configured in the **[General Settings](/docs/apim_reference/general_settings/#general-settings)** are used. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding/). You can override this setting in the **Remote Host Settings** (for more details, see [Configure an incoming remote host](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/#configure-an-incoming-remote-host)).
* **Transparent Proxy - allow bind to foreign address**: This enables you to use API Gateway as a *transparent proxy* on Linux systems with the `TPROXY` kernel option set. When selected, the value in the **Address** field can specify any IP address, and API Gateway handles incoming traffic for the configured address/port combinations. For more details and an example, see [Configure a transparent proxy](#configure-a-transparent-proxy).
* **Include correlation ID in headers**: This specifies whether to insert the correlation ID (for example, `Id-54bbc74f515d52d71a4c0000`) in outbound messages. For the HTTP transport, this means that an `X-CorrelationID` header is added to the outbound message. This is a transaction ID that is tagged to each message transaction that passes through API Gateway. You can use the correlation ID to search for messages when monitoring traffic in the API Gateway Manager web console. You can also access the its value using the `id` message attribute in an API Gateway policy. This setting is selected by default.
* **Threat Protection Settings**: This specifies the **Threat Protection Profile** that is used to protect this interface with Apache ModSecurity threat protection rules. ModSecurity is a toolkit for real-time HTTP traffic monitoring, logging, and access control, which helps to mitigate application-level threats to APIs. The ModSecurity engine is embedded in API Gateway to provide API firewalling. If no threat protection profiles have been configured, right-click the **Threat Protection Profiles** node in the dialog, and select **Add a Threat Protection Profile**. For more details, see [Manage API firewalling](/docs/apim_administration/apigtw_admin/admin_waf/).

### Configure HTTPS-specific settings

The following settings apply only to HTTPS interfaces and are not visible when creating a HTTP interface.

#### Network settings

In addition to the fields configured for an HTTP interface on the **Network** tab, you must configure the following setting:

* **X.509 Certificate**: Click this button to select the certificate that API Gateway uses to authenticate itself to clients during the SSL handshake. The list of certificates currently stored in the API Gateway certificate store is displayed. Select a single certificate from this list.

#### Mutual Authentication settings

* **Client Certificates**: Define how clients can authenticate to API Gateway on the **Mutual Authentication** tab. Choose from the following:
* **Ignore Client Certificates**: API Gateway ignores client certificates if they are presented during the SSL handshake.
* **Accept Client Certificates**: API Gateway accepts client certificates when presented, but clients that do not present certificates are not rejected.
* **Require Client Certificates**: API Gateway only accepts connections from clients that present a certificate during the SSL handshake.
* **Maximum depth of client certificate chain**: Specify how many CA certificates in a chain of one or more are trusted when validating the client certificate. By default, only one issuing CA certificate is used, and this certificate must be checked in the list of trusted root certificates. If more than one certificate is used, only the top-level CA must be considered trusted, while the intermediate CA certificates are not.

    Client certificates are typically issued by a Certificate Authority (CA). In most cases, the CA includes a copy of its certificate in the client certificate so that consumers of the certificate can decide whether or not to trust the client based on the issuer of the certificate.

    A *chain* of CAs can also issue the client certificate. For example, a top-level organization-wide CA (for example, Company CA) may have issued department-wide CAs (for example, Sales CA, QA CA, and so on), and each department CA is then responsible for issuing all department members with a client certificate. In such cases, the client certificate may contain a chain of one or more CA certificates.
* **Root Certificate Authorities trusted for mutual authentication**: Select the root CA certificates that API Gateway considers trusted when authenticating clients during the SSL handshake. Only certificates signed by the CAs selected here are accepted.

### Configure Advanced SSL settings

You can configure the followingon the **Advanced (SSL)** tab:

* **Check that the SSL certificate's Subject CN resolves to network address**: When this setting is selected, API Gateway attempts to resolve the SSL certificate's `Subject` Common Name (CN) to the network address configuring the SSL interface. If API Gateway cannot resolve the `Subject` CN to the network address, it logs a warning in the error traces. This setting is selected by default. To disable checking the certificate's `Subject` CN, deselect this setting.
* **SSL Server Name Identifier (SNI)**: Specify the host names that clients request in the **SSL Server Name Identifier (SNI)** table. SNI is an optional TLS feature where the client indicates to the server the host name used to resolve the server address. This enables a server to present different certificates for clients to ensure the correct site is contacted.

    For example, the server IP address is `192.168.0.1`. Clients consult the DNS to resolve a host name to an address and contact the server IP address using TCP/IP. If both `www.acme.com` and `www.anvils.com` resolve to `192.168.0.1`, without SNI, the server does not know which host name the client uses to resolve the address, because it is not party to the client DNS name resolution. The server may certify itself as either service, but when the connection is established, it does not know which host name the client connects to.

    With SNI, the client provides the name of the host (for example, `www.anvils.com`) in the initial SSL exchange, before the server presents its certificate in its distinguished name (for example, `CN=www.anvils.com`). This way, the server can certify itself correctly as providing a service for the client's requested host name.

    To specify an SNI, click **Add**, specify the server host name in **Client requests server name**, click **Server assumes identity** to import a Certificate Authority certificate into the Certificate Store, and click **OK**.
* **Ciphers**: Specify the ciphers that the server supports in the **Ciphers** field. The server selects the highest-strength cipher that is also supported by the client from this list as part of the SSL handshake.

    Cipher strings allows you to specify a list of TLSv1.2 and earlier cipher suites (for example, `DEFAULT`), as well as TLSv1.3 cipher suites (for example, `TLS_AES_256_GCM_SHA384`). To configure both TLSv1.2 and TLSv1.3 ciphers, you must use the `::` delimiter before the TLSv1.3 cipher suite.

    Following are some examples of cipher strings and their outcome:

    * `FIPS:!SSLv3:!aNULL`: Enables FIPS-compatible TLSv1.2 ciphers and default TLS1.3 ciper suites (`TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256   TLS_AES_128_GCM_SHA256`).
    * `FIPS:!SSLv3:!aNULL::TLS_AES_256_GCM_SHA384`: Enables FIPS-compatible TLSv1.2 ciphers and only one TLSv1.3 cipher suite (`TLS_AES_256_GCM_SHA384`).
    * `FIPS:!SSLv3:!aNULL::`: Disables TLSv1.3 ciphers, only FIPS-compatible TLSv1.2 ciphers will be available.
    * `ECDHE-ECDSA-AES256-SHA384::TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_CCM_8_SHA256`: Enables specific TLSv1.2 cipher (`ECDHE-ECDSA-AES256-SHA384`) and two specific TLSv1.3 cipher suite (`TLS_CHACHA20_POLY1305_SHA256` and `TLS_AES_128_CCM_8_SHA256`.

    The default cipher string of `FIPS:!SSLv3:!aNULL` performs the following:

    * Enables FIPS-compatible cipher suites only.
    * Explicitly blocks cipher suites that require SSLv3 or lower.
    * Forces the use of TLSv1.2 and TLSv1.3 protocols.
    * Forbids unauthenticated cipher suites.

For more information on the syntax of this setting, see the [OpenSSL documentation](https://www.openssl.org/docs/man1.1.1/man1/ciphers.html).

* **SSL session cache size**: Specify the number of idle SSL sessions that can be kept in memory. The default is `32`. If there are more simultaneous SSL sessions, new SSL connections can still be established, but no more SSL sessions are cached. If you set cache size to `0`, there is no cache. No outbound SSL connections are cached.
* At `DEBUG` level or higher, API Gateway logs a trace when an entry goes into the cache, for example:

  ```
  DEBUG   09:09:12:953 [0d50] cache SSL session 11AA3894 to support.acme.com:443
  ```

    API Gateway also logs a trace when the cache is full, for example:

  ```
  DEBUG   09:09:12:953 [0d50] enough cached SSL sessions 11AA3894 to support.acme.com:443 already
  ```

    {{< alert title="Tip" color="primary" >}}You can use this setting to improve performance because API Gateway caches the slowest part of establishing the SSL connection. A new connection does not need to go through full authentication if it finds its target in the cache.{{< /alert >}}
* **Ephemeral DH key parameters**: Specify the parameters used to generate Diffie Hellman (DH) keys. The DH key agreement algorithm is used to negotiate a shared secret between two SSL peers. This enables two parties without prior knowledge of each other to jointly establish a shared secret key over an insecure communication channel.

    When DH key parameters are not specified, the SSL client uses the public RSA key in the server's certificate to encrypt data sent to the SSL server and establish a shared secret with the server. However, if the RSA key is ever discovered, any previously recorded encrypted conversations can be decrypted. DH key agreement offers Perfect Forward Secrecy (PFS) because there is no such key to be compromised.

    There are two options when setting DH key parameters:

    * Enter a number (for example, `512`), and the server automatically generates DH parameters with a prime number of the correct size.
    * Paste the Base64 encoding of an existing serialized DH parameters file. You can use standard DH parameters based on known good prime numbers. OpenSSL ships with the `dh512.pem` and `dh1024.pem` files. For example, you can set the DH parameters to the following Base64-encoded string in `pdh512.pem`:

    ```
    -----BEGIN DH PARAMETERS-----
    MEYCQQD1Kv884bEpQBgRjXyEpwpy1obEAxnIByl6ypUM2Zafq9AKUJsCRtMIPWakXUGfnHy9iUsiGSa6q6Jew1X
    pKgVfAgEC
    -----END DH PARAMETERS-----
    ```

    The DH parameters setting is required if the server is using a DSA-keyed certificate, but also has an effect when using RSA-based certificates. DH (or similar) key agreement is required for DSA-based certificates because DSA keys cannot be trivially used to encrypt data like RSA keys can.

    {{< alert title="Note" color="primary" >}}The EDH key is always used once only to guarantee forward secrecy. This ensures that if the key is compromised, previous keys is not compromised.{{< /alert >}}
* **SSL Protocol Options**: You can configure the following SSL protocol options:

    * **Do not use the SSL v3 protocol**: SSL v3 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
    * **Do not use the TLS v1 protocol**: TLS v1.0 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
    * **Do not use the TLS v1.1 protocol**: TLS v1.1 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
    * **Do not use the TLS v1.2 protocol**: TLS v1.2 is not use for incoming connections to avoid any weaknesses in this protocol. This is *not* selected by default.
    * **Do not use the TLS v1.3 protocol**: TLS v1.3 is not use for incoming connections to avoid any weaknesses in this protocol. This is *not* selected by default.
    * **Prefer local cipher preferences over client's proposal**: When choosing a cipher during the SSL/TLS handshake, the client's preferences are selected by default from the list of ciphers supported by the client and the server. When this option is selected, the server's preferences are used instead. This option is *not* selected by default. For more details on ciphers, see the [OpenSSL documentation](https://www.openssl.org/docs/man1.1.1/man1/ciphers.html).
    * **Disable renegotiation in TLSv1.2 and earlier**: Disable renegotiation, do not send HelloRequest messages, and ignore renegotiation requests via ClientHello. This is selected by default. If you disable this option and allow SSL renegotiation, only secure renegotiation will be possible.

### Configure conditions for an HTTP Interface

You can configure API Gateway to bring down an active HTTP interface if certain *conditions* fail to hold. For example, if the back-end web service is unavailable or if the physical interface on the machine loses connectivity to the network, it is possible to shut down the HTTP interface so that it stops accepting requests.

A typical scenario where this functionality proves useful is as follows:

* A load balancer sits in front of several running instances of the API Gateway and round-robins requests between them all.
* A client sends SSL requests through the load balancer, which forwards them opaquely to one of the API Gateway instances.
* The API Gateway terminates the SSL connection, processes the message with the configured policy, and forwards the request onto the back-end web service.

In this deployment scenario, the load balancer does not want to keep sending requests to an instance of the API Gateway if it has either lost connectivity to the network or if the back-end web service is unavailable. If either of these *conditions* hold, the load balancer should stop attempting to route requests through this instance of the API Gateway and use the other instances instead.

So then, how can the load balancer determine the availability of the web service and also the connectivity of the machine hosting the API Gateway to the network on which the web service resides? Given that the request from the client to the API Gateway is over SSL, the load balancer has no way of decrypting the encrypted SSL data to determine whether or not a SOAP Fault, for example, has been returned from the API Gateway to indicate a connection failure.

The solution is to configure certain *conditions* for each HTTP interface, which must hold for the HTTP interface to remain available and accept requests. If any of the associated conditions fail, the interface is brought down and does not accept any more requests until the failed condition becomes true and the HTTP interface is restarted.

When the load balancer receives a connection failure from the API Gateway (which it does when the HTTP interface is down) it stops sending requests to this API Gateway and chooses to round-robin requests amongst the other instances instead.

The following conditions can be configured on the HTTP interface:

* Requires Endpoint:   The HTTP interface remains up only if the remote host is available. The remote host is polled periodically to determine availability so that the HTTP interface can be brought back up automatically when the Remote Host becomes available again.
* Requires Link:   The HTTP interface remains up only if a named physical interface has connectivity to the network. As soon as a down physical interface regains connectivity, the HTTP interface automatically comes back up again.

You can configure conditions for an HTTP interface using the HTTP interface **Ports** node (for example, `*:8080`) under the API Gateway instance in the Policy Studio tree. For example, this is available under **Environment Configuration** > **Listeners** > **API Gateway**.

Right click the HTTP interface in the right pane, and select **Add Condition** menu option, and then either the **Requires Endpoint** or **Requires Link** option depending on your requirements. The sections below describe how to configure these conditions.

#### Configure Requires Endpoint condition

A **Requires Endpoint** condition can be configured in cases where you only want to keep the HTTP interface up if the back-end web service (the Remote Host) is available. An HTTP Watchdog can be configured for the Remote Host, which is then responsible for polling the Remote Host periodically to ensure that the web service is available. See *Configure remote host settings* on page 1 and *Configure HTTP watchdog* on page 1 for more information.

**Remote Host**: The HTTP interface shuts down if the Remote Host selected here is deemed to be unavailable. The Remote Host can be continuously polled so that the interface can be brought up again when the Remote Host becomes available again.

#### Configure Requires Link condition

The **Requires Link** condition is used to bring down the HTTP interface if a named physical network interface is no longer connected to the network. For example, if the cable is removed from the Ethernet switch, the dependent HTTP interface is brought down immediately. The HTTP interface only starts listening again when the physical interface is connected to the network again (when the Ethernet cable is plugged back in).

{{< alert title="Note" color="primary" >}}The **Requires Link**
condition is only available on Linux platforms.{{< /alert >}}

**Interface Name**: The HTTP interface is brought down if the physical network interface named here is no longer connected to the network. On UNIX platforms, physical network interfaces are usually named `eth0`, `eth1`, and so on.

### Configure a transparent proxy

On Linux systems with the `TPROXY` kernel option enabled, you can configure the API Gateway as a *transparent proxy*. This enables the API Gateway to present itself as having the server's IP address from the point of view of the client, or having the client's IP address from the point of view of the server. This can be useful for administrative or network infrastructure purposes (for example, to keep using existing client/server IP addresses, and for load-balancing).

You can configure transparent proxy mode both for inbound and outbound API Gateway connections:

* Incoming interfaces can listen on IP addresses that are not assigned to any interface on the local host.
* Outbound calls can present the originating client's IP address to the destination server.

Both of these options act independently of each other.

#### Configure transparent proxy mode for incoming interfaces

To enable transparent proxy mode on an incoming interface, perform the following steps:

1. In the Policy Studio tree, expand the **Environment Configuration** > **Listeners** > **API Gateway** node.
2. Right-click your service, and select **Add Interface** > **HTTP** or **HTTPS** to display the appropriate dialog (for example, **Configure HTTP Interface**).
3. Select the check box labeled **Transparent Proxy (allow bind to foreign address)**. When selected, the value in the **Address** field can specify any IP address, and incoming traffic for the configured address/port combinations is handled by the API Gateway.

#### Configure transparent proxy mode for outgoing calls

Transparent proxy mode for outgoing calls must be enabled at the level of a connection filter in a policy. To enable transparent proxy mode for outbound calls, perform the following steps:

1. Ensure that your policy contains a connection filter (for example, **Connect to URL** or **Connection**, available from the **Routing** category in the filter palette).
2. In your connection filter, select the **Settings** tab and expand the **Proxy** section.
3. Select the check box labeled **Transparent Proxy (present client's IP address to server)**. When selected, the IP address of the original client connection that caused the policy to be invoked is used as the local address of the TCP connection to the destination server.

#### Transparent proxy example

A typical configuration example of transparent proxy mode is shown as follows:

![Transparent proxy example](/Images/docbook/images/transparent_proxy/transparent_proxy_example.png)

In this example, the remote client’s address is `172.16.0.99`, and it is attempting to connect to the server at `10.0.0.99` on port `80`. The front-facing firewall is configured to route traffic for `10.0.0.99` through the API Gateway at address `192.168.0.9`. The server is configured to use the API Gateway at address `10.0.0.1` as its default IP router.

The API Gateway is multihomed, and sits on both the `192.168.0.0/24` and `10.0.0.0/24` networks. In the Configure HTTP Interface dialog, the API Gateway is configured with a listening address of `10.0.0.99` and port of `80` on the **Network** tab, and with transparent proxy mode enabled on the **Advanced** tab. For example:

![Configure HTTP interface](/Images/docbook/images/transparent_proxy/configure_http_interface.png)

The API Gateway accepts the incoming call from the client, and processes it locally. However, there is no communication with the server yet. The API Gateway can process the call to completion and respond to the client—it is masquerading as the server.

If the API Gateway invokes a connection filter when processing this call (with transparent proxying enabled), the connection filter consults the originating address of the client, and binds the local address of the new outbound connection to that address before connecting. The server then sees the incoming call on the API Gateway originating from the client (`172.16.0.99`), rather than either of the API Gateway's IP addresses.

The following dialog shows the example configuration for the **Connect to URL** filter:

![Configure connection](/Images/docbook/images/transparent_proxy/configure_connect_to_url.png)

The result is a transparent proxy, where the client sees itself as connecting directly to the server, and the server sees an incoming call directly from the client. The API Gateway processes two separate TCP connections, one to the client, one to the server, with both masquerading as the other on each connection.

{{< alert title="Note" color="primary" >}}Either side of the transparent proxy is optional. By configuring the appropriate settings for the incoming interface or the connection filter, you can masquerade only to the server, or only to the client. {{< /alert >}}

## Packet sniffers

*Packet sniffers* are a type of passive service. Rather than opening up a TCP port and *actively* listening for requests, the packet sniffer *passively* reads data packets off the network interface. The sniffer assembles these packets into complete messages that can then be passed into an associated policy.

Because the packet sniffer operates passively (does not listen on a TCP port) and transparently to the client, it is most useful for monitoring and managing web services. For example, you can deploy the sniffer on a machine running a web server acting as a container for web services.

Assuming that the web server is listening on TCP port 80 for traffic, the packet sniffer can be configured to read all packets destined for port 80 (or any other port, if necessary). The packets can then be marshaled into complete HTTP/SOAP messages by the sniffer and passed into a policy that, for example, logs the message to a database.

### Configuration

Because packet sniffers are mainly used as passive monitoring agents, they are usually created in their own service group. For example, to create a new group, right-click the API Gateway instance under **Environment Configuration** > **Listeners** in the Policy Studio tree, and select **Add Service Group**. Enter `Packet Sniffer Group` in the dialog.

You can then add a relative path service to this group by right-clicking the `Packet Sniffer Group`, and selecting **Add Relative Path**. Enter a path in the field provided, and select the policy to dispatch messages to when the packet sniffer detects a request for this path (after it assembles the packets). For example, if the relative path is configured as `/a`, and the packet sniffer assembles packets into a request for this path, the request is dispatched to the policy selected in the relative path service.

Finally, to add the packet sniffer, right-click the `Packet Sniffer Group` node, select **Packet Sniffer**, then **Add** menu option, and complete the following fields:

**Device to Monitor**: Enter the name of the network interface that the packet sniffer monitors. The default is `any` (valid on Linux only). On UNIX, network interfaces are usually identified by names like `eth0` or `eth1`. On Windows, names are more complicated (for example, `\Device\NPF_{00B756E0-518A-4144 ... }`).

**Filter**: You can configure the packet sniffer to only intercept certain types of packets. For example, it can ignore all UDP packets, only intercept packets destined for port 80 on the network interface, ignore packets from a certain IP address, listen for all packets on the network, and so on.

The packet sniffer uses the `libpcap` library filter language to achieve this. This language has a complicated but powerful syntax that enables you to *filter* what packets are intercepted, and what packets are ignored. As a general rule, the syntax consists of one or more expressions combined with conjunctions, such as `and`, `or`, and `not`.

The following table lists a few examples of common filters and explains what they filter:

| Filter expression                            | Description                                                                           |
| -------------------------------------------- | ------------------------------------------------------------------------------------- |
| `port 80`                                    | Capture only traffic for the HTTP Port (`80`).                                        |
| `host 192.168.0.1`                           | Capture traffic to and from IP address `192.168.0.1`.                                 |
| `tcp`                                        | Capture only TCP traffic.                                                             |
| `host 192.168.0.1 and port 80`               | Capture traffic to and from port 80 on IP address `192.168.0.1`.                      |
| `tcp portrange 8080-8090`                    | Capture all TCP traffic destined for ports from 8080 through to 8090.                 |
| `tcp port 8080 and not src host 192.168.0.1` | Capture all TCP traffic destined for port 8080 but not from IP address `192.168.0.1`. |

The default filter of `tcp` captures all TCP packets arriving on the network interface. For more details on how to configure filter expressions, see <http://www.tcpdump.org/tcpdump_man.html>.

**Promiscuous Mode**: When listening in promiscuous mode, the packet sniffer captures all packets on the same Ethernet network, regardless of whether the packets are addressed to the network interface that the sniffer is monitoring.

## Management services

The Management Services group exposes a number of services that the Admin Node Manager and API Gateway Analytics use for remote configuration and monitoring. The Management Services interfaces and policies are displayed in the Policy Studio tree:

* The **Management Services** policy container under **Policies**
* The **Management Services** HTTP interfaces under **Environment Configuration > Listeners > Admin Node Manager**

{{< alert title="Caution" color="warning" >}}Admin Node Manager may not function correctly if you change the HTTP interfaces, relative path, servlet applications, or static content provider exposed under the Management Services group. Because of this, the Management Services group should only be modified under strict supervision from Axway Support.{{< /alert >}}

### Management services group

By default, the Management Services group consists of the following:

* **HTTP Interface**: The default port where Admin Node Manager exposes all its management services so that they can be configured remotely is port `8090`. At startup, Policy Studio can connect to this port to read and write API Gateway configuration data. By default, API Gateway Analytics exposes all its management services on port `8040`.
* **Relative Path**: The relative path `/` is mapped to a default management policy called **Protect Management Interface**, which is available in the **Management Services** policy container. The policy performs HTTP Basic authentication and passes control to a **Call Internal Service** filter, a special filter that passes messages to a servlet application or static content provider based on the path on which the request was received.

#### Request processing cycle

For example, with the default configuration, a request is received on `http://localhost:8090/api`. The following steps summarize the request processing cycle:

1. The relative path `/` matches all incoming requests, and requests are dispatched to whatever policy the relative path is mapped to, in this case, the **Protect Management Interface** policy.
2. The **Protect Management Interface** policy authenticates the originator of the request using HTTP Basic authentication. Authentication is necessary because all configuration operations are considered privileged operations and only be carried out by those with the required authority. If the originator is successfully authenticated, the policy invokes the **Call Internal Service** filter.
3. The **Call Internal Service** filter attempts to match the relative path that the request was received on against all the servlets and content providers configured in the same services group as this interface, because the message is received on the management interface (port `8090`).
4. The **Call Internal Service** filter finds `/api/`servlet matching the path the request was received on included in the servlets and content providers configured for the Management Services group, and invokes the servlet.

### Change the management services port

By default, Admin Node Manager uses port `8090` as the default management services port. To specify a different port, perform the following steps:

1. In the Policy Studio tree, click **Environment Configuration > Listeners > Node Manager > Management Services > Ports**.
2. In the **Interfaces** pane, right-click the **Management HTTPS interface**, and select **Edit**.
3. In **Port**, enter the port you want to use (for example, `8091`), and click **OK**.
4. Deploy the change to API Gateway.
5. Restart Policy Studio. You must always restart Policy Studio when Management Services are updated.
6. Use the updated port number in the URL to reconnect Policy Studio (for example, `https://HOST:8091/api`).

### Customize HTTP security headers

You edit the Management Services group the Admin Node Manager uses to customize the HTTP security headers included in the API Gateway response on port `8090`. You can edit the Admin Node Manager configuration using either Policy Studio or Entity Explorer.

{{< alert title="Caution" color="warning" >}}Management Services apply to the Admin Node Manager and API Gateway Analytics only. Any modifications must be done under strict advice and supervision from Axway Support.{{< /alert >}}

#### Edit Admin Node Manager configuration in Policy Studio

To change the Admin Node Manager configuration, create a copy of your existing configuration where to do the edits. When ready, copy the required configuration files back to your actual API Gateway configuration.

1. In Policy Studio, click **File > New Project**.
2. Enter a project **Name**, and click **Next**.
3. Select to start the new project **From existing configuration**, and click **Next**.
4. Click the browse button, select the following directory, and click **Finish**:

   ```
   INSTALL_DIR/apigateway/conf/fed
   ```
5. In the Policy Studio tree, click **Environment Configuration > Listeners > Node Manager > Management Services** and open **Paths**.
6. In the **Resolvers** pane, right-click the **/** static content node, and click **Edit**.
7. On the **General** tab, click **Add**, enter the HTTP security header name/value pair, and click **OK**. For example:

   * **HTTP Header**: `X-XSS-Protection`
   * **Value**: `1; mode=block`
8. Repeat to add any additional HTTP security header name/value pairs (for example, `Strict-Transport-Security` or `Public-Key-Pins`), then click **OK** to return to Resolvers pane.
9. Under **Paths**, right-click the **Login** static content node, and click **Edit**.
10. On the **General**, click **Add**, and add the HTTP security headers that you added to the **/** static content node.
11. After you are finished editing the configuration, manually copy the `.xml` and `.md5` configuration files from the new project you edited to `INSTALL_DIR/conf/fed/`, and make a backup.
12. Restart the Admin Node Manager.

#### Edit Admin Node Manager configuration in Entity Explorer

Perform the following steps:

1. Go to the following directory:

   ```
   INSTALL_DIR/apigateway/posix/bin
   ```
2. Run `esexplorer`.
3. Right-click on the store you want, select **Connect**, and browse to the following file:

   ```
   INSTALL_DIR/apigateway/conf/fed/configs.xml
   ```
4. Click **System Components > Service > Management Services**, and expand **/,** static content node.
5. Right-click the **/,** static content node, select **Add a new Property**, and click the new property (for example, `key-0`).
6. In the `name` row on the right, double-click the **Value** field, and enter the name of the HTTP security header (for example, `X-XSS-Protection`).
7. Right-click under this field, select **Create a value** to add a `value` row, double-click **Value** on the row, and enter the value of the HTTP security header (for example, `1; mode=block`).
8. Repeat to add any additional HTTP security header name/value pairs (for example, `Strict-Transport-Security` or `Public-Key-Pins`).
9. Click **Update** to save the changes.
10. Select **System Components > Service > Management Services**, and expand the **/login/,** static content node,
11. Right-click **/login/,**, select **Add a new Property**, and click the new property.
12. Add the HTTP security headers that you added under the **/,** static content node in the same way.
13. Click **Update** to save the changes.
14. Restart the Admin Node Manager.