{
"title": "Configure Amazon SQS queue listener",
  "linkTitle": "Configure Amazon SQS queue listener",
  "weight": 9,
  "date": "2019-10-17",
  "description": "Configure API Gateway to poll an Amazon SQS queue."
}
Amazon Simple Queue Service (SQS) is a hosted message queuing service for distributing messages amongst machines. You can configure API Gateway to poll an Amazon SQS queue at a set rate. Any message found on the SQS queue in this interval can be sent to a policy for processing. For more information, see [Amazon SQS](http://aws.amazon.com/sqs/).

To add a new Amazon SQS queue listener, in the Policy Studio tree, under the **Environment Configuration** > **Listeners** node, right-click the instance name (for example, **API Gateway**), and select **Amazon Web Services > Add SQS Queue Listener**.

## General settings

You can configure the following general settings for the Amazon SQS queue listener:

**Name**: Enter a suitable name for this SQS listener.

### AWS settings

**AWS Credential**: Click the browse button to select your AWS security credentials (API key and secret) to be used by API Gateway when connecting to Amazon SQS.

**Region**: Select the region appropriate for your deployment. You can choose from the following options:

* US East (Northern Virginia)
* US West (Oregon)
* US West (Northern California)
* EU (Ireland)
* Asia Pacific (Singapore)
* Asia Pacific (Tokyo)
* Asia Pacific (Sydney)
* South America (Sao Paulo)
* US GovCloud

**Client settings**: Click the browse button to select the AWS client configuration to be used by API Gateway when connecting to Amazon SQS. For more details, see [Configure AWS client settings](#configure-aws-client-settings).

### Poll settings

**Poll the queue `queueName` every `pollRate` milliseconds**: Enter the name of the queue to be polled and the rate at which the queue is to be polled, in milliseconds. The default queue name is `requestQueue` and the default poll rate is `10000` milliseconds (10 seconds).

**Call policy**: Click the browse button to select a policy to send the message to for processing. When a message has been consumed from the queue it is passed to the selected policy for processing.

**Process the retrieved messages as body with following Content Type**: Select this option to create a body from the message, and enter a content type. The default is `text/xml`.

**Store the content of the retrieved messages in the following attribute**: Select this option to store the content of the message in an attribute, and enter the attribute name. The default attribute name is `sqs.body`.

**Delete message on completion**: Select this option to delete the message from the queue when processing is complete. This is selected by default.

**Maximum number of messages to read from queue**: Enter the maximum number of messages to return. Amazon SQS never returns more messages than this value but it might return less. The default value is `10`.

**Visibility timeout for other consumers**: Enter the duration (in seconds) that the received messages are hidden from subsequent retrieve requests after being retrieved by API Gateway. The default value is `1000` seconds.

### Response settings

**Write a response message**: Select this option to write the contents of an attribute to a response queue. This is not selected by default.

**Response queue name**: If you selected the **Write a response message** option, enter the name of the response queue in this field. The default name is `responseQueue`.

**Use content body as the response**: Select this option to use the content body as the response.

**Use the following attribute as the response**: Select this option to use an attribute as the response, and enter an attribute selector for the attribute containing the response in this field (for example, `${sqs.body}`).

## Configure AWS client settings

API Gateway is a client of AWS, and as such you can define a client profile by which API Gateway connects to AWS. When you click the browse button on the **Client settings** field for an Amazon SQS queue listener, a **Send to Amazon SQS** filter, an **Upload to Amazon S3** filter, or an Amazon Simple Notification Service (SNS) alert destination, you can edit the configuration of the AWS client profile.

To edit the configuration, right-click the **Default AWS Client Configuration** and select **Edit**. Configure the following settings on the dialog:

**Name**: Enter a suitable name for this client configuration.

### Connection settings

**Maximum number of open HTTP connections**: Enter the maximum number of open HTTP connections. The default value is `50`.

**Socket timeout**: Enter the amount of time to wait (in milliseconds) for data to be transferred over an established, open connection before the connection is timed out. The default value is `50000` milliseconds. A value of 0 means infinity, and is not recommended.

**Connection timeout**: Enter the amount of time to wait (in milliseconds) when initially establishing a connection before giving up and timing out. The default value is `50000` milliseconds. A value of 0 means infinity, and is not recommended.

**Maximum number of retries**: Enter the maximum number of retry attempts for failed requests (that can be retried). The default value is `3`.

**User agent**: Enter the HTTP user agent header to send with all requests.

**Protocol**: Select the protocol (HTTP or HTTPS) to use when connecting to AWS.

### Proxy settings

You can optionally configure the following proxy settings:

**Proxy Host**: Enter the proxy host to connect through.

**Proxy port**: Enter the port on the proxy host to connect through.

**User name**: Enter the user name to use when connecting through a proxy.

**Password**: Enter the password to use when connecting through a proxy.

**NTLM Proxy support**: To configure Windows NT LAN Manager (NTLM) proxy support, enter the Windows domain name in the **Windows domain** field and enter the Windows workstation name in the **Windows workstation name** field.

### Advanced settings

You can optionally configure these settings to tune low level TCP parameters to try and improve performance.

**Size hint (in bytes) for the low level TCP send buffer**: Enter the size hint (in bytes) for the low level TCP send buffer.

**Size hint (in bytes) for the low level TCP receive buffer**: Enter the size hint (in bytes) for the low level TCP receive buffer.

## Further information

For more detailed information on Amazon Web Services integration, see the *AWS Integration Guide* available from [Axway Support](https://support.axway.com/kb/176876/language/en).
