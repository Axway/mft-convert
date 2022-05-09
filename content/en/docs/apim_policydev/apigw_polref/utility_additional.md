{
"title": "Additional utility filters",
  "linkTitle": "Additional utility filters",
  "weight": 106,
  "date": "2019-10-17",
  "description": "Additional utility filters, including policy shortcut and policy shortcut chain, and the scripting language filter."
}
## Insert BST filter

You can use the **Insert BST**
filter to insert a Binary Security Token (BST) into a message. A BST is a security token that is in binary form, and therefore not necessarily human readable. For example, an X.509 certificate is a binary security token. Inserting a BST into a message is normally performed as a side effect of signing or encrypting a message. However, there are also some scenarios where you might insert a certificate into a message in a BST without signing or encrypting the message.

For example, you can use the **Insert BST**
filter when the API Gateway is acting as a client to a Security Token Service (STS) that issues security tokens (for example, to create `OnBehalfOf`
tokens). For more details, see the topic on [STS client authentication](/docs/apim_policydev/apigw_polref/authn_common/#sts-client-authentication-filter). Finally, you can also use the **Insert BST**
filter to generate XML nodes without inserting them into the message. In this case, the **WS-Security Actor**
is set to blank.

You can configure the following settings on the filter dialog:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**WS-Security Actor**:
Select or enter the WS-Security element in which to place the BST. Defaults to `Current actor / role only`. To use the **Insert BST**
filter to generate XML nodes without inserting them into the message, you must ensure that this field is set to blank.

**Message Attribute**:
Select or enter the message attribute that contains the BST. The message attribute type can be `byte[]`, `String`, `X509Certificate`, or `X509Certificate[]`.

**Value Type**:
Select the BST value type, or enter a custom type. Example value types include the following:

* `http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0#X509v3`
* `http://docs.oasis-open.org/wss/oasis-wss-kerberos-token-profile-1.1#GSS_Kerberosv5_AP_REQ`
* `http://xmlns.oracle.com/am/2010/11/token/session-propagation`

**Base64 Encode**:
Select this option to Base64-encode the data. This option applies only when the data in the message attribute is not already Base64 encoded. In some cases, the input might already be Base64 encoded, so you should deselect this setting in these cases.

## Check group membership filter

The **Check Group Membership**
filter checks whether the specified API Gateway user is a member of the specified API Gateway user group. The user and the group are both stored in the API Gateway user store. For more details, see
[Manage users](/docs/apim_administration/apigtw_admin/manage_user_access/).

Configure the following required fields:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**User**:
Enter the user name configured in the API Gateway user store. You can specify this value as a string or as a selector that expands to the value of the specified message attribute at runtime. Defaults to `${authentication.subject.id}`.

**Group**:
Enter the user group name configured in the API Gateway user store (for example, `engineering`
or `sales`
). You can specify this value as a string or as selector that expands to the value of the specified message attribute at runtime (for example, `${groupName}`).

{{< alert title="Note" color="primary" >}}The message attribute specified in the selector must exist on the message whiteboard prior to calling the filter.{{< /alert >}}

### Check group membership possible results

The possible paths through this filter are as follows:

| Result         | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `True`         | The specified user is a member of the specified group.     |
| `False`        | The specified user is not a member of the specified group. |
| `CircuitAbort` | An exception occurred while executing the filter.          |

## Execute external process filter

This filter enables you to execute an external process from a policy. It can execute any external process (for example, start an SSH session to connect to another machine, run a script, or send an SMS message).

The output of the external process is captured in two variables on the message whiteboard:

* `exec.output`, which contains everything the process printed to the standard output stream, `STDOUT`.
* `exec.error`, which contains everything that the process printed to the standard error stream, `STDERR`.

These variables might be empty if the program did not print any output to those streams.

For programs that output data to files, a [Load File Contents filter](/docs/apim_policydev/apigw_polref/conversion_additional/#load-file-contents-filter) might be needed to read the output. Also, in this case, you must ensure to implement a logic to clean up old files.

Complete the following fields:

**Name**:
Enter an appropriate name for the filter to display in a policy.

The **Command**
tab includes the following fields:

**Command to execute**: Specify the full path to the command to execute (for example, `c:/cygwin/bin/mkdir.exe`).

**Arguments**: Click **Add** to add arguments to your command. Specify an argument in the **Value** field (for example, `dir1`), and click **OK**. Repeat these steps to add multiple arguments (for example, `dir2` and `dir3`).

**Working directory**: Specify the directory to run the command from. You can specify this using a selector that is expanded to the specified value at runtime. Defaults to `${environment.VINSTDIR}`, where `VINSTDIR` is the location of a running API Gateway instance.

**Expected exit code**: Specify the expected exit code for the process when it has finished. The filter will follow the `true` path when this exit code is received from the process, and it will follow the `false` path otherwise. By following the Unix convention that processes should return zero on success and non-zero on failure, this filter defaults to `0`.

**Kill if running longer than (ms)**: Specify the number of milliseconds after which the running process is killed. Defaults to `60000`.

The **Advanced**
tab includes the following fields:

**Environment variables to set**: Click **Add** to add environment variables. In the dialog, specify an **Environment variable name** (for example, `JAVA_HOME`) and a **Value** (for example, `c:/jdk1.6.0_18`), and click **OK**. Repeat to add multiple variables.

**Block till process finished**: Select whether to block until the process is finished in the check box. This is enabled by default.

## Invoke policy per message body filter

In cases where API Gateway receives a multipart related MIME message, you can use the **Invoke Policy per Message Body**
filter to pass each body part to a specified policy for processing.

For example, if other XML documents are attached to an XML message (using the SOAP with Attachments specification), you can pass each of these documents to an appropriate policy where they can be processed by the full complement of message filters.

See also [Convert multipart or compound body type message](/docs/apim_policydev/apigw_polref/conversion_additional/#convert-multipart-or-compound-body-type-message-filter).

Complete the following fields:

**Name**:
Enter a name for the filter to display in a policy.

**Policy Shortcut**:
Select the policy to invoke for each MIME body part in the message. Each body part is passed to the selected policy in turn. The filter fails if the selected policy fails for *any*
of the passed body parts.

**Maximum level to unzip**:
In cases where a MIME body part is a MIME message itself (which might, in turn, contain more multipart messages), this setting determines how many levels of enveloped MIME messages to attempt to unzip. A default value of 2 levels ensures that the server does not attempt to unwrap unnecessarily *deep*
MIME messages.

If one of the body parts is actually an archive file (for example, tar or zip), this setting determines the maximum depth of files to unzip in cases where the archive file contains other archive files, which might contain others, and so on.

## Locate XML nodes filter

You can use the **Locate XML Nodes**
filter to select a number of nodes from an XML message. The selected nodes are stored in a message attribute, which is typically used by a signature or XML encryption filter later in a policy.

The primary use of the **Locate XML Nodes**
filter is when a series of policies is auto-generated by importing a Web Services Description Language (WSDL) file that contains WS-Policy assertions. For example, because there might be many different WS-Policy assertions that describe elements in the message that must be signed, you can use the **Locate XML Nodes**
filter to build up the node list of elements. Eventually, this node list is passed into the **Sign Message**
filter (using a message attribute) so that a single signature can be created that covers all the relevant parts.

However, you can also use this filter in similar cases where the message content that must be signed depends on content of the message. For example, a given policy runs a number of XPath expressions on a message where each XPath expression checks for a particular element. If that element is found, it can be marked as an element to be signed or encrypted by selecting that element in the **Locate XML Nodes**
filter. This means that only a single signature or XML encryption filter must be configured, with each path feeding back into this filter and passing in the message attribute that contains the nodes set for each specific case.

Nodes can be selected using any combination of node locations, XPaths, or message attributes. The following sections explain how to use each different mechanism and how to store the selected nodes in a message attribute.

### Node locations

The simplest way to select nodes is using the preconfigured elements listed in the table on the **Node Locations**
tab. The table is pre-populated with elements that are typically found in secured SOAP messages, including the SOAP Body, WSSE Security Header, WS-Addressing headers, SAML Assertions, WS UsernameToken, and so on.

The elements selected here are found by traversing the SOAP message as a DOM and finding the element name with the correct namespace and with the selected index position (for example, the first `Signature`
element from the `http://www.w3.org/2000/09/xmldsig#`
namespace).

You can select the check box in the **Name**
column of the table to select the corresponding node. You can select any number of node locations in this manner.

To locate an element that is not already present in the table, click the **Add**
button below the table to add a new node location. In the **Locate XML Nodes**
dialog, enter the name of the element, its namespace, and its position in the message using the **Element Name**, **Namespace**, and **Index**
fields.

To select this node for encryption purposes, select an appropriate **Encryption Type**
. For example, WS-Security policy mandates that when encrypting the SOAP Body that only its contents are encrypted and not the SOAP Body element itself. This means that the`<xenc:EncryptedData>` is inserted as a direct child of the SOAP Body element. In this case, you should select the **Encrypt Node Content**
option.

However, in most other cases, it is typically the entire node that gets encrypted. For example, when encrypting a `<wsse:UsernameToken>`, the entire node should be encrypted. In this case, the `<EncryptedData>`
element replaces the `<UsernameToken>`
element. To encrypt the entire node in this manner, select the **Encrypt Node**
option.

### XPath expressions

To select nodes that exist under a more complicated element hierarchy, it might be necessary to use an XPath expression to locate the required nodes. The **XPaths**
table is pre-populated with a number of XPath expressions to locate SOAP elements and common security elements, including SAML Assertions and SAMLP Responses.

To select an existing XPath expression, you can select the check box next to the **Name**
of the appropriate XPath expression. You can select any number of XPath expressions in this manner.

To add a new XPath expression, click the **Add**
button. You must enter a name for the XPath expression in the **Name**
field and enter the XPath expression in the **XPath Expression**
field.

To select this node for encryption purposes, you must select an appropriate **Encryption Type**. For example, WS-Security policy mandates that when encrypting the SOAP Body that only its contents are encrypted and not the SOAP Body element itself. This means that the`<xenc:EncryptedData>`
is inserted as a direct child of the SOAP Body element. In this case, you should select the **Encrypt Node Content**
option.

However, in most other cases, it is typically the entire node that gets encrypted. For example, when encrypting a `<wsse:UsernameToken>`, the entire node should be encrypted. In this case, the `<EncryptedData>`
element replaces the `<UsernameToken>`
element. To encrypt the entire node in this manner, select the **Encrypt Node**
option.

### Message attribute

Finally, you can also retrieve nodes that have been previously stored in a named message attribute. In such cases, another filter extracts nodes from the message and stores them in a named message attribute (for example, `node.list`). The **Locate XML Nodes**
filter can then extract these nodes and store them in the message attribute configured in the **Message Attribute Name**
field.

**Extract nodes from Selector Expression** enables you to specify whether to extract nodes from a specified selector expression (for example, `${node.list}`). This setting is not selected by default. Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store (KPS), or environment variable).

#### Message attribute in which to place list of nodes

At runtime, the **Locate XML Nodes**
filter locates and extracts the selected nodes from the message. It then stores them in the specified message attribute. For example, to sign the selected nodes, it would make sense to store the nodes in a message attribute called `sign.nodeList`, which would then be specified in the **Sign Message**
filter. Alternatively, to encrypt the selected nodes, you could store the nodes in the `encrypt.nodeList`
message attribute, which would then be specified in the **XML Encryption Properties**
filter. The **Message Attribute Name**
setting defaults to the `node.list`
attribute.

Finally, you must specify whether the selected nodes should **Overwrite**
any nodes that might already exist in the specified attribute, or if they should **Append**
to any existing nodes. You can also decide to **Reset**
the contents of the message attribute. Select the appropriate option depending on your requirements.

## HTTP parser filter

The **HTTP Parser** filter parses the HTTP message headers and body. As such, it acts as a barrier in the policy to guarantee that the entire content has been received before any other filters are invoked. It can be used, for example, to wait for an entire message from the back-end service before the gateway begins to reply to the caller. It requires the `content.body` attribute.

The **HTTP Parser** filter forces the server to do *store-and-forward* routing instead of the default *cut-through* routing, where the request is only parsed on-demand. For example, you can use this filter as a simple test to ensure that the message is XML.

## Pause processing filter

The **Pause**
filter is mainly used for testing purposes. This
filter causes a policy to sleep for a specified amount of time.

Configure the following settings:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Pause for**:
When the filter is executed in a policy, it sleeps for the time specified in this field. Defaults to `10000` milliseconds.

## Policy shortcut chain filter

The **Policy Shortcut Chain**
filter enables you to run a series of configured policies in sequence without needing to wire up a policy containing several **Policy Shortcut**
filters. This enables you to adopt a design pattern of creating modular reusable policies to perform specific tasks, such as authentication, content-filtering, or logging. You can then link these policies together into a single, coherent sequence using this filter.

Each policy in the **Policy Shortcut Chain**
is evaluated in succession. The evaluation proceeds as each policy in the chain passes, until finally the filter exits with a pass status. If a policy in the chain fails, the entire **Policy Shortcut Chain**
filter also fails at that point.

Complete the following general setting:

**Name**:
Enter a meaningful name for the filter to display in a policy. For example, the name might reflect the business logic of the policies that are chained together in this filter.

### Add a policy shortcut

Click the **Add**
button to display the **Policy Shortcut Editor**
dialog, which enables you to add a policy shortcut to the chain. Complete the following settings in this dialog:

**Shortcut Label**:
Enter an appropriate name for this policy shortcut.

**Evaluate this shortcut when executing the chain**:
Select whether to evaluate this policy shortcut when executing a policy shortcut chain. When this option is selected, the policy shortcut has an **Active**
status in the table view of the policy shortcut chain. This option is selected by default.

**Choose a specific policy to execute**:
Select this option to choose a specific policy to execute. This option is selected by default.

**Policy**:
Click the browse button next to the **Policy**
field, and select a policy to reuse from the tree (for example, **Health Check**). You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically. The policy in which this **Policy Shortcut Chain**
filter is configured calls the selected policy when it is executed.

**Choose a policy to execute by label**:
Select this option to choose a policy to execute based on a specific policy label. For example, this enables you to use the same policy on all requests or responses, and also enables you to update the assigned policy without needing to rewire any existing policies.

**Policy Label**:
Click the browse button next to the **Policy Label**
field, and select a policy label to reuse from the tree (for example, **API Gateway request policy (Health Check)**). The policy in which this **Policy Shortcut Chain**
filter is configured calls the selected policy label when it is executed.

Click **OK**
when finished. You can click **Add**
and repeat as necessary to add more policy shortcuts to the chain. You can alter the sequence in which the policies are executed by selecting a policy in the table and clicking the **Up**
and **Down**
buttons on the right. The policies are executed in the order in which they are listed in the table.

### Edit a policy shortcut

Select an existing policy shortcut, and click the **Edit**
button to display the **Policy Shortcut Editor**
dialog. Complete the following settings in this dialog:

**Shortcut Label**:
Enter an appropriate name for this policy shortcut.

**Evaluate this shortcut when executing the chain**:
Select whether to evaluate this policy shortcut when executing a policy shortcut chain. When this option is selected, the policy shortcut has an **Active**
status in the table view of the policy shortcut chain.

**Policy**
or **Policy Label**:
Click the browse button next to the **Policy**
or **Policy Label**
field (depending on whether you chose a specific policy or a policy label when creating the policy shortcut). Select a policy or policy label to reuse from the tree (for example, **Health Check**
or **API Gateway request policy (Health Check)**). The policy in which this **Policy Shortcut Chain**
filter is configured calls the selected policy or policy label when it is executed.

## Policy shortcut filter

The **Policy Shortcut**
filter enables you to reuse the functionality of one policy in another policy. For example, you could create a policy called **Security Tokens**
that inserts various security tokens into the message. You can then create a policy that calls this policy using a **Policy Shortcut**
filter.

In this way, you can adopt a design pattern of building up reusable pieces of functionality in separate policies, and then bringing them together when required using a **Policy Shortcut**
filter. For example, you can create modular reusable policies to perform specific tasks, such as authentication, content-filtering, or logging, and call them as required using a **Policy Shortcut**
filter.

Complete the following fields:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Policy Shortcut**:
Select the policy that to reuse from the tree. You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically. The policy in which this **Policy Shortcut**
filter is configured calls the selected policy when it is executed.

{{< alert title="Tip" color="primary" >}}Alternatively, to speed up policy shortcut configuration, you can drag a policy from the tree on the left of the Policy Studio and drop it on to the policy canvas on the right. This automatically configures a policy shortcut to the selected policy.{{< /alert >}}

## Set response status filter

The **Set Response Status**
filter is used to explicitly set the response status of a call. This status is then recorded as a message metric for use in reporting.

This filter is primarily used in cases where the fault handler for a policy is a **Policy Shortcut**
filter. If the **Policy Shortcut**
passes, the overall `fail`
status still exists. You can use the **Set Response Status**
filter to explicitly set the response status back to `pass`, if necessary.

Configure the following:

**Name**:
Enter a meaningful name for the filter to display in a policy.

**Response Status**:
Select **Pass**
or **Fail**
to set the response status.

## Switch on attribute value filter

The **Switch on Attribute Value**
filter enables you to switch to a specific policy based on the value of a configured message attribute. You can specify various switch cases (for example, contains, is, ends with, matches regular expression, and so on). Specified switch cases are evaluated in succession until a switch case is found, and the policy specified for that case is executed. You can also specify a default policy, which is executed when none of the switch cases specified in the filter is found.

Complete the following configuration settings:

**Name**:
Enter a meaningful name for the filter to display in a policy. For example, the name might reflect the business logic of a specified switch case.

**Switch on selector expression**:
Enter or select the name of the message attribute selector to switch on (for example, `${http.request.path}`). This filter examines the specified message attribute value, and switches to the specified policy if this value meets a configured switch case.

**Case**:
You can add, edit, and delete switch cases by clicking the appropriate button on the right. All configured switch cases are displayed in the table.

**Default**:
This field specifies the default behavior of the filter when none of the specified switch cases are found in the configured message attribute value. Select one of the following options:

* **Return result of calling the following policy**: Click the browse button, and select a default policy to execute from the dialog (for example, **XML Threat Policy**). The filter returns the result of the specified policy. This option is selected by default.
* **Return true**: The filter returns true.
* **Return false**: The filter returns false.

### Add a switch case

To add a switch case, click **Add**, and configure the following fields in the dialog:

**Comparison Type**:
Select the comparison type to perform with the configured message attribute. The available options include the following:

* `Contains`
* `Doesn't Contain`
* `Ends With`
* `Equals`
* `Does not Equal`
* `Matches Regular Expression`
* `Starts With`

All of these options are case insensitive, except for `Matches Regular Expression`.

**Compare with**:
Enter the value to compare the configured message attribute value with. For example, if you select a **Comparison Type**
of `Matches Regular Expression`, enter the regular expression in this field.

**Policy**:
Click the browse button next to the **Policy**
field, and select the policy to execute from the dialog (for example, **Remove All Security Tokens**). You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically. The selected policy is executed when this switch case is found.

Click **OK**
when finished. You can click **Add**, and repeat as necessary to add more switch cases to this filter. The switch cases are examined in the order in which they are listed in the table. You can alter the sequence in which the switch cases are evaluated by selecting a policy in the table and clicking the **Up**
and **Down**
buttons on the right.

## Quote of the day filter

The **Quote of the day**
filter is a useful test utility for returning a simple SOAP response to a client. The API Gateway wraps the quote in a SOAP response, which can then be returned to the client.

To configure this filter, enter the quote in the **Quotes**
text area. This quote can be returned in a SOAP response to the client by setting the **Reflect**
filter to be the successor of this filter in the policy.

The **Quote of the day**
filter can also load a file containing a list of quotes at runtime. In this case, a random quote from the file is returned to the client in the SOAP response. Each quote should be delimited by a `%`
character on a new line. This is analogous to the *BSD fortune format*. The format of this file is shown in the following example:

```
Most powerful is he who has himself in his own power.
%
All science is either physics or stamp collecting.
%
A cynic is a man who knows the price of everything and the value of nothing.
%
Intellectuals solve problems; geniuses prevent them.
%
If you can't explain it simply, you don't understand it well enough.
```

You can also enter the quotes in this format into the **Quotes**
text area to achieve the same goal.

The following example shows a SOAP response returned by the API Gateway to a client who requested the **Quote of the day**
service:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Header/>
   <s:Body xmlns:axway/>="axway.com">
      <axway:getQuoteResponse>
         Every cloud has a silver lining
      <axway:getQuoteResponse>
   </s:Body>
</s:Envelope>
```

## Scripting language filter

The **Scripting Language**
filter uses the Java Specification Request (JSR) 223
to embed a scripting environment in the API Gateway core engine. This enables you to write custom JavaScript, Groovy, or Jython code to interact with the message as it is processed by the API Gateway. You can get, set, and evaluate specific message attributes with this filter.

Some typical uses of the **Scripting Language**
filter include the following:

* Check the value of a specific message attribute
* Set the value of a message attribute
* Remove a message attribute
* DOM processing on the XML request or response

For more details on using scripts to extend API Gateway, see the
[API Gateway Developer Guide](/docs/apigtw_devguide/).

### Write a custom script

To write a custom script, you must implement the `invoke()`
method. This method takes a `com.vordel.circuit.Message`
object as a parameter and returns a boolean result.

The API Gateway provides a **Script Library**
that contains a number of prewritten `invoke()`
methods to manipulate specific message attributes. For example, there are `invoke()`
methods to check the value of the `SOAPAction`
header, remove a specific message attribute, manipulate the message using the DOM, and assign a particular role to a user.

You can access the script examples provided in the **Script library**
by clicking the **Show script library**
button on the filter's main configuration window.

{{< alert title="Note" color="primary" >}}
When writing the JavaScript, Groovy, or Jython code, you should note the following:

* You must implement the `invoke()` method. This method takes a `com.vordel.circuit.Message`
  object as a parameter, and returns a boolean.
* You can obtain the value of a message attribute using the `get()` method of the `Message`
  object. However, do not perform attribute substitution as follows:

  ````
  ```
  msg.get("my.attribute.a") == msg.get("my.attribute.b")
  ```

  This is not thread safe and can cause performance issues.
  ````

{{< /alert >}}

#### Use local variables

The API Gateway is a multi-threaded environment, therefore, at any one time multiple threads can be executing code in a script. When writing JavaScript, Groovy, or Jython code, always declare variables locally using `var`. Otherwise, the variables are global, and global variables can be updated by multiple threads.

For example, always use the following approach:

```
var myString = new java.lang.String("hello word");
for (var i = 100; i < 100; i++) {
   java.lang.System.out.println(myString + java.lang.Integer.toString(i));
}
```

Do not use the following approach:

```
myString = new java.lang.String("hello word");
for (i = 100; i < 100; i++) {
   java.lang.System.out.println(myString + java.lang.Integer.toString(i));
}
```

Using the second example under load, you cannot guarantee which value is output because both variables (`myString`
and `i`) are global.

### Add your script JARs to the classpath

You must add your custom JavaScript, Groovy, or Jython JAR files to your API Gateway classpath and to the list of runtime dependencies in Policy Studio.

#### Add your script JARs to the API Gateway classpath

Because the scripting environment is embedded in the API Gateway engine, it has access to all Java classes on the API Gateway classpath, including all JRE classes.

If you wish to invoke a Java object, you must place its corresponding class file on the API Gateway classpath. The recommended way to add classes to the API Gateway classpath is to place them (or the JAR files that contain them) in the `INSTALL_DIR/ext/lib`
folder. For more details, see the `readme.txt`
in this folder.

#### Add your script JARs to Policy Studio

Your custom JavaScript, Groovy, or Jython script JARs must also be compiled and validated in Policy Studio. To add your JARs files to the list of runtime dependencies in Policy Studio, perform the following steps:

1. In the Policy Studio main menu, select **Window** > **Preferences** > **Runtime Dependencies**.
2. Click **Add**
   to select your script JAR file(s) and any other required third-party JARs.
3. Click **Apply**
   when finished. Copies of these JAR files are added to the `plugins`
   directory in your Policy Studio installation.
4. You must restart Policy Studio and the server for these changes to take effect. You should restart Policy Studio using the `policystudio -clean` command.

### Configure a script filter

You can write or edit the JavaScript, Groovy, or Jython code in the text area on the **Script**
tab. A JavaScript function skeleton is displayed by default. Use this skeleton code as the basis for your JavaScript code. You can also load an existing JavaScript or Groovy script from the **Script library**
by clicking the **Show script library**
button.

On the **Script library**
dialog, click any of the **Configured scripts**
in the table to display the script in the text area on the right. You can edit a script directly in this text area. Make sure to click the **Update**
button to store the updated script to the **Script library**.

### Add a script to the library

To add a new script to the library, perform the following steps:

1. Click the **Add**
   button, which displays the **Script Details**
   dialog.
2. Enter a **Name**
   and a **Description**
   for the new script in the fields provided.
3. By default, the **Language**
   field is set to **JavaScript**, but you can select from the following options:

   * **Groovy**: Groovy script
   * **JavaScript**: JavaScript based on Nashorn JavaScript engine (JRE8)
   * **JavaScript (Rhino engine JRE7 and earlier)**: JavaScript based on Rhino JavaScript engine (JRE7 and earlier)
   * **Jython**: Jython script
4. Write the script in the **Script**
   text area on the right. For example:

![Script Library](/Images/docbook/images/utility/utility_scripting_library.gif)

{{< alert title="Note" color="primary" >}}Regular expressions specified in Scripting Language
filters use the regular expression engine of the relevant scripting language. For example, JavaScript-based filters use JavaScript regular expressions. This includes regular expressions inside XSDs defined by the W3C XML Schema standard. {{< /alert >}}

Other API Gateway filters that use regular expressions are based on `java.util.regex.Pattern`, unless stated otherwise.

## Time filter

The **Time**
filter enables you to block or allow messages on a specified time of day, or day of week, or both. You can input the time of day directly in the **Time**
filter window, or configure message attributes to supply this information using the Java `SimpleDateFormat`, or specify a cron expression.

You can use the **Time**
filter in any policy (for example, to block messages at specified times when a web service is not available, or has not been subscribed for by a consumer). In this way, this filter enables you to meter the availability of a web service and to enforce Service Level Agreements (SLAs).

Configure the following general options:

**Name**:
Enter an appropriate name for this filter.

**Block Messages**:
Select this option to use this filter to block messages. This is the default option.

**Allow Messages**:
Select this option to use this filter to allow messages.

### Basic time settings

Select **Basic**
to block or allow messages at specified times of the day. This is the default option. You can configure following settings:

**User defined time**:
Select this option to input the times to block or allow messages directly in this screen. This is the default option. Configure the following settings:

* **From**: The time to start blocking or allowing messages from in hours, minutes, and seconds. Defaults to `9:00:00`.
* **To**: The time to end blocking or allowing messages in hours, minutes, and seconds. Defaults to `17:00:00`.

**Time from attribute**:
Select this option to specify times to block or allow messages using configured message attributes. You can specify these attributes using selectors, which are replaced at runtime with the values of the specified message attributes set in previous filters or messages. You must configure the following settings:

* **From**: Message attribute that contains the time to start blocking or allowing messages from (for example, `$(message.starttime)`). Defaults to a time of `9:00:00`.
* **To**: Message attribute that contains the time to end blocking or allowing messages (for example, `$(message.endtime)`). Defaults to a time of `17:00:00`.
* **Pattern**: Message attribute that contains the time format based on the Java `SimpleDateFormat` class (for example,`$(message.pattern)`). This enables you to format and parse dates in a locale-sensitive manner. Day, month, years, and milliseconds are ignored. Defaults to a format of `HH:mm:ss`.

**Days**:
To block or allow messages on specific days of the week, select the check boxes for those days. For example, to block messages on Saturday and Sunday only.

### Advanced time settings

Select **Advanced**
to block or allow messages at specified times based on a cron expression. Configure the following setting:

**Cron Expression**:
Enter a cron expression or a message attribute that contains a cron expression in this field. Alternatively, click the browse button next to this field to select a preconfigured cron expression or to create and test a new cron expression.

For example, the following cron expression blocks all messages received on April 27 and 28 2012, at any time except those received between 10:00:01 and 10:59:59.

```
* * 0-9,11-23 27-28 APR ? 2012
```

The default value is `* * 9-17 * * ? *`, which specifies a time of 9:00:00 to 17:00:00 every day.

## Management services RBAC filter

Role-Based Access Control (RBAC) is used to protect access to the API Gateway management services. For example, management services are invoked when a user accesses the server using Policy Studio or API Gateway Manager (`https://localhost:8090/`). For more information on RBAC, see [Configure Role-Based Access Control (RBAC)](/docs/apim_administration/apigtw_admin/general_rbac/).

The **Management Services RBAC**
filter can be used to perform the following tasks:

* Read the user roles from the configured message attribute (for example, `authentication.subject.role`).
* Determine which management service URI is currently being invoked.
* Return true if one of the roles has access to the management service currently being invoked, as defined in the `acl.json`
    file.
* Otherwise, return false.

{{< alert title="Caution" color="warning" >}}This filter is for management services use only. The **Management Services** HTTP services group should only be modified under strict supervision from Axway Support.{{< /alert >}}

Configure the following settings:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**Role Attribute**:
Select or enter the message attribute that contains the user roles.
