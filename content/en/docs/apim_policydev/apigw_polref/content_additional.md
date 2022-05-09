{
"title": "Additional content filtering filters",
  "linkTitle": "Additional content filtering filters",
  "weight": 65,
  "date": "2019-10-17",
  "description": "Additional filters for content filtering, including threatening content and content validation."
}

## Content validation filter

The API Gateway can examine the contents of an XML message to ensure that it meets certain criteria. It uses boolean XPath expressions to evaluate whether or not a specific element or attribute contains a certain value.

For example, you can configure XPath expressions to make sure the value of an element matches a certain string, to check the value of an attribute is greater (or less) than a specific number, or that an element occurs a fixed amount of times within an XML body.

To configure an XPath expression:

1. On the Content Validation dialog, click the **Add**
    button next to the **XPath Expression** field. Alternatively, you can select a previously configured XPath expression from the list.
2. In the XPath Expression dialog, enter a name for the expression in the **Name** field, and enter the expression in the **XPath Expression** field. Alternatively, use the XPath wizard to create a valid XPath expression.
3. To resolve any prefixes within the XPath expression, enter the namespace mappings in the table.

### Configure XPath expressions

You can configure XPath expressions manually or using a wizard.

#### Configure XPath expression manually

If you are already familiar with XPath and wish to configure the expression manually, complete the following fields, using the examples below if necessary:

1. Enter or select a name for the XPath expression in the **Name**
    drop-down list.
2. Enter the XPath expression to use in the **XPath Expression**
    field.
3. In order to resolve any prefixes within the XPath expression, enter the namespace mappings (Prefix, URI) in the table.

For example, consider the following SOAP message:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Header>
    <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="sig1">
      ...............
    </dsig:Signature>
  </soap:Header>
  <soap:Body>
    <prod:product xmlns:prod="http://www.company.com">
      <prod:name>SOA Product</prod:name>
      <prod:company>Company</prod:company>
      <prod:description>WebServices Security</prod:description>
    </prod:product>
  </soap:Body>
</soap:Envelope>
```

The following XPath expression evaluates to true if the `<company>`
element contains the value `Company`:

```
//prod:company[text()='Company']
```

In this case, you must define a mapping for the *prod*
namespace as follows:

| Prefix | URI                      |
|--------|--------------------------|
| `prod` | `http://www.company.com` |

In another example, the element to be examined by the XPath expression belongs to a default namespace. Consider the following SOAP message:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Header>
    <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="sig1">
      ...............
    </dsig:Signature>
  </soap:Header>
  <soap:Body>
    <product xmlns="http://www.company.com">
      <name>SOA Product</name>
      <company>Company</company>
      <description>Web Services Security</description>
    </product>
  </soap:Body>
</soap:Envelope>
```

The following XPath expression evaluates to true if the `<company>`
element contains the value `Company`:

```
//ns:company[text()='Company']
```

Because the `<company>` element belongs to the default (`xmlns`) namespace (`http://www.company.com`), you must make up an arbitrary prefix (`ns`) for use in the XPath expression, and assign it to `http://www.company.com`. This is necessary to distinguish between potentially several default namespaces which might exist throughout the XML message. The following mapping illustrates this:

| Prefix | URI                      |
|--------|--------------------------|
| `ns`   | `http://www.company.com` |

##### Return a nodeset

Both of the examples above dealt with cases where the XPath expression evaluated to a Boolean value. For example, the expression in the above example asks does the `<company>`
element in the `http://www.company.com`
namespace contain a text node with the value `Company`.

It is sometimes necessary to use the XPath expression to return a subset of the XML message. For example, when using an XPath expression to determine what nodes should be signed in a signed XML message, or when retrieving the nodeset to validate against an XML Schema.

The API Gateway ships with an XPath expression that returns `All Elements inside SOAP Body`. To view this expression, select it from the **Name**
field. It appears as follows:

```
/soap:Envelope/soap:Body//*
```

This XPath expression simply returns all child elements of the SOAP `<Body>`
element. To construct and test more complicated expressions, use the XPath wizard.

#### Configure XPath expression using XPath wizard

