{
"title": "Complex selector expressions",
  "linkTitle": "Complex selector expressions",
  "weight": "60",
  "date": "2019-11-27",
  "description": "Use complex selector expressions to access configuration values dynamically at runtime."
}

You can access configuration values dynamically at runtime using *selectors*. A selector is a special syntax that enables API Gateway configuration settings to be evaluated and expanded at runtime, based on metadata values (for example, from message attributes, a Key Property Store (KPS), or environment variables).

For example, when a HTTP request is received, it is converted into a set of message attributes. Each message attribute represents a specific characteristic of the HTTP request, such as the HTTP headers, HTTP body, and so on.

## Example selector expressions

For an explanation of selector syntax and some examples of selector expressions, see [Select configuration values at runtime](/docs/apim_policydev/apigw_poldev/general_selector/). The following lists some more complex examples.

### KPS with multiple read keys

```
${kps.matrix.row.column}
${kps.matrix[“row”][“column”]}
```

For a KPS with multiple read keys, the values for each key are provided in order. The result of the expression is also indexable:

```
set property test = ${kps.matrix.row}
```

`${test[“column”]}` looks up the KPS for `[row/column]`.

### Parameter from content body

```
${content.body.getParameters().get("grant_type")}
```

Gets the HTTP form post field called `grant_type`.

### Parameter from JSON content body

```
${content.body.getJSON().findValue('access_token').textValue()}  
```

If a body is of type `application/json` then it is automatically treated as a `com.vordel.mime.JSONBody`. A `JSONBody` object returns an `com.fasterxml.jackson.databind.JsonNode` object via a `getJSON()` call.

For more information, see the [Javadoc for JsonNode class](http://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.9.8/index.html?com/fasterxml/jackson/databind/JsonNode.html).

For example, if the body contains the following JSON content:

```
{
  "access_token":"2YotnFZFEj",
  "token_type":"example",
  "expires_in":3600
}
```

this selector results in the value `2YotnFZFEj`.

### Convert JSON content body to string

```
${content.body.getJSON().toString()}
```

If a body is of type `com.vordel.mime.JSONBody`, this selector converts the JSON contained in the body to a string value.

### Environment variable

```
${environment.VINSTDIR}
```

Accesses the environment variable `VINSTDIR`.

### HTTP path

```
${http.path[2]}
```

If you have the filter **Extract REST Request Attributes** in your policy, this filter adds the incoming URI to the message whiteboard as a String array, so that you can index into the path. If the incoming path is `/thisisa/test`, using this type of selector results in the following attributes on the whiteboard:

  ```
  ${http.path[1]} = thisisa
  ${http.path[2]} = test
  ```

### Database query results

You can use the **Retrieve from or write to database** filter to retrieve user attributes from a database or write user attributes to a database. You can select whether to place database query results in message attributes on the **Advanced** tab of the filter. By default, the **Place query results into user attribute list** option is selected.

The query results are represented as a list of properties. Each element in the list represents a query result row returned from the database. These properties represent pairs of attribute names and values for each column in the row. The **Prefix for message attribute** field in the filter is required to name the list of returned properties (for example, `user`).

#### Results in user attribute list

The following table shows some example selectors when the option **Place query results into user attribute list** is selected.

| Selector expression    | Result                 |
|------------------------|------------------------|
| `${user[0].NAME}`| John|
| `${user[0].LASTNAME}` |Kennedy|
| `${user[1].NAME}`| Brian|
| `${user[1].LASTNAME}`                     | O’Connor   |

You can also use standard Java function calls on the attributes. For example:

* `${user.size()}` – Number of properties (number of rows) retrieved from the database
* `${user[0].NAME.equals(“John”)}` – Returns true if the `NAME` attribute (value of column `NAME` in first row) is `“John”`

For more information, see the `java.util.ArrayList` and `java.lang.String` class interfaces.

#### Results not in user attribute list

The following table shows some example selectors when the option **Place query results into user attribute list** is not selected.

| Selector Expression    | Result                 |
|------------------------|------------------------|
| ```${user.NAME[0]}```|John  |
| ```${user.LASTNAME[0]}```|   Kennedy|
|```${user.NAME[1]}```   |  Brian |
|```${user.LASTNAME[1]}```  |  O’Connor |

You can also use standard Java function calls on the attributes. For example:

* `${user.NAME.size()}` – Number of `NAME` attributes (number of rows with column `NAME`) retrieved from the database
* `${user.NAME[0].equals(“John”)}` – Returns true if the first `NAME` attribute (value of column `NAME` in first row) is `“John”`

For more information, see the `java.util.ArrayList` and `java.lang.String` class interfaces.

### LDAP directory server search results

You can use the **Retrieve from directory server** filter to retrieve user profile data.

The filter can look up a user and retrieve that user's attributes represented as a list of search results. Each element of the list represents a list of multivalued attributes returned from the directory server. The **Prefix for message attribute field** in the filter is required to name the list of search results (for example, `user`).

The following table shows some example selectors:

| Selector Expression    | Result                 |
|------------------------|------------------------|
| ```${user[0].memberOf[0]}```| ```CN=Operator,OU=Sales```|
| ```${user[0].memberOf[1]}```| ```CN=Developer,OU=Dev``` |
| ```${user[0].memberOf[2]}```| ```CN=Operator,OU=Support``` |
| ```${user[1].memberOf[0]}```| ```CN=Operator,OU=Sales```|

You can also use standard Java function calls on the attributes. For example:

* `${user.size()}` – Number of search results returned by the LDAP directory server
* `${user[0].memberOf.size()}` – Number of `memberOf` attribute values returned in the search result
* `${user[0].memberOf.contains(“CN=Operator,OU=Sales”)}` – Returns true if one of the returned `memberOf` attributes is `“CN=Operators,OU=Sales”`
* `${user[0].memberOf[0].equals(“CN=Operator,OU=Sales”)}` – Returns true if the first `memberOf` attribute is `“CN=Operators,OU=Sales”`

For more information, see `java.util.ArrayList` and `java.lang.String` class interfaces.

## More information on KPS

A Key Property Store (KPS) is an external data store of API Gateway policy properties, which is typically read frequently, and seldom written to. Using a KPS enables metadata-driven policies, whereby policy configuration is stored in an external data store, and looked up dynamically when policies are executed.

For more information on KPS, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/apigw_poldev/) and also the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).
