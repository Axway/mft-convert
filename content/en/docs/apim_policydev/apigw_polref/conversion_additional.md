{
"title": "Additional conversion filters",
  "linkTitle": "Additional conversion filters",
  "weight": 55,
  "date": "2019-10-17",
  "description": "Additional filters for use in conversions, including XML node, create cookie, and load file."
}

## Add XML node filter

You can use the **Add XML Node** filter to add an XML element, attribute, text, comment, or CDATA section node to an XML message. The new node is inserted into the location specified by an XPath expression or a SOAP actor/role. XPath is a query language that enables you to select nodes in an XML document. A SOAP actor/role provides a way of identifying a particular WS-Security block in a message.

You can configure the following settings:

### Where to insert new nodes

You can insert the new node into a location specified by an XPath expression or into a WS-Security header for the specified SOAP actor or role. Select one of the following options:

**Insert using XPath**: Select or enter an XPath expression to specify where to insert the new node. If this expression returns more than one node, the first one is used. If the expression returns no nodes, the filter returns false. You can add, edit, or delete XPath expressions using the buttons provided.

Select one of the following options to determine where the new node is placed relative to the nodes returned by the XPath expression.

* **Append**: The new node is appended as a child node of the node returned by the XPath expression. If there are already child nodes of the node returned by the XPath expression, the new node is added as the last child node.
* **Before**: The new node is inserted as a sibling node before the node returned by the XPath expression.
* **Replace**: The node pointed to by the XPath expression is completely replaced by the new node.

**Insert into WS-Security element for SOAP Actor/Role**: Select or enter the name of the SOAP actor/role that specifies the WS-Security element into which the XML node is inserted. A SOAP actor/role provides a way of distinguishing a particular WS-Security block from others that may be present in the message. For example, this setting is useful if there is no SOAP header or WS-Security element in the message because these are created when this option is specified.

### Node source

Select one of the following options for the source of the new node:

* **Create a new node**: If this option is selected, a new node is created and inserted into the location pointed to by the XPath expression configured above.
* **Insert previously removed nodes**: You can configure a **Remove XML Node**   filter to remove XML nodes from the message and store them in the `deleted.node.list`   message attribute. You can use the **Add XML Node**   filter to reinsert these nodes back into a different location inside the message, effectively moving the deleted nodes in the message. To use this option, select the **Save deleted nodes**   option on a **Remove XML Node**   filter that is configured to run before the **Add XML Node**   filter in the policy.
* **Message attribute**: If this option is selected, a new node is created from the contents of the selected message attribute. The expected type of the message attribute is Node of List of Nodes.

### New node details

Configure the following node details:

**Node Type**: Select the type of the new node from the list.

When choosing a node type, consider the following:

* You can only append a node to a **Node Type**   of `Element`   or `Text`.
* If you append to a `Text`   node, you must append another `Text`   node.
* If you add a new `Attribute`   node using the **Replace**   option, it must replace an existing `Attribute`   node.

**Node Content**: This field contains the node to be inserted into the message. The **Node Content** field must contain valid XML when the **Node Type** is set to `Element`. You can also enter selector expressions, which are populated at runtime.

