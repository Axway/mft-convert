{
"title": "Configure SMTP services",
  "linkTitle": "Configure SMTP services",
  "weight": 4,
  "date": "2019-10-17",
  "description": "Configure API Gateway to receive email and to act as a mail relay."
}
The API Gateway provides support for Simple Mail Transfer Protocol (SMTP), which enables the API Gateway to receive email and to act as a mail relay. The API Gateway can accept incoming email messages using the SMTP protocol, and then forward them on to a configured mail server. You can also use Policy Studio to configure optional policies for specific SMTP commands (for example, `HELO/EHLO`, `AUTH`, `MAIL FROM`, and so on).

When an SMTP command is configured in Policy Studio, each time the SMTP command is accepted by the API Gateway, the appropriate policy is executed. When the policy completes successfully, the SMTP conversation resumes. This topic shows how to configure SMTP services, interfaces, and handler policies using Policy Studio.

## Add an SMTP service

To add an SMTP service to enable the API Gateway to accept SMTP connections, perform the following steps in Policy Studio:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select an API Gateway instance node (for example, the default **API Gateway**).
2. Right-click, and select **Add SMTP Services**.
3. In the **SMTP Services** dialog, specify a unique name for the SMTP service in the **Name** field.
4. In the **Outgoing Server Settings** section, complete the following settings:

   * **Host**: Host name or IP address of the remote mail server. This is the server to which the API Gateway forwards incoming SMTP commands (for example, `smtp.gmail.com`). You can also specify a mail server running locally on the same machine as the API Gateway using an address of `localhost`   or `127.0.0.1`.
   * **Port**: Port on which to connect to the remote mail server. Defaults to port `25`.
5. In the **Security** section, complete the following settings:

   * **Connection Security**: Select the type of security used for the connection to the remote mail server. Defaults to `None`. Other possible values are `SSL`   and `STARTTLS`.
   * **Trusted Certificates**: Use this tab to select the trusted CA certificates used in the security handshake for the connection to the remote mail server. This field is mandatory if SSL or STARTTLS connection security is selected.
   * **Client SSL Authentication**: Use this tab to specify the trusted client certificates used in the security handshake for the connection to the remote mail server. This field is optional if SSL or STARTTLS connection security is selected.
   * **Advanced**: Use this tab to specify a list of ciphers to use during the security handshake for the connection to the remote mail server. Defaults to `DEFAULT`. For more details, see the OpenSSL ciphers man page. This field is optional if SSL or STARTTLS connection security is selected.
6. In the **Authentication** section, complete the following settings:

   * **Username**: Specify the user name used to authenticate the API Gateway with a remote SMTP server using the `AUTH`   SMTP command.
   * **Password**: Specify the password used to authenticate the API Gateway with a remote SMTP server using the `AUTH`   SMTP command.
7. Select the **Include in real time monitoring** check box to monitor the SMTP services using the web-based API Gateway Manager monitoring console.
8. Click **OK**. This creates a tree node for the SMTP service under the selected instance in the **Services** tree.

## Add an SMTP interface

When you have configured the outbound SMTP protocol, you must then set up an inbound interface to accept client connections. You can choose from the following interface types:

* **TCP**: Non-secure connection. All traffic is sent in-the-clear.
* **SSL**: SSL handshake is performed at connection time, so the entire SMTP conversation is secure.
* **STARTTLS**: Initial connection is in the clear. The API Gateway advertises STARTTLS during the initial SMTP `HELO/EHLO` handshake. If the client supports this, it can send a STARTTLS command to the API Gateway, which in turn promotes connection security, and upgrades the connection to SSL/TLS.

Because the SSL and STARTTLS interface types have the potential to be secure (STARTTLS starts off non-secure, but can be upgraded during the SMTP conversation), a common configuration window is used for both protocols in Policy Studio.

