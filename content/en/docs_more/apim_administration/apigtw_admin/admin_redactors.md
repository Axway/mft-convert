{
"title": "Hide sensitive data",
  "linkTitle": "Hide sensitive data",
  "weight": "70",
  "date": "2019-10-14",
  "description": "Redact sensitive content message content types such as HTTP headers, JSON, XML, HTML form, and plain text."
}
API Gateway enables you to remove sensitive content from messages monitored in the API Gateway Manager web console and traffic monitoring database. You can redact sensitive content message content types such as HTTP headers, JSON, XML, HTML form, and plain text.

For example, sensitive data such as user passwords or credit card details can be redacted from both request and response messages. This means that such sensitive data is only ever present in API Gateway memory during message processing, and is never displayed onscreen or persisted to disk. This is shown in the following architecture diagram:

![Redact sensitive message content from API Gateway](/Images/APIGateway/admin_redaction.png)

## API Gateway redaction configuration

In the API Gateway configuration, message redaction rules are configured by adding a reference to include an XML file, usually called `redaction.xml`, within the `service.xml` file located at `apigateway/groups/GROUP/INSTANCE/conf/service.xml`.

The `redaction.xml` file is usually located at `apigateway/groups/GROUP/INSTANCE/conf/redaction.xml`.

When the configuration is loaded, this creates redactors for the specified message protocol and content. This XML-based configuration uses the following model:

```
<Redaction enabled="true" provider="redactors">
    <HTTPRedactor>...</HTTPRedactor>
    <JSONRedactor>...</JSONRedactor>
    <RawRedactor>...</RawRedactor>
    <XMLRedactor>...</XMLRedactor>
    <FormRedactor>...</FormRedactor>
    <TraceRedactor>...</TraceRedactor>
</Redaction >
```

The `HTTPRedactor` element is required because it lists which URLs redaction rules are applied to. The other redactor types are optional.

During the transaction processing, for each traffic monitoring stream, a chain of redactors is created for redacting the received and sent data. Each redactor removes any sensitive data that it finds and passes the data for the next redactor for processing. The redacted content is then written to the traffic monitoring database.

Each redactor defines its supported content types in `RedactMime` child elements. For example, the following shows content types for a JSON redactor:

```
<JSONRedactor>
    <RedactMime mimeType="application/json"/>
    <RedactMime mimeType="text/json"/>
     ...
</JSONRedactor>
```

You can specify the following properties in the `XMLRedactor` tag:

| Name          | Type   | Default value | Description                                           |
| ------------- | ------ | ------------- | ----------------------------------------------------- |
| maxBufferSize | number | 32768         | Maximum memory size (in bytes) used by XML redaction. |
| maxDepth      | number | 1024          | Maximum depth of XML nested nodes.                    |

For example:

```
<XMLRedactor maxBufferSize="32768" maxDepth="1024">
   <RedactMime mimeType="application/xml"/>
   ...
</XMLRedactor>
```

If an error occurs during the redaction process, including `maxBufferSize` or `maxDepth` reached, the XML redactor will redact the rest of the XML data being processed to avoid writing sensitive data to the logs.

## Enable redaction for an API Gateway

To enable redaction for an API Gateway instance, perform the following steps:

1. Copy the sample redaction configuration file from the following directory:

   ```
   apigateway/samples/redaction/sample_redaction.xml
   ```
2. Copy to the following directory:

   ```
   apigateway/groups/GROUP/INSTANCE/conf/redaction.xml
   ```
3. Ensure that redaction is enabled in `redaction.xml` as follows:

   ```
   <ConfigurationFragment>
   <Redaction enabled="true" provider="redactors">
   ...
   </ConfigurationFragment>
   ```
4. You can customize this file to configure redactors for different message payloads (HTTP, JSON, HTML form, and plain text), but you must ensure that you have an `HTTPRedactor` that specifies which URLs redaction will be applied to. This is described in the next sections.
5. Edit the following file:

   ```
    apigateway/groups/GROUP/INSTANCE/conf/service.xml
   ```
6. Add the following line at the end of the file:

   ```
   <NetService provider="NetService">
      ...
      <include file="$VINSTDIR/conf/redaction.xml"/>
   </NetService>
   ```
7. Restart the API Gateway instance.

## Redact HTTP message content

Within the redaction configuration, you must specify an `HTTPRedactor` with the URLs you wish to enable redaction on. Redaction rules are not processed for URLs not listed in one of the `HTTPURL` values in the `HTTPRedactor`. The other redactors have no effect if an `HTTPRedactor` is not defined.

