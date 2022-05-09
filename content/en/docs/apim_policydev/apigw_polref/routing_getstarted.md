{
"title": "Get started with routing configuration",
  "linkTitle": "Get started with routing configuration",
  "weight": 10,
  "date": "2019-10-17",
  "description": "Configure API Gateway to send messages to external services."
}
API Gateway offers a number of different filters that can be used to route messages. Depending on how API Gateway is perceived by the client, different combinations of routing filters can be used.

For example, API Gateway can act both as a proxy and as an endpoint (in-line) server for a client, depending on how the client is configured. In each case, the request received by API Gateway appears slightly different, and API Gateway can take advantage of this when routing the message onwards. Furthermore, API Gateway can provide *service virtualization* by shielding the underlying hierarchy of back-end web services from clients.

This topic explains how clients can use API Gateway both as a proxy and as an endpoint server. It then shows how *service virtualization* works. When these basic concepts are explained, this topic helps you to identify the combination of routing filters that is best suited to your deployment scenario.

## Proxy or endpoint server

API Gateway can be used by clients as both a proxy server and an endpoint server. When a client uses API Gateway as a proxy server, it sends up the complete URL of the destination web service in the HTTP request line. API Gateway can use the URL to determine the host and port to route the message to. The following example shows an HTTP request received by API Gateway when acting as a proxy for a client:

```
POST http://localhost:8080/services/getHello HTTP/1.1
```

Alternatively, when API Gateway is acting as an endpoint (in-line) server, the client sends the request directly to API Gateway. In this case, the request line appears as follows:

```
POST /services/getHello HTTP/1.1
```

In this case, only the path on the server is specified, and no scheme, host, or port number is included in the HTTP request line. Because this information is not provided by the client, API Gateway must be explicitly configured to route the message on to the specific destination.

## Service virtualization

It is sometimes desirable to shield the underlying structure of the directory hierarchy in which web services reside from external clients. You can do this by providing a mapping between the path that the client accesses and the actual path at which the web service resides.

For example, suppose you have two web services accessible at the `/helloService/getHello` and `/financeService/getQuote` URIs. You might wish to hide that these services are deployed under different paths, perhaps exposing them under a common `/services` base URI (for example, `/services/getHello` and `/services/getQuote` ). The client is therefore unaware of the underlying hierarchy (for example, directory structure) of the two web services. This is termed *service virtualization*.

## Use cases

To choose the correct combination of routing filters, you must first consider how your client will use API Gateway (as a proxy or as an endpoint). Additionally, you must consider if you require virtualization. This section includes several use cases that demonstrate how different filters can be used in different scenarios.

### Use case 1: Proxy without service virtualization

In this case, API Gateway is configured as an HTTP proxy for the client, and maintains the original path used by the client in the HTTP request. For example, if API Gateway is listening at `http://localhost:8080/`, and the web service is running at `http://localhost:5050/services/getQuote`, the request line of the client HTTP request appears as follows:

```
POST http://localhost:5050/services/getQuote HTTP/1.1
```

Because the client is configured to use the API Gateway instance running on `localhost` at port `8080` as its HTTP proxy, the client automatically sends all messages to the proxy. However, it includes the full URL of the ultimate destination of the message in the request line of the HTTP request.

When API Gateway receives the request, it extracts this URL from the request line and uses it to determine the destination of the message. In the above example, API Gateway routes the message on to `http://localhost:5050/services/getQuote`.

You can configure the following policy to route the message to the URL specified in the request line of the client request:

![Policy 1:Proxy without service virtualization](/Images/docbook/images/connection/proxy_without_virt.gif)

The following table explains the role of each filter in the policy.

| Filter             | Role in Policy                                                                                                                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dynamic Router** | Extracts the URL of the destination web service from the request line of the incoming HTTP request. The **Dynamic Router** is normally used when API Gateway is perceived as a proxy by the client. |
| **Connection**     | Establishes the connection to the destination web service, and sends the message over this connection. This connection can be mutually authenticated if necessary.                                  |

### Use case 2: Proxy with service virtualization

In this case, API Gateway is also perceived as an HTTP proxy by the client. However, API Gateway exposes a *virtualized* view of the services that it protects. This is termed *service virtualization*.

