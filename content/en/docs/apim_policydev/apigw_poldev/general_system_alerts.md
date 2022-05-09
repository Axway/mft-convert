{
"title": "Configure system alerts",
"linkTitle": "Configure system alerts",
"weight": 11,
"date": "2019-10-17",
"description": "Configure alert destinations for API Gateway system alerts."
}

This topic describes how to configure API Gateway system alerts. System alerts and events are usually sent when a filter fails, but you can also use the alerts for notification purposes. API Gateway can send system alerts to several alert destinations, including a Windows Event Log, UNIX/Linux syslog, SNMP Network Management System, Check Point Firewall-1, email recipient, Amazon Simple Notification Service (SNS), or Twitter.

## Configure an alert destination

The first step in configuring API Gateway to send alerts is to configure an *alert destination*. This section describes the destinations to which the API Gateway can send alerts to. To configure the alert destinations, go to **Environment Configuration > Libraries > Alerts** node in the Policy Studio tree.

### Syslog (local or remote)

Linux operating systems often provide a general purpose logging utility called `syslog`. Both local and remote processes send logging messages to a centralized system logging daemon (`syslog`) that in turn writes the messages to the appropriate log files.

You can configure the level of detail at which `syslog` logs information. This way administrators can centrally manage how log files are handled, instead of separately configuring logging for each process.

Each type of process logs to a different syslog *facility*. There are facilities for the kernel, user processes, authorization processes, daemons, and a number of placeholders used by site-specific processes. API Gateway can log to several facilities such as `auth`, `daemon`, `ftp`, `local0-7`, and `syslog` itself.

#### Remote syslog

You can configure a remote `syslog` alert destination by specifying the details of the machine on which the `syslog` daemon is running. When an alert event is triggered, API Gateway connects to this daemon and logs to the specified facility.

1. In the **Alerts** node, click **Add > Syslog Remote**.
2. Enter a name for your alert destination.
3. Enter the host name or IP address of the machine where the `syslog` daemon is running.
4. Select the facility that API Gateway sends alerts to, and click **OK**.

#### Local syslog (UNIX only)

To configure a local `syslog` alert destination, perform the following steps:

1. In the **Alerts** node, click **Add > Syslog Local (UNIX only)**.
2. Enter a name for your alert destination.
3. Select the facility that API Gateway sends alerts to, and click **OK**.

### Windows Event Log

You configure the alert messages to be written to a local or remote Windows Event Log.

1. In the **Alerts** node, click **Add > Windows Event Log**.
2. Enter a name for your alert destination.
3. In **UNC Server name**, enter the Universal Naming Code (UNC) of the host machine where the event log is located. For example, to send alerts to the event log running on a machine called `NT_SERVER`, enter `NT_SERVER`.
4. Click **OK**.

### Check Point FireWall-1 (OPSEC)

API Gateway complies with Open Platform for Security (OPSEC). OPSEC compliance is awarded by Check Point Software Technologies to products that have been successfully integrated with at least one of their products, in this case Check Point FireWall-1.

FireWall-1 is the industry leading firewall that provides network security based on a security policy created by an administrator. Although OPSEC is not an open standard, the platform is recognized worldwide as the standard for interoperability of network security, and the alliance contains over 300 different companies. OPSEC integration is achieved through a number of published APIs that enable third-party vendors to interoperate with Check Point products.

You can specify where FireWall-1 is installed, the port it is listening on, and how to authenticate to the firewall. When an alert event is triggered, API Gateway connects to the specified firewall and prevents further requests for the particular client that triggered the alert.

