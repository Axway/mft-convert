{
"title": "Web service filters",
"linkTitle": "Web service filters",
"weight": 100,
"date": "2019-10-17",
"description": "Handle requests to and responses from a web service."
}

## Web service filter

The **Web Service Filter**
is used to control and validate requests to the web service and responses from the web service. Typically, this is automatically generated and populated as a **Service Handler**
when a WSDL file is imported into the web service repository. For example, if you import the WSDL file for a web service named `ExampleService`, a **Service Handler for ExampleService**
filter is automatically generated. However, you can also configure a **Web Service Filter**
manually.

In cases where the imported WSDL file contains WS-Policy assertions, a number of policies are automatically created containing the filters required to generate and validate the relevant security tokens (for example, SAML, WS-Security UsernameToken, and WS-Addressing headers). These policies perform the necessary cryptographic operations (for example, signing and encrypting) to meet the security constraints stipulated by the WS-Policy assertions.

### Web service general settings

Configure the following general settings:

**Name**:
Enter an intuitive name for the filter to display in a policy.

**Web Service Context**:
Click the button on the right, and select a WSDL file currently registered in the web service repository from the tree to set the web service context. To register a web service, right-click the default **Web Services**
node, and select **Register Web Service**.

When you select a web service from the **Web Service Context**
field, the **Message Interception Points**
tab is automatically populated with resolvers for the operations exposed by that web service. Similarly, the **Routing**
tab is automatically populated with the routing information for the selected web service.

### Routing settings

When routing to a service, you can specify a direct connection to the web service endpoint by using the URL in the WSDL or you can override this URL by entering a URL in the field provided.

Alternatively, in cases where the routing behavior is more complex, you can delegate to a custom routing policy, which takes care of the added complexity. The top-level radio buttons on the **Routing**
tab allow for these alternative routing configurations.

**Direct Connection to Service Endpoint**:
Select this option to route to either the URL specified in the WSDL or a URL. The radio buttons in the **Routing Details**
group enable you to choose between using the URL in the WSDL and providing an override. When providing an override, you can enter the new URL in the **URL**
field. Alternatively, you can specify the URL as a selector so that the URL is built dynamically at runtime from the specified message attributes. For example:

```
${host}:${port}
${http.destination.protocol}://${http.destination.host}:${http.destination.port}
```

In both cases, you can configure SSL settings, credential profiles for authentication, and other settings for the direct connection using the tabs in the **Connection Details**
group. For more details, see [Connect to URL](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter).

**Delegate to Routing Policy**:
Select this setting to use a dedicated routing policy to send messages on to the web service. For example, you might have configured a dedicated routing policy that uses the JMS-based **Send to JMS** filter to route over JMS.

Click the browse button next to the **Routing Policy**
field. Select the policy to use to route messages, and click **OK**. You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically.

### Validation settings

The WSDL for a web service contains information about the SOAP Action, SOAP Operation, and the data types of the message parts used in a particular SOAP operation. API Gateway creates the following implicit validation incoming requests for the web service:

* **SOAPAction HTTP Header**:
    If a web service requires clients to send a certain SOAPAction HTTP header in all requests, API Gateway can check the value of this header in the incoming request against the value specified in the WSDL.
* **SOAP Operation and Namespace**:
    The WSDL defines the SOAP Operation and namespace to be used in the SOAP request. The SOAP Operation is defined as the first child element of the SOAP Body element. API Gateway can check the value of this element in an incoming SOAP request and its namespace against the values specified in the WSDL.
* **Relative Path**:
    The filter ensures that requests for this web service are received on the same URL as that specified in the `<service>`
    block of the WSDL.
* **SOAP Version**:
    API Gateway also validates requests by matching the SOAP protocol version in the message against the SOAP binding version (1.1 or 1.2) of the corresponding operation definition in the WSDL.

It is also common for a WSDL document to contain an XML schema that defines the format and types of the message parts in the request. This is usually the case for document/literal style SOAP requests, where a complete XML schema is embedded or imported into the `<wsdl:types>`
block of the WSDL.

When using a WSDL to import a service into the web service repository, Policy Studio can extract the XML schema from the WSDL and configure API Gateway to validate incoming requests against it. Select **Use WSDL Schema**
to validate incoming requests against the schema in the WSDL.

Alternatively, you can create a custom-built policy to validate the contents of incoming requests. To do this, select the **Delegate to Validation Policy**
radio button, and the click the browse button next to the **Message Validation Policy**
field. Select the policy to use to validate requests, and click **OK**.

### Configure message interception points

