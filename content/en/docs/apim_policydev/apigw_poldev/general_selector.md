{
"title": "Select configuration values at runtime",
"linkTitle": "Select configuration values at runtime",
"weight": 15,
"date": "2019-10-17",
"description": "A selector is a special syntax that enables API Gateway configuration settings to be evaluated and expanded at runtime based on metadata values (for example, from message attributes, a Key Property Store (KPS), or environment variables)."
}

The selector syntax uses the Java Unified Expression Language (JUEL) to evaluate and expand the specified values. Selectors provide a powerful feature when integrating with other systems or when customizing and extending the API Gateway.

When you press the **Shift**
key and open a filter, you can edit all filter settings using text values in the advanced filter view. This means that you can specify all filter fields using the API Gateway selector syntax. For more details on the advanced view, see [Advanced options](/docs/apim_policydev/apigw_poldev/general_ps_settings/#advanced-options).

## Selector syntax

The API Gateway selector syntax uses JUEL to evaluate and expand the following types of values at runtime:

* Message attribute properties configured in message filters, for example:

```
${authentication.subject.id}
```

* Environment variables specified in `envSettings.props`
    and `system.properties`
    files, for example:

```
${env.PORT.MANAGEMENT}
```

* Values stored in a KPS table, for example:

```
${kps.CustomerProfiles[JoeBloggs].age}
```

{{< alert title="Note" color="primary" >}}Do not use hyphens (`-`) in selector expressions. Hyphens are not supported by the Java-based selector syntax. You can use underscores (`_`) instead. {{< /alert >}}

### Access selector fields

A message attribute selector can refer to a field of that message (for example `certificate`), and you can use `.`
characters to access subfields. For example, the following selector expands to the `username`
field of the object stored in the `profile`
attribute in the message:

```
${profile.username}
```

You can also access fields indirectly using square brackets (`[`
and `]`). For example, the following selector is equivalent to the previous example:

```
${profile[field]}
```

You can specify literal strings as follows:

```
${profile["a field name with spaces"]}
```

For example, the following selector uses the `kathy.adams@acme.com`
key value to look up the `User`
table in the KPS, and returns the value of the `age`
property:

```
${kps.User["kathy.adams@acme.com"].age}
```

{{< alert title="Note" color="primary" >}}For backwards compatibility with the `.`
spacing characters used in previous versions of the API Gateway, if a selector fails to resolve with the above rules, the flat, dotted name of a message attribute still works. For example, `${content.body}`
returns the item stored with the `content.body`
key in the message.{{< /alert >}}

### Special selector keys

The following top-level keys have a special meaning:

`kps`: Subfields of the `kps` key reflect the alias names of KPS tables in the API Gateway group. Further indexes represent properties of an object in a table (for example, `${kps.User["kathy.adams@acme.com"].age}`).

`env`, `system`: In previous versions, fields from the `envSettings.props` and `system.properties` files had restrictions on prefixes. The selector syntax does not require the `env`and `system` prefixes in these files. For example, `${env.` selects settings from `envSettings.props`, and the rest of the selector chooses any properties in it. However, for compatibility, if a setting in either file starts with this prefix, it is stripped away so the selectors still behave correctly with previous versions.

### Resolve selectors

Each `${...}`
selector string is resolved step-by-step, passing an initial context object (for example, `Message`). The top-level key is offered to the context object, and if it resolves the field (for example, the message contains the named attribute), the resolved object is indexed with the next level of key. At each step, the following rules apply:

1. At the top level, test the key for the global values (for example, `kps`, `system`, and `env`) and resolve those specially.
2. If the object being indexed is a Dictionary, KPS, or Map, use the index as a key for the item’s normal indexing mechanism, and return the resulting lookup.
3. If all else fails, attempt Java reflection on the indexed object.

{{< alert title="Note" color="primary" >}}Method calls are currently only supported using Java reflection. There are currently no supported functions as specified by the Unified Expression Language (EL) standard. For more details on JUEL, see `http://juel.sourceforge.net/`.{{< /alert >}}

## Example selector expressions

This section lists some example selectors that use expressions to evaluate and expand their values at runtime.

### Message attribute

The following message attribute selector returns the HTTP `User-Agent`
header:

```
${http.headers["User-Agent"]}
```

For example, this might expand to the following value:

```
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/535.7 (KHTML, like Gecko) Chrome/16.0.912.77 Safari/535.7
```

### Environment variable

In a default configuration, the following environment variable selector returns port 8091:

```
${env.PORT.MANAGEMENT + 1}
```

### Key Property Store

The following selector looks up a KPS table with an alias of `User`:

```
${kps.User[http.querystring.id].firstName}
```

This selector retrieves the object whose Policy Studio key value is specified by the `id`
query parameter in the incoming HTTP request, and returns the value of the `firstName`
property in that object.

The following selector explicitly provides the key value, and returns the value of the `age`
property:

```
${kps.User["kathy.adams@acme.com"].age}
```

In this example, the ASCII `"`
character is used to delimit the key string.

The following selector looks up a KPS table with a composite secondary key of `firstName,lastName`:

```
${kps.User[http.querystring.firstName][http.querystring.lastName].email}
```

In this example, the key values are received as query parameters in the incoming HTTP request. The selector returns the value of the `email`
property from the resulting object.

### Examples using reflection

The following message attribute selector returns the CGI argument from an HTTP URL (for example, returns `bar`
for `http://localhost/request.cgi?foo=bar`):

```
${http.client.getCgiArgument("foo")}
```

This returns the name of the top-level element in an XML document:

```
${content.body.getDocument().getDocumentElement().getNodeName()}
```

This returns `true` if the HTTP response code lies between 200 and 299:

```
${http.response.status >= 200 && http.response.status <= 299}
```

{{< alert title="Tip" color="primary" >}}You can use the **Trace**
filter to determine the appropriate selector expressions to use for specific message attributes. When configured after another filter, the **Trace** filter outputs the available message attributes and their Java type (for example, `Map`
or `List`). For details on `com.vordel` classes, see the [API Gateway Javadoc](https://support.axway.com/htmldoc/1444954)
. For example, for the `OAuth2AccessToken`
class, you can use selector expressions such as `${accesstoken.getAdditionalInformation()}`.{{< /alert >}}

## Extract message attributes

There are a number of API Gateway filters that extract message attribute values (for example, **Extract Certificate Attributes**
and **Retrieve from HTTP Header**). Using selectors to extract message attributes offers a more flexible alternative to using such filters. For more details on using selectors instead of these filters, contact Axway Support.