To configure an inbound interface, perform the following steps in Policy Studio:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the SMTP node under the instance.
2. Right-click, and select **Add Interface** > interface type (**TCP**, **SSL**, or **STARTTLS**).
3. Complete the settings on the relevant dialog. For full details on these settings, see [Configure HTTP services](/docs/apim_policydev/apigw_gw_instances/general_services/).
4. Click **OK**.

## Configure policy handlers for SMTP commands

You can use Policy Studio to configure optional policy handlers for each of the following SMTP commands:

* `HELO/EHLO`
* `AUTH`
* `MAIL FROM`
* `RCPT TO`
* `DATA`

The next sections explain how to configure policy handlers for each command.

### Add an HELO/EHLO policy handler

The **HELO/EHLO** policy handler is invoked when a `HELO/EHLO` SMTP command is received from a client. This handler enables you to modify the `HELO/EHLO` greeting and the client domain. You can configure the greeting message sent back to the client from the API Gateway during the `HELO/EHLO` handshake as required. You can also configure a policy to replace the value of `smtp.helo.greeting`. The domain specified by the connected client in the `HELO/EHLO` command can be modified before forwarding on to the remote mail server. You can also configure a policy to replace the value of `smtp.helo.domain`.

To configure a policy handler for the `HELO/EHLO` command, perform the following steps:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the SMTP node under the instance.
2. Right-click, and select **Add Policy Handler** > **HELO/EHLO**.
3. In the **Configure HELO Host** dialog, specify the **Greeting** to be sent back to the client as part of the `HELO/EHLO` handshake. Defaults to `Hello ${smtp.helo.domain}`.
4. In the **Policy** tree, select the policy that you wish to handle the `HELO/EHLO` command.
5. Click **OK**.

**Message attributes**: The following message attributes are generated during processing:

| Message Attribute       | Description                                                                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `smtp.helo.domain`      | The client domain specified in the `HELO/EHLO` SMTP command received from the client.                                                        |
| `smtp.helo.greeting`    | The HELO greeting to be sent back to the client after `HELO/EHLO` processing is performed. The default value is `Hello ${smtp.helo.domain}`. |
| `message.source`        | The inbound port on which SMTP traffic is received by the API Gateway.                                                                       |
| `message.protocol.type` | The protocol used for the connection. This can be `smtp-tcp` or `smtp-ssl`.                                                                  |
| `monitoring.enabled`    | Set to `true` if monitoring is enabled for the protocol, otherwise `false`.                                                                  |

### Add an AUTH policy handler

The **AUTH** policy handler is invoked when an `AUTH` SMTP command is received from a client. You can use the **AUTH** handler to run a policy to perform user authentication checks. For example, during the Authentication phase of the SMTP conversation, the client-supplied username and password can be verified against an Authentication Repository using a policy containing an **Attribute Authentication** filter.

To configure a policy handler for the `AUTH` command, perform the following steps:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the **SMTP** node under the instance.
2. Right-click, and select **Add Policy Handler** > **AUTH**.
3. In the **Configure AUTH** dialog, in the **Policy** tree, select the policy that you wish to handle the `AUTH` command.
4. Click **OK**.

**Message attributes**: The following message attributes are generated during processing:

| Message Attribute                 | Description                                                                 |
| --------------------------------- | --------------------------------------------------------------------------- |
| `authentication.subject.id`       | The user name supplied by the client.                                       |
| `authentication.subject.password` | The password supplied by the client.                                        |
| `message.source`                  | The inbound port on which SMTP traffic is received by the API Gateway.      |
| `message.protocol.type`           | The protocol used for the connection. This can be `smtp-tcp` or `smtp-ssl`. |
| `monitoring.enabled`              | Set to `true` if monitoring is enabled for the protocol, otherwise `false`. |

### Add a MAIL policy handler

