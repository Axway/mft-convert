{
"title": "Compressed content encoding",
  "linkTitle": "Compressed content encoding",
  "weight": 40,
  "date": "2019-10-17",
  "description": "Configure input and output content encodings globally, per remote host, or per listening interface."
}
The API Gateway supports HTTP content encoding for the `gzip` and `deflate` compressed content encodings. This enables the API Gateway to compress files and deliver them to clients (for example, web browsers) and to back-end servers.

For example, HTML, text, and XML files can be compressed to approximately 20% to 30% of their original size, thereby saving on server traffic. Compressing files causes a slightly higher load on the server, but this is compensated by a significant decrease in client connection times to the server. A client that takes six seconds to download an uncompressed XML file might only take two seconds for a compressed version of the same file.

In Policy Studio, an **Input Encodings** list specifies the content encodings that the API Gateway can accept from peers, and an **Output Encodings** list specifies the content encodings that the API Gateway can apply to outgoing messages. You can configure these settings globally, per remote host, or per listening interface.

## Encoding of HTTP responses

Content encoding of HTTP responses is negotiated using the `Accept-Encoding` HTTP request header. This enables a client to indicate its willingness to receive a particular encoding in this header. The server can then choose to encode the response with one of these client-supported encodings, and indicate this with the `Content-Encoding` header.

When the API Gateway is acting as a client communicating with a server, it uses the currently configured **Input Encodings** list to format the `Accept-Encoding` header sent to the server, thereby requesting the server to apply one of these encodings to it. If the server decides to apply one of these encodings, the API Gateway automatically inflates the compressed response when it is received.

When acting as a server, the API Gateway selects an output encoding from the intersection of what the client specified in its `Accept-Encoding` header, and the currently configured **Output Encodings**, and applies that encoding to the response.

## Encoding of HTTP requests

Because a request arrives unsolicited from a client to a server, there is not normally a chance to negotiate the server's ability to process any optional features, so the automatic negotiation provided by the `Accept-Encoding` header is not available.

When acting as a client, the API Gateway selects the first configured encoding from the **Output Encodings** list to encode its request to the server. If the server fails to accept this encoding, it most likely responds with an HTTP 415 error, and the API Gateway treats this as a general HTTP error. Therefore, if the server is unable to accept content encodings, the API Gateway must be configured not to send them to that particular server.

By default, the API Gateway always accepts any supported encoding from clients, regardless of settings. For example, if a client sends gzipped content, the API Gateway inflates it regardless of configured settings.

## Delimit the end of an HTTP message

HTTP sessions can encode a number of request/response pairs. The rules for delimiting the end of each message and the start of the next one are well defined, but complex due to requirements for historical compatibility, and poor support from HTTP entities.

### HTTP requests

There are two ways to delimit the end of an HTTP request:

* A `Content-Length`   header in the request indicates to the server the exact length of the payload entity following the HTTP headers, and can be used by the receiving server to locate the end of that entity.
* Alternatively, an HTTP/1.1 server should accept chunked transfer encoding, which precedes each chunk of the entity with a length, until a zero-length chunk indicates the end.

The benefit of using chunked transfer encoding is that the client does not need to know the length of the transmitted entity when it sends HTTP request headers (unlike when inserting a `Content-Length` header).

Because the API Gateway compresses the requests on the fly, it is prohibitively expensive to calculate the content length before compressing the body. As a result, outbound content encoding is only supported when talking to HTTP/1.1 servers that support chunked transfer encoding.

{{< alert title="Note" color="primary" >}}All HTTP/1.1 servers are required to support chunked transfer encoding, but unfortunately some do not, so you can use **Remote Host**
settings to configure whether a destination is capable of supporting the chunked encoding in HTTP/1.1, regardless of its advertised HTTP protocol version. For more details, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts).{{< /alert >}}

### HTTP responses

For HTTP responses, the server has three options for delimiting the end of the entity. The two mentioned above, and also the ability to close the HTTP connection after the response is transmitted. The receiving client can then infer that the entire message has been received due to the normal end of the TCP/IP session.

When content encoding responses, the API Gateway avoids using `Content-Length` headers in the response, and uses chunked encoding or TCP/IP connection closure to indicate the end of the response. This means that content encoding of responses is supported for HTTP/1.0 or HTTP/1.1 clients.

## Configure content encodings

In Policy Studio, you can configure HTTP content encodings in the **Content Encodings** dialog. You can configure the following settings globally per API Gateway instance, per remote host, or per listening interface:

* **Input Encodings**: Specifies the content encodings that the API Gateway can accept from peers.
* **Output Encodings**: Specifies the content encodings that the API Gateway can apply to outgoing messages.

The available content encodings include `gzip` and `deflate`.

API Gateway uses the content encodings configured in the general settings, **Environment Configuration** > **Server Settings** > **General**, by default. This configuration applies at the API Gateway instance level. When these settings list a value of `DEFAULT`, this results in no content encodings unless the value is overridden.

You can override these settings at the listening interface level (on the **Advanced** tab of the HTTP or HTTPS Interface dialog), and the remote host level (on the **Advanced** tab of the Remote Host Settings dialog). When any of these settings have a value of `DEFAULT`, they try to use whatever is configured in the general settings. If the general settings also have a value of `DEFAULT`, then no content is encoded.

{{< alert title="Note" color="primary" >}}
When an [incoming remote host](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts/#configure-an-incoming-remote-host) is configured with **Output Encodings** set to `Default`, the **Output Encodings** values from the HTTP or HTTPS interfaces are used. To use the **Output Encodings** values from the incoming remote host, you must define the AXWAY_LEGACY_INCOMING_OUTPUT_ENCODINGS environment variable.
{{< /alert >}}

### Add content encodings

To add content encodings, click the browse button next to the **Input Encodings** or **Output Encodings** field, and perform the following steps in the **Content Encodings** dialog:

1. Deselect the **Use Default** setting.
2. Select the content encodings to add in the **Available Content Encodings** list on the left.
3. Click the **\>** or **\>>** button to move the content encodings to the **Content Encodings** list on the right, which displays the active settings. You can also double-click a content encoding to move it to the right or left.
4. Click **OK**. This displays the configured encodings in the **Input Encodings** or **Output Encodings** field (for example, `gzip`, `deflate`).

### Configure no content encodings

To configure no content encodings, deselect the **Use Default** setting, and click **OK**. This displays `None` in the **Input Encodings** or **Output Encodings** field.

You can select the **Use Default** setting to switch to the **Default Settings** without losing your original content encoding selection.

For more details on HTTP content encoding, see [HTTP RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616.html).
