{
"title": "Route to JMS",
  "linkTitle": "Route to JMS",
  "weight": 12,
  "date": "2019-10-17",
  "description": "Send messages to or read messages from a JMS messaging system."
}
API Gateway provides all the required third-party JAR files for IBM WebSphere MQ and Apache ActiveMQ (both embedded and external).

For other third-party JMS providers only, you must add the required third-party JAR files to the API Gateway classpath for messaging to function correctly. If the provider's implementation is platform-specific, copy the provider JAR files to `INSTALL_DIR/ext/PLATFORM`. `INSTALL_DIR` is your API Gateway installation, and `PLATFORM` is the platform on which API Gateway is installed (`Linux.x86_64`). If the provider implementation is platform-independent, copy the JAR files to `INSTALL_DIR/ext/lib`.

## Send to JMS filter

The **Send to JMS** filter enables you to configure a JMS messaging system to which the API Gateway sends messages. You can configure various settings for the message request and response (for example, destination and message type, how the message system should respond, and so on).

### Configure request settings

The **Request** tab specifies the following properties of the request sent to the messaging system:

**JMS Service**: Click the browse button on the right, and select an existing JMS service in the tree. To add a JMS Service, right-click the **JMS Services** tree node, and select **Add a JMS Service**.

**Destination type**: Select one of the following from the list:

* **Queue**
* **Topic**
* **JNDI lookup**

Defaults to **Queue**.

**Destination**: Enter the name of the JMS queue, JMS topic, or JNDI lookup to specify where you want to drop the messages.

The destination name to use depends on the configured **Destination type**. For example, for IBM WebSphere MQ, this name may need to use a format of `queue://`. For details on destination name requirements, see the user documentation for your third-party JMS provider tool.

**Delivery Mode**: Select one of the following delivery modes:

* **Persistent**:   Instructs the JMS provider to ensure that a message is not lost in transit if the JMS provider fails. A message sent with this delivery mode is logged to persistent storage when it is sent. This is the default mode.
* **Non-persistent**:   Does not require the JMS provider to store the message. With this mode, the message may be lost if the JMS provider fails.

**Priority Level**: You can use message priority levels to instruct the JMS provider to deliver urgent messages first. The ten levels of priority range from 0 (lowest) to 9 (highest). If you do not specify a priority level, the default level is 4. A JMS provider tries to deliver higher priority messages before lower priority ones but does not have to deliver messages in exact order of priority.

**Time to Live**: By default, a message never expires. However, if a message becomes obsolete after a certain period, you may want to set an expiry time (in milliseconds). The default value is `0`, which means the message never expires.

**Message ID**: Enter an identifier to be used as the unique identifier for the message. By default, the unique identifier is the ID assigned to the message by API Gateway (`${id}`). However, you can use a proprietary correlation system, perhaps using MIME message IDs instead of API Gateway message IDs.

**Correlation ID**: Enter an identifier for the message that API Gateway uses to correlate response messages with the corresponding request messages. Usually, if `${id}` is specified in the **Message ID** field, it is also used here to correlate request messages with their correct response messages.

