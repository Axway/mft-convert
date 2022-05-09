{
"title": "Routing filters",
  "linkTitle": "Routing filters",
  "weight": 11,
  "date": "2019-10-17",
  "description": "Commonly used routing filters, including Connect to URL and Connection."
}
<!-- TODO reorder based on usage from GA-->

Depending on how the API Gateway is perceived by the client, different combinations of routing filters can be used. For an introduction to routing scenarios and the filters in the **Routing** category, see [Get started with routing configuration](/docs/apim_policydev/apigw_polref/routing_getstarted/).

## Connect to URL filter

The **Connect to URL** filter is the simplest routing filter to use to connect to a target web service. To configure this filter to send messages to a web service, you need only enter the URL of the service in the **URL** field. If the web service is SSL enabled or requires mutual authentication, you can use the other tabs on the **Connect to URL** filter to configure this.

Depending on how the API Gateway is perceived by the client, different combinations of routing filters can be used. Using the **Connect to URL** filter is equivalent to using the following combination of routing filters:

* Static Router
* Rewrite URL
* Connection

The **Connect to URL** filter enables the API Gateway to act as the endpoint to the client connection (and not as a proxy), and to hide the deployment hierarchy of protected web services from clients. In other words, the API Gateway performs *service virtualization*.

### Configure general settings

Configure the following general settings:

* **Name**: Enter an appropriate name for the filter to display in a policy.
* **URL**: Enter the complete URL of the target web service. You can specify this setting as a selector, which enables values to be expanded at runtime. Defaults to `${http.request.uri}`. You can also enter any query string parameters associated with the incoming request message as a selector, for example, `${http.request.uri}?${http.raw.querystring}`.

### Configure request settings

On the **Request** tab, you can use the API Gateway selector syntax to evaluate and expand request details at runtime. The values specified on this tab are used in the outbound request to the URL.

* **Method**: Enter the HTTP verb used in the incoming request (for example, `GET`). Defaults to `${http.request.verb}`.
* **Request Body**: Enter the content of the incoming request message body. Defaults to `${content.body}`.

    {{< alert title="Note" color="primary" >}} You must enter the body headers and body content in the **Request Body** text area. For example, enter the `Content-Type` followed by a return     and then the required message payload:
    ```
    Content-Type:text/html
    
    <!DOCTYPE html>
    <html>
    <body>
    <h1>Hello World</h1>
    </body>
    </html>
    ```
    {{< /alert >}}

* **Request Protocol Headers**: Enter the HTTP headers associated with the incoming request message. Defaults to `${http.headers}`. Ensure to avoid extra spaces before or after any selector expressions and to use CRLF after each raw header value, or the filter will abort with an IO error when executed.

### Configure SSL settings

Configure the SSL settings on the **SSL** tab. You can select the server certificates to trust on the **Trusted Certificates** tab, and the client certificates on the **Client Certificates** tab. You can select the SSL/TLS protocol options and specify the ciphers that API Gateway supports on the **Advanced (SSL)** tab.

#### Trusted certificates

When API Gateway connects to a server over SSL, it must decide whether to trust the server's SSL certificate. You can select a list of CA or server certificates from the **Trusted Certificates** tab that are considered trusted by the API Gateway when connecting to the server specified in the **URL** field on this dialog.

The table on the **Trusted Certificates** tab lists all certificates imported into the API Gateway Certificate Store. To *trust* a certificate for this particular connection, select the box next to the certificate in the table.

To select all certificates for a particular CA, select the box next to the CA parent node in the table.

Alternatively, you can select the **Trust all certificates in the Certificate Store** option to trust all certificates in the store. This is selected by default.

#### Client certificates

In cases where the destination server requires clients to authenticate to it using an SSL certificate, you must select a client certificate on the **Client Certificates** tab.

To select a client certificate click the **Signing Key** button, and complete the following fields on the **Select Certificate** dialog:

**Choose the certificate to present for mutual authentication (optional)**: Select to choose a certificate from the Certificate Store. Select the client certificate to use to authenticate to the server specified in the **URL** field.

#### Advanced SSL

You can configure the following settings on the **Advanced (SSL)** tab.

**SSL Protocol Options**:

You can configure the following SSL protocol options:

* **Do not use the SSL v3 protocol**: SSL v3 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
* **Do not use the TLS v1 protocol**: TLS v1.0 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
* **Do not use the TLS v1.1 protocol**: TLS v1.1 is not used for incoming connections to avoid any weaknesses in this protocol. This is selected by default.
* **Do not use the TLS v1.2 protocol**: TLS v1.2 is not used for incoming connections to avoid any weaknesses in this protocol. This is *not* selected by default.
* **Do not use the TLS v1.3 protocol**: TLS v1.3 is not used for incoming connections to avoid any weaknesses in this protocol. This is *not* selected by default.
* **Disable renegotiation in TLSv1.2 and earlier**: Disable renegotiation, do not send HelloRequest messages, and ignore renegotiation requests via ClientHello. This is selected by default. If you disable this option and allow SSL renegotiation, both secure and legacy insecure renegotiation with unpatched servers will be possible.

**Ciphers**:

You can specify the ciphers that the server supports in this field. The API Gateway sends this list of supported ciphers to the destination server, which selects the highest strength common cipher as part of the SSL handshake. The selected cipher is used to encrypt the data as it is sent over the secure channel.

The cipher string allows you to specify a list of TLS v1.2 and below cipher suites (for example, `DEFAULT`) as well as TLS v1.3 cipher suites (for example, `TLS_AES_256_GCM_SHA384`) if they are used standalone. You must use the delimiter `::` before TLS v1.3 cipher suites to allow and configure both TLS v1.2 and TLS v1.3 ciphers.

The following are some examples of cipher strings:

* `FIPS:!SSLv3:!aNULL`: Enables FIPS-compatible TLS v1.2 ciphers and default TLS v1.3 cipher suites (`TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256, TLS_AES_128_GCM_SHA256`).
* `FIPS:!SSLv3:!aNULL::TLS_AES_256_GCM_SHA384`: Enables FIPS-compatible TLS v1.2 ciphers and only one TLS v1.3 cipher suite ( `TLS_AES_256_GCM_SHA384`).
* `FIPS:!SSLv3:!aNULL::`: Disables TLS v1.3 ciphers, and only FIPS-compatible TLS v1.2 ciphers will be available.
* `ECDHE-ECDSA-AES256-SHA384::TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_CCM_8_SHA256`: Enables specific TLS v1.2 cipher (`ECDHE-ECDSA-AES256-SHA384`) and two specific TLS v1.3 cipher suites (`TLS_CHACHA20_POLY1305_SHA256 and TLS_AES_128_CCM_8_SHA256`).

