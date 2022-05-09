{
"title": "Configure SMTP servers",
"linkTitle": "Configure SMTP servers",
"weight": 11,
"date": "2019-10-17",
"description": "Configure the API Gateway to use a specified SMTP server to relay messages to an email recipient."
}

You can configure the SMTP server as a global configuration item under the **Environment Configuration** > **External Connections** node. The SMTP server is then available for selection in the **SMTP** filter in the **Routing** category.

To configure an SMTP server, right-click the **Environment Configuration** > **External Connections > SMTP Servers** node, and select **Add an SMTP Server**. Alternatively, in the **SMTP** filter window, click the button beside the **SMTP Server Settings** field, right-click the **SMTP Servers** node, and select **Add an SMTP Server**.

## Configuration

Configure the following fields in the **SMTP Server settings** dialog:

* **Name**: Enter an appropriate name for the SMTP server.
* **SMTP Server Settings**: Specify the following fields:
    * **SMTP Server Hostname**: Enter the host name or IP address of the SMTP server.
    * **Port**: Enter the SMTP port on the specified server hostname. By default, SMTP servers run on port 25.
    * **Connection Security**: Select the security required for the connection to the SMTP server (`SSL`, `TLS`, or `NONE`). Defaults to `NONE`.
* **Log on using**: If you are required to authenticate to the SMTP server, specify the following fields:
    * **User Name**: Enter the user name of a registered user that can access the SMTP server.
    * **Password**: Enter the password for the user name specified.

{{< alert title="Note" color="primary" >}}The SMTP server connection supports a simple user name and password type of authentication. Microsoft Windows NT LAN Manager (NTLM) authentication is not supported.{{< /alert >}}

## Configure trusted certificates for SMTP connections

SMTP server connections use a Java keystore instead of the API Gateway certificate store. You must import the certificate chain into the following file:

```
<install-dir>\apigateway\<platform>\jre\lib\security\cacerts
```

The default password is `changeit`.
