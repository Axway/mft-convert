{
    "title": "Get started with sample policies",
    "linkTitle": "Get started with sample policies",
    "weight": 4,
    "date": "2019-10-17",
    "description": "Learn about and set up the example policies available in the `samples` directory of your API Gateway installation."
}

## Set up the sample policies

The example policies include the following:

* **Conversion**: exposes a SOAP service over REST.
* **Security**:
    * Verifies the digital signature on the request and creates a signature on the response.
    * Decrypts the request and encrypts part of the response.
* **Throttling**: limits the number of calls for a service.
* **Virtualized Service**: combines threat protection, content-based routing (target a server according to request contents), and message transformation.

### Enable the sample services interface

The HTTP interface for the sample policy services is disabled by default. To enable this interface in Policy Studio, perform the following steps:

1. In the navigation tree, select **Environment Configuration** > **Listeners** > **API Gateway** > **Sample Services** > **Ports**.
2. In the **Interfaces** pane on the right, select **Samples HTTP Interface**.
3. Right-click, and select **Edit**
    to display the Configure HTTP Interface
    dialog.
4. Select the **Enable interface**
    setting.
5. Click **OK**.

![Configure HTTP Interface](/Images/docbook/images/samples/enable_interface.png)

Alternatively, you can also enable this HTTP interface using the web-based API Gateway Manager tool running on `http://HOST:8090`, where `HOST`
is the machine on which the Node Manager is running.

1. Click the **Settings**
    button in the API Gateway Manager toolbar.
2. Select the HTTP interface node under **Sample Services**
    on the left.
3. Select the **Interface Enabled**
    setting on the right.
4. Click the **Apply**
    button.

{{< alert title="Note" color="primary" >}}Settings made in the web-based API Gateway Manager tool are dynamic settings only, which are not persisted.{{< /alert >}}

### Configure a different sample services interface

All sample policy services are defined in an HTTP services group named `Sample Services`. This group uses an HTTP interface running on the port specified in the `${env.PORT_SAMPLE_SERVICES}`
environment variable. This external environment variable is set to `8081`
by default. To use a different port, you must configure this variable in the `INSTALL_DIR/conf/envSettings.props`
file. For example, you could add the following entry:

```
env.PORT.SAMPLE.SERVICES=8082
```

### StockQuote demo service

All sample policies use a demo service named `StockQuote`, which is implemented using a set of policies. This service exposes two operations:

* **getPrice**: the policy for this operation uses a sample script to randomly calculate a quote value. Each call to `getPrice()` returns a different value.
* **update**: returns an `Accepted` HTTP code (202).

The `StockQuote` service is exposed on the following relative paths:

* `/stockquote/instance1`
* `/stockquote/instance2`

These relative paths are used in the virtualized service sample for content-based routing.

A **Connect to URL** filter with the following URL is used to invoke the `StockQuote`
service from each of the sample policies:

```
http://stockquote.com/stockquote/instance1
```

The first part of this URL uses a *remote host*
definition of `stockquote.com`. Remote hosts are logical names that decouple the host name in a URL from the server (or group of servers) that handles the request.

### Remote host settings

In Policy Studio, the remote host configuration is displayed under the API Gateway instance name (`API Gateway`) in the navigation tree, and is named `stockquote.com:80`. To view the remote host configuration, select **Environment Configuration** > **Listeners** > **StockQuote Host** and click **Edit**:

![Remote Host Settings](/Images/docbook/images/samples/remote_host_settings.png)

On the **General** tab, the remote host is set to:

* Use HTTP 1.1.
* Use port `80` by default.
* Include the `ContentLength` header in the request to the back-end server.
* In case of an SSL connection, verify the Distinguished Name (DN) in the certificate presented by the server against the server’s host name.

On the **Addresses and Load Balancing** tab, the remote host is set to send requests to `localhost:${env.PORT_SAMPLE_SERVICES}`, which resolves to `localhost:8081` by default. You could also specify several servers in the **Addresses to use instead of DNS lookup** list, and the API Gateway would load balance the requests across servers in the same group using the specified algorithm.