The configuration settings on the **Message Interception Points**
tab determine how the request and response messages for the service are processed as they pass through API Gateway. Several message interception points are exposed to enable you to hook into different stages of the API Gateway's request processing cycle.

At each of these interception points, it is possible to run policies that are specific to that stage of the request processing cycle. For example, you can configure a logging policy to run just before the request has been sent to the web service and then again just after the response has been received.

Typically, the configuration settings on this window are automatically configured when importing a service into the web service repository based on information contained in the WSDL. In cases where the WSDL contains WS-Policy assertions, a number of policies are automatically generated and hooked up to perform the relevant security operations on the message.

For example, policies are created to insert SAML assertions, WS-Security `UsernameToken`
elements, WS-Addressing headers, and WS-Security timestamps into the message. Similarly, filters are created to sign and encrypt the outbound message, if necessary, and to decrypt and validate the signature on the response from the web service.

#### Order of execution

The order of execution of message interception points is as follows:

1. Request from client
2. User-defined request hooks
3. Request to service
4. Response from service
5. User-defined response hooks
6. Response to client

In steps 1, 3, 4, and 6, the execution order is as follows:

A.  Before operation-specific policy

B.  Operation-specific policy Shortcuts

C.  After operation-specific policy

The overall order of all the message interception points is described in the sequence that follows.

##### Request from client

This is the first message interception point, which enables you to run a policy against the request as it is received by API Gateway. Typically, this is where authentication and authorization events should occur.

* **1A) Before operation-specific policy**:
    This is usually where authentication policies should be configured because it is the earliest point in the request cycle that you can hook into. To select a policy to run at this point, click the browse button, and select the check box next to a previously configured policy.
* **1B) Operation-specific policy shortcuts**:
    To run policies that are specific to the different operations exposed by the web service, click **Edit**
    at the bottom of the table to set this up. For example, you can perform different validation on requests for the different operations.

    On the **Policy Shortcut Editor**
    dialog, enter the **Operation Namespace**
    and **Operation Name**
    in the fields provided. Enter a regular expression used to match the value of the SOAPAction HTTP header in the **SOAPAction Regular Expression**
    field. Finally, select the policy to run requests for this operation by clicking the browse button next to the **Policy Shortcut**
    field. Select the policy to run.
* **1C) After operation-specific policy**:
    This enables you to run a policy on the request *after*
    all the operation-level policies have been executed on the request. Select the appropriate policy as described earlier by clicking the browse button.

##### User-defined request hooks

You should primarily use this interception point to hook in your own custom-built *request*
processing policies.

* **User-defined request policy**:
    Click to browse to your custom-built request processing policy.

##### Request to service

This enables you to alter the message before it is routed to the web service. For example, if the service requires the message to be signed and encrypted, you can configure the necessary policies here.

* **3A) Before operation-specific policy**:
    This enables you to run policies on the message *before*
    the operation-level policies are run. Select the policy to run as outlined in the previous sections.
* **3B) Operation-specific policy shortcuts**:
    Operation-level policies on the request to the web service can be run here. For example, if the input policy for a particular operation requires the body to be signed and encrypted, a **Locate XML Nodes**
    filter can be run here to mark the required nodes.
* **3C) After operation-specific policy**:
    This is the last interception point available before the message is routed on to the web service. For example, if certain operation-level policies have been run to mark parts of the message to be signed and encrypted, the signing and encrypting filters should be run here.

##### Response from service

This is executed on the response returned from the web service.

* **4A) Before operation-specific policy**:
    If the response from the web service is encrypted, this interception point enables you to decrypt the message *before*
    any of the operation-level policies are run on the decrypted message.
* **4B) Operation-specific policy shortcuts**:
    The policies configured at this point run on specific operation-level responses (for example, `getHelloResponse`) from the web service.
* **4C) After Operation-specific policy**:
    This should be used to run policies *after*
    the operation-level policies have been run. For example, this is the appropriate point to place an **XML Signature Verification**
    filter.

##### User-defined response hooks

You should primarily use this interception point to hook in custom-built *response*
processing policies.

**User-defined response policy**:
Click to browse to your custom-built response processing policy.

##### Response to client

This enables you to process the response before it is returned to the client.

* **6A) Before operation-specific policy:**
    This enables you to process the message with a policy before the operation-level policies are run on the response.
* **6B) Operation-specific policy shortcuts:**
    The policies listed here are run on each operation response.
* **6C) After operation-specific policy:**
    This is the very last point at which you can run policies to process the response message before it is returned to the client. For example, if you are required to return a signed and encrypted response message to the client, the signing and encrypting should be done at this point.

### WSDL settings

