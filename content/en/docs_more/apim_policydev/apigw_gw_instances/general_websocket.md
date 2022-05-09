{
"title": "Configure WebSocket connections",
  "linkTitle": "Configure WebSocket connections",
  "weight": 3,
  "date": "2019-10-17",
  "description": "Configure API Gateway to act as a WebSocket proxy."
}

## WebSocket protocol overview

The WebSocket protocol provides an extension to the HTTP 1.1 protocol to establish persistent, bidirectional communication between a client and a server.

The WebSocket protocol can be summarized as follows:

1. To establish a communication channel between a client and a server, the client needs to send an HTTP `Upgrade` request to the server. This is known as the WebSocket protocol handshake.
2. If the server is capable and willing to upgrade the connection, it sends a HTTP `101` response to the requesting client. At this point the handshake is considered successful and the connection between the server and the client is upgraded to the WebSocket protocol.

   As soon as the client receives the HTTP `101` response the connection is no longer considered an HTTP connection.
3. Messages can now flow bidirectionally between the server and the client over the WebSocket connection.
4. Any participant in the data exchange can request the WebSocket connection be terminated by sending a `Close` request to the other participant.

For a detailed description of the protocol, see [RFC 6455](http://tools.ietf.org/html/rfc6455).

## Configure a WebSocket connection

API Gateway can act as a WebSocket proxy, whereby it is deployed in front of a WebSocket capable web server (for example, Jetty or Apache Tomcat) and provides governance (security, monitoring, and so on) on the WebSocket traffic flowing between the client, API Gateway, and the web server.

### WebSocket configuration settings

Configure the following fields on the **WebSocket configuration** dialog:

**Enable this path resolver**: Select or deselect the check box to enable or disable the WebSocket handler. It is enabled by default.

#### Policies settings

You can assign specific policies on this tab to specific URIs that define the WebSocket endpoints. For example, you might need to handle frames being exchanged between a client and `ws://example.org/echo` differently to frames being exchanged between a client and `ws://example.org/voip`.

{{< alert title="Note" color="primary" >}}In the above scenario, different sets of policies need to be defined for each URI (`/echo`
and `/voip`). This requires different relative paths. For more information on relative paths, see [Configure relative paths](/docs/apim_policydev/apigw_gw_instances/general_relative_path/).{{< /alert >}}

**When a request arrives that matches the path**: Enter the path on which WebSocket connections are to be accepted. This defines the URI of the WebSocket endpoint. A relative path resolver for this path must already exist.

**Default MIME type for message body**: Enter the MIME type of the messages. The default is `application/json`.

When messages are flowing bidirectionally between a WebSocket server and client, they are no longer HTTP messages and as such they do not contain a Content type header. For API Gateway to process the content of the message it needs to know what type of content the message is. This field enables you to specify the type of message being exchanged between the server and the client.

**On Upgrade request from client**: Click the browse button to select a policy to be used by API Gateway when an `Upgrade` request is received.

This policy is executed when a connection is being upgraded from HTTP to WebSocket. For example, you might statically route all WebSocket requests to `ws://example.org` to `wss://example.com` by using a policy. A policy for an HTTP connection must be provided and this policy must provide a mechanism to connect to the remote server.

For example, to route all requests to `ws://example.org` to `wss://example.com`, you can use the **Static Router** filter. Similarly, for dynamic routing you can use the **Dynamic Router** or **Connect to URL** filters. In the latter case, the remote host that the client is attempting to connect to can be extracted from the Host header of the request. This value can then be passed to the **Connect to URL** filter and used by that filter to establish a connection to the remote host.

{{< alert title="Note" color="primary" >}}In the routing filter, you must use `http://` or `https://` URLs as API Gateway does not recognize `ws://` and `wss://` URLs.{{< /alert >}}

The on upgrade policy is also a good place to perform authentication and authorization of the requesting client.

**On WebSocket communication from client**: Click the browse button to select a policy to be used by API Gateway when frames are received from the WebSocket client.

**On WebSocket communication from server**: Click the browse button to select a policy to be used by API Gateway when frames are received from the WebSocket server.

{{< alert title="Note" color="primary" >}}After a successful upgrade request, the context used to upgrade the connection from HTTP to WebSocket protocol is accessible to the policies called for specific frames. This context is not shared between different WebSocket connections and it is destroyed when the connection is closed. The context contains all the message attributes (including authorization and authentication data) used by the upgrade request. This context should be accessed by the server and client policies in read-only mode. {{< /alert >}}

The following figure shows the flow of messages between the client, API Gateway, and the server in a typical WebSocket communication. It also shows at which point each of the API Gateway policies are called.

![Sequence diagram showing flow of messages for WebSocket communication](/Images/docbook/images/general/websocket_sequence.png)

**Connection expire time**: Enter a numerical value and choose the units from the list. The available options are seconds, minutes, hours, and days. The default value is `0`, which means unlimited.

This is the absolute time for the connection to be active. For example, if this value is set to 1 hour, then after 1 hour the connection is dropped by API Gateway even if the connection is still active (frames are being sent).

{{< alert title="Tip" color="primary" >}}You can define an idle timeout for the connection as a part of the remote host configuration.{{< /alert >}}

#### Advanced settings

For details on the fields on this tab, see [Advanced settings](/docs/apim_policydev/apigw_gw_instances/general_relative_path/#advanced-settings).

#### CORS settings

For details on the fields on this tab, see [CORS settings](/docs/apim_policydev/apigw_gw_instances/general_relative_path/#cors-settings).

## Monitor a WebSocket connection

You can use the API Gateway Manager web console to monitor WebSocket traffic. You can view the initial HTTP `Upgrade` request and response on the **Traffic > HTTP** tab. You can view all the WebSocket frames processed by API Gateway on the **Traffic > WebSockets** tab.

To view all the WebSocket frames processed by API Gateway in API Gateway Manager, follow these steps:

1. Click the **Traffic** button at the top of the window.
2. Click the **WebSockets** tab. A view of all WebSocket frames sent from or received by API Gateway is displayed.

   A message might consist of one or more frames.
3. Click a message frame to see a detailed view of the content. For example, if you click a frame sent from the client to the server, the origin, opcode (interpretation of the payload data), duration, length of the data, key used to mask the data, and payload itself are shown.

## WebSocket connection example

This example shows you how to create and test a WebSocket connection in API Gateway. API Gateway acts as a proxy for WebSocket services.

To create and test a WebSocket connection, perform the following steps:

1. Create a HTTP/1.1 remote host for the back-end WebSocket service.
2. Create a policy to handle the initial `Upgrade` request from the client.
3. (Optional) Create policies to handle data frames from the client and server.
4. Create a WebSocket handler.
5. Test the connection using a WebSocket client.

### Create a HTTP/1.1 remote host

Each WebSocket server that API Gateway is routing to must be defined as an HTTP/1.1 remote host.

To add a remote host to an API Gateway instance, right-click the API Gateway instance under **Environment Configuration** > **Listeners** in the Policy Studio tree, and select **Add Remote Host**.

In this example, the back-end WebSocket service is provided by a public echo service on the URL `http://echo.websocket.org/`. The remote host configuration is as follows:

![Example WebSocket remote host configuration](/Images/docbook/images/general/websocket_remotehost_example.png)

When configuring the remote host:

* Select the **Allow HTTP 1.1** check box.
* Set **Maximum connections** to `-1` (unlimited connections).

### Create a policy to handle the Upgrade request

This policy handles the `Upgrade` request from the client. You can also perform authentication and authorization of the requesting client in this policy.

In this example, the sample Upgrade policy contains a simple **Connect to URL** filter:

![Sample Upgrade policy](/Images/docbook/images/general/websocket_upgrade_policy.png)

The **Connect to URL** filter is configured as follows:

![Connect to URL example](/Images/docbook/images/general/websocket_connect_url.png)

{{< alert title="Note" color="primary" >}}The filter points to the URL `http://echo.websocket.org/` and not `ws://echo.websocket.org`. In the URL `http://` is used instead of `ws://` (and similarly `https://` instead of `wss://`) as API Gateway does not recognize `ws://` and `wss://` URLs. This works because the upgrade connection is an HTTP call until the WebSocket server returns `101 - switching protocols`. {{< /alert >}}

Alternatively, you could use a **Static Router** or **Dynamic Router** filter in combination with a **Connection** filter for the Upgrade policy.

### Create policies to handle data frames

These are optional policies that trigger on data frames from the client or server. The policies that trigger on data frames from the client and server do not need to make any connections, as the connection is already established. They can modify the WebSocket frames found in `content.body` to alter the frame being sent. To access the HTTP headers from the original Upgrade request, you can find a copy of the `HeaderSet` object inside the `websock.context` attribute that is available on each invocation (the `websock.context` attribute should be considered read-only).

This example does not use any policies to handle data frames.

### Create a WebSocket handler

To enable API Gateway to accept an HTTP `Upgrade` request from a client you must add a WebSocket handler to your API Gateway configuration and configure it with the HTTP path that the upgrade can be expected on.

To add a WebSocket handler, follow these steps:

1. In the Policy Studio tree, select a list of relative paths (for example, **Environment Configuration** > **Listeners > API Gateway > Default Services > Paths**).
2. In the **Resolvers** window on the right, click **Add > WebSocket** to display the **WebSocket configuration** dialog.
3. Configure the WebSocket as follows:

![WebSocket handler example](/Images/docbook/images/general/websocket_handler.png)

{{< alert title="Note" color="primary" >}}Ensure that you have not set `gzip` or `deflate` compression, as it is not compatible for WebSocket connections.{{< /alert >}}

### Test the WebSocket connection

To test the WebSocket connection, follow these steps:

1. Deploy the configuration on the API Gateway instance. Click the Deploy button on the toolbar in Policy Studio or press **F6**.
2. Go to <https://www.websocket.org/echo.html> (a test WebSocket client).
3. Enter the address of your API Gateway WebSocket proxy in the **Location** field and follow the instructions to connect and send messages. For example: ![WebSocket test](/Images/docbook/images/general/websocket_test_client.png)
4. View the traffic in API Gateway Manager. For example: ![HTTP traffic in API Gateway Manager](/Images/docbook/images/general/websocket_http_traffic.png)

   ![WebSocket traffic in API Gateway Manager](/Images/docbook/images/general/websocket_traffic.png)
