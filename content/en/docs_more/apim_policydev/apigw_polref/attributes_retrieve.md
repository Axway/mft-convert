{
"title": "Retrieve attribute filters",
  "linkTitle": "Retrieve attribute filters",
  "weight": 25,
  "date": "2019-10-17",
  "description": "Filters to retrieve message attributes."
}
<!-- TODO reorder based on usage from GA-->

## Retrieve attributes with JSON path filter

[JSON Path](http://code.google.com/p/jsonpath/) is an XPath like query language for JSON (JavaScript Object Notation) that enables you to select nodes in a JSON document. The **Retrieve Attributes with JSON Path** filter enables you to retrieve specified message attributes from a JSON message using JSON Path expressions.

### Configure retrieve attributes with JSON path

Configure the following fields on the **Retrieve Attributes with JSON Path** filter window:

**Name**: Enter an appropriate name for this filter to display in a policy.

**Extract attributes using the following JSON Path expressions**: Specify the list of attributes for the API Gateway to retrieve using appropriate JSON Path expressions.

To add an attribute to the list, click the **Add** button, and enter the following values in the dialog:

* **Attribute name**:   Enter the message attribute name that you wish to extract using JSON Path (for example, `bicycle.price`).
* **JSON Path Expression**:   Enter the JSON Path expression that you wish to use to extract the message attribute (for example, `$.store.bicycle.price`). Policy Studio prompts if you enter an unsupported JSON Path expression.
* **Unmarshal as**:   Enter the data type to unmarshal the message attribute value as (defaults to `java.util.List`). For example, typical data types to unmarshal as include the following:

    * `java.lang.String`: Enter this value when using this filter to extract a single value and store it in an API Gateway attribute on the message whiteboard.
    * `com.fasterxml.jackson.databind.JsonNode`: Enter this value when using this filter to store the content in a message attribute and then place it in a JSON message using a **JSON Add Node** filter.
* **Fail if JSON Path Fails**:   Select whether the filter should fail if the specified JSON Path expression fails. This option is not selected by default.

{{< alert title="Note" color="primary" >}}If no attributes are specified, the API Gateway retrieves all the attributes in the message and sets them to the `attribute.lookup.list`
attribute.{{< /alert >}}

### JSON Path examples

The following are some examples of using the **Retrieve Attributes with JSON Path** filter to retrieve data from a JSON message.

#### Retrieving attributes

The following example retrieves three different data items from the JSON message and stores them in the specified message attributes as strings:

![Retrieve Attribute with JSON Path Expression](/Images/docbook/images/json/json_path_add_attribute.png)

When the extracted attributes are added to the `content.body` message attribute, the following example shows the corresponding request and response message in Axway API Tester:

![Retrieved Attribute](/Images/docbook/images/json/json_path_add_attribute_sb.png)

#### Retrieving multiple attributes in a list

The following example retrieves all the authors from the JSON message and stores them in the specified message attribute as a `List`:

![Retrieve Attribute List using JSON Path Expression](/Images/docbook/images/json/json_path_add_attribute_list.png)

The following example shows the corresponding request and response in Axway API Tester:

![Retrieved Attribute List](/Images/docbook/images/json/json_path_add_attribute_list_sb.png)

### Exceptions

The **Retrieve Attributes with JSON Path** filter aborts with a `CircuitAbortException` if the content body of the payload is not in valid JSON format.

## Retrieve from HTTP header filter

The **Retrieve from HTTP Header** attribute retrieval filter can be used to retrieve the value of an HTTP header and set it to a message attribute. For example, this filter can retrieve an X.509 certificate from a `USER_CERT` HTTP header, and set it to the `authentication.cert` message attribute. This certificate can then be used by the filter's successors. The following HTTP request shows an example of such a header:

```
POST /services/getEmployee HTTP/1.1
Host:localhost:8095
Content-Length:21
SOAPAction:HelloService
USER_CERT:MIIEZDCCA0 ...9aKD1fEQgJ
```

You can also retrieve a value from a named query string parameter and set this to the specified message attribute. The following example shows a request URL that contains a query string:

```
http://hostname.com/services/getEmployee?first=john&last=smith
```

In the above example, the query string is `first=john&last=smith`. As is clear from the example, query strings consist of attribute name-value pairs. Each name-value pair is separated by the `&`character.

The following fields are available on the **Retrieve from HTTP Header** filter configuration window:

**Name**: Enter an appropriate name for this filter to display in a policy.

**HTTP Header Name**: Enter the name of the HTTP header contains the value that we want to set to the message attribute.

**Base64 Decode**: Check this box if the extracted value should be Base64 decoded before it is set to the message attribute.

**Use Query String Parameters**: Select this setting if the API Gateway should attempt to extract the **HTTP Header Name** from the query string parameters instead of from the HTTP headers.

**Attribute ID**: Finally, select the attribute used to store the value extracted from the request.

## Retrieve attribute from SAML attribute assertion filter

A SAML (Security Assertion Markup Language) attribute assertion contains information about a user in the form of a series of attributes. The **Retrieve from SAML Attribute Assertion** filter can retrieve these attributes and store them in the `attribute.lookup.list` message attribute.

The following SAML attribute assertion contains three attributes, `"role"`, `"email"`, and `"dept"`. The **Retrieve from SAML Attribute Assertion** filter stores all three attributes and their values in the `attribute.lookup.list` message attribute.

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsi="http://www.w3.org/2000/10/XMLSchema-instance">
    <soap:Header>
      <wsse:Security>
        <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
          ID="Id-0000010a3c4ff12c-0000000000000002"
          IssueInstant="2006-03-27T15:26:12Z" Version="2.0">
        <saml:Issuer Format="urn:oasis ... WindowsDomainQualifiedName">
          TestCA
        </saml:Issuer>
        <saml:Subject>
          <saml:NameIdentifier Format="urn:oasis ... WindowsDomainQualifiedName">
            TestUser
          </saml:NameIdentifier>
        </saml:Subject>
        <saml:Conditions NotBefore="2005-03-27T15:20:40Z"
          NotOnOrAfter="2028-03-27T17:20:40Z"/>
        <saml:AttributeStatement>
          <saml:Attribute Name="role" NameFormat="http://www.axway.com">
            <saml:AttributeValue>admin</saml:AttributeValue>
          </saml:Attribute>
          <saml:Attribute Name="email" NameFormat="http://www.axway.com">
            <saml:AttributeValue>joe@axway.com</saml:AttributeValue>
          </saml:Attribute>
          <saml:Attribute Name="dept" NameFormat="">
            <saml:AttributeValue>engineering</saml:AttributeValue>
          </saml:Attribute>
        </saml:AttributeStatement>
        </saml:Assertion>
      </wsse:Security>
    </soap:Header>
    <soap:Body>
      <product>
        <name>API Gateway</name>
        <company>Axway</company>
        <description>Web Services Security</description>
      </product>
    </soap:Body>
</soap:Envelope>
```

### Configure details settings

The following fields are available on the **Details** tab:

**SOAP Actor/Role**: If you expect the SAML assertion to be embedded within a WS-Security block, you can identify this block by specifying the SOAP Actor or Role of the WS-Security header that contains the assertion.

**XPath Expression**: Alternatively, if the assertion is not contained within a WS-Security block, you can enter an XPath expression to locate the attribute assertion. XPath expressions can be added by selecting the **Add** button. Expressions can be edited and deleted by selecting an XPath expression and clicking the **Add** and **Delete** buttons respectively.

**SAML Namespace**: Select the SAML namespace that must be used on the SAML assertion for this filter to succeed. If you do not wish to check the namespace, select the `Do not check version` option from the list.

**SAML Version**: Enter the SAML version that the assertion must adhere to by entering the major version in the first field, followed by the minor version in the second field. For example, for SAML 2.0, enter `2` in the first field and `0` in the second field.

**Drift Time**: When the API Gateway receives a SAML attribute assertion, it first checks to make sure that it has not expired. The lifetime of the assertion is specified using the `NotBefore` and `NotOnOrAfter` attributes of the `<Conditions>` element in the assertion itself. The API Gateway makes sure that the time at which it validates the assertion is between the `NotBefore` and `NotOnOrAfter` times.

The **Drift Time** is used to account for differences in the clock time of the machine that generated the assertion and the machine hosting the API Gateway. The time specified here is subtracted from the time at which the API Gateway attempts to validate the assertion.

### Configure trusted issuer settings

You can use the table on this tab to select the issuers that you consider trusted. In other words, this filter only accepts assertions that have been issued by the SAML authorities selected here.

Click the **Add** button to display the **Trusted Issuers** window. Select the Distinguished Name of a SAML authority whose certificate has been added to the certificate store and click the **OK** button. Repeat this step to add more SAML authorities to the list of trusted issuers.

### Configure subject settings

The API Gateway can perform some very basic authentication checks on the subject or sender of the assertion using the options available on the **Subject** tab. The API Gateway can compare the subject of the assertion (for example, the `<NameIdentifier>`) to one of the following values:

* **The subject of the authentication filter**:   Select this option if the user specified in the `<NameIdentifier>`   element must match the user that authenticated to the API Gateway. The subject of the authentication event is stored in the `authentication.subject.id`   message attribute.
* **The following value**:   This option can be used if the `<NameIdentifier>`   must match a user-specified value. Select this radio button and enter the value in the field provided.
* **Neither of the above**:   If the **Neither of the above**   radio button is selected, the API Gateway does not attempt to match the `<NameIdentifier>`   to any value.

### Configure lookup attributes settings

The **Lookup Attributes** tab is used to determine what attributes the API Gateway should extract from the SAML attribute assertion. Extracted attributes and their values are set to the `attribute.lookup.list` message attribute.

The table lists the attributes that the API Gateway extracts from the assertion and set to the `attribute.lookup.list`.

Alternatively, check the **Extract all of the attributes from the SAML assertion** check box to configure the API Gateway to extract all attributes from the assertion. All attributes are set to the `attribute.lookup.list` message attribute.

To configure a specific attribute to look up in the message, click the **Add** button to display the **Attribute Lookup** dialog. Enter the value of the "Name" attribute of the `<Attribute>` element in the **Attribute name** field. Enter the value of the "NameFormat" attribute of the `<Attribute>` element in the **Namespace** field.

## Retrieve attribute from SAML PDP filter

The API Gateway can request information about an authenticated end user in the form of user *attributes* from a SAML PDP (Policy Decision Point) using the SAML Protocol (SAMLP). In such cases, the API Gateway presents evidence to the PDP in the form of some user credentials, such as the Distinguished Name of a client's X.509 certificate.

The PDP looks up its configured user store and retrieves attributes associated with that user. The attributes are inserted into a SAML attribute assertion and returned to the API Gateway in a SAMLP response. The assertion or SAMLP response is usually signed by the PDP.

When the API Gateway receives the SAMLP response, it performs a number of checks on the response, such as validating the PDP signature and certificate, and examining the assertion. It can also insert the SAML attribute assertion into the original message for consumption by a downstream web service.

### Configure request settings

The **Request** tab describes how the API Gateway should package the SAMLP request before sending it to the SAML PDP.

You can configure the following fields on the **Request** tab:

**SAML PDP URL Set**: You can configure a group of SAML PDPs to which the API Gateway connects in a round-robin fashion if one or more of the PDPs are unavailable. This is known as a SAML PDP URL set. Click the button on the right, and select a previously configured SAML PDP URL set in the tree. To add a URL set, right-click the **SAML PDP URL Sets** tree node, and select **Add a URL Set**. Alternatively, you can configure a SAML PDP URL set under the **Environment Configuration** > **External Connections** node in the Policy Studio tree.

**SOAP Action**: Enter the SOAP action required to send SAMLP requests to the PDP. Click the **Use Default** button to use the following default SOAP action as specified by SAMLP:

```
http://www.oasis-open.org/committees/security
```

**SAML Version**: Select the SAML version to use in the SAMLP request.

**Signing Key**: If the SAMLP request is to be signed, click the **Signing Key** button, and select the appropriate signing key from the certificate store.

#### SAML subject settings

You can describe the *subject* of the SAML assertion on the **SAML Subject** tab. Complete the following fields:

**Subject Selector Expression**: Enter a selector expression for the message attribute that contains the user name of an authenticated user. The default value is `${authentication.subject.id}`.

**Subject Format**: Select the format of the subject selected in the **Subject Selector Expression** field above.

There is no need to select a format here if the **Subject Attribute** field is set to `authentication.subject.id`.

#### Subject confirmation settings

The settings on the **Subject Confirmation** tab determine how the `<SubjectConfirmation>` block of the SAML assertion is generated. When the assertion is consumed by a downstream web service, the information contained in the `<SubjectConfirmation>` block can be used to authenticate either the end user that authenticated to the API Gateway, or the issuer of the assertion, depending on what is configured.

The following is a typical `<SubjectConfirmation>` block:

```xml
<saml:SubjectConfirmation>
  <saml:ConfirmationMethod>
    urn:oasis:names:tc:SAML:1.0:cm:holder-of-key
  </saml:ConfirmationMethod>
  <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=axway</dsig:X509SubjectName>
      <dsig:X509Certificate>
        MIICmzCCAY ...... mB9CJEw4Q=
      </dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</saml:SubjectConfirmation>
```

You must configure the following fields on the **Subject Confirmation** tab:

**Method**: The selected value determines the value of the `<ConfirmationMethod>` element. The following table shows the available methods, their meanings, and their respective values in the `<ConfirmationMethod>` element:

* **Holder Of Key**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The `<SubjectConfirmation>` contains a `<dsig:KeyInfo>` section with the certificate of the user selected to sign the SAMLP request. The user selected to sign the SAMLP request must be the authenticated subject (`authentication.subject.id`). Select the **Include Certificate** option if the signer's certificate is to be included in the `SubjectConfirmation` block. Alternatively, select the **Include Key Name** option if only the key name is to be included. Value: `urn:oasis:names:tc:SAML:1.0:cm:holder-of-key`
* **Bearer**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`
* **SAML Artifact**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:artifact`
* **Sender Vouches**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The SAMLP request must be signed by a user. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`

If the **Method** field is left blank, no `<ConfirmationMethod>` block is inserted into the assertion.

**Include Certificate**: Select this option to include the SAML subject's certificate in the `<KeyInfo>` section of the `<SubjectConfirmation>` block.

**Include Key Name**: Alternatively, if you do not want to include the certificate, you can select this option to only include the key name in the `<KeyInfo>` section.

#### Attributes

You can list a number of user attributes to include in the SAML attribute assertion that is generated by the API Gateway. If no attributes are explicitly listed in this section, the API Gateway inserts all attributes associated with the user (all user attributes in the `attribute.lookup.list` message attribute) in the assertion.

To add a specific attribute to the SAML attribute assertion, click the **Add** button. A user attribute can be configured using the **Attribute Lookup** dialog. Enter the name of the attribute that is added to the assertion in the **Attribute name** field. Enter the namespace that is associated with this attribute in the **Namespace** field. You can edit and remove previously configured attributes using the **Edit** and **Remove** buttons.

### Configure response settings

The **Response** tab configures the SAMLP response returned from the SAML PDP. The following fields are available:

**SOAP Actor/Role**: If the SAMLP response from the PDP contains a SAML attribute assertion, the API Gateway can extract it from the response and insert it into the downstream message. The SAML assertion is inserted into the WS-Security block identified by the specified SOAP actor/role.

**Drift Time**: The SAMLP request to the PDP is time stamped by the API Gateway. To account for differences in the times on the machines running the API Gateway and the SAML PDP the specified time is subtracted from the time at which the API Gateway generates the SAMLP request.

## Retrieve attribute from Tivoli filter

You can use the **Retrieve from Tivoli** filter when you need to retrieve user attributes independently from authorizing the user against Tivoli Access Manager for e-business. This filter is found in the **Attributes** category.

For details on prerequisites for integration with IBM Tivoli, see the [API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).

Complete the following fields to configure the **Retrieve from Tivoli** filter:

**Name**: Enter an appropriate name for the filter to display in a policy.

**User ID**: Enter the ID of a user to retrieve attributes for. You can enter a static user name, Distinguished Name (DName), or selector representing a message attribute. The selector is expanded to the value of the message attribute at runtime.

For example, you can enter `${authentication.subject.id}`. This means that the ID of the authenticated user, which is normally a DName, is used to retrieve attributes for. For this to work correctly, you must configure an authentication filter to run before this filter in the policy.

**Attributes**: You can specify a list of user attributes to retrieve from the Tivoli server. To add individual attributes to be retrieved, click **Add** and enter the attributes in the dialog. If you want all attributes to be retrieved, leave the table blank.

**Tivoli Configuration Files**: A Tivoli configuration file that contains all the required connection details is associated with a particular API Gateway instance. Click **Settings** to display the **Tivoli Configuration** dialog. On the **Tivoli Configuration** dialog, select the API Gateway instance which connection details you want to edit. For more details on configuring this wizard, see [Configure Tivoli connections](/docs/apim_policydev/apigw_external_connections/connector_tivoli_connection/).

## Retrieve attribute from message filter

The **Retrieve from Message** filter uses XPath expressions to extract the value of an XML element or attribute from the message and set it to an internal message attribute. The XPath expression can also return a `NodeList`, and the `NodeList` can be set to the specified message attribute.

The following fields are available on the **Retrieve from Message** filter configuration window:

**Name**: Enter an appropriate name for this filter to display in a policy.

**Use the following XPath**: Configure an XPath expression to retrieve the desired content.

Click the **Add** button to add an XPath expression. You can add and remove existing expressions by clicking the **Edit** and **Remove** buttons respectively.

**Store the extracted content**: Select an option to specify how the extracted content is stored. The options are:

* **of the node as text (java.lang.String)**: This option saves the content of the node retrieved from the XPath expression to the specified message attribute as a `String`.
* **for all nodes found as text (java.lang.String)**: This option saves all nodes retrieved from the XPath expression to the specified message attribute as a `String`   (for example, `<node1>test</node1>`). This option is useful for extracting `<Signature>`, `<Security>`, and `<UsernameToken>`   blocks, as well as proprietary blocks of XML from messages.
* **for all nodes found as a list (java.util.List)**: This option saves the nodes retrieved from the XPath expression to the specified message attribute as a Java `List`, where each item is of type `Node`. For example, if the XPath returns `<node1>test</node1>`, there is one node in the `List`   (`<node1>`). The child text node (`test`) is accessible from that node, but is not saved as an entry in the `List`   at the top-level.

**Extracted content will be stored in attribute named**: The API Gateway sets the value of the message attribute selected here to the value extracted from the message. You can also enter a user-defined message attribute.

**Optionally the message payload can be replaced by the extracted content**: Select this option to take the value being set into the attribute and add it to the content body of the response. This option is not selected by default.

**Use the following content type for new payload**: This field is only available if the preceding option is selected. This allows you to specify the content type for the response, based on what is added to the content body.

## Retrieve attribute from database filter

Use the **Retrieve from or write to database** filter to retrieve user attributes from a specified database, or write user attributes to a specified database. API Gateway can do this by running an SQL query on the database, or by invoking a stored procedure call. The query results are represented as a list of properties. Each element in the list represents a query result row returned from the database for the specified SQL query or stored procedure call. These properties represent pairs of attribute names and values for each column in the row.

{{< alert title="Note" color="primary" >}}
It is important to be aware of the following:

* You cannot use a selector expression in a **Retrieve from or write to database** filter to select a table name to make the query dynamic. For example, putting `SELECT * FROM ${my_table} WHERE ...` into the filter will fail.
* All selector expressions (for example `${my_table}`) are turned into parameters for a prepared statement in order to prevent SQL injection attacks. Using selector expressions in places where SQL syntax does not permit parameters (for example, table names) will not work and is not supported.
* You can use an attribute only as a parameter to compare with data stored in a table. For example, `select FirstName from Persons where LastName=${att1};`.

{{< /alert >}}

### Configure database settings

Configure the following fields on the **Database** tab:

**Database Location**: The API Gateway searches the selected database for the user's attributes. Click the browse button to select the database to search. To use an existing database connection (for example, `Default Database Connection`), select it in the tree. To add a database connection, right-click the **Database Connections** tree node, and select **Add DB connection**. Alternatively, you can add database connections under the **Environment Configuration** > **External Connections** node in the Policy Studio tree view. For more details, see [Configure database connections](/docs/apim_policydev/apigw_external_connections/common_db_conf/).

**Database Statements**: The **Database Statements** table lists the currently configured SQL queries or stored procedure calls. These queries and calls retrieve certain user attributes from the database selected in the **Database Location** field. You can edit and delete existing queries by selecting them from the list and clicking the **Edit** and **Delete** buttons. Queries are executed in the order they are listed in the filter. You can change the order of execution using the **Up** and **Down** buttons. For details, see [Configure database queries](/docs/apim_policydev/apigw_external_connections/common_db_conf/#configure-database-queries).

### Configure advanced settings

On the **Advanced** tab, configure the following fields in the **User Attribute Extraction** section:

**Place query results into user attribute list**: Select whether to place database query results in message attributes. When selected, the message attribute names are generated based on the message attribute prefix and the attribute name. For example, if the specified prefix is `user` and the attributes are `PHONE` and `EMAIL`, the `user.PHONE` and `user.EMAIL` attributes are generated. This setting is selected by default.

**Associate attributes with user ID returned by selector**: When the **Place query results into message attribute list** setting is selected, you can specify a user ID to associate with the user attributes. For example, if the user name is stored as `admin` in the database, you must select the message attribute containing the value `admin`. The API Gateway then looks up the database using this name. By default, the user ID is stored in the `${authentication.subject.id}` message attribute.

Configure the following fields on the **Attribute Naming** section:

**Enable legacy attribute naming for retrieved attributes**: Specifies whether to enable legacy naming of retrieved message attributes. This field is not selected by default. Prior to version 7.1, retrieved attributes were stored in message attributes in the following format:

```
user.<retrieved_attribute_name>
```

For example, `${user.email}`, `${user.role}`, and so on. If the retrieved attribute was multivalued, you would access the values using `${user.email.1}` or `${user.email.2}`, and so on. In version 7.1 and later, by default, you can query for multivalued retrieved attributes using an array syntax (for example, `${user.email[0]}`, or `${user.email[1]}`, and so on). Select this setting to use the legacy format for attribute naming instead.

**Prefix for message attribute**: Specifies an optional prefix for message attribute names used to store query results. The default prefix is `user`.

**Attribute name for stored procedure out parameters**: You can also specify an attribute name for stored procedure out parameters. The default prefix is `out.param.value`.

**Case for attribute names**: You can specify whether attribute names are in lower case or upper case. The default is lower case.

Configure the following fields on the **Result Set Options** section:

**Fail on empty result set**: Specify whether this filter fails if the result set is empty. This setting is not selected by default.

**Attribute name for result set size**: Specify the attribute name used to store the size of the result set. Defaults to `db.result.count`.

## Retrieve attribute from directory server filter

The API Gateway can leverage an existing directory server by querying it for user profile data. The **Retrieve from Directory Server** filter can look up a user and retrieve that user's attributes represented as a list of search results. Each element of the list represents a list of multivalued attributes returned from the directory server.

### Configure database settings for directory server

Configure the following fields on the **Database** tab:

**LDAP Directory**: The API Gateway queries the selected Lightweight Directory Access Protocol (LDAP) directory for user attributes. An LDAP connection is retrieved from a pool of connections at runtime. Click the browse button to select the LDAP directory to query. To use an existing LDAP directory, (for example, `Sample Active Directory Connection`), select it in the tree.

To add an LDAP directory, right-click the **LDAP Connections** tree node, and select **Add an LDAP Connection**. Alternatively, you can add LDAP connections under the **Environment Configuration > External Connections** node in the Policy Studio tree.

The **Retrieve Unique User Identity** section enables you to select the user whose profile the API Gateway looks up in the directory server. The user ID can be taken from a message attribute or looked up from an LDAP directory. Configure the following fields:

**From Selector Expression**: Select this option if the user ID is stored in a message attribute, and specify the selector expression used to obtain its value at runtime (for example, `${authentication.subject.id}`). A user's credentials are stored in the `authentication.subject.id` message attribute after authenticating to the API Gateway, so this is the most likely attribute to enter in this field.

Typically, this contains the Distinguished Name (DName) or user name of the authenticated user. The name extracted from the specified message attribute is used to query the directory server.

**From LDAP Search**: In cases where you have not already obtained the user's identity and the `authentication.subject.id` attribute has not been prepopulated by a prior authentication filter, you must configure the API Gateway to retrieve the user's identity from an LDAP search. Click **Configure Directory Search** to configure the search criteria to use to retrieve the user's unique DName from the LDAP repository. The User Search dialog is used to search a given LDAP directory for a unique user according to the criteria configured in the following fields:

* **Base Criteria**: The value entered here tells the API Gateway where it should begin searching the LDAP directory. For example, it might be appropriate to search for a given user under the `C=IE`   tree in the LDAP hierarchy.
* **Query Search Filter**: The value entered here is what the API Gateway uses to determine whether it has obtained a successful match. Since you are searching for a specific user, you can use the user name of an authenticated user (the value of the `authentication.subject.id` message attribute) to lookup in the LDAP directory. You must also specify the object class that defines users for the particular type of LDAP directory that you are searching against. For example, object classes representing users amongst common LDAP directories are `inetOrgPerson`, `givenName`, and `User`.

    For example, to search for an authenticated user against Microsoft's Active Directory, you might specify the following as the **Query Search Filter**:

  ```
  (objectclass=User)(cn=${authentication.subject.id})
  ```

    The string representation of the LDAP search filter must comply with the `RFC2254` grammar and format. However, you might use the following Java system property in `jvm.xml` to ignore `RFC2254` escaping mechanism in the filter:

  ```
  <ConfigurationFragment>
  <SystemProperty name="avoidLDAPQueryEscaping" value="true" />
  </ConfigurationFragment>
  ```
* **Search Scope**: These settings specify the depth of the LDAP tree to search. This depends largely on the structure of your LDAP directory.

The **Retrieve Attributes** section instructs the API Gateway to search the LDAP tree to locate a specific user profile. When the appropriate profile is retrieved, the API Gateway extracts the specified user attributes. Configure the following fields:

**Base Criteria**: You can specify where the API Gateway should begin searching the LDAP directory using a selector. The selector represents the value of a message attribute that is expanded at runtime. The two most likely message attributes to be used here are the authenticated user's ID and Distinguished Name. Select one of the predefined selectors from the list:

* `${authentication.subject.id}`
* `${authentication.subject.dname}`

Alternatively, you can enter a selector representing other message attributes using the same syntax.

**Search Filter**: This is the name given by the particular LDAP directory to the *User* class. This depends on the type of LDAP directory configured. You can also use a selector to represent the value of a message attribute. For example, you can use the `user.role` attribute to store the user class. The syntax for using the selector representing this attribute is as follows:

```
(objectclass=${user.role})
```

**Search Scope**: If the API Gateway retrieves a user profile node from the LDAP tree, the option selected here dictates the level that the API Gateway searches the node to. The available options are:

* **Object level**
* **One level**
* **Sub-tree**

**Unique Result**: Select this option to force the API Gateway to retrieve a unique user profile from the LDAP directory. This is useful in cases where the LDAP search has returned several profiles.

**Attribute Name**: The **Attribute Name** table lists the attributes the API Gateway retrieves from the user profile. If no attributes are listed, the API Gateway extracts all user attributes. In both cases, retrieved attributes are set to the `attribute.lookup.list` message attribute. Click **Add** to add the name of an attribute to extract from the returned user profile. Enter the attribute name to extract from the profile in the **Attribute Name** field of the **Attribute Lookup** dialog.

{{< alert title="Note" color="primary" >}} It is important to be aware of the following:

* If the search returns results for more than one user, and the **Unique Result**   option is enabled, an error is generated. If this option is not enabled, all attributes are merged.
* If an attribute is configured that does not exist in the repository, no error is generated.
* If no attributes are configured, all attributes present for the user are retrieved. {{< /alert >}}

### Configure advanced settings for directory server

Configure the following fields on the **Advanced** tab:

**Enable legacy attribute naming for retrieved attributes**: Specifies whether to enable legacy naming of retrieved message attributes. This field is not selected by default. In versions earlier than version 7.1, retrieved attributes were stored in message attributes in the following format:

```
user.<retrieved_attribute_name>
```

For example, `${user.email}`, `${user.role}`, and so on. If the retrieved attribute was multi-valued, you would access the values using `${user.email.1}` or `${user.email.2}`, and so on. In version 7.1 and later, by default, you can query for multivalued retrieved attributes using an array syntax (for example, `${user.email[0]}`, or `${user.email[1]}`, and so on). Select this setting to use the legacy format for attribute naming instead.

**Prefix for message attribute**: You can specify an optional prefix for message attribute names. The default prefix is `user`.

{{< alert title="Note" color="primary" >}} When legacy attribute naming is not enabled, if you call the **Retrieve from Directory Server** filter multiple times in a row, the results are appended to the original `user` attributes, instead of replacing them.

For example, in single request, two **Retrieve from Directory Server** filters consecutively query an LDAP server for attributes, and both use the default prefix name of `user`. However, the attribute results of the second LDAP call do not replace the first results, and the `user` attribute gets a second entry instead. This means that to retrieve the attributes returned by the second call, you must use `${user[1].anotherattribute[0]}`. {{< /alert >}}

## Retrieve attribute from user store filter

The API Gateway user store contains user profiles, including attributes relating to each user. After a user has successfully authenticated to the API Gateway, the **Retrieve From User Store** filter can retrieve attributes belonging to that user from the user store.

### Configure database settings for user store

Configure the following fields on the **Database** tab:

**User ID**: Select or enter the name of the message attribute that contains the name of the user to look up in the user store. For example, if the user name is stored as `admin`, select the message attribute containing the value `admin`. The API Gateway then looks up the user in the user store using this name.

**Attributes**: Enter the list of attributes that the API Gateway should retrieve if it successfully looks up the user specified by the **User ID** field. To add attributes, click the **Add** button. Similarly, to edit or remove existing attributes, click the **Edit** or **Remove** buttons.

### Configure advanced settings for user store

Configure the following fields on the **Advanced** tab:

**Enable legacy attribute naming for retrieved attributes**: Specifies whether to enable legacy naming of retrieved message attributes. This field is not selected by default. Prior to version 7.1, retrieved attributes were stored in message attributes in the following format:

```
user.<retrieved_attribute_name>
```

For example, `${user.email}`, `${user.role}`, and so on. If the retrieved attribute was multivalued, you would access the values using `${user.email.1}` or `${user.email.2}`, and so on. In version 7.1 and later, by default, you can query for multivalued retrieved attributes using an array syntax (for example, `${user.email[0]}`, or `${user.email[1]}`, and so on). Select this setting to use the legacy format for attribute naming instead.

**Prefix for message attribute**: You can specify an optional prefix for message attribute names. The default prefix is `user`.

**Fail on empty result set**: Specify whether this filter fails if the result set is empty. This setting is not selected by default.
