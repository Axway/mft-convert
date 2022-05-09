{
"title": "Configure a file transfer service",
  "linkTitle": "Configure a file transfer service",
  "weight": 5,
  "date": "2019-10-17",
  "description": "Configure API Gateway to act as a file transfer service."
}
The API Gateway can act as a file transfer service that listens on a port for remote clients to connect to it. The API Gateway file transfer service supports the following protocols:

* **FTP**: File Transfer Protocol.
* **FTPS**: FTP over Secure Sockets Layer (SSL).
* **SFTP**: Secure Shell (SSH) File Transfer Protocol.

For all file transfer protocols, you must configure a file upload policy and an authentication policy. For FTP and FTPS, you must configure a password authentication policy. While for SFTP, you can configure a password authentication policy or a public key authentication policy. The API Gateway can also restrict access to the server based on IP address.

When a file transfer service is configured, users are presented with a personal file system view when they log in. The root of this file system is specified in a configurable request directory. Any files they upload are processed by the file upload policy. If this policy succeeds, the output of the policy is stored in a configurable response directory. If the policy fails, the original file is moved to a configurable quarantine directory.

Configuring a file transfer service can be useful when integrating with Business-to-Business (B2B) partner destinations or with legacy systems. For example, instead of making drastic changes to either system, the other system can upload files to the API Gateway. The added benefit is that the file transfer can be controlled and secured using API Gateway policies designed to suit system needs.

For details on how to use the API Gateway to poll a remote file server, to query and retrieve files to be processed, see [Configure an FTP poller](/docs/apim_policydev/apigw_gw_instances/general_ftp_scanner/).

## General settings

You can configure the following settings in the **General** section:

**Name**: Enter an appropriate name for the file transfer service.

**Service Type**: Select the file transfer protocol type for this service from the drop-down list (`ftp`, `ftps`, or `sftp`). Defaults to `ftp`.

**Implicit**: This setting applies to FTPS only. When selected, security is automatically enabled as soon as the remote client makes a connection to the file transfer service. No clear text is passed between the client and server at any time. In this case, the file transfer service defines a specific port for the remote client to use for secure connections (`990`). This setting is not selected by default.

**Explicit**: This setting applies to FTPS only. When selected, the remote client must explicitly request security from the file transfer server, and negotiate the required security. If the client does not request security, the file transfer server can allow the client to continue insecure or refuse and/or limit the connection. This setting is selected by default.

**Binding Address**: Enter a network interface to bind to. Defaults to `*`, which means bind to all available network interfaces on the host on which the API Gateway is installed. You can enter an IP address to bind to a specific network interface.

**Port**: Enter a file transfer port to listen on for remote clients to connect to. Defaults to `21`.

{{< alert title="Note" color="primary" >}}The API Gateway must execute with superuser privileges to bind to a port number less than `1024`
on Linux.{{< /alert >}}

## File upload settings

For all file transfer protocols, you must specify a **File Upload** policy to be invoked with the file data. This enables files and directories to be uploaded to a user subdirectory of the **Request Directory**.

For example, files uploaded by user `fred` are placed in `${environment.VINSTDIR}/file-transfer/in/persistent/fred`, where `VINSTDIR` is the location of the running API Gateway instance. The specified policy is then invoked with the raw file data. If this policy returns true, the output is placed in the corresponding **Response Directory**. If this policy returns false or an exception is thrown, the uploaded file is moved to the **Quarantine Directory**.

You can configure the following settings in the **File Upload** section:

**Request Directory**: Specifies the directory into which files and directories are uploaded. Defaults to `${environment.VINSTDIR}/file-transfer/in/`.

**Delete File on Successful Response**: Select whether to delete the uploaded files from the **Request Directory** when successfully processed. This setting is not selected by default.

**Response Directory**: Specifies the name of the directory in which files output by the API Gateway processing of uploaded files are placed if the **File Upload** policy returns true. Defaults to `out`. The original files remain in the **Request Directory**.

**Response Suffix**: Specifies the file name suffix that is appended to the output files in the **Response Directory**. Defaults to `.resp.${id}`, which enables a unique suffix to be appended to the files.

