{
"title": "General settings in Policy Studio",
  "linkTitle": "General settings",
  "weight": "70",
  "date": "2019-10-18",
  "description": "Set global configuration settings in Policy Studio to optimize the behavior of API Gateway for your environment."
}
To configure these settings, in the Policy Studio tree, select the **Server Settings** node, and click **General**. To confirm updates to these settings, click **Apply changes** at the bottom right of the screen.

After changing any settings, you must deploy to API Gateway for the changes to be enforced. You can do this in the Policy Studio main menu by selecting **Server > Deploy**. Alternatively, click the **Deploy** button in the toolbar, or press F6.

## General settings

You can configure the following settings in the **General** screen:

**Tracing level**: Enables you to set the trace level for API Gateway at runtime. Select the appropriate option from the drop-down list. Defaults to `INFO`.

**Active timeout**: When the API Gateway receives or sends blocks of data over a network connection, if the time between reading or writing successive blocks of data exceeds the **Active Timeout** specified in milliseconds, API Gateway closes the connection. This guards against a host closing the connection in the middle of sending data. For example, if the host's network connection is pulled out of the machine while in the middle of sending data to API Gateway. When API Gateway has read all the available data off the network, it waits the **Active Timeout** period before closing the connection. Defaults to `30000` milliseconds.

This setting applies to outgoing connections. For incoming connection settings, see [HTTP and HTTPS interfaces](/docs/apim_policydev/apigw_gw_instances/general_services#http-and-https-interfaces).

The **Active Timeout** value is also used as a wait time when the maximum number of connections for a host is reached. For example, when a host reaches the **Maximum connections** value, API Gateway waits the active timeout period before giving up on trying to make a new connection. The global default value for **Maximum connections** is `128` and cannot be changed. However, you can configure **Maximum connections** and **Active Timeout** on a per-host basis using the **Remote Hosts** setting.

**Date format**: Configures the format of the date for the purposes of transaction audit logging and historic metrics. Defaults to `MM.dd.yyyy HH:mm:ss,SSS`. For more details on this format, see [Class SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html). See also [Transaction audit log settings](/docs/apim_reference/log_global_settings/).

**Cache refresh interval**: Configures the number of seconds that the server caches data loaded from an external source before refreshing data from that source. Defaults to `5` seconds. To disable the cache, set this to `0`. This cache applies to attributes retrieved from external databases, LDAP directories, internal user stores, and IBM Tivoli. It also applies to query results for authentication against LDAP or databases, and to certificate revocation lists for certificate validation (CRL and XKMS only).

**Transaction timeout**: A configurable transaction timeout that detects slow HTTP attacks (slow header write, slow body write, slow read) and rejects any transaction that keeps the worker threads occupied for an excessive amount of time.

Unlike other timeouts, this is not measured with respect to any network transaction, rather it is measured from the start of the API Gateway transaction until the timeout occurs. A rejected transaction will run to completion, though it will not return any response to the client, therefore, you must set this value longer than any legitimate traffic. The default value is 240,000 milliseconds (4 minutes). To override this setting, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/).

**Maximum sent bytes per transaction**: The maximum number of bytes sent in a transaction. This is the maximum length for the transmitted data on transactions that API Gateway can handle. This helps to prevent denial-of-service (DoS) attacks. This setting limits the entire amount of data sent over the link, regardless of whether it consists of body, headers, or request line. The default value is 20 MiB (20,971,520 bytes) and the maximum is 16,384 PiB (18,446,744,073,709,551,615 bytes). You can override this setting in the **Remote host** settings. For more details, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/).

**Maximum received bytes per transaction**: The maximum number of bytes received in a transaction. This is the maximum length for the received data on transactions that API Gateway can handle. This helps to prevent denial-of-service (DoS) attacks. This setting limits the entire amount of data received over the link, regardless of whether it consists of the body, headers, or request. The default value is 20 MiB (20,971,520 bytes) and the maximum is 16,384 PiB (18,446,744,073,709,551,615 bytes). In a multi-datacenter environment with a large amount of APIs and data, you may need to increase this value to optimize the performance of the API Manager web console. For more details, see [Configure API Management in multiple datacenters](/docs/apimgmt_multi_dc/). You can override this setting in the **Remote host** settings. For more details, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/).

**Idle timeout**: API Gateway supports HTTP 1.1 persistent connections. The **Idle Timeout** specified in milliseconds is the time that API Gateway waits after sending a message over a persistent connection before it closes the connection. Typically, the host tells API Gateway that it wants to use a persistent connection. API Gateway acknowledges this instruction and decides to keep the connection open for a certain amount of time after sending the message to the host. If the connection is not reused within the **Idle Timeout** period, API Gateway closes the connection. Defaults to `15000` milliseconds.