![Remote Host Address Settings](/Images/docbook/images/samples/remote_host_address_settings.png)

## Conversion sample policy

The conversion sample policy takes a REST-style request and converts it into a SOAP call. This topic describes the **REST to SOAP** policy, and explains how to run this sample.

### REST to SOAP policy

The **REST to SOAP** policy is as follows:

![Conversion policy](/Images/docbook/images/samples/conversion_sample_policy.gif)

The **REST to SOAP** policy performs the following tasks:

1. Extracts the information from the request (a message attribute is created for each query string and/or HTTP header).
2. Creates a SOAP message using the **Set Message** filter.
3. Sends the request to the `StockQuote` demo service.
4. Extracts the value of the stock from the response using XPath.
5. Creates a plain text response.
6. Sets the HTTP status code to 200.

### Run the conversion sample

You can call the sample service using the send request (`sr`) command:

```
sr http://HOSTNAME:8081/rest2soap?symbol=ABC
```

You can also send requests using your preferred REST client.

## Security sample policies

The security sample policies demonstrate digital signature verification and cryptographic operations (encryption and decryption).

### Signature verification

The **Signature Verification**
sample policy sends a digitally signed version of the `StockQuote`
request to the API Gateway. The message carries the signature into the web service header. A sample certificate/key pair (`Samples Test Certificate`) is used to sign the message and verify the signature. Signature verification is used for authentication purposes, and therefore an HTTP 403 error code is returned if a problem occurs.

The **Signature Verification** policy is as follows:

![Signature Verification policy](/Images/docbook/images/samples/digital_signature_sample_policy.gif)

The **Signature Verification** policy performs the following tasks:

1. The signature contained in the request is verified. The signature must be located in a WS-Security block.
2. If the verification is successful, the `StockQuote` demo service is invoked.
3. The response body is signed and returned to the client.
4. If the verification fails, an HTTP 403 error code is returned to the client.

#### Run the signature verification sample

You can call the sample service using the send request (`sr`) command:

```
sr -f INSTALL_DIR/samples/SamplePolicies/Security/SignatureVerification/Request.xml http://localhost:8081/signatureverification
```

### Encryption and decryption

This sample uses XML decryption on the request and applies encryption on the response. The sample policy includes a **Main** policy, which chains together the calls that decrypt the request, the invocation of the back-end service, and the encryption of the response.

The **Main** policy is as follows:

![Main policy](/Images/docbook/images/samples/encryption_main_sample_policy.gif)

The **Main** policy performs the following tasks:

1. **Decrypt Request** is a policy shortcut, which invokes another policy that takes the inbound request and decrypts it.
2. The decrypted request is routed to the back-end service.
3. The **Encrypt Response** policy shortcut invokes a policy that encrypts the response from the back-end service.

The **Decrypt** policy is as follows:

![Decrypt policy](/Images/docbook/images/samples/decrypt_sample_policy.gif)

The **Decrypt** policy performs the following tasks:

1. The decryption settings are defined: what to decrypt and which key to use.
2. The XML decryption is executed based on the defined settings.

The **Encrypt** policy is as follows:

![Encrypt policy](/Images/docbook/images/samples/encrypt_sample_policy.gif)

The **Encrypt** policy performs the following tasks:

1. The encryption settings are defined: what to encrypt, which symmetric key to use, which certificate to use, and how to encrypt (algorithm and where to place the encryption information).
2. The XML encryption is executed based on the defined settings.

#### Run the encryption and decryption sample

You can call the sample service using the send request (`sr`) command:

```
sr -f INSTALL_DIR/samples/SamplePolicies/Security/Encryption/Request.xml http://HOSTNAME:8081/encryption
```

## Throttling sample policy

The throttling sample policy is used to limit the number of calls for a service.

Throttling refers to restricting incoming connections and the number of messages to be processed. It can be applied to XML, SOAP, REST, or any payload, request, or protocol. Traffic can be regulated for a single API Gateway or for a cluster of API Gateways. You can apply traffic restrictions rules for a service, an operation, or even time of day. For example, these restrictions can be applied depending on the service name, user identity, IP address, content from the payload, protocol headers, and so on.