**Quarantine Directory**: Specifies the directory into which the uploaded files are moved if the **File Upload** policy returns false, or an exception is thrown. Defaults to `quarantine`.

{{< alert title="Note" color="primary" >}} The response and quarantine directories can be relative or absolute. Relative directories reside under the request directory. The user can manage the uploaded files using their file transfer session (for example, by accessing API Gateway file processing results). Absolute directories must reside outside of the request directory. The user cannot view or manage uploaded files using their file transfer session. Specifying absolute directories hides API Gateway file processing from the user.

In this way, the request directory can be seen as the user's home directory for the duration of the connection. Therefore, anything created under the same home directory is visible to the user. However, if the response is created outside this directory (for example, in `/tmp/response`), the files are not visible to the user. {{< /alert >}}

### Specify a file upload policy

You must specify a **File Upload** policy to be invoked with the raw file data. Perform the following steps:

1. Click the **Add** button to display the dialog.
2. In the **Pattern** field, select or enter a regular expression to match against the file name. For example, the following expression means that the configured policy is run against these files types only:

   ```
   ([^\s]+(\.(?i)(xml|xhtml|soap|wsdl|asmx))$)
   ```
3. In the **Policy** field, click the browse button on the right to select a policy, and click **OK**.
4. Click **OK** to display the configured pattern and policy in the table.

### Message attributes

The **File Upload** policy uses the following message attributes:

* `content.body`: Raw message file content.
* `file.src.name`: File name of the uploaded file.
* `file.src.path`: Full file path of the uploaded file.

## Secure services settings

On the **Secure Services** tab, you can configure the following **Client Authentication** policies:

**Password Authentication Policy**: For FTP and FTPS, you must configure a **Password Authentication Policy**. Click the browse button on the right to select a configured policy. This policy uses the `authentication.subject.id` and `authentication.subject.password` message attributes, which store the user name and password entered by the client user.

**Public Key Authentication Policy**: For SFTP, you can configure a **Public Key Authentication Policy** or **Password Authentication Policy**. Click the browse button on the right to select a configured policy. This policy uses the `authentication.subject.id` and `authentication.subject.public.key` message attributes, which store the user name and public key used by the client.

You can configure the following **Server Authentication** settings:

**Server Certificate**: For FTPS or SFTP, click the **Signing Key** button to specify a server certificate. You can select a certificate in the dialog, or click to create or import a certificate. The selected certificate must contain a private key. Alternatively, you can specify a certificate to bind to at runtime using an environment variable selector (for example, `${env.serverCertificate}`).

**Server Key Pair**: For SFTP, click the button on the right, and select a previously configured key pair that the file transfer service must present from the tree. To add a key pair, right-click the **Key Pairs** node, and select **Add**. Alternatively, you can import key pairs under the **Environment Configuration** > **Certificates and Keys** node in the Policy Studio tree.

## Command settings

For FTP and FTPS, the **Commands** tab enables you to specify commands that can be enabled for this service (other FTP and FTPS commands are enabled by default). The following commands are specified in the table:

* `DELE`: Allow user to delete files
* `PASV`: Support passive mode
* `REST`: Support restart mode
* `RMD`: Allow user to remove directories
* `STOU`: Support unique file name

To enable an existing command, click **Edit**, select **Enabled**, and click **OK**. The command is displayed as enabled in the table.

### Add new commands

 To add a new command, perform the following steps:

1. Click the **Add** button to display the dialog.
2. Enter the command **Name** (for example, `STAT`).
3. Select the file transfer protocol **Type** from the drop-down list (for example, `ftp` or `ftp(s)`).
4. Enter the command **Description**.
5. Select whether the command is **Enabled**.
6. Click **OK** to display the new command in the table.

### Supported commands

For a full list of supported commands, see <http://mina.apache.org/ftpserver-project/ftpserver_commands.html>.

## Access control settings

The **Access Control** tab enables you to restrict or block access to the file transfer service based on IP address. All IP addresses are allowed by default.

**Restrict Access to the following IP Ranges**: To restrict access to specified IP addresses, perform the following steps:

1. Click the **Add** button to display the dialog.
2. Enter an **IP Address** (for example, `192.168.0.16`).
3. Enter a **Net Mask** for the specified IP address. For example, if you wish to restrict access to the specified IP address only, enter `255.255.255.255`. Alternatively, you can restrict access to a range of IP addresses by entering a value such as `255.255.255.253`, which restricts access to `192.168.0.16`, `192.168.0.17`, and `192.168.0.18`.
4. Click **OK** to display the IP address details in the table.

**Block Access to the following IP Ranges**: To block access to specified IP addresses, perform the following steps:

1. Click the **Add** button to display the dialog.
2. Enter an **IP Address** (for example, `192.168.0.16`).
3. Enter a subnet **Mask** for the specified IP address. For example, if you wish to block access to the specified IP address only, enter `255.255.255.255`. Alternatively, you can block access to a range of IP addresses by entering a value such as `255.255.255.253`, which blocks access to `192.168.0.16`, `192.168.0.17`, and `192.168.0.18`.
4. Click **OK** to display the IP address details in the table.

## Message settings

For FTP and FTPS, the you can specify a welcome message and a goodbye message on the **Messages** tab. These are output to the user at the start and at the end of each session.

**Welcome Message**: Enter a short welcome text message for the file transfer service (for example, `Connected to FTP Test Service...`).

**Goodbye Message**: Enter a short goodbye text message for the file transfer service (for example, `Leaving FTP Test Service...`).

## Directory settings

You can configure the following settings on the **Directory** tab:

**Directory Type**: Select one of the following directory types:

* **Persistent**: This is a sharable directory created per user, which persists between connections (for example, `${environment.VINSTDIR}/file-transfer/in/persistent/fred`). This is the default directory type.
* **Transient**: This is an isolated directory created per connection, which is destroyed after the connection (for example, `${environment.VINSTDIR}/file-transfer/in/2/fred`).

{{< alert title="Note" color="primary" >}}Some clients, notably FileZilla, generate multiple client connection sessions. For these clients, files uploaded in one session are not available in the viewing session.{{< /alert >}}

**Directory expiry in seconds**: Specifies how long the directory remains on the system. Defaults to 3600 seconds (1 hour). A setting of 0 seconds means that the directory never expires.

**Directory expiry check period in seconds**: Specifies how often the API Gateway checks to see if a directory has expired. Defaults to 1800 seconds (30 minutes). A setting of 0 seconds means that it never checks.

## Logging settings

The **Logging Settings** tab enables you to configure the logging level for the file transfer service, and to configure when message payloads are logged. For details see [Relative path logging settings](/docs/apim_policydev/apigw_gw_instances/general_relative_path/#logging-settings).

**Include in real time monitoring** Select whether to monitor traffic for the file transfer service. This means that traffic for this service is monitored in the API Gateway Manager and API Gateway Analytics web-based consoles. This setting is selected by default.

## Traffic monitor settings

The **Traffic Monitor** tab enables you to configure traffic monitoring settings for the file transfer service. To override the system-level traffic monitoring settings, select **Override system-level settings**, and configure the relevant options.

## Configure passive transfer mode

In *active transfer mode*, the client actively supplies an IP address and port number to open the client connection, which is the default mode. In *passive transfer mode*, the client passively waits for the server to supply an IP address and port number for the client to create the connection. To use passive transfer mode, you must configure the passive transfer properties in the following file:

```
INSTALL_DIR/apigateway/system/conf/jvm.xml
```

The passive transfer properties include the port range, address, and external address, for example:

```
<SystemProperty name="ftpPassivePorts" value="10000-11000"/>
<SystemProperty name="ftpPassiveAddress" value="<internal-ip>"/>
<SystemProperty name="ftpPassiveExternalAddress" value="<external-ip>"/>
```

These properties are explained as follows:

`ftpPassivePorts`: Ports or range of ports on which the server accepts passive data connections (for example, `123`, or `123,124`, or `123-126`). `ftpPassiveAddress`: IP address on which the server listens for passive data connections. `ftpPassiveExternalAddress`: IP address on which the server claims to be listening on in the `PASV` reply. For example, this is useful when the server is behind a NAT firewall and the client sees a different server address.