You can expose an imported WSDL file to clients of API Gateway. A client can retrieve a WSDL for a service by appending `WSDL`
to the query string of the relative path on which the service is accepting requests.

For example, if the service is accepting requests at the URL `http://server:8080/services/getHello`, the client can retrieve the WSDL on the following URL:

```
http://server:8080/services/getHello?WSDL
```

When API Gateway returns the WSDL to the client, it dynamically modifies the service URL of the original WSDL to point to the machine on which API Gateway is running. For example, the original WSDL contains the following service element, where `www.service.com` resolves to an internal IP address that is not accessible to the public Internet:

```
<wsdl:service name="GetHelloService">
   <wsdl:port name="GetHelloServiceSoap" binding="tns:ServiceSoap">
      <soap:address location="http://www.service.com/getHello"/>
   </wsdl:port>
</wsdl:service>
```

When API Gateway returns this WSDL to the client, it dynamically modifies the value of the `location`
attribute to point to the name of the machine hosting API Gateway. In the following example, the `location`
attribute has been modified to point to the API Gateway instance running on port 8080 on the `Axway_SERVER`
host:

```
<wsdl:service name="GetHelloService">
   <wsdl:port name="GetHelloServiceSoap" binding="tns:ServiceSoap">
      <soap:address location="http://Axway_SERVER:8080/getHello"/>
   </wsdl:port>
</wsdl:service>
```

When the client receives the WSDL, it can automatically generate the SOAP request for the `getHello`
service, which it then sends to the`Axway_SERVER`
machine on port 8080.

Complete the following fields if you wish to expose the WSDL for this service to clients.

**Advertise WSDL to the Client**:
Select this option to publish the WSDL for the selected web service.

{{< alert title="Note" color="primary" >}}The exposed WSDL represents a *virtualized*
view of the back-end web service. In this way, clients can retrieve the WSDL from API Gateway, generate SOAP requests, and send these requests to API Gateway. API Gateway then routes the requests on to the web service. {{< /alert >}}

**WSDL Access Policy**:
To configure a policy to control or monitor access to the WSDL for this service, you can select the policy by clicking the browse button to the right of this field. Select the policy to run on requests to retrieve the WSDL.

### Monitoring options

The fields on this tab enable you to configure whether API Gateway displays usage metrics data for this web service. For example, this information can be used by API Gateway Analytics to produce reports showing who is calling this web service. You can configure the following fields:

* **Enable monitoring**:
    Select this option to enable monitoring for this web service in the **Monitoring**
    view in API Gateway Manager, and in API Gateway Analytics.
* **Which attribute is used to identify the client**:
    Enter the message attribute to use to identify authenticated clients. The default is `authentication.subject.id`, which stores the identifier of the authenticated user (for example, the user name or user's X.509 Distinguished Name).
* **Composite context**:
    This setting enables you to select a service context as a composite context in which multiple service contexts are monitored during the processing of a message. This setting is not selected by default.
* For example, API Gateway receives a message, and sends it to `serviceA`
    first, and then to `serviceB`
    . Monitoring is performed separately for each service by default. However, you can set a composite service context before `serviceA`
    and `serviceB`
    that includes both services. This composite service passes if both services complete successfully, and monitoring is also performed on the composite service context.

## Set web service context filter

The **Set Web Service Context**
filter is used in a policy to determine the service to obtain resources from in the web service repository.

For example, by pointing this filter at a preconfigured `getQuote`
service in the web service repository, the policy knows to return the WSDL for this particular service when a WSDL request is received. The **Return WSDL**
filter is used in conjunction with this filter to achieve this.

{{< alert title="Note" color="primary" >}}The **Set Web Service Context**
filter is configured automatically when auto-generating a policy from a WSDL file and is not normally manually configured.{{< /alert >}}

### Service WSDL settings

The **Service WSDL**
tab enables you to select the web service to obtain resources from in the web service repository.

Click the browse button to select a service definition (WSDL file) currently registered in the web service repository from the tree. To register a web service, right-click the default **APIs** > **Web Services**
node, and select **Register Web Service**.

### Monitoring settings

The fields on this tab enable you to configure to configure whether API Gateway displays usage metrics data for this web service. For example, this information can be used by API Gateway Analytics to produce reports showing how and who is calling this web service. For details on the fields on this tab, see [Monitoring options](#monitoring-options).

## Return WSDL filter

The **Return WSDL** filter returns a WSDL file from the web service repository.

{{< alert title="Note" color="primary" >}}This filter is configured automatically when auto-generating a policy from a WSDL file and is not normally manually configured.{{< /alert >}}

This filter is used with the **Set Web Service Context**
filter, which is used to identify web services.