The **MAIL** policy handler is invoked when a `MAIL FROM` SMTP command is received from a client. Emails can be rejected based on wildcard matching of the supplied sender address in the `MAIL FROM` SMTP command. For example, email addresses containing `GMAIL.COM` (`fromAddress` of `*@gmail.com`) as the domain could be accepted using a simple **True** filter. Whereas, email addresses containing `YAHOO.COM` (`fromAddress` of `*@yahoo.com`) could be rejected using a simple **False** filter.

To configure a policy handler for the `MAIL FROM` command, perform the following steps:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the SMTP node under the instance.
2. Right-click, and select **Add Policy Handler** > **MAIL**.
3. In the **Configure MAIL Address** dialog, you must specify the **From Address**. This is an email address used to filter addresses specified in the `MAIL FROM` SMTP command. You can specify this as a wildcard. The following are some example values:

   * `*`: Runs the policy for any email address received.
   * `*@gmail.com`: Runs the policy for all email addresses with the `gmail.com` domain.
   * `S*@axway.*`: Runs the policy for all email addresses with any `axway` domain, and beginning with the letter `s`.
4. The policy selection is performed on a best-match basis.
5. In the **Policy** tree, select the policy that you wish to handle the `MAIL FROM` command.
6. Click **OK**.

You can configure multiple **MAIL** handlers so that different policies are executed, depending on the received mail address.

**Message attributes** The following message attributes are generated during processing:

| Message Attribute       | Description                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------- |
| `smtp.helo.domain`      | The client domain specified in the `HELO/EHLO` SMTP command received from the client. |
| `smtp.mail.from`        | The email address specified in the `MAIL FROM` SMTP command received from the client. |
| `message.source`        | The inbound port on which SMTP traffic is received by the API Gateway.                |
| `message.protocol.type` | The protocol used for the connection. This can be `smtp-tcp` or `smtp-ssl`.           |
| `monitoring.enabled`    | Set to `true` if monitoring is enabled for the protocol, otherwise `false`.           |

### Add a RCPT policy handler

The **RCPT** policy handler is invoked when a `RCPT TO` SMTP command is received from a client. You can use this handler to filter addresses specified in the `RCPT TO` SMTP command. Recipients can be rejected based on wildcard matching of the supplied recipient address in the `RCPT` SMTP command.

For example, recipient addresses containing `GMAIL.COM` (`toAddress` of `*@gmail.com`) as the domain could be accepted using a simple **True** filter. Whereas, addresses containing `YAHOO.COM` (`toAddress` of `*@yahoo.com`) could be rejected using a simple **False** filter. You can configure multiple **RCPT** handlers so that different policies are executed, depending on the received email address.

To configure a policy handler for the `RCPT TO` command, perform the following steps:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the SMTP node under the instance.
2. Right-click, and select **Add Policy Handler** > **RCPT**.
3. In the **Configure Recipient Address** dialog, you must specify the **To Address**. This is an email address used to filter addresses specified in the `RCPT TO` SMTP command. You can specify this as a wildcard. The following are some example values:

   * `*`: Runs the policy for any email address received.
   * `*@axway.com`: Runs the policy for all email addresses with the `axway.com` domain.
   * `d*@yahoo.*`: Runs the policy for all email addresses with any `yahoo` domain, and beginning with the letter `d`.
4. The policy selection is performed on a best-match basis.
5. In the **Policy** tree, select the policy that you wish to handle the `MAIL FROM` command.
6. Click **OK**.

**Message attributes**: The following message attributes are generated during processing:

| Message Attribute       | Description                                                                                                                                                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `smtp.helo.domain`      | The client domain specified in the `HELO/EHLO` SMTP command received from the client.                                                                                                                                                |
| `smtp.mail.from`        | The email address specified in the `MAIL FROM` SMTP command received from the client.                                                                                                                                                |
| `smtp.rcpt.to`          | The email address specified in the `RCPT TO` SMTP command received from the client. This is the current recipient being processed, whereas `smtp.rcpt.recipients` is the list of recipients processed so far.                        |
| `smtp.rcpt.recipients`  | The list (collection of strings) of recipients (email addresses) received or processed *so far* by the SMTP transaction. This is read-only and updated by the API Gateway each time it receives a `RCPT TO` command from the client. |
| `message.source`        | The inbound port on which SMTP traffic is received by the API Gateway.                                                                                                                                                               |
| `message.protocol.type` | The protocol used for the connection. This can be `smtp-tcp` or `smtp-ssl`.                                                                                                                                                          |
| `monitoring.enabled`    | Set to `true` if monitoring is enabled for the protocol, otherwise `false`.                                                                                                                                                          |

### Add a DATA policy handler

The **DATA** policy handler is invoked when a `DATA` SMTP command is received from a client. For example, for emails that contain SOAP/XML content, you can add an XML signature to the XML data, stored in the `content.body` message attribute, using an **XML Signature Generation** filter. For emails containing attachments, the attached mail data can be run through one of the API Gateway anti-virus filters.

Alternatively, you can use **SMIME Encrypt** or **SMIME Decrypt** filters to encrypt or decrypt emails (including attachments) passing through the API Gateway. You can also digitally sign emails using an **SMIME Sign** filter, or verify signatures on already digitally signed emails using an **SMIME Verify** filter.

To configure a policy handler for the `DATA` command, perform the following steps:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select the **SMTP** node under the instance.
2. Right-click, and select **Add Policy Handler** > **DATA**.
3. In the **Policy** tree, select the policy that you wish to handle the `DATA` command.
4. Click **OK**.

**Message attributes**: The following message attributes are added during processing:

| Message Attribute       | Description                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| `smtp.helo.domain`      | The client domain specified in the `HELO/EHLO` SMTP command received from the client.                    |
| `smtp.mail.from`        | The email address specified in the `MAIL FROM` SMTP command received from the client.                    |
| `smtp.rcpt.recipients`  | The full list (collection of strings) of recipients (email addresses) processed by the SMTP transaction. |
| `content.body`          | The stream representing the body of the mail. The `content.body` does not include MIME headers.          |
| `message.source`        | The inbound port on which SMTP traffic is received by the API Gateway.                                   |
| `message.protocol.type` | The protocol used for the connection. This can be `smtp-tcp` or `smtp-ssl`.                              |
| `monitoring.enabled`    | Set to `true` if monitoring is enabled for the protocol, otherwise `false`.                              |

## SMTP authentication

The SMTP protocol supports Extended SMTP (ESMTP) PLAIN authentication. The following matrix shows the possible authentication scenarios and actions based on the SMTP Services configuration:

| Scenario | AUTH handler | AUTH user name and password | Mail server advertises AUTH | API Gateway advertises AUTH | Proxy client AUTH | Authenticate API Gateway to server |
| -------- | ------------ | --------------------------- | --------------------------- | --------------------------- | ----------------- | ---------------------------------- |
| 1        | No           | No                          | No                          | No                          | No                | No                                 |
| 2        | No           | No                          | Yes                         | Yes                         | Yes               | No                                 |
| 3        | No           | Yes                         | No                          | No                          | No                | No                                 |
| 4        | No           | Yes                         | Yes                         | No                          | No                | Yes                                |
| 5        | Yes          | No                          | No                          | Yes                         | No                | No                                 |
| 6        | Yes          | No                          | Yes                         | Yes                         | No                | No                                 |
| 7        | Yes          | Yes                         | No                          | Yes                         | No                | No                                 |
| 8        | Yes          | Yes                         | Yes                         | Yes                         | No                | Yes                                |

These authentication scenarios are described as follows:

1. No authentication user name and password are specified so the API Gateway does not attempt to authenticate with the server. The server does not support authentication anyway. The mail server does not advertise authentication so the API Gateway does not advertise AUTH to the client. The client authentication is not proxied because the server does not support it.
2. No authentication user name and password are specified so the API Gateway does not attempt to authenticate with the server. The server does not support authentication anyway. The mail server advertises AUTH, so the API Gateway advertises AUTH to the client. No AUTH handler is configured, so the client authentication details are proxied to the server.
3. Same as 1 above.
4. The authentication user name and password are specified so the API Gateway authenticates with the server. The mail server advertises AUTH, but because a user name and password are specified, the API Gateway does not advertise AUTH to the client because the API Gateway authenticates with the server using the configured credentials. This also implies no client authentication proxying.
5. No authentication user name and password are specified so the API Gateway does not attempt to authenticate with the server. The server does not support authentication anyway. AUTH handler configured, which implies the API Gateway performs authentication, so advertise AUTH to the client.
6. Same as 5 above.
7. AUTH handler configured, which implies the API Gateway performs authentication, so advertise AUTH to the client. No proxying occurs because the API Gateway performs the authentication. No authentication is performed with the server because the server does not support it.
8. AUTH advertised to the client because the API Gateway performs authentication (and the mail server supports it). AUTH handler configured, which implies the API Gateway performs authentication. No proxying occurs because the API Gateway performs the authentication. Authentication is performed with the server because the server supports AUTH and a user name and password is configured.

## SMTP Content-Transfer-Encoding

The SMTP protocol supports automatic Content-Transfer-Encoding/Decoding. For `DATA` SMTP commands, the content of the incoming mail body may be encoded. To enable policy filters to view and/or manipulate the raw body data, the contents are automatically decoded before policy execution, and re-encoded afterwards (before being forwarded on to the configured outbound mail server).

**Supported encodings**: The following encodings are supported:

* Base64
* 7-bit
* 8-bit
* quoted-printable
* binary

However, Base64 is the only encoding that results in decoding/re-encoding of the mail data.

Multipart MIME content, generally used for sending attachments in SMTP, is also supported. Each separate body in the multipart is checked for a Content-Transfer-Encoding, and the decoding/re-encoding is performed as appropriate.

## Deployment example

This section provides a step-by-step example of how to configure and deploy SMTP services using the API Gateway. In this example, the API Gateway acts as a relay between a Thunderbird email client and the Google Gmail service.

### Configure the API Gateway SMTP services

 The API Gateway connects to the Gmail STARTTLS interface, which is available at `smtp.gmail.com`, and listening on port `587`.To configure the SMTP Services, perform the following steps in Policy Studio:

1. Under the **Environment Configuration** > **Listeners** node in the tree, select a Process node (for example, the default **API Gateway**).
2. Right-click, and select **Add SMTP Services**.
3. Enter `smtp.gmail.com` for the **Host**.
4. Enter `587` for the **Port**.
5. Select `STARTTLS` from the **Connection Security** drop-down list. This is selected because `smtp.gmail.com:587` exposes the Gmail STARTTLS SMTP interface.
6. Because STARTTLS has the potential to be upgraded to a secure connection, you must also select some **Trusted Certificates**.
7. Accept all other defaults, and click **OK** to add the SMTP services.

### Configure the SMTP client interface

 To configure a STARTTLS client interface, perform the following steps in Policy Studio:

1. Right-click the **SMTP** Services node, and select **Add Interface** > **STARTTLS**.
2. Enter a **Port** (for example, `8026`). This is the port on which the API Gateway’s incoming SMTP traffic is accepted. You can enter any port that is not already in use.
3. Because STARTTLS has the potential to be upgraded to a secure connection, you must configure a trusted certificate. Click the **X.509 Certificate** button.
4. Select a certificate in the **Select Certificate** dialog.
5. Click **OK** to return to the **Configure STARTTLS Interface** dialog.
6. When the certificate has been configured, accept all other defaults, and click **OK** to add the incoming STARTTLS interface.

When the SMTP services and STARTTLS client interface have been configured, you must deploy the changes to the API Gateway.

