{
"title": "Configure ICAP servers",
"linkTitle": "Configure ICAP servers",
"weight": 4,
"date": "2019-10-17",
"description": "Configure API Gateway to connect to an ICAP server."
}

The Internet Content Adaptation Protocol (ICAP) is a lightweight HTTP-based protocol used to optimize proxy servers, which frees up resources and standardizes how features are implemented. For example, ICAP is typically used to implement features such as virus scanning, content filtering, ad insertion, or language translation in the HTTP proxy server cache. *Content Adaptation*
refers to performing a specific value-added service (for example, virus scanning) for a specific client request and/or response.

You can configure ICAP servers under the **Environment Configuration** > **External Connections**
tree node, which you can then specify in an **ICAP**
filter later. To configure an ICAP server, right-click the **ICAP Servers**
node, and select **Add an ICAP Server**
to display the **ICAP Server Settings**
dialog.

## Server settings

Configure the following settings on the **Server**
tab:

**Host**: The machine name or IP address of the remote ICAP host. Defaults to the localhost (`127.0.0.1`).

**Port**: The port on which the ICAP server is listening. Defaults to `1344`.

**Request Service**: The path to the service (exposed by the ICAP server) that handles Request Modification (REQMOD) requests. The default value is `/request`.

**Response Service**: The path to the service exposed by the ICAP server that handles Response Modification (RESPMOD) requests. The default value is `/response`.

**Options Service**: The path to the service (exposed by the ICAP server) that handles OPTIONS requests. OPTIONS requests enable server capabilities to be queried. The default value is `/options`.

## Security settings

The following settings on the **Security**
tab enable you to secure the connection to the ICAP server:

**Trusted Certificates**:
When the API Gateway connects to the ICAP server over SSL, it must decide whether to trust the ICAP server's SSL certificate. You can select a list of CA or server certificates on the **Trusted Certificates**
tab that are considered trusted by API Gateway when connecting to the ICAP server. The table displayed on the **Trusted Certificates**
tab lists all certificates imported into the API Gateway Certificate Store. To *trust*
a certificate for this particular connection, select the box next to the certificate in the table.

**Client SSL Authentication**:
In cases where the destination ICAP server requires clients to authenticate to it using an SSL certificate, you must select a client certificate on the **Client SSL Authentication**
tab. Select the check box next to the client certificate that you want to use to authenticate to the ICAP server.

**Advanced**:
The **Ciphers**
field enables you to specify the ciphers that API Gateway supports. API Gateway sends this list of supported ciphers to the destination ICAP server, which then selects the highest strength common cipher as part of the SSL handshake.
The selected cipher is used to encrypt the data when it is sent over the secure channel.
The default value is `FIPS:!SSLv3:!aNULL`.

## Advanced settings

Select one of the following ICAP server modes on the **Advanced**
tab:

**Request Modification Mode (REQMOD)**: Specifies that the **ICAP**
filter in the API Gateway sends a Request Modification (REQMOD) request to the ICAP server. The ICAP server returns a modified version of the request, an HTTP response, or a 204 response code indicating that no modification is required. This mode is selected by default.

**Response Modification Mode (RESPMOD)**: Specifies that the **ICAP**
filter in the API Gateway sends a Response Modification (RESPMOD) request to the ICAP server. For example, the API Gateway sends an origin server's HTTP response to the ICAP server. The response from the ICAP server can be an adapted HTTP response, an error, or a 204 response code indicating that no adaptation is required.

### Custom headers

The **ICAP** filter supports custom headers. Custom header support allows for integration with additional ICAP servers, for example, OPSWAT.

You can configure custom headers in the **Advanced** tab for the above **REQMOD** and **RESPMOD** options.
When the **ICAP** filter is executed, the specified custom headers are added to outgoing **REQMOD** and **RESPMOD** requests.

Custom headers override default headers with the same name, so if a custom header has the same name as a header that is automatically added by the filter implementation, only the custom header value is sent.
