{
"title": "Fault and error handling filters",
  "linkTitle": "Fault and error handling filters",
  "weight": 79,
  "date": "2019-10-17",
  "description": "Handle faults and errors in your policies."
}

## Generic error filter

In cases where a transaction fails, API Gateway can use a generic error to convey error information to the client based on the message type (SOAP or JSON). By default, API Gateway returns a very basic error when a message filter fails. You can add the **Generic Error** filter to a policy to return more meaningful error information based on the message type.

When the **Generic Error** filter is configured, API Gateway examines the incoming message and attempts to infer the type of message to be returned.

For example, for an incoming SOAP message, it sends an appropriate SOAP response (SOAP 1.1 or 1.2) using the SOAP fault processor. For an incoming JSON message, it sends an appropriate JSON response. If the inference process fails, API Gateway sends a SOAP message by default.

For security reasons, it is good practice to return as little information as possible to the client. However, for diagnostic reasons, it is useful to return as much information to the client as possible. Using the **Generic Error** filter, administrators have the flexibility to configure just how much information to return to clients, depending on their individual requirements.

Configure the following settings on the **General** tab:

Name : Enter an appropriate name for this filter to display in a policy.

HTTP Response Code Status : Enter the HTTP response code status. This ensures that a meaningful response is sent to the client in the case of an error occurring in a configured policy. Defaults to `500` (Internal Server Error). For a complete list of status codes, see the [HTTP Specification](http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html).

Configure the following options in the **Generic Error Contents** section:

Show detailed explanation of error : Returns a detailed explanation of the generic error in the error message. This makes it possible to suppress the reason for the exception in a tightly locked down system (the reason is displayed as `message blocked` in the generic error). Defaults to the value of the `${circuit.failure.reason}` message attribute selector.

Show filter execution path : Returns a generic error containing the list of filters run on the message before the error occurred. For each filter listed in the generic error, the status is given (`pass` or `fail`).

Show stack trace : Return the Java stack trace for the error to the client. This option should only be enabled under instructions from Axway Support.

Show current message attributes : Return the message attributes present at the time the generic error was generated to the client. For example, for an incoming SOAP message, each message attribute forms the content of a `<fault:attribute>` element.

{{< alert title="Caution" color="warning" >}}For security reasons, do not use **Show filter execution path**, **Show stack trace**, and **Show current message attributes** in a production environment.{{< /alert >}}

Use Stylesheet : Select this option to transform the error message returned by the **Generic error** filter by applying an XSLT stylesheet. Click the browse button next to the **Stylesheet** field. Select an existing stylesheet from the list, or right-click the **Stylesheets** node to add a new stylesheet.

Because XSLT stylesheets accept XML as input, API Gateway implicitly transforms the incoming message into XML. Next, it retrieves the selected XSLT stylesheet and applies the transformation to the message, and sends the response in the format specified.

### Create customized generic error using the Generic Error filter

On the **Advanced** tab, you can customize the generation of a response message when the request cannot be inferred as a SOAP or JSON request.

**Apply Response Rules**: When this setting is selected, you can select one of the following rules:

* **Delegate to Policy**: Select a policy to generate the response message. For example, the policy could use a **JSON Error** filter to generate a JSON response.
* **Response Content**: Set the response body content and headers directly in the respective text areas. You can use the API Gateway selector syntax to evaluate and expand request details at runtime. The values specified on this tab are used in the outbound request to the URL.

    * **Body**: Enter the content of the incoming request message body (body headers and body content). Defaults to `${content.body}`.

      For example, enter the `Content-Type` followed by a return, and then the required message payload:

    ```
    Content-Type:text/html
    <!DOCTYPE html><html><body><h1>Hello World</h1></body></html>
    ```
    * **Headers**: Enter the HTTP headers associated with the incoming request message. Defaults to `${http.headers}`.

### Create customized generic error using the Set Message filter