1. In the **Alerts** node, click **Add > OPSEC**.
2. Enter a name for your alert destination.
3. If you have an existing OPSEC configuration file, click **Browse** and load the file. If you do not have an exiting file, click **Template** to generate the required settings into the text area.
4. Ensure that the following settings are set correctly for your firewall:
    * **sam_server auth_port**: The port number used to establish Secure Internal Communications (SIC) connections with the firewall
    * **sam_server auth_type**: The authentication method used to connect to the firewall
    * **sam_server ip**: The host name or IP address of the machine that hosts Check Point Firewall
    * **sam_server opsec_entity_sic_name**: The firewall's SIC name
    * **opsec_sic_name**: The OPSEC application SIC Name (application's full DName defined by the VPN-1 SmartCenter Server)
    * **opsec_sslca_file**: The name of the file containing the OPSEC application's digital certificate
5. For API Gateway to establish the SSL connection to the firewall, the specified `opsec_sslca_file` must be uploaded to API Gateway. To do this, click **Add** and select the correct file.
6. Click **OK**.

For more information on OPSEC settings, see the documentation for your OPSEC application.

### SNMP Network Management System

API Gateway can send Simple Network Management Protocol (SNMP) traps to a Network Management System (NMS).

1. In the **Alerts** node, click **Add > SNMP**.
2. Enter a name for your alert destination, and configure the following:
    * **Host**: The host name or IP address of the machine where the NMS is located
    * **Port**: The port where the NMS is listening
    * **Timeout**: The timeout (in seconds) for connections from API Gateway to the NMS
    * **Retries**: The number of retry attempts if a connection failure occurs
    * **SNMP Version**: The version of SNMP used
3. Click **OK**.

### Email recipient

You can configure API Gateway to send alert messages as emails.

1. In the **Alerts** node, click **Add > Email**.
2. Enter a name for your alert destination, and the recipients of the alert mail. If you want to include multiple recipients, use semicolon to separate the email addresses.
3. In **Email Sender (From)**, enter the email address where you want the email appears to be *from*.

    {{< alert title="Note" color="primary" >}}Some mail servers do not allow relaying mail when the sender in the **From** field is not recognized by the server.{{< /alert >}}
4. In **Email Subject**, enter the subject that the email alerts will use.
5. In the **SMTP Server Settings**, set the following:
    * **Outgoing Mail Server (SMTP)**: The SMTP server that API Gateway uses to relay the alert email
    * **Port**: The SMTP server port (default port is `25`)
    * **Connection Security**: The connection security used to send the alert email: `SSL`, `TLS`, or `NONE` (default)
6. If the SMTP server requires authentication, specify the credentials in **Log on Using**.
7. To set API Gateway to log information on any errors encountered when attempting to send email alerts, select the **Email Debugging**. All trace files are written to the `/trace` directory of your API Gateway installation. This setting is diasbled by default.
8. Click **OK**.

### Amazon SNS

You can configure API Gateway to send alert messages to the Amazon Simple Notification Service (SNS).

Amazon SNS is a managed push messaging service that you can used to send push notifications to mobile and smart devices connected to the Internet, as well as to other distributed services. For more details on Amazon SNS, go to <http://aws.amazon.com/sns/>.

1. In the **Alerts** node, click **Add > Amazon SNS**.
2. Enter a name for this alert destination.
3. In **AWS Credential**, select your AWS security credentials (API key and secret) that API Gateway uses when connecting to Amazon SNS.
4. Select the region appropriate for your deployment.
5. In **Client settings**, select the AWS client configuration API Gateway uses when connecting to Amazon SNS. For more details, see [Configure Amazon SQS queue listener](/docs/apim_policydev/apigw_gw_instances/general_aws_poller).
6. In **Topic ARN**, enter the topic Amazon Resource Name (ARN) to send alerts to.
7. When you create a topic, Amazon SNS assigns it a unique ARN that includes the service name (for example, SNS), the region, the AWS ID of the user, and the topic name. The ARN is returned as part of the API call to create the topic.

    For example, `arn:aws:sns:us-east-1:1234567890123456:mytopic` is the ARN for a topic named `mytopic` created by a user with the AWS account ID `123456789012` and hosted in the US East region.

    Whenever a publisher or subscriber needs to perform any action on the topic, they reference the unique topic ARN.

8. Enter the subject of the alerts will use, and click **OK**.

### Twitter

If you have a Twitter account, you can configure API Gateway to send tweet alerts to Twitter. API Gateway acts as a client application to make API calls and post alerts on behalf of the user.

Twitter uses the OAuth open authentication standard, and requires that API calls are made for both the user and the client application. Twitter API requires the following credentials to determine which application is calling the API and to verify that the Twitter user in question has authorized access to their account using the specified application:

* The consumer key of the client application
* The consumer secret key of the client application
* The access token that allows the client application to post on behalf of the user
* The access token secret to verify the access token

Twitter identifies and authenticates all requests as coming from both the user performing the request and the registered API Gateway application working on the user's behalf.

#### Register a client application

To use the Twitter API, you must first have a Twitter account you can use. Then, register a client application for API Gateway:

1. Go to [http://dev.twitter.com](http://dev.twitter.com/).
2. On the Twitter toolbar, select **Your apps**.
3. Click **Register a new app**.
4. Enter the details for the API Gateway client application. Some details are arbitrary, but you must specify the following:
    * **Application Type**: `Client`
    * **Default Access Type**: `Read & Write`

    {{< alert title="Note" color="primary" >}}The application name may already be registered to another user, so you may need to specify a different unique name.{{< /alert >}}

5. Click **Register Application**. Each client application you register is provisioned a consumer key and consumer secret. These are used, in conjunction with the OAuth library, to sign every request you make to the Twitter API. Using this signing process, Twitter trusts that the traffic identifying itself as you is indeed you.
6. Select your registered application, and select **My Access Token** to view the access token and an access token secret. You must store these safely.

#### Configure a Twitter alert destination

To configure a Twitter alert destination, perform the following steps:

1. In the **Alerts** node, click **Add > Twitter**.
2. Specify the credentials for the Twitter user that API Gateway sends an alert to:
    * **Consumer Key**: The consumer key of the registered client application
    * **Consumer Secret**: The consumer secret of the registered client application
    * **Access Token**: The access token that represents you
    * **Access Token Secret**: The access token secret that represents you

## Configure an alert policy

To send alert notifications to alert destinations, configure an alert policy. For example, you can configure an **Alert** filter to send the notifications, or use a **Scripting Language** filter for more fine-grain notifications.
