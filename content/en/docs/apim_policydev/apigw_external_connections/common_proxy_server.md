{
"title": "Configure proxy servers",
"linkTitle": "Configure proxy servers",
"weight": 7,
"date": "2019-10-17",
"description": "Configure settings for individual proxy servers, which you can then specify at the filter level."
}

You can configure settings for individual proxy servers under the **Environment Configuration** > **External Connections**
node in the Policy Studio tree, which you can then specify at the filter level (in the **Connection**
and **Connect To URL**
filters). When configured, the filter connects to the HTTP proxy server, which in turn routes the message on to the destination server named in the request URI.

{{< alert title="Note" color="primary" >}}API Gateway does not support persistent SSL connections to back-end servers using proxy tunneling. The API Gateway connection caching mechanism is not designed for proxy tunnel connections.{{< /alert >}}

## Configuration

To configure a proxy server under the **Environment Configuration** > **External Connections**
tree node, right-click the **Proxy Servers**
node, and select **Add a Proxy Server**. You can configure the following settings in the dialog:

| Proxy Server Setting | Description                                                                            |
|----------------------|----------------------------------------------------------------------------------------|
| **Name**             | Unique name or alias for these proxy server settings.                                  |
| **Host**             | Host name or IP address of the proxy server.                                           |
| **Port**             | Port number on which to connect to the proxy server.                                   |
| **Username**         | Optional user name when connecting to the proxy server.                                |
| **Password**         | Optional password when connecting to the proxy server.                                 |
| **Scheme**           | Specifies whether the proxy server uses the HTTP or HTTPS transport. Defaults to HTTP. |

{{< alert title="Tip" color="primary" >}}These proxy server settings are different from the global proxy settings in the **Preferences**
dialog in Policy Studio, which apply only when downloading WSDL, XSD, and XSLT files from Policy Studio. For more details, see [Proxy settings](/docs/apim_policydev/apigw_poldev/general_ps_settings/#proxy-settings).{{< /alert >}}