### Throttling policy

The **Throttle** policy is as follows:

![Throttling policy](/Images/docbook/images/samples/throttle_sample_policy.gif)

The **Throttle** policy performs the following tasks:

1. The first filter checks whether the limit has been reached. The limit is set to 3 requests per 15 sec. The caller’s IP address is used to track the consumer ID. The counter is kept in a local cache.
2. If the limit has been reached, an error message is created, and the response status code is set to 500.
3. If the authorized limit has not been reached, the back-end service is invoked, and the HTTP status code is set to 200.

### Run the throttling sample

You can call the sample service using the send request (`sr`) command:

```
sr -f INSTALL_DIR/samples/SamplePolicies/Throttling/Request.xml http://HOSTNAME:8081/throttle
```

## Virtualized service sample policy

The virtualized service sample policy is more advanced and combines the following features:

* Content filtering, XML complexity, and message size filters to block unwanted SOAP messages.
* Content filtering to block unwanted REST requests.
* Fault handling.
* Content-based routing.

This topic describes the policies displayed in the **Sample Policies** > **Web Services** > **Virtualized StockQuote Service** policy container in Policy Studio, and explains how to run this sample.

### Virtualized service policies

The **Virtualized StockQuote Service** sample policy container includes the following policies:

* Virtualized service main policy
* Threat protection policy
* Content-based routing policies
* Response transformation policy

The **Main Policy** is as follows:

![Virtualized Service Main Policy](/Images/docbook/images/samples/virtualization_main_sample_policy.gif)

The **Main Policy** uses policy shortcuts to perform the following tasks:

1. The main fault handler relies on some variables to be initialized, which is performed as soon as the policy is entered.
2. The **Threat Detection** policy is applied to the incoming SOAP message and HTTP headers.
3. The symbol value is extracted from the incoming message, and used to decide whether the request should be sent to one server instance or another.
4. The name of the instance that served the request is added to the response.
5. In case of errors, a global fault handler is invoked. This is used to return a custom error message to the user.

The **Threat Protection** policy is as follows:

![Threat Protection policy](/Images/docbook/images/samples/virtualization_threat_sample_policy.gif)

The **Threat Protection** policy performs the following tasks:

1. The incoming request size (including attachments) is checked to be less than 1500 KB.
2. The complexity of the XML is checked in terms of number of nodes, attributes per node, or number of child nodes per node.
3. XML and eventually HTTP headers are checked for threatening content such as SQL injection or XML processing instructions.
4. If any of these filters return an error, the corresponding error handler is called. The error handler is implemented as a policy that sets the value of the error code and message for this error, and then re-throws the exception so that the global fault handler catches it.

#### Content-based routing policies

The **Route Based on Symbol Value** policy extracts the contents of the symbol XML node and checks whether the first letter’s value is between `A-L` or `K-Z`. Depending on the result, it routes the request to the first or second instance of the `StockQuote` server. These servers are simulated by the following relative path URIs defined in the API Gateway:

* `/stockquote/instance1`
* `/stockquote/instance2`

The **Route Based on Symbol Value** policy is as follows:

![Route Based on Symbol Value](/Images/docbook/images/samples/virtualization_route_symbol_sample_policy.gif)

The **Route Based on Symbol Node** policy performs the following tasks:

1. The value of the symbol node is extracted from the request using XPath. The result is placed in a message attribute named `message.symbol.value`.
2. A **Switch on attribute value** filter is used to check the value of the message attribute (using a regular expression), and a different policy is called to send the request to `instance1` or `instance2`.

The **Route to Instance1** policy is as follows:

![Route to Instance1 Policy](/Images/docbook/images/samples/virtualization_route_instance_sample_policy.gif)

The **Route to Instance1** policy (called from the Switch filter) performs the following tasks:

1. Connects to the `instance1` URI.
2. If successful, the instance name (`instance1`) is placed in a message attribute (`stockquote.instance.name`). This is used later on to insert the instance name into the response.

The **Route to Instance2** policy performs the same tasks but using the `instance2` URI instead.