The XPath wizard
assists you in creating correct and accurate XPath expressions. The wizard enables you to load an XML message and then run an XPath expression on it to determine what nodes are returned. To launch the XPath wizard, click the **XPath Wizard** button
on the XPath Expression
dialog.

To use the XPath wizard, enter (or browse to) the location of an XML file in the **File**
field. The contents of the XML file are displayed in the main window of the wizard. Enter an XPath expression in the **XPath**
field, and click the **Evaluate**
button to run the XPath against the contents of the file. If the XPath expression returns any elements (or returns true), those elements are highlighted in the main window.

If you are not sure how to write the XPath expression, you can select an element in the main window. An XPath expression to isolate this element is automatically generated and displayed in the **Selected**
field. To use this expression, select the **Use this path**
button, and click **OK**.

## Send to ICAP filter

You can use an **ICAP**
filter to send a message to a preconfigured ICAP server for content adaptation. For example, this includes specific operations such as virus scanning, content filtering, ad insertion, and language translation.

Configure the following settings:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**ICAP Server**:
Click the button next to this field, and select a preconfigured ICAP server in the tree. To add an ICAP server, right-click the **ICAP Servers**
tree node, and select **Add an ICAP Server**. Alternatively, you can configure ICAP servers under the **Environment Configuration** > **External Connections**
node in the Policy Studio tree.

### Example policies

This section shows some example use cases of the **ICAP**
filter configured in policies.

#### Request Modification Mode

The following policy shows an ICAP filter used in Request Modification (REQMOD) mode:

![Request Modification Mode](/Images/docbook/images/content/icap_reqmod.gif)

This example policy is essentially an internet proxy but with all incoming messages being sent to an ICAP server for virus-checking before being sent to the destination. All ICAP server-bound messages in this instance are REQMOD requests.

#### Response Modification Mode

The following policy illustrates an ICAP Filter used in Response Modification (RESPMOD) mode:

![Response Modification Mode](/Images/docbook/images/content/icap_respmod.gif)

This example policy also is an internet proxy but with all responses being sent to an ICAP server for virus-checking after being sent to the destination and before being sent back to the client. All ICAP server-bound messages in this instance are RESPMOD requests.

## WS-Security policy layout filter

Web services can use the WS-Policy specification to advertise the security requirements that clients must adhere to in order to successfully connect and send messages to the service. For example, a typical WS-Policy would mandate that the SOAP request body be signed and encrypted (using XML Signature and XML Encryption) and that a signed WS-Utility Timestamp must be present in a WS-Security header.

To guarantee that the security tokens used to *protect*
the message are added to the request in the most efficient and interoperable manner, WS-Policy uses the `<wsp:Layout>`
assertion. The semantics of this assertion are implemented by the **WS-Security Policy Layout**
filter and are outlined in the configuration details in the next section.

To check a SOAP message for a particular WS-Policy layout, complete the following fields:

**Name**:
Enter an intuitive name for this filter to display in a policy (for example, `Check SOAPRequest for Lax Layout`).

**Actor**:
Enter the name of the SOAP Actor/Role where the security tokens are present.

**Select Required Layout Type**:
Select the required layout from the following WS-Policy options:

* **Strict**:
    Select this option to check that a SOAP message adheres to the WS-Policy strict layout rules. For more information, see the [WS-Policy specification](http://docs.oasis-open.org/ws-sx/ws-securitypolicy/v1.3/errata01/ws-securitypolicy-1.3-errata01-complete.html).
* **Lax**:
    Select this option if you want to ensure that the security tokens in the SOAP header have been inserted according to the Lax WS-Policy layout rules. The WS-Policy Lax rules are effectively identical to those stipulated by the SOAP Message Security specification.
* **LaxTimestampFirst**:
    This layout option ensures that the WS-Policy Lax rules have been followed, but also checks to make sure that the WS-Utility Timestamp is the first security token in the WS-Security header.
* **LaxTimestampLast**:
    This option ensures that the WS-Utility Timestamp is the last security token in the WS-Security header and that all other Lax layout rules have been followed.

**WS-Security Version**:
The layout rules for WS-Security versions 1.0 and 1.1 are slightly different. Select the version of the layout rules to apply to SOAP requests. For details on the differences between these versions, see the WS-Security specifications ([WSS 1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf) and [WSS 1.1](http://www.oasis-open.org/committees/download.php/16790/wss-v1.1-spec-os-SOAPMessageSecurity.pdf)).

## Threatening content filter

The **Threatening Content**
filter can run a series of regular expressions that identify different attack signatures against request messages to check if they contain threatening content. Each expression identifies a particular attack signature, which can run against different parts of the request, including the request body, HTTP headers, and the request query string. In addition, you can configure the MIME types on which the **Threatening Content**
filter operates.

The threatening content regular expressions are stored in the global **Black list**
library, which is displayed under the **Environment Configuration** > **Libraries**
node in the Policy Studio tree. By default, this library contains regular expressions to identify SQL syntax to guard against SQL injection attacks, DOCTYPE DTD references to avoid against DTD expansion attacks, Java exception stack trace information to prevent call stack information getting returned to the client, and expressions to identify other types of attack signature.

### Scanning settings

To configure the **Scanning Details**
tab, complete the following:

**Additional message parts to scan**:
This section configures what parts of the incoming request are scanned for threatening content. By default, the **Threatening Content**
filter acts on the request body. However, it can also scan the HTTP headers and the request query string for threatening content. Select the appropriate check boxes to indicate what additional parts of the request message to scan.

**Blacklist**:
The table lists all the regular expressions that have been added to the global **Black list**
library. These regular expressions are used to identify threatening content. For example, there are regular expressions to match SQL syntax, ASCII control characters, and XML processing instructions, all of which can be used to attack a web service.

Select the regular expressions to run against incoming requests using the check boxes in the table. You can add new expressions using the **Add**
button. When adding new regular expressions on the **Add Regular Expression**
dialog, the expressions are added to the global **Black list**
library.

You can edit or remove existing regular expressions by selecting the expression in the tree, and selecting the **Edit**
or **Delete**
button.

This filter uses the regular expression syntax specified by `java.util.regex.Pattern`. For more details, see [Class Pattern](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).

### MIME type settings

The **MIME Types**
tab lists the MIME types to be scanned for incoming messages. By default, all text- and XML-related types are scanned for threatening content. However, you can select any type from the list.

Similar to the way in which the **Black list**
regular expressions are global, so too are the MIME types. You can add these globally by selecting the **Environment Configuration** > **Server Settings**
node in the Policy Studio tree, and clicking the **General > MIME**
option.

You can add new types by selecting the **Add**
button and entering a type name and corresponding extension on the **Configure MIME Type**
dialog. You can enter a list of extensions by separating them with spaces. You can edit or delete existing types by selecting the **Edit**
and **Delete**
buttons.

## XML complexity filter

Parsing XML documents is a notoriously processor-intensive activity. This can be exploited by hackers by sending large and complex XML messages to web services in a type of denial-of-service attack attempting to overload them. The **XML Complexity**
filter can protect against such attacks by performing the following checks on an incoming XML message:

* Checking the total number of nodes contained in the XML message.
* Ensuring that the message does not contain deeply nested levels of XML nodes.
* Making sure that elements in the XML message can only contain a specified maximum number of child elements.
* Making sure that each element can have a maximum number of attributes.

By performing these checks, the API Gateway can protect back-end web services from having to process large and potentially complex XML messages.

You can also use the **XML Complexity**
filter on the web service response to prevent a dictionary attack. For example, if the web service is a phone book service, a `name=*`
parameter could return all entries.

The **XML Complexity**
filter should be configured as the first filter in the policy that processes the XML body. This enables this filter to block any excessively large or complex XML message *before*
any other filters attempt to process the XML.

To configure the **XML Complexity**
filter, complete the following fields:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**Maximum Total Number of Nodes**:
Specify the maximum number of nodes to allow in an XML message.

{{< alert title="Note" color="primary" >}}This number does not include text nodes or comments. You can use the **Message size** filter to stop large text nodes or comments. {{< /alert >}}

**Maximum Number of Levels of Descendant Nodes**:
Enter the maximum number of descendant nodes that an element is allowed to have. Again, this number does not include text nodes or comments.

**Maximum Number of Child Nodes per Node**:
Enter the maximum number of child nodes that an element in an XML message is allowed to have.

**Maximum Number of Attributes per Node**:
Enter the maximum number of attributes that an element is allowed to have.
