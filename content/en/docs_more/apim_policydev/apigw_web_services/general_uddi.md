{
"title": "Work with a UDDI registry",
"linkTitle": "Work with a UDDI registry",
"weight": 10,
"date": "2019-10-17",
"description": "Connect to a UDDI registry from Policy Studio to retrieve or publish WSDL files."
}

## Connect to a UDDI registry

This section explains how to configure a connection to a UDDI registry in the **Registry Connection Details**
dialog. It explains how to configure connections to UDDI v2 and UDDI v3 registries, and how to secure a connection over SSL.

### Configure a registry connection

Configure the following fields in the **Registry Connection Details**
dialog:

**Registry Name**:
Enter the display name for the UDDI registry.

**UDDI v2**:
Select this option to use UDDI v2.

**UDDI v3**:
Select this option to use UDDI v3.

**Inquiry URL**:
Enter the URL on which to search the UDDI registry (for example, `http://HOSTNAME:PORT/uddi/inquiry`).

**Publish URL**:
Enter the URL on which to publish to the UDDI registry, if required (for example, `http://HOSTNAME:PORT/uddi/publishing`).

**Security URL (UDDI v3)**:
For UDDI v3 only, enter the URL for the security service, if required (for example, `http://HOSTNAME:PORT/uddi/security.wsdl`).

For UDDI v3, the **Inquiry URL**, **Publish URL**, and **Security URL**
specify the URLs of the WSDL for the inquiry, publishing, and security web services that the registry exposes. These fields can use the same URL if the WSDL for each service is at the same URL.

For example, a WSDL file at `http://HOSTNAME:PORT/uddi/uddi_v3_registry.wsdl`
can contain three URLs:

* `http://HOSTNAME:PORT/uddi/inquiry`
* `http://HOSTNAME:PORT/uddi/publishing`
* `http://HOSTNAME:PORT/uddi/security`

These are the service endpoint URLs that Policy Studio uses to browse and publish to the registry. These URLs are not set in the connection dialog, but discovered using the WSDL. However, for UDDI v2, WSDL is *not*
used to discover the service endpoints, so you must enter these URLs directly in the connection dialog.

**Max Rows**:
Enter the maximum number of entries returned by a search. Defaults to `20`.

**Registry Authentication**:
The registry authentication settings are as follows:

* **Type**
* This optional field applies to UDDI v2 only. The only supported authentication type is `UDDI_GET_AUTHTOKEN`.
* **Username**
* Enter the user name required to authenticate to the registry, if required.
* **Password**
* Enter the password for this user, if required.

The user name and password apply to UDDI v2 and v3. These are generally required for publishing, but depend on the configuration on the registry side.

**HTTP Proxy**:
The HTTP proxy settings apply to UDDI v2 and v3:

* **Proxy Host**
* If the UDDI registry location entered above requires a connection to be made through an HTTP proxy, enter the host name of the proxy.
* **Proxy Port**
* If a proxy is required, enter the port on which the proxy server is listening.
* **Username**
* If the proxy has been configured to only accept authenticated requests, Policy Studio sends this user name and password to the proxy using HTTP Basic authentication.
* **Password**
* Enter the password to use with the user name specified in the field above.

**HTTPS Proxy**:
The HTTPS proxy settings apply to UDDI v2 and v3:

* **SSL Proxy Host**
* If the **Inquiry URL**
    or **Publish URL**
    uses the HTTPS protocol, the SSL proxy host entered is used instead of the HTTP proxy entered above. In this case, the HTTP proxy settings are not used.
* **Proxy Port**
* Enter the port that the SSL proxy is listening on.

### Secure a connection to a UDDI registry

You can communicate with the UDDI registry over SSL. All communication may not need to be over SSL (for example, you may wish publish over SSL, and perform inquiry calls without SSL). For UDDI v2 and v3, you can use a mix of `http`
and `https`
URLs for WSDL and service address locations.

You can specify some or all of the **Inquiry URL**, **Publish URL**, and **Security URL**
settings as `https`
URLs. For example, with UDDI v3, you could use a single URL like the following:

```
https://HOSTNAME:PORT/uddi/wsdl/uddi_v3_registry.wsdl
```

If any URLs (WSDL or service address location) use `https`
, you must configure the Policy Studio so that it trusts the registry SSL certificate.

#### Configure Policy Studio to trust a registry certificate

For an SSL connection, you must configure the registry server certificate as a trusted certificate. Assuming mutual authentication is not required, the simplest way to configure an SSL connection between Policy Studio and UDDI registry is to add the registry certificate to the Policy Studio default truststore (the `cacerts`
file). You can do this by performing the following steps in Policy Studio:

1. Select the **Environment Configuration** > **Certificates and Keys** > **Certificates**
    node in the Policy Studio tree.
2. Click **Create/Import**, and click **Import Certificate**.
3. Browse to the UDDI registry SSL certificate file, and click **Open**.
4. Click **Use Subject**
    on the right of the **Alias Name**
    field, and click **OK**. The registry SSL certificate is now imported into the certificate store, and must be added to the Java keystore.
5. Click **Keystore**
    on the **Certificate**
    window.