This setting applies to outgoing connections, and its value can be overridden on a per-host basis using the **Remote host** settings. For more information, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/). For incoming connection settings, see [HTTP and HTTPS interfaces](/docs/apim_policydev/apigw_gw_instances/general_services#http-and-https-interfaces).

**LDAP service provider**: Specifies the service provider used for looking up an LDAP server (for example, `com.sun.jndi.ldap.LdapCtxFactory`). The provider is typically used to connect to LDAP directories for certificate and attribute retrieval.

**Maximum memory per request**: The maximum amount of memory in bytes that API Gateway can allocate to each request. This setting helps protect against denial of service caused by undue pressure on memory. As a general rule for XML messages, if you need to process a message of size N, you should allocate 6–7 times N amount of memory. This setting applies to outgoing connections For incoming connection settings, see [HTTP and HTTPS interfaces](/docs/apim_policydev/apigw_gw_instances/general_services#http-and-https-interfaces).

**Realm**: Specifies the realm for authentication purposes.

**Schema pool size**: Sets the size of the Schema Parser pool.

**Server brand**: Specifies the branding to be used in API Gateway.

**SSL session cache size**: Specifies the number of idle SSL sessions that can be kept in memory. You can use this setting to improve performance because the slowest part of establishing the SSL connection is cached. A new connection does not need to go through full authentication if it finds its target in the cache. Defaults to `32`. If there are more than 32 simultaneous SSL sessions, this does not prevent another SSL connection from being established, but means that no more SSL sessions are cached. A cache size of `0` means no cache, and no outbound SSL connections are cached.

**Token drift time**: Specifies the number of seconds drift allowed for WS-Security tokens. This is important in cases where API Gateway is checking the date on incoming WS-Security tokens. It is likely that the machine on which the token was created is out-of-sync with the machine on which API Gateway is running. The drift time allows for differences in the respective machine clock times.

**Allowed number of operations to limit XPath transforms**: Specifies the total number of node operations permitted in XPath transformations done by LibXML. Defaults to `4096`. Complex XPath expressions (or those constructed together with content to produce expensive processing) might lead to a denial-of-service risk. The number of operations required scales with both the size of the XML document and the complexity of the XPath expression, but it can be reduced to improve performance or get under the limit. When this limit is surpassed, it results in an **XPath complexity limit exceeded** error, indicating that this setting must be increased to permit the operation.

There is no way to determine how high a limit must be for a particular operation other than repeated testing. This number should be tuned by slowly increasing it in increments of a few thousand while monitoring the performance to ensure that the chosen XPath evaluation will not take an unreasonable amount of time.

**Input encodings**: Click the browse button to specify the HTTP content encodings that API Gateway instance can accept from peers. The available content encodings include `gzip` and `deflate`. Defaults to no context encodings. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding). This setting can be overridden at **HTTP interface** level and at **Remote Host** level.

**Output encodings**: Click the browse button to specify the HTTP content encodings that API Gateway instancecan apply to outgoing messages. The available content encodings include `gzip` and `deflate`. Defaults to no context encodings. For more details, see [Compressed content encoding](/docs/apim_policydev/apigw_gw_instances/common_compress_encoding). This setting can be overridden at **HTTP interface** level and at **Remote Host** level.

**Server's SSL cert's name must match name of requested server**: Ensures that the certificate presented by the server matches the name of the host address being connected to. This prevents host spoofing and man-in-the-middle attacks. This setting is enabled by default.

**Send desired server name to server during TLS negotiation**: Specifies whether to add a field to outbound TLS/SSL calls that shows the name that the client used to connect. For example, this can be useful if the server handles several different domains, and needs to present different certificates depending on the name that the client used to connect. This setting is not selected by default.

**Add correlation ID to outbound headers**: Specifies whether to insert the correlation ID in outbound messages. For the HTTP transport, this means that an `X-CorrelationID` header is added to the outbound message. This is a transaction ID that is tagged to each message transaction that passes through API Gateway, and which is used for traffic monitoring in the API Gateway Manager web console. You can use the correlation ID to search for messages in the console. You can also access the its value using the `id` message attribute in an API Gateway policy. An example correlation ID value is `Id-54bbc74f515d52d71a4c0000`. This setting is selected by default.

## MIME settings

The MIME settings list a number of default common content types that are used when transmitting Multipurpose Internet Mail Extensions (MIME) messages. You can configure API Gateway's **Content Type** filter to accept or block messages containing specific MIME types. Therefore, the contents of the MIME types library act as the set of all MIME types that API Gateway can filter messages with.

All of the MIME types are available for selection in the **Content Type** filter. For example, you can configure this filter to accept only XML-based types, such as `application/xml`, `application/*+xml`, `text/xml`, and so on. Similarly, you can block certain MIME types (for example, `application/zip`, `application/octet-stream`, and `video/mpeg`).

The file extensions are used to set the `Content-Type` headers returned by a [Static Content Provider](/docs/apim_policydev/apigw_gw_instances/general_services). If the file extension is not listed in the MIME settings configuration, a default `Content-Type` of `application/octet-stream` will be sent by the Static Content Provider.

### MIME settings configuration

To configure the MIME settings, in the Policy Studio main menu, select **Tasks > Manage Gateway Settings > General > MIME**. Alternatively, in the Policy Studio tree, select the **Environment Configuration > Server Settings** node, and click **General > MIME**. To confirm updates to these settings, click **Apply changes** at the bottom right of the screen.

The MIME settings screen lists the actual MIME types on the left column of the table, together with their corresponding file extensions (where applicable) in the right column.

To add a new MIME type, click the **Add** button. In the **Configure MIME Type** dialog, enter the new content type in the **MIME Type** field. If the new type has a corresponding file extension, enter this extension in the **Extension** field. Click the **OK** button when finished.

Similarly, you can edit or delete existing types using the **Edit** and **Delete** buttons.

## Namespace settings

API Gateway exposes global settings that enable you to configure which versions of the SOAP and WSSE specifications it supports. You can also specify which attribute is used to identify the XML Signature referenced in a SOAP message.

To configure the namespace settings, in the Policy Studio tree, select the **Environment Configuration > Server Settings** node, and click **General > Namespaces**. Alternatively, in the Policy Studio main menu, select **Tasks > Manage Gateway Settings > General > Namespaces**. To confirm updates to these settings, click **Apply changes** at the bottom right of the screen.

### SOAP Namespace

The **SOAP Namespace** tab can be used to configure the SOAP namespaces that are supported by API Gateway. In a similar manner to the way in which API Gateway handles WSSE namespaces, API Gateway will attempt to identify SOAP messages belonging to the listed namespaces in the order given in the table.

The default behavior is to attempt to identify SOAP 1.1 messages first, and for this reason, the SOAP 1.1 namespace is listed first in the table. API Gateway will only attempt to identify the message as a SOAP 1.2 message if it can't be categorized as a SOAP 1.1 message first.

### Signature ID Attribute

The **Signature ID Attribute** tab allows you to list the supported attributes that can be used by API Gateway to identify a Signature reference within an XML message.

An XML-signature `<signedInfo>` section may reference signed data via the `URI` attribute. The `URI`
value may contain an id that identifies data in the message. The referenced data will hold the "URI" field value in one of its attributes.

By default, the server uses the `Id` attribute for each of the WSSE namespaces listed above to locate referenced signed data. The following sample XML Signature illustrates the use of the `Id` attribute:

```
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Header>
      <dsig:Signature id="Sample" xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
       <dsig:SignedInfo>
         ...
         <dsig:Reference URI="#Axway:sLmDCph3tGZ10">
          ...
         </dsig:Reference>
       </dsig:SignedInfo>
       ....
      </dsig:Signature>
   </soap:Header>
   <soap:Body>
       <getProduct wsu:Id="Axway:sLmDCph3tGZ10"
           xmlns:wsu="http://schemas.xmlsoap.org/ws/2003/06/utility">
           <Name>SOA Test Client</Name>
           <Company>Company</Company>
       </getProduct>
   </soap:Body>
</soap:Envelope>
```

It is clear from this example that the Signature reference identified by the `URI` attribute of the `<Reference>` element refers to the nodeset identified with the `Id` attribute (the `<getProduct>` block).

Because different toolkits and implementations of the XML-Signature specification can use attributes other than the `Id` attribute, API Gateway allows the user to specify other attributes that should be supported in this manner. By default, API Gateway supports the `Id`, `ID`, and `AssertionID` attributes for the purposes of identifying the signed content within an XML Signature.

However you can add more attributes by clicking the **Add** button and adding the attribute in the interface provided. The priorities of attributes can be altered by clicking the **Up** and **Down** buttons. For example, if most of the XML Signatures processed by API Gateway use the `ID` attribute, this attribute should be given the highest priority.

### WSSE Namespace

The **WSSE Namespace** tab is used to specify the WSSE (and corresponding WSSU) namespaces that are supported by API Gateway.

API Gateway attempts to identify WS Security blocks belongingto the WSSE namespaces listed in this table. It first attempts to locate Security blocks belonging to the first listed namespace, followed by the second, then the third, and so on until all namespaces have been utilized. If no Security blocks can be found for any of the listed namespaces, the message will be rejected on the grounds that API Gateway does not support the namespace specified in the message.To add a new namespace, click the add button.

Every WSSE namespace has a corresponding WSSU namespace. For example, the following WSSE and WSSU namespaces are inextricably bound:

* WSSE Namespace - `http://schemas.xmlsoap.org/ws/2003/06/secext`
* WSSU Namespace - `http://schemas.xmlsoap.org/ws/2003/06/utility`

First, enter the WSSE namespace in the **Name** field. Then enter the corresponding WSSU namespace in the **WSSU Namespace** field.

## HTTP Session settings

The **HTTP Session** settings enable you to configure session management settings for the selected cache. For example, you can configure the period of timebefore expired sessions are cleared from the `HTTP Sessions` cache, which is selected by default.

To configure HTTP session settings, select the **Environment Configuration > Server Settings** node in the Policy Studio tree, and click **General > HTTP Session**.Alternatively, in the Policy Studio main menu, select **Tasks > ManageGateway Settings > General > HTTP Session**. To confirm updates to these settings, click **Apply changes** at the bottom right of the screen.

### HTTP Session configuration

Configure the following session settings:

**Cache**: Specifies the cache that you wish to configure. Defaults to `HTTP Sessions`.To configure a different cache, click the button on the right, and select the cache touse. The list of currently configured caches is displayed in the tree.

To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration > Libraries** node in the Policy Studio tree.

**Clear Expired Sessions Period**: Enter the number of seconds before expired sessions are cleared from the selected cache. Defaults to `60`.

## Zero downtime settings

The **Zero Downtime** settings enable you to configure zero downtime deployment and zero downtime shutdown. You can enable zero downtime deployment and set delays before and after deployment. You can also enable zero downtime shutdown and set the delay before shutdown.

To configure zero downtime settings, select the **Server Settings** node in the Policy Studio tree, and click **General > Zero Downtime**. To confirm updates to these settings, click **Save** at the bottom right of the window.

For more information of performing a zero downtime deployment, see [Perform zero downtime deployment](/docs/apim_administration/apigtw_admin/deploy_get_started/#perform-zero-downtime-deployment). For more information on performing a zero downtime shutdown, see [Perform zero downtime shutdown](/docs/apim_administration/apigtw_admin/manage_operations/#zero-downtime-shutdown).

Zero downtime deployment and shutdown rely on the **Health Check LB** policy to alert the load balancer when a maintenance operation is about to begin. To use the zero downtime deployment or shutdown features, the Health Check LB policy must be present in your API Gateway configuration.

### New projects

The Health Check LB policy is included in the default factory configuration.

When creating a new project in Policy Studio, choose **From a template configuration** and select the **Factory template with samples** template. This template includes the Health Check LB policy.

### Existing projects

If you created a project in Policy Studio from any other template (for example, **Factory template**, **Team Development – Common Project**, or **Team Development – API Project**), you must manually import the Health Check LB policy into your configuration.

To import the Health Check LB policy, select **File > Import > Import Configuration Fragment** from the Policy Studio main menu, and select the following file:

```
$VDISTDIR/samples/SamplePolicies/HealthCheck/HealthCheckLB.xml
```

You must also add the Health Check LB policy to a listener so that the load balancer can ping it to determine the health of the API Gateway (for example, path `/healthchecklb` on HTTP port `8080`).

You can customize the Health Check LB policy for your own environment. For example, you can modify the response code and message that are returned to the load balancer.

### Zero downtime configuration

Configure the following zero downtime settings:

**Zero-downtime deployment enabled**: Select the check box to enable zero downtime deployment. The default is disabled.

**Delay before deployment**: Enter the delay before deployment in seconds. This is the delay in the API Gateway between receiving a deployment request and starting the deployment. During this delay, the Health Check LB policy returns an error response, giving the load balancer time to route traffic away from the API Gateway before the deployment begins. The default is 10 seconds. You can choose a value between 1 and 20 seconds.

**Delay after deployment**: Enter the delay after deployment in seconds. This is the delay in the API Gateway between the end of deployment and a response being sent to the deployment request. During this delay, the Health Check LB policy returns a successful response, giving the load balancer time to start routing traffic to the API Gateway before deployment begins on the next API Gateway in the group. The default is 10 seconds. You can choose a value between 1 and 20 seconds.

**Zero-downtime shutdown enabled**: Select the check box to enable zero downtime shutdown. The default is disabled.

**Delay before shutdown**: Enter the delay before shutdown in seconds. This is the delay in the API Gateway between receiving a shutdown request and starting the shutdown procedure. During this delay, the Health Check LB policy returns an error response, giving the load balancer time to route traffic away from the API Gateway before shutdown begins. The default is 10 seconds. You can choose a value between 1 and 20 seconds.