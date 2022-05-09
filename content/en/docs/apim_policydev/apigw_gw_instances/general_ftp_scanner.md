{
"title": "Configure an FTP poller",
  "linkTitle": "Configure an FTP poller",
  "weight": 6,
  "date": "2019-10-17",
  "description": "Use API Gateway to poll a remote file server, to query and retrieve files to be processed."
}
The FTP poller enables you to query and retrieve files to be processed by polling a remote file server. When the files are retrieved, they can be passed to the API Gateway core message pipeline for processing. For example, this is useful where an external application drops files on to a remote file server, which can then be validated, modified, and potentially routed on over HTTP or JMS by the API Gateway.

This kind of protocol mediation is useful when integrating with Business-to-Business (B2B) partner destinations or with legacy systems. For example, instead of making drastic changes to either system, the API Gateway can download the files from a remote file server, and route them over HTTP to another back-end system. The added benefit is that messages are exposed to the full complement of API Gateway message processing filters. This ensures that only properly validated messages are routed on to the target system.

The FTP poller supports the following file transfer protocols:

* **FTP**: File Transfer Protocol.
* **FTPS**: FTP over Secure Sockets Layer (SSL).
* **SFTP**: Secure Shell (SSH) File Transfer Protocol.

To add a new FTP poller, in the Policy Studio tree, under the **Environment Configuration** > **Listeners** node, right-click the instance name (for example, `API Gateway`), and select **FTP Poller**

For details on how to configure the API Gateway to act as a file transfer service that listens on a port for remote clients, see [Configure a file transfer service](/docs/apim_policydev/apigw_gw_instances/general_file_transfer/).

## General settings

This filter includes the following general settings:

**Name**: Enter a descriptive name for this FTP poller.

**Enable Poller**: Select whether this FTP poller is enabled. This is selected by default.

**Host**: Enter the host name of the file transfer server to connect to.

**Port**: Enter the port on which to connect to the file transfer server. Defaults to `21`.

**User name**: Enter the user name to connect to the file transfer server.

**Password**: Specify the password for this user.

## Scan settings

The fields configured in the **Scan details** tab determine when to scan, where to scan, and what files to scan:

**Poll every (ms)**: Specifies how often in milliseconds the API Gateway scans the specified directory for new files. Defaults to `60000`. To optimize performance, it is good practice to poll often to prevent the number of files building up.

**Look in directory on FTP server**: Enter the path of the target directory on the FTP server to scan for new files. For example, `outfiles`.

**For files that match the pattern**: Specifies to scan only for files based on a pattern in a regular expression. For example, to scan only for files with a particular file extension (for example, `.xml`), enter an appropriate regular expression. Defaults to:

```
([^\s]+(\.(?i)(xml|xhtml|soap|wsdl|asmx))$)
```

**Establish new session for each file found**: Select whether to establish a new file transfer session for each file found. This is selected by default.

**Limit the number of files to be processed**: Select this option to limit the number of files that the FTP poller can will process on each poll of the FTP server. This option is not selected by default.

**Specify the max number of files to be processed**: Enter the maximum number of files to be processed on each poll of the FTP server. The default is `100`.

**Process file with following policy**: Click the browse button to select the policy to process each file with. For example, this policy may perform tasks such as validation, threat detection, content filtering, or routing over HTTP or JMS. You can select what action to take after the policy processes the file in the **On Policy Success** and **On Policy Failure** fields.

**On Policy Success**: This field enables you to choose the behavior if the policy passes. Select one of the following options:

* Do Nothing — Take no action.
* Delete File — Delete the file.
* Move File — Move the file to a new location. Enter the directory path in the **Move to directory on FTP server** field. This path is relative to the **Look in directory on FTP server** entered above. For example, if **Look in directory** is `outfiles` and **Move to directory** is `processed`, then files are moved to `outfiles/processed` on the FTP server. The **Move to directory** is created if it does not exist.

**On Policy Failure**: This field enables you to choose the behavior if the policy fails. Select one of the following options:

* Do Nothing — Take no action.
* Delete File — Delete the file.
* Move File — Move the file to a new location. Enter the directory path in the **Move to directory on FTP server** field. This path is relative to the **Look in directory on FTP server** entered above.

## Connection type settings

The fields configured in the **Connection Type** tab determine the type of file transfer connection. Select the connection type from the list:

* FTP — File Transfer Protocol
* FTPS — FTP over SSL
* SFTP — SSH File Transfer Protocol

### FTP and FTPS connections

The following general settings apply to FTP and FTPS connections:

**Passive transfer mode**: Select this option to prevent problems caused by opening outgoing ports in the firewall relative to the file transfer server (for example, when using *active* FTP connections). This is selected by default.

{{< alert title="Note" color="primary" >}}To use passive transfer mode, you must perform the steps described in [Configure passive transfer mode](/docs/apim_policydev/apigw_gw_instances//general_file_transfer#configure-passive-transfer-mode).{{< /alert >}}

**File Type**: Select **ASCII** mode for sending text-based data or **Binary** mode for sending binary data over the connection. Defaults to **ASCII** mode.

### FTPS connections

The following security settings apply to FTPS connections only:

**SSL Protocol**: Enter the SSL protocol used (for example, `SSL` or `TLS`). Defaults to `SSL`.

**Implicit**: When this option is selected, security is automatically enabled as soon as the FTP poller client makes a connection to the remote file transfer service. No clear text is passed between the client and server at any time. In this case, the client defines a specific port for the remote file transfer service to use for secure connections (`990`). This option is not selected by default.

**Explicit**: When this option is selected, the remote file transfer service must explicitly request security from the FTP poller client, and negotiate the required security. If the file transfer service does not request security, the client can allow the file transfer service to continue insecure or refuse and/or limit the connection. This option is selected by default.

**Trusted Certificates**: To connect to a remote file server over SSL, you must trust that server's SSL certificate. When you have imported this certificate into the Certificate Store, you can select it on the **Trusted Certificates** tab.

**Client Certificates**: If the remote file server requires the FTP poller client to present an SSL certificate to it during the SSL handshake for mutual authentication, you must select this certificate from the list on the **Client Certificates** tab. This certificate must have a private key associated with it that is also stored in the Certificate Store.

### SFTP connections

The following security settings apply to SFTP connections only:

**Present following key for authentication**: Click the button on the right, and select a previously configured key to be used for authentication from the tree. To add a key, right-click the **Key Pairs** node, and select **Add**. Alternatively, you can import key pairs under the **Environment Configuration** > **Certificates and Keys** node in the Policy Studio tree.

**SFTP server must present key with the following fingerprint**: Enter the fingerprint of the public key that the SFTP server must present, for example, `SHA-256` hash algorithm and `Zs5O+Y+gyl7pmq0hC68nz3M1ehZOTTn5Vyl3WbWERzE` fingerprint value.