6. Click **Browse**
    next to the **Keystore**
    field.
7. Browse to the following file:

    ```
    INSTALL_DIR/policystudio/jre/lib/security/cacerts
    ```

8. Click **Open**, and enter the **Keystore password**.
9. Click **Add to Keystore**.
10. Browse to the registry SSL certificate imported earlier, select it, and click **OK**.
11. Restart Policy Studio. You should now be able to connect to the registry over SSL.

#### Configure mutual SSL authentication

If mutual SSL authentication is required (if Policy Studio must authenticate to the registry), Policy Studio must have an SSL private key and certificate. In this case, you should create a keystore containing the Policy Studio key and certificate. You must configure Policy Studio to load this file. For example, edit the `INSTALL_DIR/policystudio/policystudio.ini` file, and add the following arguments:

```
-Djavax.net.ssl.keyStore=/home/axway/osr-client.jks
-Djavax.net.ssl.keyStorePassword=changeit
```

This example shows an `osr-client.jks`
keystore file used with Oracle Service Registry (OSR), which is the UDDI registry provided by Oracle.

{{< alert title="Note" color="primary" >}}You can also use Policy Studio to create a new keystore (`.jks`) file. Click **New keystore**
instead of browsing to the `cacerts`
file as described earlier.{{< /alert >}}

## Retrieve WSDL files from a UDDI registry

Policy Studio can retrieve a WSDL file from the file system, from a URL, or from a UDDI registry. This section explains how to retrieve a WSDL file from a UDDI registry.

You can also browse a UDDI registry in Policy Studio directly without registering a WSDL file. Under the **APIs**
node in the tree, right-click the **Web Service Repository**
node, and select **Browse UDDI Registry**.

### UDDI concepts

Universal Description, Discovery and Integration (UDDI) is an OASIS-led initiative that enables businesses to publish and discover web services on the Internet. A business publishes services that it provides to a public XML-based registry so that other businesses can dynamically look up the registry and discover these services. Enough information is published to the registry to enable other businesses to find services and communicate with them. In addition, businesses can also publish services to a private or semi-private registry for internal use.

A business registration in a UDDI registry includes the following components:

* **Green Pages**:
    Contains technical information about the services exposed by the business
* **Yellow Pages**:
    Categorizes the services according to standard taxonomies and categorization systems
* **White Pages**:
    Gives general information about the business, such as name, address, and contact information

You can search the UDDI registry according to a whole range of search criteria, which is of key importance to Policy Studio. You can search the registry to retrieve the WSDL file for a particular service. Policy Studio can then use this WSDL file to create a policy for the service, or to extract a schema from the WSDL to check the format of messages attempting to use the operations exposed by the Web service.

For a more detailed description of UDDI, see the UDDI specification. In the meantime, the next section gives high-level definitions of some of the terms displayed in the Policy Studio interface.

### UDDI definitions

Because UDDI terminology is used in Policy Studio windows, such as the **Import WSDL**
wizard, the following list of definitions explains some common UDDI terms. For more detailed explanations, see the UDDI specification.

**businessEntity**
This represents all known information about a particular business (for example, name, description, and contact information). A `businessEntity`
can contain a number of `businessService`
entities. A `businessEntity`
may have an `identifierBag`, which is a list of name-value pairs for identifiers, such as Data Universal Numbering System (DUNS) numbers or taxonomy identifiers. A `businessEntity`
may also have a `categoryBag`, which is a list of name-value pairs used to tag the `businessEntity`
with classification information such as industry, product, or geographic codes. There is no mapping for a WSDL item to a `businessEntity`. When a WSDL file is published, you must specify a `businessEntity`
for the `businessService`.

**businessService**
A `businessService`
represents a logical service classification, and is used to describe a web service provided by a business. It contains descriptive information in business terms outlining the type of technical services found in each `businessService`
element. A `businessService`
may have a `categoryBag`, and may contain a number of `bindingTemplate`
entities. In the WSDL to UDDI mapping, a `businessService`
represents a `wsdl:service`. A `businessService`
has a `businessEntity`
as its parent in the UDDI registry.

**bindingTemplate**
A `bindingTemplate`
contains pointers to the technical descriptions and the access point URL of the web service, but does not contain the details of the service specification. A `bindingTemplate`
may contain references to a number of `tModel`
entities, which do include details of the service specification. In the WSDL to UDDI mapping, a `bindingTemplate`
represents a `wsdl:port`.

**tModel**
A `tModel`
is a web service type definition, which is used to categorize a service type. A `tModel`
consists of a key, a name, a description, and a URL. `tModel`
s are referred to by other entities in the registry. The primary role of the `tModel`
is to represent a technical specification (for example, WSDL file). A specification designer can establish a unique technical identity in a UDDI registry by registering information about the specification in a `tModel`. Other parties can express the availability of web services that are compliant with a specification by including a reference to the `tModel`
in their `bindingTemplate`
data.