#### Response transformation policy

When the response is obtained from the back-end server, the **Add Instance Name to Response** policy changes it to insert the instance name into a new XML node (`instanceName`). The **Add Instance Name to Response** policy is as follows:

![Add Instance Name to Response Policy](/Images/docbook/images/samples/virtualization_route_response_sample_policy.gif)

This policy adds the instance name (the value of the `stockquote.message.name` message attribute) to the response, using an **Add XML node** filter, as part of the `SOAPbody`. XPath is used to define where the new node must be added.

### Run the virtualized service sample

You can call the sample service using the send request (`sr`) command:

```
sr -f INSTALL_DIR/samples/SamplePolicies/VirtualizedService/Request.xml http://HOSTNAME:8081/main/stockquote
```

## Stress test with send request `sr`

The API Gateway provides a command-line tool for stress testing named send request (`sr`). The sr tool is available in the following directory of your API Gateway installation:

```
INSTALL_DIR/posix/lib
```

On Linux, the `LD_LIBRARY_PATH` environment variable must be set to the directory from which you are running the `sr` tool, and you must use the `vrun sr` command. For example:

```
vrun sr http://testhost:8080/stockquote
```

### Basic `sr` command examples

The following are some basic examples of using the `sr` command:

HTTP GET:

```
sr http://testhost:8080/stockquote
```

POST file contents (content-type inferred from file extension):

```
sr -f StockQuoteRequest.xml http://testhost:8080/stockquote
```

Send XML file with SOAP Action 10 times:

```
sr -c 10 -f StockQuoteRequest.xml http://testhost:8080/stockquote
```

Send XML file with SOAP Action 10 times in 3 parallel clients:

```
sr -c 10 -p 3 -f StockQuoteRequest.xml http://testhost:8080/stockquote
```

Send the same request quietly:

```
sr -c 10 -p 3 -qq -f StockQuoteRequest.xml http://testhost:8080/stockquote
```

Run test for 10 seconds:

```
sr -d 10 -qq -f StockQuoteRequest.xml http://testhost:8080/stockquote
```

POST file contents with SOAP Action:

```
sr -f StockQuoteRequest.xml -A SOAPAction:getPrice http://testhost:8080/stockquote
```

### Advanced `sr` command examples

The following are some advanced examples of using the `sr` command:

Send form.xml to `http://192.168.0.49:8080/healthcheck` split at 171 character size, and trickle 200 millisecond delay between each send with a 200 Content-Length header:

```
sr -h 192.168.0.49 -s 8080 -u /healthcheck -b 171 -t 200 -f form.xml -a "Content-Type:application/x-www-form-urlenprogramlistingd" -a "Content-Length:200"
```

Send a multipart message to `http://192.168.0.19:8080/test`, 2 XML docs are attached to message:

```
sr -h 192.168.0.49 -s 8080 -u /test -{ -a Content-Type:text/xml -f soap.txt -a Content-Type:text/xml -f attachment.xml -a Content-Type:text/xml -} -A c-timestamp:1234
```

Send only headers using a GET over one-way SSL running 10 parallel threads for 86400 seconds (1 day) using super quiet mode:

```
sr -h 192.168.0.54 -C -s 8443 -u /nextgen -f test_req.xml -a givenName:SHViZXJ0 -a sn:RmFuc3dvcnRo -v GET -p10 -d86400 -qq
```

Send query string over mutual SSL presenting client certificate and key doing a GET running 10 parallel threads for 86400 seconds (1 day) using super quiet mode:

```
sr -h 192.168.0.54 -C -s 8443 -X client.pem -K client.key -u "https://localhost:8443/idp?TargetResource=http://axway.test.com" -f test_req.xml -v GET -p10 -d86400 -qq
```

Send zip file in users home directory to testhost on port 8080 with /zip URI, save the resulting response content into the result.zip file, and do this silently:

```
sr -f ~/test.zip -h testhost -s 8080 -u /zip -a Content-Type:application/zip -J result.zip -qq
```

### `sr` command arguments

For a listing of all arguments, enter `sr --help`.