To achieve this, API Gateway must provide a mapping between the path used by the client and the path under which the service is deployed. Assuming API Gateway is running at `http://localhost:8080/services`, and the web service is deployed at `http://localhost:5050/financialServices/quotes/getQuote`, the following example shows what the client might send up in the HTTP request line:

```
POST http://localhost:5050/services/getQuote HTTP/1.1
```

To achieve this, API Gateway must provide a mapping between what the client requests (`/services/getQuote`), and the address of the web service (`/financialServices/quotes/getQuote`). The **Rewrite URL** filter in the following policy fulfills this role:

![Policy 2:Proxy with service virtualization](/Images/docbook/images/connection/proxy_with_virt.gif)

The following table explains the roles of each filter in the policy.

| Filter             | Role in Policy                                                                                                                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dynamic Router** | Extracts the URL of the destination web service from the request line of the incoming HTTP request. The **Dynamic Router** is normally used when API Gateway is perceived as a proxy by the client. |
| **Rewrite URL**    | Specifies the mapping between the path requested by the client and the path under which the web service is deployed, therefore providing service virtualization.                                    |
| **Connection**     | Establishes the connection to the destination web service, and sends the message over this connection. This connection can be mutually authenticated if necessary.                                  |

### Use case 3: Endpoint without service virtualization

In this scenario, the client sees API Gateway as the endpoint to its connection, and API Gateway must be configured to route messages on to a specific destination. For example, assuming that API Gateway is running at `http://localhost:8080/services`, the request line of the client's HTTP request is received by API Gateway as follows:

```
POST /services HTTP/1.1
```

The request line above shows that no information about the scheme, host, or port of the destination web service is specified. Therefore, this information must be configured in API Gateway so that it knows where to route the message on to. The **Static Router** enables you to enter connection details for the destination web service.

Assuming that the web service is running at `http://localhost:5050/stockquote/getPrice`, the host, port, and scheme respectively are: `localhost`, `5050`, and `http`. You must explicitly configure this information in the **Static Router**. The following policy illustrates this scenario:

![Policy 3:Endpoint without service virtualization](/Images/docbook/images/connection/endpoint_without_virt.gif)

The following table explains the role of each filter in the policy.

| Filter            | Role in Policy                                                                                                                                                                                                                                              |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Static Router** | Enables the user to explicitly specify the host, port, and scheme at which the web service is listening. This filter must be used when the client sees API Gateway as the endpoint to its connection (API Gateway is not acting as a proxy for the client). |
| **Connection**    | Establishes the connection to the destination web service, and sends the message over this connection. This connection can be mutually authenticated if necessary.                                                                                          |

### Use case 4: Endpoint with service virtualization

In this case, API Gateway acts as the endpoint to the client connection (and not as a proxy), and hides the deployment hierarchy of protected web services from clients (performs *service virtualization*).

In this scenario, the client sends messages directly to API Gateway. For example, assuming that API Gateway is running at `http://localhost:8080/services`, and the web service is running at `http://localhost:5050/stockquote/getPrice`, the request line of the client HTTP request is received by API Gateway as follows:

```
POST /services HTTP/1.1
```

You can then configure the **Static Router** filter to route the message on to port `8080` on `localhost` using the `http` scheme, while the **Rewrite URL** filter provides the mapping between the path requested by the client (`/services`) and the path under which the web service is deployed (`/stockquote/getPrice`). The following policy illustrates a sample policy that provides *service virtualization* when API Gateway is used as an endpoint:

![Policy 4:Endpoint with service virtualization](/Images/docbook/images/connection/endpoint_with_virt.gif)

The following table explains the role of each filter in the policy.

| Filter            | Role in Policy                                                                                                                                                                                                                      |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Static Router** | Enables you to explicitly specify the host, port, and scheme at which the web service is listening. This filter can be used when the client sees the API Gateway as the endpoint to its connection (not as a proxy for the client). |
| **Rewrite URL**   | Provides the mapping between the path requested by the client and the path under which the web service is deployed.                                                                                                                 |
| **Connection**    | Establishes the connection to the destination web service, and sends the message over this connection. This connection can be mutually authenticated if necessary.                                                                  |

Alternatively, instead of using the **Static Router**, **Rewrite URL**, and **Connection** filters, you can use the **Connect to URL** filter, which is equivalent to using these three filters combined. You can configure the **Connect to URL** filter to send messages to a web service simply by specifying the destination URL.