{{< alert title="Note" color="primary" >}}To insert an XML node containing a namespace, you must define the namespace before using it in the filter, or the insertion will fail due to invalid XML. For more details, see [Knowledge Base article 178251](https://support.axway.com/en/articles/article-details/id/178251) on Axway Support.{{< /alert >}}

### Attribute node details

The following attribute-related fields are only enabled when you select `Attribute` from the **Node Type** list:

**Attribute Name**: Enter the name of the attribute in this field.

**Attribute Namespace URL**: Enter the namespace of the attribute in this field.

**Attribute Namespace Prefix**: Specify the prefix to use to map the element to the namespace entered above in this field. The new attribute is prefixed with this value.

### Add XML node examples

The following are some examples of using the **Add XML Node** filter to replace and add attributes and elements.

#### Replace an attribute value

To replace an attribute value, perform the following steps:

1. In the **Configure where to insert the new nodes** section, select **Insert using XPath**.
2. Select a value from the list (for example, `SOAP Header "mustUnderstand" attribute`).
3. Select the **Replace** option.
4. In the **Configure new node details** section, select `Attribute` from the a **Node Type** list.
5. Enter the **Node Content** in the text box (for example, in this case, `1` or `0`).
6. In the **Attribute Details** section, you must enter the **Attribute Name**.

#### Add an attribute

To add an attribute to an element, perform the following steps:

1. In the **Configure where to insert the new nodes** section, select **Insert using XPath**.
2. Select a value from the drop-down list (for example, `SOAP Header Element`).
3. Select the **Append** option.
4. In the **Configure new node details** section, select `Attribute` from the a **Node Type** list.
5. Enter the **Node Content** in the text box (for example, in this case `1` or `0`).
6. In the **Attribute Details** section, you must enter the **Attribute Name**.

#### Add an element

To add an element, perform the following steps:

1. In the **Configure where to insert the new nodes** section, select **Insert using XPath**.
2. Select a value from the drop-down list (for example, `SOAP Header Element`).
3. Select the **Append** option.
4. In the **Configure new node details** section, select `Element` from the a **Node Type** list.
5. Enter the **Node Content** in the text box (for example, in this case, the contents of the SOAP header).

#### Replace an element

To replace element A with new element B, perform the following steps:

1. In the **Configure where to insert the new nodes** section, select **Insert using XPath**.
2. Select a value from the drop-down list (for example, `SOAP Method Element`).
3. Select the **Replace** option.
4. In the **Configure new node details** section, select `Element` from the a **Node Type** list.
5. Enter the **Node Content** in the text box (for example, in this case, the contents of the SOAP method).

## Remove XML node filter

You can use the **Remove XML Node** filter to remove an XML element, attribute, text, or comment node from an XML message. You can specify the node to remove using an XPath expression. The XPath query language enables you to select nodes in an XML document.

To configure this filter, specify the following fields:

**Name**: Enter a suitable name that reflects the role of the filter in the policy. For example, if the purpose of this filter is to remove an `<ID>` element from the message, it would be appropriate to name this filter **Remove ID Element**.

**XPath Location**: Specify an XPath expression to indicate the node to remove. When the expression is configured correctly, you can remove an element, attribute, text, or comment node. If this expression returns more than one node, all returned nodes are removed.

You can select XPath expressions from the list, and edit or add expressions by clicking the relevant button. The following are some example expressions:

| Name                            | XPath Expression                  | Prefix | URI                                                                                 |
| ------------------------------- | --------------------------------- | ------ | ----------------------------------------------------------------------------------- |
| The First WSSE Security element | `//wsse:Security[1]`              | `wsse` | `http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd` |
| Text Nodes in SOAP Body         | `/soap:Envelope/soap:Body/text()` | `soap` | `http://schemas.xmlsoap.org/soap/envelope/`                                         |

**Fail if no nodes returned from XPath**: If this option is selected, and the XPath expression returns no nodes, the filter returns false. If this option is *not* selected, and the XPath returns no nodes, the filter returns true, and no nodes are removed.

**Save deleted nodes to be reinserted to new location**: You can use this option in cases where you want to move XML nodes from one location in the message to another. By selecting this option, the deleted nodes are stored in the `deleted.node.list` message attribute. You can then use the **Add XML Node** filter to insert the deleted nodes back into a different location in the message.

### Remove XML node filter exceptions

The **Remove XML Node** filter aborts with a `CircuitAbortException` if the content body of the payload is not in valid JSON format.

## Convert multipart or compound body type message filter

The **Convert Multipart/compound body type** filter is typically used to compress a MIME message before transferring a message using FTP to an FTP server. You could create a simple policy containing a **Convert Multipart/compound body type** filter as a predecessor of an **FTP Upload** filter to achieve this functionality.

The **Convert Multipart/compound body type** filter outputs a `content.body` message attribute, which represents the last part processed. For example, in a three part multipart message, the value of the `content.body` attribute is the third and last part in the series.

Configure the following fields for this filter:

**Name**: Enter a suitable name for this filter to display in a policy.

**Multipart content type**: Enter the MIME content type to compress the multipart message to. For example, to compress a multipart message to a ZIP file, enter `multipart/x-zip` in this field.

## Create cookie filter

An HTTP cookie is data sent by a server in an HTTP response to a client. The client can then return an updated cookie value in subsequent requests to the server. For example, this enables the server to store user preferences, manage sessions, track browsing habits, and so on.

The **Create Cookie** filter is used to create a `Set-Cookie` header or a `Cookie` header. The `Set-Cookie` header is used when the server instructs the client to store a cookie. The `Cookie` header is used when a client sends a cookie to a server. This filter adds the appropriate HTTP cookie header to the message header, and saves the cookie as a message attribute.

Configure the following fields:

**Filter Name**: Enter an appropriate name to display for this filter to display in a policy.

**HTTP Header Type**: Select the HTTP cookie header type to create: **Set-Cookie (Server)** or **Cookie (Client)**. When this is set to **Set-Cookie (Server)**, all attributes from the **Cookie Details** list are used. When this is set to **Cookie (Client)**, only the **Cookie Value** attribute is used.

You can configure the following settings for the cookie in the **Cookie Details** section:

**Cookie Name**: Enter the name of the cookie.

**Cookie Value**: Enter the value of the cookie.

**Domain**: Enter the domain name for this cookie.

**Path**: Enter the path on the server to which the browser returns this cookie.

**Max-Age**: Enter the maximum age of the cookie in days, hours, minutes, and seconds.

**Secure**: Select this option to restrict sending this cookie to a secure protocol. This setting is not selected by default, which means that it can be sent using any protocol.

**HTTPOnly**: Select this option to restrict the browser to use cookies over HTTP only. This setting is not selected by default.

## Set HTTP verb filter

You can use the **Set HTTP Verb** filter to explicitly set the HTTP verb in the message that is sent from the API Gateway. By default, all messages are routed onwards using the HTTP verb that the API Gateway received in the request from the client. If the message originated from a non-HTTP client (for example, JMS), the messages are routed using the HTTP POST verb.

Complete the following fields:

**Name**: Enter a name for the filter to display in a policy.

**HTTP Verb**: Specify the HTTP verb to use in the message that is routed onwards.

## Load file contents filter

The **Load File** filter enables you to load the contents of the specified file, and set them as message content to be processed. When the contents of the file are loaded, they can be passed to the core message pipeline for processing by the appropriate message filters. For example, you might use the **Load File** filter in cases where an external application drops XML or JSON files on to the file system to be validated, modified, and potentially routed on over HTTP, JMS, or stored to a directory where the application can access them again.

For example, this sort of protocol mediation can be useful when integrating legacy systems. Instead of making drastic changes to the legacy system by adding an HTTP engine, the API Gateway can load files from the file system, and route them on over HTTP to another back-end system. The added benefit is that messages are exposed to the full range of message processing filters available in the API Gateway. This ensures that only properly validated messages are routed on to the target system.

Configure the following fields.

### Input settings

**File**: Enter the name of the file to load, or browse to the file in the file system. This setting is required.

### Processing settings

The fields in this section determine what processing is performed on the input files, and where files are placed before and after processing.

**Processing Directory**: Enter or browse to the directory to which the input file is copied prior to processing. This field is optional. If this is not specified, the input file remains in the current input directory.

**Response Directory**: Enter or browse to the directory to which the response file is copied. This field is optional. If this is not specified, the response file is not written to disk.

**Processing Policy**: Select the policy executed on the input file. For example, the policy could perform message validation, routing, virus checking, or XSLT transformation. This field is optional.

**File Type**: Specifies how the input file is interpreted. Select one of the following options:

* **Raw**:   Assumes a content-type of `application/octet-stream`. This is the default.
* **Treat as HTTP Message (including headers)**:   Assumes the inbound file contains an HTTP request (optionally with HTTP headers).
* **Infer content-type from extension**:   Performs a lookup on configured MIME types to determine the content-type of the file based on its extension.
* **Use Content-type**:   Enables you to specify a content-type in the text box.

### On completion settings

You can specify what to do when the file processing has completed. Select one of the following options:

* **Do Nothing**:   The input file remains in the input directory or in the **Processing Directory**. This is the default.
* **Delete Input File**:   The input file is deleted from the input directory or the **Processing Directory**.
* **Move Input File**:   The input file is moved (archived) to the directory specified in the **To Directory**   field. You can also specify an optional **File Prefix**   or **File Suffix**   for the archived file.

## Remove attachments filter

You can use the **Remove attachment** filter to remove *all* attachments from either a request or a response message, depending on where the filter is placed in the policy.

## Restore message filter

You can use the **Restore Message** filter to restore message content at runtime using a specified selector expression. You can restore the contents of a request message or a response message, depending on where the filter is placed in the policy.

For example, you could use this filter to restore original message content if you needed to manipulate the message for authentication or authorization. Typically, this filter is used with the **Store Message** filter, which is first used to store the original message content.

Configure the following fields:

**Name**: Enter a suitable name for this filter to display in a policy.

**Selector Expression to retrieve message**: Enter the selector expression used to restore the message content. Defaults to `${store.content.body}`.

## Store message filter

You can use the **Store Message** filter to store message content in a specified message attribute. You can store the contents of a request message or a response message, depending on where the filter is placed in the policy.

For example, you could use this filter to store the original message content for reuse later if you need to manipulate the message for authentication or authorization. Typically, this filter is used with the **Restore Message** filter, which is then used to restore the original message content.

Configure the following fields:

**Name**: Enter a suitable name for this filter to display in a policy.

**Attribute to store message**: Enter the name of the message attribute used to store the message content. Defaults to `store.content.body`.
