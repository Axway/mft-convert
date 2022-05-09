{
"title": "Utility filters",
  "linkTitle": "Utility filters",
  "weight": 105,
  "date": "2019-10-17",
  "description": "Commonly used utility filters, including set and remove attribute, and reflect message."
}
## String replace filter

The **String Replace** filter enables you to replace all or part of the value of a specified message attribute. You can use this filter to replace any specified string or substring in a message attribute. For example, changing the `from` attribute in an email, or changing all or part of a URL.  The filter only works on `java.lang.String` values.  You will need to convert any object types that cannot be cast to a `java.lang.String`, like `java.net.URI`, before using this filter.  There is an example of how to convert a URI in Axway Knowledge Base article [KB 177221](https://support.axway.com/kb/177221/language/en).

To configure the **String Replace** filter, specify the following fields:

**Name**: Enter the name of the filter to be displayed in a policy.

**Message Attribute**: Select the name of the message attribute to be replaced from the list. This is required. If this is not specified, a `MissingPropertyException` is thrown, which results in a `CircuitAbortException`.

**Specify Destination Attribute**: By default, the value of the specified **Message Attribute** is both the source and destination, and is therefore overwritten. To specify a different destination attribute, select this check box to enable the **Destination Attribute** field, and select a value from the list.

**Replacement String**: The string used to replace the value of the specified source attribute. You can specify this as a selector, which is expanded to the specified value at runtime (for example, `${http.request.uri}`).  You can also use values like `$1`, `$2`, and so on to reference values from capturing groups in your regular expression. If you need to use a literal dollar sign you must escape it as `\$`, or it will cause an error due to being misinterpreted. This is a required field if you specify the **Specify Destination Attribute**.

**Straight**: A match string used to search the value of the specified source attribute. You can specify this as a selector, which is expanded to the specified value at runtime. If a straight (exact) match is found, it is replaced with the specified **Replacement String**.

**Regexp**: A match string, specified as a Java syntax regular expression, used to search the value of the specified source attribute. You can specify this as a selector, which is expanded to the specified attribute value at runtime. If a match is found, it is replaced with the specified **Replacement String**.

**First Match**: If a match is found, only replace the first occurrence.

**All Matches**: If a match is found, replace all occurrences.

{{< alert title="Note" color="primary" >}}The possible paths available through this filter are `True`
(even if no replacement takes place), and `CircuitAbort`. Under certain circumstances, if the **Replacement String**
contains a selector, a `MissingPropertyException`
can occur, which results in a `CircuitAbortException`.{{< /alert >}}

## Copy or modify attributes filter

The **Copy/Modify Attributes**
filter copies the values of message or user attributes to other message or user attributes. You can also set the value of a message or user attribute to a user-specified value.

The table lists the configured attribute-copying rules. To add a new rule, click **Add** and enter values in the dialog to copy a message or user attribute to a different message or user attribute. The **From attribute**
represents the source attribute, while the **To attribute**
represents the destination attribute.

### From attribute settings

The attribute value can be copied from one of the following sources:

* **Message**:
    Select this option to copy the value of a message attribute. Enter the name of the source attribute in the **Name**
    field.
* **User**:
    Select this option to copy a user attribute stored in the `attribute.lookup.list`. Enter the name (and namespace if the attribute was extracted from a SAML attribute assertion) of the user attribute in the **Name**
    and **Namespace**
    fields.

    If there are multiple values stored in the `attribute.lookup.list`
    for the attribute entered in the **Name**
    field, only the first value is copied.
* **User entered value**:
    Select this option to copy a user-specified value to an attribute. Enter the new attribute value in the **Value**
    field. You can enter a selector to represent the value of a message attribute instead of entering a specific value directly (for example, `${authentication.subject.id}`). In this case the value of the `authentication.subject.id`
    attribute is copied to the named attribute.

### To attribute settings

The message can be copied to one of the following types of attributes:

* **Message**:
    The attribute can be copied to any message attribute. Enter the name of the attribute in the **Name**
    field.
* **User**:
    Select this option if the attribute or value should be copied to a user attribute stored in the `attribute.lookup.list`. Specify the name and namespace (if necessary) of this attribute in the **Name**
    and **Namespace**
    fields.

    If there are multiple values stored in the `attribute.lookup.list`
    for the attribute entered in the **Name**
    field of the **From attribute**
    section, the attribute value is copied to the first occurrence of the attribute name in list.

Select **Create list attribute**
if the new attribute can contain several items.

## Set attribute filter

The **Set Attribute**
filter enables you to set the value of a specified message attribute.

Complete the following fields:

* **Name**:
    Enter a meaningful name for the filter to display in a policy.
* **Attribute Name**:
    Enter the name of the message attribute in which to store a value.
* **Attribute Value**:
    Enter the value of the message attribute specified above.

{{< alert title="Note" color="primary" >}}When using a selector expression as the attribute value, the result of evaluating the selector may be converted to a `java.lang.String` value. For some objects that fall back to `Object.toString()`, this can result in a value like `example.Class@00000000` instead of a copy of the original object. Use the **Copy/Modify Attributes** filter when copying attributes to avoid this conversion.{{< /alert >}}

## Remove attribute filter

You can use the **Remove Attribute**
filter to remove a specified message attribute from a request message or a response message, depending on where the filter is placed in the policy.

**Name**:
Enter a suitable name for this filter to display in a policy.

**Attribute Name**:
Select or enter the message attribute name to be removed from the message (for example, `authentication.subject.password`).

## Evaluate selector filter

The **Evaluate Selector**
filter enables you to evaluate the contents of a specified selector expression, and return a boolean result. A selector is a special syntax that enables API Gateway configuration settings to be evaluated and expanded at runtime.

This filter enables you to evaluate a specified selector expression and make a decision in a policy based on whether the expression value fails or passes. For example, you could use the following expression to check if the user belongs to a particular group that allows the user to access a particular resource:

```
${user[0].memberOf.contains("CN=Group Policy Creator Owners,CN=Users,DC=acmeqa,DC=com")}
```

This expression checks if the `memberOf`
attribute retrieved for the first `user`
contains the specified value (in this case, membership of a particular group). If the expression matches, the filter passes.

Alternatively, you could use the following selector expression to check if the user email address is valid:

```
${user[0].mail.contains("admin@qa.acme.com")}
```

This expression checks if the `mail`
attribute retrieved for the first `user`
contains the specified value (in this case, a particular email address). If the expression matches, the filter passes.

Configure the following settings:

**Name**:
Enter a descriptive name for this filter to display in a policy.

**Expression**:
Enter the selector expression to be evaluated. Defaults to the following selector expression:

```
${1 + 1 == 2}
```

## Reflect message filter

The **Reflect Message**
filter echoes the HTTP request headers, body, and attachments back to the client.

Configure the following settings:

**Name**:
Enter a name for the filter to display in a policy.

**HTTP response code status**:
Specify an HTTP status response code to return to the client.

## Reflect message and attributes filter

The **Reflect Message and Attributes**
filter echoes the HTTP request headers, body, and attachments back to the client. It also echoes back the message attributes that were stored in the message at the time when the message completed the policy.

## Abort filter

You can use the **Abort**
filter to force a policy to throw an exception. You can use it to test the behavior of the policy when an exception occurs.

For example, to quickly test how the policy behaves when a **Message Size**
filter throws an exception, you can place an **Abort**
filter before it in the policy. The following policy diagram illustrates this:

![Abort policy](/Images/docbook/images/utility/utility_abort.gif)

## False filter

You can use the **False**
filter to force a path in the policy to return false. This can be useful to create a *false positive*
path in a policy.

The following policy parses the HTTP request and then runs a **Message Size**
filter on the message to make sure that the message is no larger than 1000 bytes. To make sure that the message cannot be greater than this size, you can connect a **False**
filter to the *success*
path of the **Message Size**
filter. This means that an exception is raised if a message exceeds 1000 bytes in size.

![Policy with a False filter](/Images/docbook/images/utility/utility_false_circuit.gif)

## True filter

You can use the **True**
filter to force a path in a policy to return true. For example, this can be useful to prevent a path from ending on a false case and consequently throwing an exception.

The following policy parses the HTTP request, and then runs **Attachment1**
on the message. If **Attachment1**
passes, the message is echoed back to the client by the **Reflect**
filter. However, if **Attachment1**
fails, the **Attachment2**
filter is run on the message. Because this is an *end*
node, if this filter fails, an exception is thrown.

![Policy with 2 attachment filters](/Images/docbook/images/utility/utility_true_circuit1.gif)

By adding a **True**
filter to the **Attachment2**
filter, this path always ends on a true case, and so does not throw an exception if **Attachment2**
fails.

![Policy with True filter](/Images/docbook/images/utility/utility_true_circuit2.gif)

## Trace filter

The **Trace**
filter outputs the current message attributes to the configured trace destinations. By default, output is traced to the system console.

Configure the following settings:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Include the following text in trace**:
Enter an optional custom text message to include in the trace output.

**Trace Level**:
Select the trace level. `DATA`
is the most verbose level, while `FATAL`
is the least verbose.

**Include Attributes**:
Select this option to trace all current message attributes to the configured trace destination.

**Include Body**:
Select this option to trace the entire message body.

**Indent XML**:
If this option is selected, the XML message is pretty-printed (indented) before it is output to the trace destination.