For more information on the syntax of this setting, see the [OpenSSL documentation](http://www.openssl.org/docs/apps/ciphers.html).

The default cipher string of `FIPS:!SSLv3:!aNULL` performs the following:

* Enables FIPS-compatible cipher suites only
* Explicitly blocks cipher suites that require SSLv3 or lower
* Forces the use of TLS v1.2 and TLS v1.3 protocols
* Forbids unauthenticated cipher suites

### Configure authentication settings

The **Authentication** tab enables you to select a client credential profile for authentication. You can use client credential profiles to configure client credentials and provider settings for authentication using API keys, HTTP basic or digest authentication, Kerberos, or OAuth.

Click the browse button next to the **Choose a Credential Profile** field to select a credential profile. You can configure client credential profiles globally under the **Environment Configuration** > **External Connections** node in the Policy Studio tree.

### Configure additional settings

The **Settings** tab enables you to configure the following additional settings:

* **Retry**
* **Failure**
* **Proxy**
* **Redirect**
* **Headers**
* **Response Body**
* **Connection**

By default, these sections are collapsed. Click a section to expand it.

#### Retry settings

To specify the retry settings for this filter, complete the following fields:

* **Perform Retries**: Select whether the filter performs retries. By default, this setting is not selected, no retries are performed, and all **Retry** settings are disabled. This means that the filter only attempts to perform the connection once.
* **Retry On**: Select the HTTP status ranges on which retries can be performed. If a host responds with an HTTP status code that matches one of the selected ranges, this filter performs a retry. Select one or more ranges in the table (for example, `Client Error 400-499`). For details on adding custom HTTP status ranges, see the next subsection.
* **Retry Count**: Enter the maximum number of retries to attempt. Defaults to `5`.
    {{< alert title="Note" color="primary" >}}When **Retry** setting is enabled, the **Retry Count** value is used as the default number of redirects to follow in the **Redirect** settings.{{< /alert >}}
* **Retry Interval (ms)**: Enter the time to delay between retries in milliseconds. Defaults to `500` ms.
* **Add an HTTP status range**: To add an HTTP status range to the default list displayed in the **Retry On** table, click the **Add** button. In the **Configure HTTP Status Code** dialog, complete the following fields:
    * **Name**: Enter a name for the HTTP status range.
    * **Start status**: Enter the first HTTP status code in the range.
    * **End status**: Enter the last HTTP status code in the range.

To add one specific status code only, enter the same code in the **Start status** and **End status** fields. Click **OK** to finish. You can manage existing HTTP status ranges using **Edit** and **Delete**.

#### Failure settings

To specify the failure settings for this filter, complete the following fields:

* **Consider SLA Breach as Failure**: Select whether to attempt the connection if a configured SLA has been breached. This is not selected by default. If this option is selected, and an SLA breach is encountered, the filter returns `false`.
* **Save Transaction on Failure (for replay)**: Select whether to store the incoming message in the specified directory and file if a failure occurs during processing. This is not * selected by default.
* **File name**: Enter the name of the file that the message content is saved to. You can specify this using a selector, which is expanded to the specified value at runtime. Defaults to `$* {id}.out`.
* **Directory**: Enter the directory that the file is saved to. You can specify this using a selector, which is expanded to the specified value at runtime. Defaults to `${environment.* VINSTDIR}/message-archive`, where `VINSTDIR` is the location of a running API Gateway instance.
* **Maximum number of files in directory**: Enter the maximum number of files that can be saved in the directory. Defaults to `500`.
* **Maximum file size**: Enter the maximum file size in MB. Defaults to `1000`.
* **Include HTTP Headers**: Select whether to include HTTP headers in the file. HTTP headers are not included by default.
* **Include Request Line**: Select whether to include the HTTP request line from the client in the file. The request line is not included by default.
* **Call policy on Connection Failure**: Select whether to execute a policy in the event of a connection failure. This is not selected by default.
* **Connection Failure Policy**: Click the browse button on the right, and select the policy to run in the event of a connection failure in the dialog.

#### Proxy settings

To specify the proxy settings for this filter, complete the following fields:

* **Send via Proxy**: Select this option if the API Gateway must connect to the destination web service through an HTTP proxy. In this case, the API Gateway includes the full URL of the destination web service in the request line of the HTTP request. For example, if the destination web service resides at `http://localhost:8080/services`, the request line is as follows:
    ```
    POST http://localhost:8080/services HTTP/1.1
    ```

    If the API Gateway was not routing through a proxy, the request line is as follows:

    ```
    POST /services HTTP/1.1
    ```
* **Proxy Server**: When **Send via Proxy** is selected, you can configure a specific proxy server to use for the connection. Click the browse button next to this field, and select an existing proxy server.
    {{< alert title="Note" color="primary" >}}API Gateway does not support persistent SSL connections to back-end servers using proxy tunneling. The API Gateway connection caching mechanism is not designed for proxy tunnel connections.{{< /alert >}}
* **Transparent Proxy (present client's IP address to server)**: Enables the API Gateway as a *transparent proxy* on Linux systems with the `TPROXY` kernel option set. When selected, the IP address of the original client connection that caused the policy to be invoked is used as the local address of the connection to the destination server.

#### Redirect settings

To specify the redirect settings for this filter, complete the following fields:

* **Follow Redirects**: Specifies whether the API Gateway follows HTTP redirects, and connects to the redirect URL specified in the HTTP response. This setting is enabled by default, and the default number of redirects to follow is 2.

{{< alert title="Note" color="primary" >}}When the **Retry Count** property of the **Retry** settings is enabled, its value is used as the default number of redirects to follow.{{< /alert >}}

#### Header settings

To specify the header settings for this filter, complete the following fields:

* **Forward spurious received Content headers**: Specifies whether the API Gateway sends any content-related message headers when sending an HTTP request with no message body to the HTTP server. For example, select this setting if content-related headers are required by an out-of-band agreement. If there is no body in the outbound request, any content-related headers from the original inbound HTTP request are forwarded. These are extracted from the `http.content.headers` message attribute, generally populated by the API Gateway for the incoming call. This attribute can be manipulated in a policy using the appropriate filters, if required. This field is not selected by default.
* **HTTP Host Header**: An HTTP 1.1 client must send a `Host` header in all HTTP 1.1 requests. The `Host` header identifies the host name and port number of the requested resource as specified in the original URL given by the client.

    When routing messages on to target web services, the API Gateway can forward on the `Host` as received from the client, or it can specify the address and port number of the destination web service in the `Host` header that it routes onwards.

    Select **Use Host header specified by client** to force the API Gateway to always forward on the original `Host` header that it received from the client. Alternatively, to configure the API Gateway to include the address and port number of the destination web service in the `Host` header, select the **Generate new Host header** radio button.

#### Response body settings

**Load response body and release connection**: Select to load the response message body into API Gateway memory before exiting the filter, forcing it to do store-and-forward processing instead of pass-through. Loading and parsing durations are included in the corresponding traffic monitoring leg duration. The HTTP connection is released after the response body is parsed. This has the same effect on the policy as using the [HTTP Parser filter](/docs/apim_policydev/apigw_polref/utility_additional/index.html#http-parser-filter).

#### Connection settings

**Release previously opened connection**: This option makes the **Connect to URL** filter release the connection it makes after the filter is completed, rather than releasing it at the end of the entire policy execution. This is only important when the rest of the policy runs for a long time after the **Connect to URL** filter finishes execution.

This option defaults to disabled, causing the connection not to be closed until the end of the policy, because it changes how the execution time get charged to the **Connect to URL** filter in trace files and Traffic Monitor, unless used in conjunction with the **Load response body and release connection** option.

{{< alert title="Note" color="primary" >}}This option replaces the system property `<VMArg name="-DConnectToUrlFilter.removePreviousConnections=true"/>` that was added in 7.5.3 SP3 to enable releasing previously opened connections. If you were using this system property, you must select this option on each connection filter requiring this behavior, as the system property no longer exists.{{< /alert >}}

## Connection filter

The **Connection** filter makes the connection to the remote web service. It relies on connection details that are set by the other filters in the **Routing** category. Because the **Connection** filter connects out to other services, it negotiates the SSL handshake involved in setting up a mutually authenticated secure channel.

### Configure SSL, authentication, and additional settings

You can configure SSL settings, such as trusted certificates, client certificates, SSL/TLS protocols, and ciphers on the **SSL** tab. For details on the fields on this tab, see [Configure SSL settings](#configure-ssl-settings).

You can select credential profiles to use for authentication on the **Authentication** tab. For details on the fields on this tab, see [Configure authentication settings](#configure-authentication-settings).

The **Settings** tab allows you to configure the following additional settings:

* **Retry**
* **Failure**
* **Proxy**
* **Redirect**
* **Headers**
* **Response Body**
* **Connection**

By default, these sections are collapsed. Click a section to expand it. For details on the fields on this tab, see [Configure additional settings](#configure-additional-settings).

## Static router filter

API Gateway uses the information configured in the **Static Router** filter to connect to a machine that is hosting a web service. You should use the **Static Router** filter in conjunction with a **Rewrite URL** filter to specify the path to send the message to on the remote machine.

Depending on how API Gateway is perceived by the client, different combinations of routing filters can be used. For an introduction to routing scenarios and the filters in the **Routing** category, see [Get started with routing configuration](/docs/apim_policydev/apigw_polref/routing_getstarted/).

### Configure a static router

Configure the following fields on the **Static Router** configuration window:

* **Name**: Enter a name for the filter.
* **Host**: Enter the host name or IP address of the remote machine that is hosting the destination web service.
* **Port**: Enter the port on which the remote service is listening.
* **HTTP**: Select this option if API Gateway should send the message to the remote machine over plain HTTP.
* **HTTPS**: Select this option if API Gateway should send the message to the remote machine over a secure channel using SSL. You can use a **Connection** filter to configure API Gateway to mutually authenticate to the remote system.

## Rewrite URL filter

You can use the **Rewrite URL** filter to specify the path on the remote machine to send the request to. This filter normally used in conjunction with a **Static Router** filter, whose role is to supply the host and port of the remote service.

### Configure a URL rewrite

Configure the following fields on the **Rewrite URL** filter configuration window:

* **Name**: Enter an appropriate name for the filter to display in a policy.
* **URL**: Enter the relative path of the web service in the **URL** field. API Gateway combines the specified path with the host and port number specified in the **Static Router** filter to build up the complete URL to route to.

Alternatively, you can perform simple URL rewrites by specifying a fully qualified URL into the **URL** field. You can then use a **Dynamic Router** to route the message to the specified URL.

## Dynamic router filter

API Gateway can act as a proxy for clients of the secured web service. When a client uses a proxy, it includes the fully qualified URL of the destination in the request line of the HTTP request. It sends this request to the configured proxy, which then forwards the request to the host specified in the URL. The relative path used in the original request is preserved by the proxy on the outbound connection.

The following is an example of an HTTP request line that was made through a proxy, where `WEB_SERVICE_HOST` is the name or IP address of the machine hosting the destination web service:

```
POST http://WEB_SERVICE_HOST:80/myService HTTP/1.0
```

When API Gateway acts as a proxy for clients, it can receive requests like the one above. The **Dynamic Router** filter can route the request on to the URL specified in the request line (`http://WEB_SERVICE_HOST:80/myService`).