### Configure Thunderbird client settings

This example uses Thunderbird as the email client. However, you can use any standard email client that supports SMTP. Thunderbird is available as a free download from <http://www.mozillamessaging.com/>.

To configure a STARTTLS outgoing server in your Thunderbird client, perform the following steps:

1. Launch the Thunderbird email client.
2. From the main menu, select **Tools** > **Account Settings**.
3. Expand the **Local Folders** tree node in the left pane.
4. Select the **Outgoing Server** node to create a new outgoing server configuration.
5. Click **Add** to display the **SMTP Server** dialog.
6. Enter `Axway API Gateway [STARTTLS]` in the **Description** field.
7. Enter `localhost` (or the IP Address of the machine on which the API Gateway service is running) in the **Server Name** field.
8. Enter `8026` in the **Port** field. This sends SMTP traffic to the STARTTLS interface configured above, so the ports must match.
9. Select `STARTTLS`     from the **Connection security**     drop-down list. Traffic on this connection may be upgraded to secure during the SMTP conversation.
   10.Select `Normal Password`     from the **Authentication method**     drop-down list. This indicates that Authentication is to be performed.
   11.Enter a valid Gmail user-name for the **User Name**. 12.Click **OK**     to add the new outgoing server configuration.

### Configure certificates in Thunderbird

To enable Thunderbird to successfully negotiate the STARTTLS conversation with the API Gateway, you must import a CA certificate into Thunderbird. This is also because a certificate was already generated and imported into the API Gateway when configuring its STARTTLS client interface.

To configure a STARTTLS outgoing server in your Thunderbird client, perform the following steps:

1. From the Thunderbird main menu, select **Tools** > **Options**.
2. Select the **Certificates** tab.
3. Click the **View Certificates** button, to display the **Certificate Manager** dialog.
4. Click **Import**, and import the appropriate CA certificate.
5. Click **OK** when finished.

### Test the STARTTLS client interface

 To test the STARTTLS client interface using Thunderbird, perform the following steps:

1. Launch the Thunderbird email client, and create a new mail message.
2. Enter a valid Gmail address in the **To** field.
3. Enter `API Gateway Test` as the **Subject**.
4. Enter `This mail has been sent using Axway API Gateway` in the mail body.
5. To specify the appropriate outgoing mail server, select **Tools** > **Account Settings** from the main menu.
6. Select `Axway API Gateway [STARTTLS] - localhost` from the **Outgoing Server** drop-down list.
7. Click **OK**.
8. Send the mail.

The following is an example from the API Gateway trace showing the SMTP commands that occur.