This approach facilitates searching for registered web services that are compatible with a particular specification. `tModel`s are also used in `identifierBag`
and `categoryBag`
structures to define organizational identity and various classifications. In this way, a `tModel`
reference represents a relationship between the keyed name-value pairs to the super-name, or namespace in which the name-value pairs are meaningful. A `tModel`
may have an `identifierBag`
and a `categoryBag`. In the WSDL to UDDI mapping, a `tModel`
represents a `wsdl:binding`
or `wsdl:portType`.

**Identifier**
The purpose of identifiers in a UDDI registry is to enable others to find the published information using more formal identifiers such as DUNS numbers, Global Location Numbers (GLN), tax identifiers, or any other kind of organizational identifiers, regardless of whether these are private or shared.

The following are identification systems used commonly in UDDI registries:

| Identification System             | Name                            | tModel Key                                  |
|-----------------------------------|---------------------------------|---------------------------------------------|
| Dun and Bradstreet D-U-N-S Number | `dnb-com:D-U-N-S`               | `uuid:8609C81E-EE1F-4D5A-B202-3EB13AD01823` |
| Thomas Registry Suppliers         | `thomasregister-com:supplierID` | `uuid:B1B1BAF5-2329-43E6-AE13-BA8E97195039` |

**Category**
Entities in the registry may be categorized according to categorization system defined in a `tModel`
(for example, geographical region). The `businessEntity`, `businessService`, and `tModel`
types have an optional `categoryBag`. This is a collection of categories, each of which has a name, value, and `tModel`
key.

The following are categorization systems used commonly in UDDI registries:

| Categorization System                                              | Name                  | tModel Key  |
|--------------------------------------------------------------------|-----------------------|-------------|
| UDDI Type Taxonomy                                                 | `uddi-org:types`      | `uuid:C1ACF26D-9672-4404-9D70-39B756E62AB4` |
| North American Industry Classification System (NAICS) 1997 Release | `ntis-gov:naics:1997` | `uuid:C0B9FE13-179F-413D-8A5B-5004DB8E5BB2` |

#### Example tModel mapping for WSDL portType

The following shows an example `tModel`
mapped for a WSDL `portType`:

```
<tModel tModelKey="uuid:e8cf1163-8234-4b35-865f-94a7322e40c3">
    <name>
    StockQuotePortType
    </name>
    <overviewDoc>
    <overviewURL>
        http://location/sample.wsdl
    </overviewURL>
    </overviewDoc>
    <categoryBag>
    <keyedReference tModelKey="uuid:d01987d1-ab2e-3013-9be2-2a66eb99d824" keyName="portType namespace"
        keyValue="http://example.com/stockquote/" />
    <keyedReference tModelKey="uuid:6e090afa-33e5-36eb-81b7-1ca18373f457" keyName="WSDL type"
        keyValue="portType" />
    </categoryBag>
</tModel>
```

In this example, the `tModel`
name is the same as the local name of the WSDL `portType`
(in this case, `StockQuotePortType`), and the `overviewURL`
links to the WSDL file. The `categoryBag`
specifies the WSDL namespace, and shows that the `tModel`
is for a `portType`.

### Select or add a UDDI registry

You first need to select the UDDI registry to search for WSDL files. Complete the following fields to select or add a UDDI registry:

**Select Registry**:
Select an existing UDDI registry to browse for WSDL files from the **Registry**
drop-down list. To configure the location of a new UDDI registry, click **Add**. Similarly, to edit an existing UDDI registry location, click **Edit**. Then configure the fields in the **Registry Connection Details**
dialog. For more details, see [Connect to a UDDI registry](#connect-to-a-uddi-registry).

**Select Locale**:
You can select an optional language locale from this list. The default is `No Locale`.

### WSDL search

When you have configured a UDDI registry connection, you can search the registry using a variety of different search options on the **Search**
tab. **WSDL Search**
is the default option. This enables you to search for the UDDI entries that the WSDL file is mapped to. You can also do this using the **Advanced Search**
option. The following different types of WSDL searches are available:

**WSDL portType (UDDI tModel)**:
Searches for a `uddi:tModel`
that corresponds to a `wsdl:portType`. You can enter optional search criteria for specific categories in the `uddi:tModel`
(for example, **Namespace of portType**).

**WSDL binding (UDDI tModel)**:
Searches for a `uddi:tModel`
that corresponds to a `wsdl:binding`. You can enter optional search criteria for specific categories in the `uddi:tModel`
(for example, **Name of binding**, or **Binding Transport Type**).

**WSDL service (UDDI businessService)**:
Searches for a `uddi:businessService`
that corresponds to a `wsdl:service`. You can enter optional search criteria for specific categories in the `uddi:businessService`
(for example, **Namespace of service**).

**WSDL port (UDDI bindingTemplate)**:
Searches for a `uddi:bindingTemplate`
that corresponds to a `wsdl:port`.This search is more complex because a `serviceKey`
is required to find a `uddi:bindingTemplate`. This means that at least two queries are carried out, first to find the `uddi:businessService`, and another to find the `uddi:bindingTemplate`.

For example, a `bindingTemplate`
contains a reference to the `tModel`
for the `wsdl:portType`. You can use the `tModel`
key to find all implementations (`bindingTemplate`s) for that `wsdl:portType`. The search looks for `businessService`s that have `bindingTemplate`s that refer to the `tModel`
for the `wsdl:portType`. Then with the `serviceKey`, you can find the `bindingTemplate`
that refers to the `tModel`
for the `wsdl:portType`.

In all cases, click **Next**
to start the WSDL search. The **Search Results**
tree shows the `tModel`
URIs as top-level nodes. These URIs are all WSDL URIs, and you can use these to generate policies on import by selecting the URI, and clicking the **Finish**
button.

You can click any of the nodes in the tree to display detailed properties about that node in the table below the **Search Results**
tree. The properties listed depend on the type of the node that is selected. You can also right-click a node to edit it (for example, add a description, add a category or identifier node, or delete a duplicate node).

### Quick search

The **Quick Search** option enables you to search the UDDI registry for a specific `tModel`
name or category.

**tModel Name**:
You can enter a **tModel Name**
for a fine-grained search. This is a partial or full name pattern with wildcard searching as specified by the *SQL-92 LIKE*
specification. The wildcard characters are percent `%`, and underscore `_`, where an underscore matches any single character and a percent matches zero or more characters.

You can select one of the following categories to search by:

**wsdlSpec**: Search for `tModel`s classified as `wsdlSpec` (`uddi-org:types`category set to `wsdlSpec`). This is the default.

**Reusable WS-Policy Expressions**: Search for `tModel`s classified as reusable WS-Policy Expressions.

**All**: Search for all `tModel`s.

Click **Next** to start the search. The **Search Results** tree shows the `tModel` URIs as top-level nodes. These URIs are all WSDL URIs, and you can use these to generate policies on import by selecting the URI, and clicking the **Finish**
button.

You can click any of the nodes in the tree to display detailed properties about that node in the table below the **Search Results** tree. The properties listed depend on the type of the node that is selected. You can also right-click a node to edit it (for example, add a description, add a category or identifier node, or delete a duplicate node).

### Name search

The **Name Search**
option enables you to search for a `businessEntity`, `businessService`, or `tModel`
by name. In the **Select Registry Data Type**, select one of the following UDDI entity levels to search for:

* **businessEntity**
* **businessService**
* **tModel**

You can enter a name in the **Name**
field to narrow the search. You can also use wildcards in the name. The name applies to a `businessEntity`, `businessService`, or `tModel`, depending on which registry entity type has been selected. If no name is entered, all entities of the selected type are retrieved.

Click the **Search**
button to start the search. The search results display the matching entities in the tree. For example, if you enter `MyTestBusiness`
for **Name**, and select **businessEntity**, this searches for a `businessEntity`
with the name `MyTestBusiness`. Child nodes of the matching `businessEntity`
nodes are also shown. `tModel`s are displayed in the results if any child nodes of the `businessEntity`
refer to `tModel`s. Only referred to `tModel`s are displayed. The same applies if you search for a `businessService`. If you select `tModel`, and search for `tModel`s, only `tModel`s are displayed.

{{< alert title="Note" color="primary" >}}The `tModel`
URIs shown in the resulting tree may not all be categorized as `wsdlSpec`
according to the `uddi-org:types`
categorization system. You can choose to load any of these URIs as a WSDL file, but you are warned if it is not categorized as `wsdlSpec`.{{< /alert >}}

As before, you can click any node in the results tree to display properties about that node in the table. You can also right-click a node to edit it (for example, add a description, add a category or identifier node, or delete a duplicate node).

#### UDDI v3 name searches

By default, a UDDI v3 name search is an exact match. To perform a search using wildcards (for example, `%`, `_`, and so on), you must select the **approximateMatch**
find qualifier in the **Advanced Options**
tab. This applies to anywhere you enter a name for search purposes (for example, **Name Search**, **Quick Search**, and **Advanced Search**).

### Advanced search

The **Advanced Search**
option enables you to search the UDDI registry using any combination of **Names**, **Keys**, **tModels**, **Discovery URLs**, **Categories**, and **Identifiers**. You can also specify the entity level to search for in the tree. All of these options combine to provide a very powerful search facility. You can specify search criteria for any of the categories listed above by right-clicking the folder node in the **Enter Search Criteria**
tree, and selecting the **Add**
menu option. You can enter more than one search criteria of the same type (for example, two **Key**
search criteria).

{{< alert title="Note" color="primary" >}}The `tModel`
URIs shown in the resulting tree may not all be categorized as `wsdlSpec`
according to the `uddi-org:types`
categorization system. You can choose to load any of these URIs as a WSDL file, but you are warned if it is not categorized as `wsdlSpec`.{{< /alert >}}
The following options enable you to add a search criteria for each of the types listed in the **Enter Search Criteria**
tree. All search criteria are configured by right-clicking the folder node, and selecting the **Add**
menu option.

**Names**:
Enter a name to be used in the search in the **Name**
field in the **Name Search Criterion**
dialog. For example, the name could be the **businessEntity**
name. The name is a partial or full name pattern with wildcards allowed as specified by the *SQL-92 LIKE*
specification. The wildcard characters are percent `%`, and underscore `_`, where an underscore matches any single character and a percent matches zero or more characters. A name search criterion can be used for `businessEntity`, `businessService`, and `tModel`
level searches.

**Keys**:
In the **Key Search Criterion**
dialog, you can specify a key to search the registry for in the **Key**
field. The key value is a Universally Unique Identifier (UUID) value for a registry object. You can use the **Key Search Criterion**
on all levels of searches. If one or more keys are specified with no other search criteria, the keys are interpreted as the keys of the selected type of registry object and used for a direct lookup, instead of a find/search operation. For example, if you enter `key1`
and `key2`, and select the `businessService`
entity type, the search retrieves the `businessService`
object with key `key1`, and another `businessService`
with key `key2`.

If you enter a key with other search criteria, a key criterion is interpreted as follows:

* For a businessService entity lookup, the key is the `businessKey`
    of the services.
* For a `bindingTemplate`
    entity lookup, the key is the `serviceKey`
    of the binding templates.
* Not applicable for any other object type.

**tModels**:
You can enter a key in the **tModel Key**
field on the **tModel Search Criterion**
window. The key entered should correspond to the UUID of the `tModel`
associated with the type of object you are searching for. A `tModel`
search criterion may be used for `businessEntity`, `businessService`, and `bindingTemplate`
level searches.

**Discovery URLs**:
Enter a URL in the **Discovery URL**
field on the **Discovery URL Search Criterion**
dialog. The **Use Type**
field is optional, but can be used to further fine-grain the search by type. You can use a Discovery URL search criterion for `businessEntity`
level searches only.

**Categories**:
Select a previously configured categorization system from the **Type**
drop-down list in the **Category Search Criterion**
dialog. This prepopulated with a list of common categorization systems. You can add a new categorization system using the **Add**
button.

In the **Add/Edit Category**
dialog, enter a **Name**, **Description**, and **UUID**
for the new category type in the fields provided. When the categorization system has been selected or added, enter a value to search for in the **Value**
field. The **Name**
field is optional.

**Identifiers**:
Select a previously configured identification system from the **Type**
drop-down list in the **Identifier Search Criterion**
dialog. This is prepopulated with well-known identification systems. To add a new identification system, click the **Add**
button.

In the **Add/Edit Identifier**
dialog, enter a **Name**, **Description**, and **UUID**
for the new identifier in the fields provided.

**Select Registry Data Type**:
Select one of the following UDDI entity levels to search for:

* **businessEntity**
* **businessService**
* **bindingTemplate**
* **tModel**

The search also displays child nodes of the matching nodes. `tModel`s are also returned if they are referred to.

### Advanced options

This tab enables you to configure various aspects of the search conditions specified on the previous tabs. The following options are available:

| UDDI Find Qualifier                | Description  |
|------------------------------------|-------------|
| **andAllKeys**                     | By default, identifier search criteria are ORed together. This setting ensures that they are ANDed instead. This is already the default for `categoryBag` and `tModelBag`. |
| **approximateMatch (v3)**          | This applies to a UDDI v3 registry only. Specifies wildcard searching as defined by the `uddi-org:approximatematch:SQL99 tModel`, which means approximate matching where percent sign (`%`) indicates any number of characters, and underscore (`_`) indicates any single character. The backslash character (`\`) is an escape character for the percent sign, underscore and backslash characters. This option adjusts the matching behavior for `name`, `keyValue`, and `keyName` (where applicable).  |
| **binarySort (v3)**                | This applies to a UDDI v3 registry only. Enables greater speed in sorting, and causes a binary sort by name, as represented in Unicode codepoints.|
| **bindingSubset (v3)**             | This applies to a UDDI v3 registry only. Specifies that the search uses only `categoryBag` elements from contained `bindingTemplate` elements in the registered data, and ignores any entries found in the `categoryBag` that are not direct descendents of registered `businessEntity` or `businessService` elements. |
| **caseInsensitiveMatch (v3)**      | This applies to a UDDI v3 registry only. Specifies that that the matching for `name`, `keyValue`, and `keyName` (where applicable) should be performed without regard to case. |
| **caseInsensitiveSort (v3)**       | This applies to a UDDI v3 registry only. Specifies that the result set should be sorted without regard to case. This overrides the default case sensitive sorting behavior. |
| **caseSensitiveMatch (v3)**        | This applies to a UDDI v3 registry only. Specifies that that the matching for `name`, `keyValue`, and `keyName` (where applicable) should be case sensitive. This is the default behavior.|
| **caseSensitiveSort (v3)**         | This applies to a UDDI v3 registry only. Specifies that the result set should be sorted with regard to case. This is the default behavior.  |
| **combineCategoryBags**            | Makes the `categoryBag` entries of a `businessEntity` behave as if all `categoryBag`s found at the `businessEntity` level and in all contained or referenced `businessService`s are combined. Searching for a category yields a positive match on a registered business if any of the `categoryBag`s contained in a `businessEntity` (including the `categoryBag`s in contained or referenced `businessService`s) contain the filter criteria. |
| **diacriticInsensitiveMatch (v3)** | This applies to a UDDI v3 registry only. Specifies that matching for `name`, `keyValue`, and `keyName` (where applicable) should be performed without regard to diacritics. Support for this qualifier by nodes is optional.  |
| **diacriticSensitiveMatch (v3)**   | This applies to a UDDI v3 registry only. Specifies that matching for `name`, `keyValue`, and `keyName` (where applicable) should be performed with regard to diacritics. This is the default behavior. |
| **exactMatch (v3)**                | This applies to a UDDI v3 registry only. Specifies that only entries with `name`, `keyValue`, and `keyName` (where applicable) that exactly match the name argument passed in, after normalization, are returned. This qualifier is sensitive to case and diacritics where applicable. This is the default behavior.  |
| **exactNameMatch (v2)**            | This applies to a UDDI v2 registry only. Specifies that the name entered as part of the search criteria must exactly match the name specified in the UDDI registry.   |
| **orAllKeys**                      | By default, `tModel` and category search criteria are ANDed. This setting ORs these criteria instead.   |
| **orLikeKeys**                     | When a bag container contains multiple `keyedReference` elements (`categoryBag` or `identifierBag`), any `keyedReference` filters from the same namespace (for example, with the same `tModelKey` value) are ORed together rather than ANDed. For example, this enables you to search for `any of these four values from this namespace, and any of these two values from this namespace`.     |
| **serviceSubset**                  | Causes the component of the search that involves categorization to use only the `categoryBag`s from directly contained or referenced `businessService`s in the registered data. The search results return only those businesses that match based on this modified behavior, in conjunction with any other search arguments provided.      |
| **signaturePresent (v3)**          | This applies to a UDDI v3 registry only. This restricts the result to entities that contain, or are contained in, an XML Digital `Signature` element. The `Signature` element should be verified by the client. This option, or the presence of a `Signature` element, should only be used to refine a search result, and should not be used as a verification mechanism by UDDI clients. |
| **sortByDateAsc (v3)**             | This applies to a UDDI v3 registry only. Sorts the results alphabetically in order of ascending date.  |
| **sortByDateDsc (v3)**             | This applies to a UDDI v3 registry only. Sorts the results alphabetically in order of descending date.|
| **sortByNameAsc**                  | Sorts the results alphabetically in order of ascending name.   |
| **sortByNameDsc**                  | Sorts the results alphabetically in order of descending name. |
| **suppressProjectedServices (v3)** | This applies to a UDDI v3 registry only. Specifies that service projections must not be returned when searching for services or businesses. This option is enabled by default when searching for a service without a `businessKey`. |
| **UTS-10 (v3)**                    | This applies to a UDDI v3 registry only. Specifies sorting of results based on the Unicode Collation Algorithm on elements normalized according to Unicode Normalization Form C. A sort is performed according to the Unicode Collation Element Table in conjunction with the Unicode Collation Algorithm on the name field, and normalized using Unicode Normalization Form C. Support for this qualifier by nodes is optional. |

### Publish

Click the **Publish**
radio button to view the **Published UDDI Entities Tree View**. This enables you to manually publish UDDI entities to the specified UDDI registry (for example, `businessEntity`, `businessService`, `bindingTemplate`, and `tModel`
entities). You must already have the appropriate permissions to write to the UDDI registry.

#### Add a businessEntity

To add a business, perform the following steps:

1. Right-click the tree view, and select **Add businessEntity**.
2. In the **Business**
    dialog, enter a **Name**
    and **Description**
    for the business. Click **OK**.
3. You can right-click the new `businessEntity`
    node to add child UDDI entities in the tree (for example, `businessService`, `Category`, and `Identifier`
    entities).

#### Add a tModel

To add a `tModel`
, perform the following steps:

1. Right-click the tree view, and select **Add tModel**.
2. In the **tModel**
    dialog, enter a **Name**, **Description**, and **Overview URL**
    for the `tModel`
    . For example, you can use the **Overview URL**
    to specify the location of a WSDL file. Click **OK**.
3. You can right-click the new `tModel`
    node to add child UDDI entities in the tree (for example, `Category`
    and `Identifier`
    entities).

As before, you can click any node in the results tree to display properties about that node in the table. You can also right-click a node to edit it (for example, add a description, add a category or identifier node, or delete a duplicate node). At any stage, you can click the **Clear**
button on the right to clear the entire contents of the tree. This does not delete the contents of the registry.

## Publish WSDL files to a UDDI registry

When you have registered a WSDL file in the web service repository, you can use the **Publish WSDL**
wizard to publish the WSDL file to a UDDI registry. You can also use the **Find WSDL**
wizard to search for the selected WSDL file in a UDDI registry.

### Find WSDL files

You can search a UDDI registry to determine if a web service is already published in the registry. To search for a selected WSDL file in a specified UDDI registry, perform the following steps:

1. In the Policy Studio tree, expand the **APIs** > **Web Service Repository**
    node.
2. Right-click a WSDL node and select **Find in UDDI Registry**
    to launch the **Find WSDL**
    wizard.
3. In the **Find WSDL**
    dialog, select a UDDI registry from the list. You can add or edit a registry connection using the buttons provided.
4. You can select an optional language **Locale**
    from the list. The default is `No Locale`.
5. Click **Next**. The **WSDL Found in UDDI Registry**
    window displays the result of the search in a tree. The **Node Counts**
    field shows the total numbers of each UDDI entity type returned from the search (`businessEntity`, `businessService`, `bindingTemplate`, and `tModel`).
6. You can right-click to edit a UDDI entity node in the tree, if necessary (for example, add a description, add a category or identifier node, or delete a duplicate node).
7. Click the **Refresh**
    button to run the search again.
8. Click **Finish**.

The **Find WSDL**
wizard provides is a quick and easy way of finding a selected WSDL file published in a UDDI registry. For more fine-grained ways of searching a UDDI registry (for example, for specific WSDL or UDDI entities), see [Retrieve WSDL files from a UDDI registry](#retrieve-wsdl-files-from-a-uddi-registry).

### Publish WSDL files

To publish a WSDL file registered in the **Web Service Repository**
to a UDDI registry, perform the following steps:

1. Expand the **API** > **Web Service Repository**
    tree node.
2. Right-click a WSDL node and select **Publish WSDL to UDDI Registry**
    to launch the **Publish WSDL Wizard**.
3. Perform the steps in the wizard as described in the next sections.

#### Enter virtualized service address and WSDL URL for publishing in UDDI registry

When you register a WSDL file in the web service repository, API Gateway exposes a *virtualized*
version of the web service. The host and port for the web service are changed dynamically to point to the machine running API Gateway. The client can then retrieve the WSDL for the virtualized web service from API Gateway, without knowing its real location.

This window enables you to optionally override the service address locations in the WSDL file with the virtualized addresses exposed by API Gateway. You can also override the WSDL URL published to the UDDI registry.

Complete the following fields:

**Mapping of Service Addresses to Virtualized Service Addresses**:
You can enter multiple virtual service address mappings for each service address specified in the selected WSDL file. If you do not enter a mapping, the original address location in the WSDL file is published to the UDDI registry. If one or more mappings are provided, corresponding UDDI `bindingTemplates`
are published in the UDDI registry, each with a different access point (virtual service address). This enables you to publish the access points of a service when it is exposed on different ports/schemes using API Gateway.

When you launch the wizard, the mapping table is populated with a row for each `wsdl:service`, `wsdl:port`, `soap:address`, `soap12:address`, or `http:address`
in the selected WSDL file. To modify an existing entry, select a row in the table, and click **Edit**. Alternatively, click **Add**
to add an entry. In the **Virtualize Service Address**
dialog, enter the virtualized service address. For example, if API Gateway is running on a machine named `roadrunner`, the new URL on which the web service is available to clients is: `http://roadrunner:8080/TestServices/StockQuote.svc`.

**WSDL URL**:
You can enter a WSDL URL to be published to the UDDI registry in the corresponding `tModel overviewURL`
fields. If you do not enter a URL, the WSDL URL in the **Original WSDL URL**
field is used. For example, an endpoint service is at `http://coyote.qa.acmecorp.com/TestService/StockQuote.svc`. Assume this service is virtualized in API Gateway and exposed at `http://HOST:8080/TestService/StockQuote.svc`, where `HOST`
is the machine on which API Gateway is running. The `http://HOST:8080/TestService/StockQuote.svc`
URL is entered as the virtual service address, and `http://HOST:8080/TestService/StockQuote.svc?WSDL`
is entered as the WSDL URL to publish.

{{< alert title="Note" color="primary" >}}If incorrect URLs are published, you can edit these in the UDDI tree in later steps in this wizard, or when browsing the registry. {{< /alert >}}
Click **Next**
when finished.

#### View WSDL to UDDI mapping result

You can use this window to view the unpublished mapping of the WSDL file to a UDDI registry structure. You can also edit a specific mapping in the tree view. This window includes the following fields:

**Mapping of WSDL to a UDDI Registry Structure**:
The unpublished mappings from the WSDL file to the UDDI registry are displayed in the table. For example, this includes the relevant `businessService`, `bindingTemplate`, `tModel`, `Identifier`, `Category`
mappings. You can select a tree node to display its values in the table below.

You can optionally edit the values for a specific mapping in the table (for example, update a value, or add a key or description for the selected UDDI entity). You can also right-click a tree node to edit it (for example, add a description, add a category or identifier node, or delete a duplicate node).

**Retrieve service address from WSDL instead of bindingTemplate access point**:
When selected, this ensures that the `bindingTemplate`
access point does not contain the service port address, and is set to `WSDL`
instead. This means that you must retrieve the WSDL to get the service access point. When selected, the `bindingTemplate`
contains an additional `tModelInstanceInfo`
that points to the `uddi:uddi.org.wsdl:address tModel`. This option is not selected by default.

**Include WS-Policy as**:
When selected, you can choose one of the following options to specify how WS-Policy statements in the WSDL file are included in the registry:

* **Remote Policy Expressions**: Each WS-Policy URL in the WSDL that is associated with a mapped UDDI entity is accessed remotely. For example, a `businessService` is categorized with the `uddi:w3.org:ws-policy:v1.5:attachment:remotepolicyreference tModel` where the `keyValue` holds the remote WS-Policy URL. This is the default option.
* **Reusable Policy Expressions**: Each WS-Policy URL in the WSDL that is associated with a mapped UDDI entity has a separate `tModel` published for it. Other UDDI entities (for example, `businessService`) can then refer to these `tModel`s. These are reusable because UDDI entities published in the future can also use these `tModel`s. You can do this in [Select a duplicate publishing approach](#select-a-duplicate-publishing-approach), by selecting the **Reuse duplicate tModels** option.

Click **Next** when finished.

#### Select a registry for publishing

Use this window to select a UDDI registry in which to publish the WSDL to UDDI mapping. Complete the following fields:

**Select Registry**:
Select an existing UDDI registry to browse for WSDL files from the **Registry**
drop-down list. To configure the location of a new UDDI registry, click **Add**. Similarly, to edit an existing UDDI registry location, click **Edit**.

**Select Locale**:
You can select an optional language locale from this list. The default is `No Locale`.

Click **Next** when finished.

#### Select a duplicate publishing approach

This window is displayed only if mapped WSDL entities already exist in the UDDI registry. Otherwise, the wizard skips to the next step. This window includes the following fields:

**Select Duplicate Mappings**:
The **Mapped WSDL to publish**
pane on the left displays the unpublished WSDL mappings from an earlier step. The **Duplicates for WSDL mappings in UDDI registry**
pane on the right displays the nodes already published in the registry. The **Node List**
at the bottom right shows a breakdown of the duplicate nodes.

**Edit Duplicate Mappings**:
You can eliminate duplicate mappings by right-clicking a tree node in the right or left pane, and selecting edit to update a specific mapping in the dialog. Select the **Refresh**
button on the right to run the search again, and view the updated **Node List**. Alternatively, you can configure the options in the next field.

**Select Publishing Approach for Duplicate Entries**:
Select one of the following options:

* **Reuse duplicate tModels**: Publishes the selected entries from the tree on the left, and reuses the selected duplicate entries in the tree on the right. This is the default option. Some or all duplicate `tModel`s (for example, for `portType`, `binding`, and reusable WS-Policy expressions) that already exist in the registry can be reused. This means that a new `businessService`
  that points to existing `tModel`s is published. Any entries selected on the left are published, and any referred to `tModel`s on the left now point to selected duplicate `tModel`s on the right. By default, this option selects all `businessServices`
  on the left, and all duplicate `tModel`s on the right. If there is more than one duplicate `tModel`s, only the first is selected.
* **Overwrite duplicates**: Publishes the selected entries from the tree on the left, and overwrites the selected duplicate entries in the tree on the right. When a UDDI entity is overwritten, its UUID key stays the same, but all the data associated with it is overwritten. This is not just a transfer of additions or differences. You can also overwrite some duplicates and create some new entries. By default, this option selects all `businessService`s and `tModel`s on the left and all duplicate `businessService`s and `tModel`s on the right. If there is more than one duplicate, only the first is selected. The default overwrites all selected duplicates and does not create any new UDDI entries, unless there is a new referred to `tModel` (for example, for a reusable WS-Policy expression).
* **Ignore duplicates**: Publishes the selected entries from the tree on the left, and ignores all duplicates. You can proceed to publish the mapped WSDL to UDDI data. New UDDI entries are created for each item that is selected in the tree on the left.

Click **Next** when finished.

{{< alert title="Note" color="primary" >}}If you select duplicate `businessService`s in the tree, and select **Overwrite duplicates**, the wizard skips to [Publish WSDL](#publish-wsdl) when you click **Next**.{{< /alert >}}

#### Create or search for business

Use this window to specify a `businessEntity`
for the web service. You can create a new `businessEntity`
or search for an existing one in the UDDI registry. Complete the following fields:

**Create a new businessEntity**:
This is the default option. Enter a **Name**
and **Description**
for the `businessEntity`, and click **Publish**.

**Search for an existing businessEntity**:
To search for an existing `businessEntity`
name, perform the following steps:

1. Select the **Search for an existing businessEntity in the UDDI registry**
    option.
2. In the **Search**
    tab, ensure the **Name Search**
    option is selected.
3. Enter a **Name**
    option (for example, `Acme Corporation`).

Alternatively, you can select the **Advanced Search**
option to search by different criteria such as **Keys**, **Categories**, or **tModels**. You can also select a range of search options on the **Advanced**
tab (for example, **Exact match**, **Case sensitive**, or **Service subset**).

The **Node Counts**
field shows the total numbers of each UDDI entity type returned from the search (`businessEntity`, `businessService`, `bindingTemplate`,and `tModel`).

Click **Next**
when finished.

#### Publish WSDL

Use this to publish the WSDL to the UDDI registry.

**Selected businessEntity for Publishing**:
This field displays the name and `tModel`
key of the `businessEntity`
to be published. Click the **Publish WSDL**
button on the right.

**Published WSDL**:
This field displays the tree of the UDDI mapping for the WSDL file. You can right-click to edit or delete any nodes in the tree if necessary, and click **Refresh**
to run the search again. Click **Publish WSDL**
to publish your updates.

Click **Finish**.