You can redact any HTTP header or parameter value from the API Gateway message stream based on HTTP URLs specified in configuration. This applies to both HTTP requests and responses. The following shows a simple example configured in `redaction.xml`:

```
<HTTPRedactor>
   <HTTPURL value="/payment"/>
   <HTTPParam value="credit_card"/>
   <HTTPParam value="password"/>
   <HTTPHeader value="Authorization"/>
</HTTPRedactor>
```

This example specifies to remove the `credit_card` and `password` query string parameters and `Authorization` header from messages sent to and from the `/payment` URL.

### URL path matching

Each `HTTPURL` value is used to match URL paths, and to determine if the redaction applies to the transaction. You can use the `match` attribute to specify a match for an exact URL path or for a URL prefix. The following example shows an exact URL path match:

```
<HTTPURL value="/secure_folder" match="exact"/>
```

In this exact match example:

* `/secure_folder` matches
* `/secure_folder/` does not match
* `/secure_folder/123` does not match

The following example shows a URL prefix match:

```
<HTTPURL value="/creditcard/" match="prefix"/>
```

In this prefix match example:

* `/creditcard/` matches
* `/creditcard/charge` matches
* `/creditcard/charge/1234` matches
* `/creditcard` does not match

`HTTPURL` values are also case sensitive. For example:

```
<HTTPURL value="/ORDER/shiptoaddress"/>
```

This is different from:

```
<HTTPURL value="/order/shiptoaddress"/>
```

Finally, to define an `HTTPURL` that matches everything, use the following:

```
<HTTPURL value="/" match="prefix"/>
```

### Supported HTTP features

HTTP features such as the following are supported:

* Chunked transfer encoding
* Multipart body entities (`Content-Type:multipart/`)

#### Example: Redact an HTTP Basic authorization header

This section shows an end-to-end example of redacting an HTTP Basic authorization header. Given the following HTTP request message:

```
GET /securefiles/ HTTP/1.1
Host:www.httpwatch.com
Authorization:Basic aHR0cHdhdGNoOmY=
```

And the following HTTP redactor configuration:

```
<HTTPRedactor>
   <HTTPURL value="/securefiles/" match="exact"/>
   <HTTPHeader value="Authorization"/>
</HTTPRedactor>
```

The HTTP message is redacted and stored in the traffic monitoring database as follows:

```
GET /securefiles/ HTTP/1.1Host:www.httpwatch.com
```

## Redact JSON message content

You can redact JSON content from a message by configuring a specific path to be removed. You can define a relative or absolute location for elements inside a JSON document. When you configure a specific path in the JSON redactor configuration, all elements found in that element are removed.The following general syntax is used to remove JSON content:

```
rule = path_seg [“.”path_seg]*
path_seg = wildcard/rel_wildcard/identifier/array_elem
array_elem = identifier ”[”wildcard/number“]”
identifier = char*
wildcard = “*”
rel_wildcard = “**”
```

The following simple examples show how this syntax works:

```
ca.b.c, a.*.c, a.**.c, **.b.c,**.b[0].c,**.b[*].c, *.b[0].*
```

This results in the following configuration model:

```
<JSONRedactor>
   <RedactMime mimeType="text/json"/>
    ...
   <JSONPath path="a.b.c"/>
   <JSONPath path="**.b[0].c"/>
</JSONRedactor>
```

### JSON redactor configuration

The following shows a simple example from `redaction.xml`:

```
<JSONRedactor>
   <RedactMime mimeType="application/json"/>
   <JSONPath path="**.subject[0].id"/>
</JSONRedactor>
```

This example removes JSON content such as the following:

```
authentication.subject[0].id
cert.subject[0].id
attribute.subject[0].id
```

#### Example: Redact OAuth message tokens from a JSON message

This section shows an end-to-end example of redacting an OAuth message token. Given the following JSON request message:

```
{
   "access_token":"2YotnFZFEjr1zCsicMWpAA",
   "token_type":"example",
   "expires_in":3600,
   "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
   "example_parameter":"example_value"
}
```

And the following JSON redactor configuration:

```
<JSONRedactor>
    <RedactMime mimeType="application/json"/>
    <JSONPath path="**.access_token"/>
    <JSONPath path="**.refresh_token"/>
</JSONRedactor>
```

The JSON message is redacted and stored in the traffic monitoring database as follows:

```
{
  "access_token":null,
  "token_type":"example",
  "expires_in":3600,
  "refresh_token":null,
  "example_parameter":"example_value"
}
```

## Redact XML message content

You can redact specific XML content from a message by configuring XML elements or attributes to be removed. For example, to redact all the children of an element named `axway:sensitive_data`, where `xmlns:axway` is `axway.com/`, you can use the following syntax:

```
<XMLRedactedElement localname=”sensitive_data” namespace=”http://axway.com”
  redactionDisposition=”redactChildren”/>
```

You can specify the following XML redaction directives:

* `redactChildren`: Removes all children of a specified element
* `redactElement`: Redacts the specified element and all its descendants
* `redactText`: Removes all text nodes from the specified element
* `redactDescendants`: Redacts children and text descendants of the specified node
* `redactAttributes`:  Removes the specified attributes

The `redactAttributes` directive is the default value if `redactionDisposition` is not specified in the `XMLRedactedElement` configuration node.

If you need to redact attributes of the specified node, you can configure this using `XMLRedactedAttribute` (child of `XMLRedactedElement`). `XMLRedactedElement` has two mandatory attributes, `localname` and `namespace`, which have the same meaning for `XMLRedactedAttribute`.

An empty XML namespace name is the same as the default document namespace.

### XML redactor configuration

The following example from `redaction.xml` removes all children from`a_namespace:a_name`. It also removes the `an_attribute_name` and `another_attribute_name` attributes:

```
<XMLRedactor>
   <RedactMime mimeType="application/xml"/>
   <RedactMime mimeType="text/xml"/>
   <!--Remove children of a_namespace:a_name and some attributes-->
   <XMLRedactedElement localname="a_name" namespace="a_namespace"
     redactionDisposition="redactChildren">
     <XMLRedactedAttribute localname="an_attribute_name" namespace="an_attribute_namespace"/>
     <XMLRedactedAttribute localname="another_attribute_name" namespace="o"/>
   </XMLRedactedElement>
</XMLRedactor>
```

The following example removes the `b:a` element and all its children:

```
<XMLRedactor>
   <RedactMime mimeType="application/xml"/>
   <RedactMime mimeType="text/xml"/>
   <!---Remove element b:a and all its descendants-->
   <XMLRedactedElement localname="a" namespace="b" redactionDisposition="redactElement"/>
</XMLRedactor>
```

#### Example: Redact a WS-Security username token from an XML message

This section shows an end-to-end example of redacting a WS-Security user name token. Given the following XML request message:

```
<?xml version="1.0" encoding="UTF-8"?>
<Envelope xmlns="http://www.w3.org/2003/05/soap-envelope">
 <Header>
  <Security xmlns="http://docs.oasis-open.org/wss/2004/01/
    oasis-200401-wss-wssecurity-secext-1.0.xsd">
    <UsernameToken>
      <Username>root</Username>
      <Password Type="http://docs.oasis-open.org/wss/2004/01/
         oasis-200401-wss-username-token-profile-1.0#PasswordDigest">EVfkjdkljla=
      </Password>
      <Nonce>tKUH8ab3Rokm4t6IAlgcdg9yaEw=</Nonce>
      <Created xmlns="http://docs.oasis-open.org/wss/2004/01/
         oasis-200401-wss-wssecurity-utility-1.0.xsd">2010-08-10T10:52:42Z
      </Created>
    </UsernameToken>
  </Security>
 </Header>
 <Body>
  <SomeRequest xmlns="http://example.ns.com/foo/bar" />
 </Body>
</Envelope>
```

And the following XML redactor configuration:

```
<XMLRedactor>
    <RedactMime mimeType="text/xml" />
    <XMLRedactedElement localname="UsernameToken"
        namespace="http://docs.oasis-open.org/wss/2004/01/
          oasis-200401-wss-wssecurity-secext-1.0.xsd
          "redactionDisposition="redactChildren">
    </XMLRedactedElement>
</XMLRedactor>
```

The XML message is redacted and stored in the traffic monitoring database as follows:

```
<?xml version="1.0" encoding="UTF-8"?>
<Envelope xmlns="http://www.w3.org/2003/05/soap-envelope">
 <Header>
  <Security xmlns="http://docs.oasis-open.org/wss/2004/01/
    oasis-200401-wss-wssecurity-secext-1.0.xsd">
      <UsernameToken></UsernameToken>
  </Security>
 </Header>
 <Body>
  <SomeRequest xmlns="http://example.ns.com/foo/bar" />
 </Body>
</Envelope>
```

## Redact HTML form message content

You can redact the content of specific HTML form fields by configuring the fields to be removed. The following shows an example from `redaction.xml`:

```
<FormRedactor>
    <RedactMime mimeType="application/x-www-form-urlencoded"/>
    <FormField value="credit_card"/>
    <FormField value="phone_number"/>
</FormRedactor>
```