```
DEBUG 14:46:46:546 [14b4] incoming call on interface *:8026 from 127.0.0.1:1487
DEBUG 14:46:46:546 [14b4] new connection 08133248, settings source incoming interface
(force 1.0=no, idleTimeout=60000, activeTimeout=60000)
DATA 14:46:46:546 [14b4] snd 0018: <220 doejAxway>
DATA 14:46:46:562 [14b4] rcv 18: <EHLO [127.0.0.1]>
DEBUG 14:46:46:562 [14b4] 080BE260: new connection cache set SMTP Client
DEBUG 14:46:46:562 [159c] idle connection monitor thread running
DEBUG 14:46:46:562 [14b4] new endpoint smtp.gmail.com:587
DEBUG 14:46:46:640 [14b4] Resolved smtp.gmail.com:587 to:
DEBUG 14:46:46:640 [14b4] 209.85.227.109:587
DEBUG 14:46:46:718 [14b4] connected to 209.85.227.109:587
DEBUG 14:46:46:718 [14b4] new connection 08135BA0, settings source service-wide
defaults (force 1.0=no, idleTimeout=15000, activeTimeout=30000)
DATA 14:46:46:765 [14b4] rcv 44: <220 mx.google.com ESMTP v11sm7979387weq.40>
DATA 14:46:46:765 [14b4] snd 0018: <ehlo [127.0.0.1]>
DATA 14:46:46:812 [14b4] rcv 125: <250-mx.google.com at your service, [87.198.245.194]
250-SIZE 35651584
250-8BITMIME
250-STARTTLS
250 ENHANCEDSTATUSCODES>
DATA 14:46:46:812 [14b4] snd 0010: <starttls>
DATA 14:46:46:843 [14b4] rcv 30: <220 2.0.0 Ready to start TLS>
DEBUG 14:46:46:843 [14b4] push SSL protocol on to connection
DEBUG 14:46:46:906 [14b4] No SSL host name provided: using default certificate for
interface
DEBUG 14:46:46:906 [14b4] verifyCert: preverify=1, depth=2, subject /C=US/O=Equifax/
OU=Equifax Secure Certificate Authority, issuer /C=US/O=Equifax/OU=Equifax Secure
Certificate Authority
DEBUG 14:46:46:906 [14b4] ca cert? 1
DEBUG 14:46:46:906 [14b4] verifyCert: preverify=1, depth=1, subject /O=Google
Inc/CN=Google Internet Authority, issuer /C=US/O=Equifax/OU=Equifax Secure Certificate
Authority
DEBUG 14:46:46:906 [14b4] verifyCert: preverify=1, depth=0, subject
/C=US/ST=California/L=Mountain View/O=Google Inc/CN=smtp.gmail.com,
issuer /C=US/O=Google Inc/CN=Google Internet Authority
DEBUG 14:46:46:952 [14b4] negotiated SSL cipher "RC4-MD5",session 00000000 (not reused)
DATA 14:46:46:952 [14b4] snd 0018: <ehlo [127.0.0.1]>
DATA 14:46:46:999 [14b4] rcv 140: <250-mx.google.com at your service, [87.198.245.194]
250-SIZE 35651584
250-8BITMIME
250-AUTH LOGIN PLAIN XOAUTH
250 ENHANCEDSTATUSCODES>
DATA 14:46:46:999 [14b4] snd 0109: <250-AxwayAPI Gateway Hello [127.0.0.1]
250-SIZE 35651584
250-8BITMIME
250-STARTTLS
250 ENHANCEDSTATUSCODES>
DEBUG 14:46:46:999 [14b4] delete transaction 0B95D2C0 on connection 08133248
DATA 14:46:46:999 [14b4] rcv 10: <STARTTLS>
DATA 14:46:46:999 [14b4] snd 0014: <220 Go ahead>
DEBUG 14:46:46:999 [14b4] push SSL protocol on to connection
DEBUG 14:46:46:999 [14b4] Servername CB: SSL host name: localhost, not in host map -
using default certificate for interface
DEBUG 14:46:47:031 [14b4] negotiated SSL cipher "AES256-SHA", session 00000000
(not reused)
DATA 14:46:47:031 [14b4] rcv 18: <EHLO [127.0.0.1]>
DATA 14:46:47:031 [14b4] snd 0018: <ehlo [127.0.0.1]>
DATA 14:46:47:077 [14b4] rcv 140: <250-mx.google.com at your service, [87.198.245.194]
250-SIZE 35651584
250-8BITMIME
250-AUTH LOGIN PLAIN XOAUTH
250 ENHANCEDSTATUSCODES>
DATA 14:46:47:077 [14b4] snd 0124: <250-AxwayAPI Gateway Hello [127.0.0.1]
250-SIZE 35651584
250-8BITMIME
250-AUTH LOGIN PLAIN XOAUTH
250 ENHANCEDSTATUSCODES>
DEBUG 14:46:47:077 [14b4] delete transaction 0B95D2C0 on connection 08133248
DATA 14:46:47:077 [14b4] rcv 41: <AUTH PLAIN ADGzaHllaDe0SHF1ex2r82Su555=>
DATA 14:46:47:077 [14b4] snd 0041: <auth PLAIN ADGzaHllaDe0SHF1ex2r82Su555=>
DATA 14:46:47:718 [14b4] rcv 20: <235 2.7.0 Accepted>
DATA 14:46:47:718 [14b4] snd 0020: <235 2.7.0 Accepted>
DATA 14:46:47:718 [14b4] rcv 45: <MAIL FROM:<john.doe@axway.com> SIZE=444>
DATA 14:46:47:718 [14b4] snd 0036: <mail from:<john.doe@axway.com>>
DATA 14:46:47:765 [14b4] rcv 33: <250 2.1.0 OK v11sm7979387weq.40>
DATA 14:46:47:765 [14b4] snd 0033: <250 2.1.0 OK v11sm7979387weq.40>
DATA 14:46:47:765 [14b4] rcv 30: <RCPT TO:<test@gmail.com>>
DATA 14:46:47:765 [14b4] snd 0030: <rcpt to:<test@gmail.com>>
DATA 14:46:47:812 [14b4] rcv 33: <250 2.1.5 OK v11sm7979387weq.40>
DATA 14:46:47:812 [14b4] snd 0033: <250 2.1.5 OK v11sm7979387weq.40>
DATA 14:46:47:812 [14b4] rcv 6: <DATA>
DATA 14:46:47:812 [14b4] snd 0006: <data>
DATA 14:46:48:609 [14b4] rcv 34: <354 Go ahead v11sm7979387weq.40>
DATA 14:46:48:609 [14b4] snd 0008: <354 OK>
DATA 14:46:48:609 [14b4] rcv 447: <Message-ID: <4CB85B46.4060205@axway.com>
Date: Fri, 15 Oct 2010 14:46:46 +0100
From: John Doe <john.doe@axway.com>
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-GB; rv:1.9.2.9) Gecko/20100915
Thunderbird/3.1.4
MIME-Version: 1.0
To: test@gmail.com
Subject: API Gateway Test
Content-Type: text/plain; charset=ISO-8859-1; format=flowed
Content-Transfer-Encoding: 7bit
This mail has been sent via an Axway API Gateway
.>
DATA 14:46:48:609 [14b4] snd 0442: <Message-ID: <4CB85B46.4060205@axway.com>
Date: Fri, 15 Oct 2010 14:46:46 +0100
From: John Doe <john.doe@axway.com>
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-GB; rv:1.9.2.9) Gecko/20100915
Thunderbird/3.1.4
MIME-Version: 1.0
To: test@gmail.com
Subject: API Gateway Test
Content-Type: text/plain; charset=ISO-8859-1; format=flowed
Content-Transfer-Encoding: 7bit
This mail has been sent via an Axway API Gateway>
DATA 14:46:48:609 [14b4] snd 0005: <.>
DATA 14:46:49:874 [14b4] rcv 44: <250 2.0.0 OK 1287150409 v11sm7979387weq.40>
DATA 14:46:49:874 [14b4] snd 0044: <250 2.0.0 OK 1287150409 v11sm7979387weq.40>
DEBUG 14:46:49:874 [14b4] delete transaction 0B95D2C0 on connection 08133248
DATA 14:46:49:874 [14b4] rcv 6: <QUIT>
DATA 14:46:49:874 [14b4] snd 0006: <quit>
DATA 14:46:49:921 [14b4] rcv 49: <221 2.0.0 closing connection v11sm7979387weq.40>
DEBUG 14:46:49:921 [14b4] delete transaction 08040BD8 on connection 08135BA0
DATA 14:46:49:921 [14b4] snd 0049: <221 2.0.0 closing connection v11sm7979387weq.40>
DEBUG 14:46:49:921 [14b4] delete transaction 0B95D2C0 on connection 08133248
DEBUG 14:46:49:921 [14b4] delete connection 08133248, current transaction 00000000
```