### Use case 5: Simple redirect

In some cases, API Gateway must route the incoming message to an entirely different URL. You can use the **Rewrite URL** filter for this purpose, in addition to rewriting the path on which the request is received (as described in [Use case 2: Proxy with service virtualization](#use-case-2-proxy-with-service-virtualization) and [Use case 4: Endpoint with service virtualization](#use-case-4-endpoint-with-service-virtualization)).

{{< alert title="Note" color="primary" >}}The full URL of the destination web service should be specified in the **Rewrite URL** filter.{{< /alert >}}

The following policy illustrates the use of the **Redirect URL** filter to route messages to a fully qualified URL:

![Policy 5:Simple redirect](/Images/docbook/images/connection/simple_redirect.gif)

The following table describes the role of each filter in the policy.

| Filter             | Role in Policy                                                                                                                                                                                                                                                                         |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rewrite URL**    | Used to specify the fully qualified URL of the destination web service.                                                                                                                                                                                                                |
| **Dynamic Router** | In this case, the **Dynamic Router** filter is used to parse the URL specified in the **Rewrite URL** filter into its constituent parts. The HTTP scheme, port, and host of the web service are extracted and set to the internal message object for use by the **Connection** filter. |
| **Connection**     | Establishes the connection to the destination web service, and sends the message over this connection. This connection can be mutually authenticated if necessary.                                                                                                                     |

### Use case 6: Routing on to an HTTP proxy

This is a more advanced case where API Gateway is configured to route on through an HTTP proxy to the back-end web service sitting behind the proxy. When API Gateway is configured to route through a proxy, it connects directly to the proxy, and sends a request including the full URL of the target web service in the HTTP request URI. When the HTTP proxy receives this request, it uses the URL in the request line to determine where to route the message to. The following example shows the request line of a request made through a proxy:

```
POST http://localhost:8080/services/getQuote HTTP/1.1
```

The following filters are required to configure API Gateway to route through an HTTP proxy.

| Filter            | Role in Policy                                                                                                                                                                                                                                             |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Static Router** | You must explicitly specify the host, port, and scheme of the HTTP proxy.                                                                                                                                                                                  |
| **Rewrite URL**   | Enter the full URL of the web service (for example, `http://HOST:8080/myServices`). Because you are routing through a proxy, the full URL is sent in the request line of the HTTP request.                                                                 |
| **Connection**    | In this case, the **Connection** filter connects to the HTTP proxy, which in turn routes the message on to the destination server named in the request URI. The **Send via Proxy** option must be enabled in the **Connection** filter to facilitate this. |

There are differences between how the filters are configured to route on through a proxy and the scenario described in [Use case 4: Endpoint with service virtualization](#use-case-4-endpoint-with-service-virtualization) where no proxy is involved:

* **Static Router**: When API Gateway routes on to an HTTP proxy, the **Static Router**   filter is configured with the details of the HTTP proxy. Otherwise, the **Static Router**   filter is given the details of the web service endpoint directly.
* **Rewrite URL**: The full URL of the web service endpoint must be specified in this filter when API Gateway routes through a proxy. The full URL is then included in the request line of the HTTP request to the proxy. In cases where no proxy is involved, the **Rewrite URL**   filter is only necessary when the back-end web services are virtualized. In this case, API Gateway must send the request to a different URI than that requested by the client.
* **Connection**: When routing through a proxy, the **Send via Proxy**   option must be enabled in the **Connection**   filter. This is not necessary when no proxy sits between API Gateway and the back-end web service.

## Summary

The following are the key concepts to consider when configuring API Gateway to connect to external web services:

* The **Connection**   or **Connect to URL**   filter *must*   always be used because it establishes the connection to the web service.
* *Service virtualization*   can be achieved using the **Rewrite URL**   or **Connect to URL**   filter.
* If the client is configured to use API Gateway as a proxy, API Gateway can use the **Dynamic Router**   filter to extract the URL from the request line of the HTTP request. It can then route the message on to this URL.
* If the client sees API Gateway as the endpoint of the connection, not as a proxy, the **Static Router**   filter can be used to explicitly configure the host, port, and scheme of the destination web service. Alternatively, you can use the **Connect to URL**   filter to specify a URL.