You can also use the **Set Message** filter to create customized generic errors. The **Set Message** filter can change the contents of the message body to any arbitrary content. When an exception occurs in a policy, you can use this filter to customize the body of the generic error.

For details on how to use the **Set Message** filter to generate customized faults and return them to the client, see the example in [SOAP fault filter](#soap-fault-filter). You can use the same approach to generate customized generic errors.

## JSON error filter

In cases where a JavaScript Object Notation (JSON) transaction fails, the API Gateway can use a *JSON error* to convey error information to the client. By default, the API Gateway returns a very basic fault to the client when a message filter fails.

You can add the **JSON Error** filter to a policy to return more meaningful error information to the client. For example, the following message extract shows the format of a JSON error raised when a **JSON Schema Validation** filter fails:

```json
{
    "reasons":[
       {
          "language":"en",
          "message":"JSON Schema Validation filter failed"
       }
    ],
    "details":{
       "msgId":"Id-f5aab7304f6c754804f70000",
       "exception message":"JSON Schema Validation filter failed",
        ...
    }
}
```

For security reasons, it is good practice to return as little information as possible to the client. However, for diagnostic reasons, it is useful to return as much information to the client as possible. Using the **JSON Error** filter, administrators have the flexibility to configure just how much information to return to clients, depending on their individual requirements. For more details, see [Introducing JSON](http://www.json.org/index.html).

Configure the following general settings:

**Name**: Enter an appropriate name for this filter to display in a policy.

**HTTP Response Code Status**: Enter the HTTP response code status for this JSON error filter. This ensures that a meaningful response is sent to the client in the case of an error occurring in a configured policy. Defaults to `500` (Internal Server Error). For a complete list of status codes, see the [HTTP Specification](http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html).

The following configuration options are available in the **JSON Error Contents** section:

**Show detailed explanation of error**: Select this option to return a detailed explanation of the JSON error in the error message. This makes it possible to suppress the reason for the exception in a tightly locked down system. By default, the reason is displayed as `message blocked` in the JSON error. This option displays the value of the `${circuit.failure.reason}` message attribute selector.

**Show filter execution path**: Select this option to return the list of filters run on the message before the error occurred. For each filter listed in the JSON Error, the status is output (`Pass` or `Fail`). The following message extract shows a *filter execution path* returned in a JSON error:

```json
"path" :{
   "policy" :"test_policy",
   "filters" :[ {
      "name" :"True Filter",
      "status" :"Pass"
   }, {
    "name" :"JSON Schema Validation",
    "status" :"Fail",
   "filterMessage" :"Filter failed"
   }, {
    "name" :"Generic Error",
    "status" :"Fail",
    "filterMessage" :"Filter failed"
   } ]
},
```

**Show stack trace**: Select this option to return the Java stack trace for the error to the client. This option should only be enabled under instructions from Axway Support.

**Show current message attributes**: Select this option to return the message attributes present when the JSON error is generated to the client. The value of each message attribute is output as shown in the following example:

```json
"attributes":[
   {
    "name":"circuit.exception",
    "value":"com.vordel.circuit.CircuitAbortException:JSON Schema Validation filter failed"
   },
   {
    "name":"circuit.failure.reason",
    "value":"JSON Schema Validation filter failed"
   },
   {
    "name":"content.body",
    "value":"com.vordel.mime.JSONBody@185afba1"
   },
   {
    "name":"failure.reason",
    "value":"JSON Schema Validation filter failed"
   },
   {
    "name":"http.client",
    "value":"com.vordel.dwe.http.ServerTransaction@7d3e1384"
   },
   {
    "name":"http.headers",
    "value":"com.vordel.mime.HeaderSet@76737f58"},
   {
    "name":"http.response.info",
    "value":"ERROR"
   },
   {
    "name":"http.response.status",
    "value":"500"
   },
   {
    "name":"id",
    "value":"Id-f5aab7304f6c754804f70000"
   },
   {
    "name":"json.errors",
    "value":"org.codehaus.jackson.JsonParseException: Unexpected character ('\"' (code 34)):was expecting comma to separate OBJECT entries\n at [Source:com.vordel.dwe.InputStream@592c34b; line:3, column:25]"
    },
...
]
```

{{< alert title="Caution" color="warning" >}}For security reasons, **Show filter execution path**, **Show stack trace**, and **Show current message attributes** should not be used in a production environment.{{< /alert >}}

### Create customized JSON error using the Generic Error filter

Instead of using the **JSON Error** filter, you can use the **Generic Error** filter to transform the JSON error message returned by applying an XSLT stylesheet. The **Generic Error** filter examines the incoming message and infers the type of message to be returned (for example, JSON or SOAP). You can use the **Advanced** tab to customize the generation of a response message if the request cannot be inferred as a SOAP or JSON request.

For more details, see [Generic error filter](#generic-error-filter).

### Create customized JSON error using the Set Message filter

You can create customized JSON errors using the **Set Message** filter with the **JSON Error** filter. The **Set Message** filter can change the contents of the message body to any arbitrary content. When an exception occurs in a policy, you can use this filter to customize the body of the JSON error.

For details on how to use the **Set Message** filter to generate customized faults and return them to the client, see the example in [SOAP fault filter](#soap-fault-filter). You can use the same approach to generate customized JSON errors.

## SOAP fault filter

In cases where a typical SOAP transaction fails, a *SOAP fault* can be used to convey error information to the SOAP client. The following message shows the format of a SOAP fault:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<env:Envelope xmlns:env="http://www.w3.org/2003/05/soap-envelope">
   <env:Body>
      <env:Fault>
         <env:Code>
            <env:Value>Receiver</env:Value>
            <env:Subcode>
                <env:Value>policy failed</env:Value>
            </env:Subcode>
         </env:Code>
         <env:Detail xmlns:axwayfault="axway.com/soapfaults" axwayfault:type="exception" type="exception"/>
      </env:Fault>
   </env:Body>
</env:Envelope>
```

By default, the API Gateway returns a very basic SOAP fault to the client when a message filter fails. You can add the **SOAP Fault** filter to a policy to return more complicated error information to the client.

For security reasons, it is good practice to return as little information as possible to the client. However, for diagnostic reasons, it is useful to return as much information to the client as possible. Using the **SOAP Fault** filter, administrators have the flexibility to configure just how much information to return to clients, depending on their individual requirements.

The following configuration options are available in the **SOAP Fault Format** section:

**SOAP Version**: Select the appropriate SOAP version. You can send either a SOAP Fault 1.1 or 1.2 response to the client.

**Fault Namespace**: Select the default namespace to use in SOAP faults, or enter a new one if necessary.

**Indent SOAP Fault**: If this option is selected, an XSL stylesheet is run over the SOAP fault to indent nested XML elements. The indented SOAP fault is returned to the client.

The following configuration options are available in the **SOAP Fault Contents** section:

**Show Detailed Explanation of Fault**: Select this option to return a detailed explanation of the SOAP fault in the fault message. This makes it possible to suppress the reason for the exception in a tightly locked down system (the reason is displayed as `message blocked` in the SOAP fault).

**Show Filter Execution Path**: Select this option to return a SOAP fault containing the list of filters run on the message before the error occurred. For each filter listed in the SOAP fault, the status is given (`pass` or `fail`). The following message shows a *filter execution path* returned in a SOAP fault:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<env:Envelope xmlns:env="http://www.w3.org/2003/05/soap-envelope">
   <env:Header></env:Header>
   <env:Body>
      <env:Fault>
         <env:Code>
            <env:Value>Receiver</env:Value>
            <env:Subcode>
                <env:Value>policy failed</env:Value>
            </env:Subcode>
         </env:Code>
         <env:Detail xmlns:axwayfault="http://www.axway.com/soapfaults" axwayfault:type="exception" type="exception">
             <axwayfault:path>
                <axwayfault:visit node="HTTP Parser" status="Pass"></axwayfault:visit>
                <axwayfault:visit node="/services" status="Fail"></axwayfault:visit>
                <axwayfault:visit node="/status" status="Fail"></axwayfault:visit>
             </axwayfault:path>
         </env:Detail>
       </env:Fault>
   </env:Body>
</env:Envelope>
```

**Show Stack Trace**: Select this option to return the Java stack trace for the error to the client. This option should only be enabled under instructions from Axway Support.

**Show Current Message Attributes**: Select this option to return the message attributes present at the time the SOAP fault was generated to the client. Each message attribute forms the content of a `<fault:attribute>` element, as shown in the following example:

```xml
<fault:attributes>
   <fault:attribute name="circuit.failure.reason" value="null">
   <fault:attribute name="circuit.lastProcessor" value="HTTP Digest">
   <fault:attribute name="http.request.clientaddr" value="/127.0.0.1:4147">
   <fault:attribute name="http.response.status" value="401">
   <fault:attribute name="http.request.uri" value="/authn">
   <fault:attribute name="http.request.verb" value="POST">
   <fault:attribute name="http.response.info" value="Authentication Required">
   <fault:attribute name="circuit.name" value="Digest AuthN">
</fault:attributes>
```

### Create customized SOAP fault using the Generic Error filter

Instead of using the **SOAP Fault** filter, you can use the **Generic Error** filter to transform the SOAP fault message returned by applying an XSLT stylesheet. The **Generic Error** filter examines the incoming message and infers the type of message to be returned (for example, JSON or SOAP). You can use the **Advanced** tab to customize the generation of a response message if the request cannot be inferred as a SOAP or JSON request.

For more details, see [Generic error filter](#generic-error-filter).

### Create customized SOAP fault using the Set Message filter

You can create customized SOAP faults using the **Set Message** filter with the **SOAP Fault** filter. The **Set Message** filter can change the contents of the message body to any arbitrary content. When an exception occurs in a policy, you can use this filter to customize the body of the SOAP fault. The following example demonstrates how to generate customized SOAP faults and return them to the client.

#### Create the top-level policy

This example first creates a very simple policy called **Main Policy**. This policy ensures the size of incoming messages is between 100 and 1000 bytes. Messages in this range are echoed back to the client.

![Main Policy](/Images/docbook/images/fault/main_circuit_start.gif)

#### Create the fault policy

Next, create a second policy called **Fault Circuit**. This policy uses the **Set Message** filter to customize the body of the SOAP fault. When configuring this filter, enter the contents of the customized SOAP fault to return to clients in the text area provided.

![Fault Circuit](/Images/docbook/images/fault/fault_circuit.gif)

#### Create a shortcut to the fault policy

Add a **Policy Shortcut** filter to the **Main Policy** and configure it to refer to the **Fault Circuit**. Do *not* connect this filter to the policy. Instead, right-click the filter, and select **Set as Fault Handler**. The **Main Policy** is displayed as follows:

![Main Policy with Fault Handler](/Images/docbook/images/fault/main_circuit.gif)

#### How this example works

Assume a 2000-byte message is received by the API Gateway and is passed to the **Main Policy** for processing. The message is parsed by the **HTTP Parser** filter, and the size of the message is checked by the **Message Size** filter. Because the message is greater than the size constraints set by this filter, and because there is no failure path configured for this filter, an exception is thrown.

When an exception is thrown in a policy, it is handled by the designated *fault handler*, if one is present. In the **Main Policy**, a **Policy Shortcut** filter is set as the fault handler. This filter delegates to the **Fault Circuit**, meaning that when an exception occurs, the **Main Policy** invokes (or delegates to) the **Fault Circuit**.

The **Fault Circuit** consists of two filters, which play the following roles:

1. **Set Message**: This filter is used to set the body of the message to the contents of the customized SOAP fault.
2. **Reflect**: When the SOAP fault has been set to the message body, it is returned to the client using the **Reflect** filter.