This example removes the contents of the `credit_card` and `phone_number` form fields from the message.

Supported HTML form content types are as follows:

* `application/x-www-form-urlencoded`
* `multipart/formdata`

## Redact raw message content

You can redact specific plain text by configuring regular expressions to define content to be removed. The following shows a configuration example:

```
<RawRedactor>
     <RedactMime mimeType="text/plain"/>
     <Regex exp="creditcard\s*=\s*(\d{16})" redact="1" icase="true"/>
     <Regex exp="source:\b(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})\b" redact="1,2" icase="true"/>
     <Regex exp="\d{16}" redact="0" icase="false" />
</RawRedactor>
```

In this configuration model, the `Regex` element includes the following attributes to define the redactor behavior:

* `exp`: Regular expression used to match the desired content. Possible values are valid regular expressions.
* `redact`: Specifies which groups in the match are redacted. Possible values are comma-separated lists of group indexes (for example, `1` or `1,2` or `4,6,7`, and so on). You can specify `0` to redact the entire match.
* `icase`: Specifies whether the match is case insensitive. Possible values are `true` (case insensitive) and `false` (case sensitive).

### Example: Redact credit card details from raw text

This section shows some configured regular expressions and the behavior with specific raw message content. The following expression specifies to redact a defined group:

```
<Regex exp="creditcard\s*=\s*(\d{16})" redact="1" icase="false"/>
```

The following shows example message content and the behavior with this expression:

|Message content|Behavior|
|---------------|--------|
|`&creditcard=1234123412341234`|Content matches expression. Defined group 1 `(\d{16})` is redacted (in this case, `1234123412341234`).|

The following expression specifies to redact multiple defined groups:

```
<Regex exp="ccdigits:(\d{1,4})\.(\d{1,4})\.(\d{1,4})\.(\d{1,4})" redact="1,2,3" icase="false"/>
```

The following shows example message content and the behavior with this expression:

|Message content|Behavior|
|---------------|--------|
|`ccdigits:1234.2345.3456.4567`|Content matches expression. Defined groups 1 `(\d{1,4})`, 2 `(\d{1,4}))`, and 3 `(\d{1,4})` are redacted (in this case `1234`, `2345`, and `3456`. Defined group 4 `(\d{1,4})` is left intact (in this case `4567`).|

The following expression specifies to redact content using case insensitivity:

```
<Regex exp="creditcard\s*=\s*(\d{16})" redact="1" icase="true"/>
```

The following shows example message content and the behavior with this expression:

|Message content|Behavior|
|---------------|--------|
|`credit card 123456781234567`|Content matches expression. Entire match (`credit card 1234567812345678`) is redacted.|
|`Credit Card 1234567812345678`|Content matches expression because of the `icase` attribute. Entire match (`Credit Card 1234567812345678`) is redacted.|

## Redact trace log records

You can redact API Gateway trace log records by configuring regular expressions to define content to be removed at a trace level. This configuration applies to trace records equal or lower than the level configured. For example, `INFO` level redacts messages at levels `INFO`, `DEBUG`, and `DATA`. For details on trace levels, see [Configure API Gateway diagnostic trace](/docs/apim_administration/apigtw_admin/tracing).

The following shows a configuration example, at an `INFO` trace level, which uses `Regex` elements already defined in the [Redact raw message content](#redact-raw-message-content) section.

```
<TraceRedactor level="INFO">
        <Regex exp="creditcard\s*=\s*(\d{16})" redact="1" icase="true"/>
        <Regex exp="source:\b(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})\b" redact="1,2" icase="true"/>
        <Regex exp="\d{16}" redact="0" icase="false" />
</TraceRedactor>
```

Redaction for trace log records works as follows:

* Applies for a single trace log record.
* Executes sequentially in the order the regular expressions are declared. For example, when an expression matches, the matching parts are removed and the resulting string will be the input for the next regular expression.
* Replaces only the `text/data` part of the record. For example, the prefix `LEVEL date [ids]` is not taken into account.
* Most of the startup messages are not redacted because redaction only starts when the service is loaded.

## Redact sensitive data from log files

For details on how to redact sensitive data from domain audit log and access log files, see the following topics:

* [Configure API Gateway logging and events](/docs/apim_administration/apigtw_admin/logging)
* [Transaction access log settings](/docs/apim_reference/log_global_settings/#transaction-access-log-setting)

## Disable redaction for an API Gateway

To disable redaction, modify `redaction.xml` as follows:

```
<ConfigurationFragment>
<Redaction enabled="false" provider="redactors">
...
</ConfigurationFragment>
```