**Message Type**: This list enables you to specify the type of data to be serialized and sent in the JMS message to the JMS provider. The option selected depends on what part of the message you want to send to the consumer. For example, to send the message body, select the option to format the body according to the rules defined in the [SOAP over JMS](http://www.w3.org/TR/soapjms/) recommendation. Alternatively, to serialize a list of name-value pairs to the JMS message, choose the option to create a `MapMessage`.

Select one of the following serialization options:

* **Use content.body attribute to create a message in the format specified in the SOAP over Java Message Service recommendation**:   If this option is selected, messages are formatted according to the [SOAP over JMS](http://www.w3.org/TR/soapjms/)   recommendation. This is the default option because in most cases the message body is routed to the messaging system. When this option is selected, a `javax.jms.BytesMessage`   is created and a JMS property containing the content type `text/xml`) is set on the message.
* **Create a MapMessage from the java.lang.Map in the attribute named below**:   Select this option to create a `javax.jms.MapMessage`   from the API Gateway message attribute named below that consists of name-value pairs.
* **Create a BytesMessage from the attribute named below**:   Select this option to create a `javax.jms.BytesMessage`   from the API Gateway message attribute named below.
* **Create an ObjectMessage from the java.lang.Serializable in the attribute named below**:   Select this option to create a `javax.jms.ObjectMessage`   from the API Gateway message attribute named below.
* **Create a TextMessage from the attribute named below**:   Select this option to create a `javax.jms.TextMessage`   from the message attribute named below.
* **Use the javax.jms.Message stored in the attribute named below**:   If a `javax.jms.Message`   has already been stored in a message attribute, select this option, and enter the name of the attribute in the field below.

**Attribute Name**: Enter the name of the API Gateway message attribute that holds the data that is to be serialized to a JMS message and sent over the wire to the JMS provider. The type of the attribute named here must correspond to that selected in the **Message Type** field above.

**Custom Message Properties**: You can set custom properties for messages in addition to those provided by the header fields. Custom properties may be required to provide compatibility with other messaging systems. You can use message attribute selectors as property values. For example, you can create a property called `AuthNUser`, and set its value to `${authenticated.subject.id}`. Other applications can then filter on this property (for example, only consume messages where `AuthNUser` equals `admin`). To add a new property, click **Add**, and enter a name and value in the fields provided on the **Properties** dialog.

**Use the following policy to change JMS request message**: This setting enables you to customize the JMS message before it is published to a JMS queue or topic. Click the browse button on the right, and select a configured policy in the dialog. The selected policy is then invoked before the JMS request is sent to the queuing system.

When the selected policy is invoked, the JMS request message is available on the white board in the `jms.outbound.message` message attribute. You can therefore call JMS API methods to manipulate the JMS request further. For example, you could configure a policy containing a **Scripting Language** filter that runs a script such as the following against the JMS message:

```
function invoke(msg) {
  var jmsMsg = msg.get("jms.outbound.message");
  jmsMsg.setIntProperty("My_JMS_Report", 123);
  return true;
}
```

### Configure response settings

The **Response** tab specifies whether API Gateway uses asynchronous or synchronous communication when talking to the messaging system. For example, to use asynchronous communication, select the **Do not set response** option. If synchronous communication is required, you can select to read the response from a temporary queue or from a named queue or topic.

You can also specify whether API Gateway waits on a response message from a queue or topic from the messaging system. API Gateway sets the `JMSReplyTo` property on each message that it sends. The value of the `JMSReplyTo` property is the temporary queue, queue, or topic selected in this dialog. It is the responsibility of the application that consumes the message from the queue (JMS consumer) to send the message back to the destination specified in `JMSReplyTo`.

API Gateway sets the `JMSCorrelationID` property to the value of the **Correlation ID** field on the **Request** tab to correlate requests messages to their corresponding response messages. If you select to use a temporary queue or temporary topic, this is created when API Gateway starts up.

**Configure how messaging system should respond**: Select where the response message is to be placed using one of the following options:

* **Do not set response**:   Select this option if you do not expect or do not care about receiving a response from the JMS provider.
* **Use temporary queue**:   Select this option to instruct the JMS provider to place the response message on a temporary queue. In this case, the temporary queue is created when API Gateway starts up. Only API Gateway can read from the temporary queue, but any application can write to it. API Gateway uses the value of the `JMSReplyTo`   header to indicate the location where it reads responses from.
* **Use queue**:   If you want the JMS provider to place response messages on a queue, select this option, and enter the queue name in the text box. This is used in the `JMSReplyTo`   field of the response message.
* **Use topic**:   If you want the JMS provider to place response messages on a topic, select this option, and enter the topic name in the text box. This is used in the `JMSReplyTo`   field of the response message.
* **Use named queue or topic (JNDI)**:   If you want the JMS provider to place response messages on a named queue or topic using JNDI lookup, select this option, and enter the JNDI name for the queue or topic in the text box. This is used in the `JMSReplyTo`   field of the response message.

**Wait for response**: If **Do not set response** is not selected, you can select whether API Gateway waits to receive a response:

* **Wait with timeout (ms)**:   API Gateway waits a specific time period to receive a response before it times out. If API Gateway times out waiting for a response, the **Send to JMS** filter fails. Enter the timeout value in milliseconds. The default value of `10000` means that API Gateway waits for a response for 10 seconds. The accepted range of values is `10000`   –`20000` ms.
* **Selector for response**:   If **Wait with timeout (ms)**   is selected, you can enter an SQL selector expression that specifies a response message. The expression entered specifies the messages that the consumer is interested in receiving. By using a selector, the task of filtering the messages is performed by the JMS provider instead of by the consumer.

    The selector is a string that specifies an expression whose syntax is based on the SQL92 conditional expression syntax. The API Gateway instance only receives messages whose headers and properties match the selector. For example:

  ```
  JMSCorrelationID='${params.query.id}'
  ```

{{< alert title="Note" color="primary" >}}
The JMS consumer automatically returns the results of the invoked policy to the JMS destination specified in the `JMSReplyTo`
header in the request. This means that you do not need to send a reply using the **Send to JMS**
filter.
If the incoming JMS message contains a `JMSReplyTo`
header, the queue or topic expects a response. So when the JMS consumer policy completes, API Gateway sends a message to the `JMSReplyTo`
source in reverse. For example, the consumer reads the JMS message, and populates an attribute with a value inferred from the message type to Java (for example, from `TextMessage`
to `String`
). When the policy completes, the consumer looks up this attribute and infers the JMS response message type based on the object type stored in the message.
{{< /alert >}}

## Read JMS filter

The **Read from JMS** filter enables you to configure a JMS messaging system from which the API Gateway reads messages. You can configure various settings for the JMS message source, message type, and processing options.

### Configure message source

The **Message source** settings enable you to configure the following:

**JMS Service**: Click the browse button on the right, and select an existing JMS service in the tree. To add a JMS Service, right-click the **JMS Services** tree node, and select **Add a JMS Service**. Alternatively, you can configure JMS services under the **Environment Configuration** > **External Connections** node in the Policy Studio tree. For more details, see [Configure messaging services](/docs/apim_policydev/apigw_poldev/general_messaging/).

**Source type**: Select one of the following from the list:

* **Queue**
* **Topic**
* **JNDI lookup**

Defaults to **Queue**.

**Source Name**: Enter the name of the JMS queue, JMS topic, or JNDI lookup to specify where you want read the messages from.

{{< alert title="Note" color="primary" >}}The source name to use depends on the configured **Source type**. For example, for IBM WebSphere MQ, this name may need to use a format of `queue://`. For details on source name requirements, please see the user documentation for your third-party JMS provider tool.{{< /alert >}}

**Selector**: Enter an SQL selector expression that specifies a response message. The expression entered specifies the messages that the consumer is interested in receiving. By using a selector, the task of filtering the messages is performed by the JMS provider instead of by the consumer.

The selector is a string that specifies an expression whose syntax is based on the SQL92 conditional expression syntax. The API Gateway instance only receives messages whose headers and properties match the selector. For example, the following expression gets the selected message from the queue:

```
JMSCorrelationID='${params.query.id}'
```

**Read timeout (ms)**: Enter the timeout after which the **Read from JMS** filter fails. The accepted range of values is 1–20000 ms. Defaults to `1000` ms.

### Configure JMS consumer type

The **JMS consumer type** settings enable you to configure the following:

**Durable subscription**: Create or use a durable topic subscription to consume messages from the server. This option is only available for **Topic** and **JNDI lookup** source types.

{{< alert title="Note" color="primary" >}}This is only available with a **Topic**
source and the JMS service used must have a client ID configured. If a **JNDI lookup**
source is configured, the name must not point to a topic. {{< /alert >}}

**Topic subscriber name**: Enter the JMS subscriber name used to identify the durable subscription.

### Configure message processing

The **Message processing** settings enable you to configure the following:

**Extraction Method**: Specify how to extract the data from the JMS message from the list:

* Insert the JMS message directly into the attribute named below (this is the default).
* Populate the attribute below with the value inferred from message type to Java.

**Attribute Name**: The name of the API Gateway message attribute that holds the data extracted from the JMS message. Defaults to the `jms.message` message attribute.

**Policy**: Select the appropriate policy to run on the JMS message after it has been consumed by the API Gateway.

**Send Response to Configured Destination**: Specifies whether the API Gateway sends a reply to the response queue named in the incoming message (in the `ReplyTo` header). This option is selected by default. Deselecting this option means that the API Gateway never sends a reply to the response queue named in the `ReplyTo` header.