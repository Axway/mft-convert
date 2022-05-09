{
"title": "Manage WSDL and XML schema documents",
"linkTitle": "Manage WSDL and XML schema documents",
"weight": 3,
"date": "2019-10-17",
"description": "Manage the global cache of WSDL and XML schema documents."
}

WSDL files often contain XML schemas that define the elements that appear in SOAP messages. When you import a WSDL file to register a web service, the imported WSDL file, and any XML schemas included in the WSDL, are added to a global cache of WSDL and XML schema documents. You can also add XML schemas and WSDL documents to the cache independently.

If you select a cached WSDL file or XML schema in a **Schema Validation** filter, API Gateway can retrieve it from the cache instead of fetching it from its original location. This improves the runtime performance of the filter, and also ensures that an administrator has complete control over the schemas used to validate messages.

API Gateway can maintain multiple versions of WSDL and XML schema documents in the global cache, and keeps an explicit version history as they change over time. The cache is prepopulated with many of the common XML schema documents that are used in web services, and these are shared across multiple web services.

## Structure of the global cache

The global cache consists of both WSDL documents and XML schema documents.

The global cache of WSDL documents contains all WSDL documents that have been imported (either directly, or by registering a web service) into API Gateway.

The global cache of schema documents contains:

* User-defined catalog – This contains all user-defined schema documents that have been imported into API Gateway.
* System catalog – This contains all common schemas (for example, the SOAP encoding schema) that are preloaded during API Gateway installation.

The following figure shows the structure.

![Structure of the global cache](/Images/docbook/images/general/schema_catalog_example.png)

## View cached WSDL or XML schema documents

Policy Studio provides a *read-only*
view of cached WSDL and XML schema documents.

To view the global cache of WSDL documents, expand the **Resources > WSDL Document Bundles**
tree node. The list of documents present in the cache is shown in the tree. To view the contents of a document, click the document node. A read-only view of the document is displayed in a tab on the right. WSDL documents can be added directly to this node, and they can be resynchronized.

To view the global cache of XML schema documents, expand the **Resources > XML Schema Document Bundles**
tree node. This node contains two subnodes: **User-defined Catalog**
and **System Catalog**. To view the documents in each catalog, expand the catalog node. The list of documents present in the catalog is shown in the tree. To view the contents of a document, click the document node. A read-only view of the document is displayed in a tab on the right.

XML schemas in the **System Catalog**
are preloaded during API Gateway installation. You cannot add schemas to the system catalog, and schemas in the system catalog cannot be resynchronized. This is indicated by a key icon on these nodes.

The **User-defined Catalog**
contains schemas that you have imported. These schemas can be resynchronized, and you can add new schemas directly to this catalog.

Document bundles are categorized by namespace/name, with subcategories used to indicate where the node is being tracked from (the URL it was retrieved from), and further subcategories for version information. The following figure shows an example of this.

![Example of XML schema document bundles](/Images/docbook/images/general/schema_bundle_example.png)

Click a document bundle in the tree to list the versions that have been imported. Click a version to list all the documents contained in that version. Click a WSDL or XML schema document to view the WSDL or XML source for the document. A read-only view of the source is shown in a tab on the right.

## Add XML schemas to the cache

To add an XML schema to the user-defined catalog in the global cache, perform the following steps:

1. In the Policy Studio tree, right-click the **XML Schema Document Bundles > User-defined Catalog**
    node, and select **Add Schema**. The **Load Schema**
    dialog enables you to load a schema directly from the file system.
2. In the **File location**
    field, enter or browse to the location of the schema file and click **Next**.
3. In the **Retrieve and Validate**
    window, enter a user name and a comment for this version of the schema. Click **Next**.

    {{< alert title="Note" color="primary" >}}If the WSDL fails validation, an error is displayed. For details, see [XML schema and WSDL document validation](#xml-schema-and-wsdl-document-validation) and [XML schema and WSDL document limitations](#xml-schema-and-wsdl-document-limitations){{< /alert >}}

4. Click the **Finish** button to import the schema into the cache.

## Add WSDL documents to the cache

WSDL documents are cached automatically when you import a WSDL file to register a web service.

Alternatively, you can import a WSDL document directly into the cache. To add a WSDL to the global cache, perform the following steps:

1. In the Policy Studio tree, right-click the **WSDL Document Bundles**
    node, and select **Add a WSDL**. The **Load WSDL**
    dialog enables you to load a WSDL file directly from the file system, from a URL, or from a UDDI registry.
2. Select the appropriate option and enter or browse to the location of the WSDL file in the fields provided. To retrieve the WSDL file from a UDDI registry, click the **WSDL from UDDI**
    option, and click the **Browse UDDI**
    button. This option enables you to connect to a UDDI registry and search it for a particular WSDL file. Click **Next**
    to continue.
3. In the **Retrieve and Validate**
    window, enter a user name and a comment for this version of the WSDL.
4. Click the **Finish**
    button to import the WSDL document into the cache.

    {{< alert title="Note" color="primary" >}}If the WSDL fails validation, an error is displayed. For details, see [XML schema and WSDL document validation](#xml-schema-and-wsdl-document-validation) and [XML schema and WSDL document limitations](#xml-schema-and-wsdl-document-limitations){{< /alert >}}

If you import a WSDL document directly into the cache using the **WSDL Document Bundles**
node, Policy Studio does *not* automatically generate policies and service handlers. To auto-generate policies and service handlers for a web service, you must use the **Register Web Service** option under the **APIs** > **Web Service Repository** node.

## Update cached WSDL or XML schema documents

You can update a WSDL document or XML schema in the cache by using the **Resynchronize WSDL**
or **Resynchronize Schema**
options. This enables you to import a more recent version of a WSDL or schema directly to the global cache.

To update a WSDL, perform the following steps:

1. Expand the **WSDL Document Bundles**
    node.
2. Expand the document bundle for the WSDL to be updated.
3. Right-click the **Tracking from**
    node and select **Resynchronize WSDL**.
4. Enter the location of the latest version of the WSDL and click **Next**. This defaults to the location from which the WSDL was originally loaded.
5. In the **Retrieve and Validate**
    window, enter a user name and a comment for this version of the WSDL.
6. Click **Finish**
    to import the new version of the WSDL document into the cache.

API Gateway compares the contents of the WSDL and any of its imported schemas to those stored in its cache. If the contents are different, it creates a new version of the WSDL.

To update an XML schema, perform the following steps:

1. Expand the **XML Schema Document Bundles > User-defined Catalog**
    node.
2. Expand the document bundle for the schema to be updated.
3. Right-click the **Tracking from**
    node and select **Resynchronize Schema**.
4. Enter the location of the latest version of the schema and click **Next**. This defaults to the location from which the schema was originally loaded.
5. In the **Retrieve and Validate**
    window, enter a user name and a comment for this version of the schema.
6. Click **Finish**
    to import the new version of the schema document into the cache.

API Gateway compares the contents of the schema and any of its imported schemas to those stored in its cache. If the contents are different, it creates a new version of the schema.

See [Update a web service](/docs/apim_policydev/apigw_web_services/general_ws_repository/#update-a-web-service)
for more information on the types of changes to a WSDL or schema that trigger creation of a new version.

## Delete cached WSDL or XML schema documents

You can delete a WSDL document or XML schema in the cache by using the **Remove Tracking Details**
option.

To delete a WSDL, perform the following steps:

1. Expand the **WSDL Document Bundles**
    node.
2. Expand the document bundle for the WSDL to be deleted.
3. Right-click the **Tracking from**
    node and select **Remove Tracking Details**.
4. Click **Yes**
    to confirm the removal.

API Gateway removes the WSDL from the cache.

To delete an XML schema from the user-defined catalog, perform the same steps under the **XML Schema Document Bundles > User-defined Catalog**
node.

## XML schema and WSDL document validation

XML schemas and WSDL documents are validated during import. If validation fails, an error is displayed. For example, validation fails if a schema type is referenced in a WSDL or schema, but that WSDL or schema does not contain an explicit import statement for the schema that contains the type definition for the referenced type.

{{< alert title="Note" color="primary" >}}API Gateway requires all XML schemas and WSDL documents to be present and valid at runtime, and applies very strict validation checks during import. Therefore, WSDL documents that validate successfully in other tools do not necessarily validate in API Gateway, and you might need to preprocess any XML schemas or WSDL documents to make them valid, before attempting to import them in Policy Studio.{{< /alert >}}

A good starting place is to ensure that all WSDLs and their schemas are Web Service Interoperability (WS-I) compliant before attempting to import them into Policy Studio. WS-I compliance ensures maximum interoperability between different vendors' implementation of web services standards, such as SOAP and WSDL.

An out-of-the-box installation of API Gateway contains a number of common XML schemas (for example, from OASIS and W3C) preloaded in the global cache. If you import a WSDL that imports any of these standard schemas, API Gateway uses the cached version of those schemas, instead of retrieving them from the web.

For example, the SOAP encoding schema is often imported into WSDLs that serialize message parts as SOAP encoding arrays. When such a WSDL is imported into Policy Studio, it recognizes that this schema already exists in the cache and does not download and import a duplicate version.

This optimization avoids unnecessary duplication of shared schema resources and reduces the overall memory footprint of the API Gateway's configuration store.

## XML schema and WSDL document limitations

There are some additional limitations when importing XML schema or WSDL documents:

* **Non-SOAP bindings**\
    Any HTTP and MIME bindings in the WSDL document are ignored, and only SOAP 1.1 and SOAP 1.2 bindings are imported.
* **Multiple ports for the same service**\
    If the WSDL contains multiple ports for the same service (for example, a service is available over SSL and in the clear, where the URL differs, but the binding is to the same SOAP service), you can select only one of the ports for import.

    If you absolutely require both endpoints to be virtualized on API Gateway, you can create a separate service for each port in the WSDL. A distinct service handler is created for each service, which is responsible for processing requests for that service and routing them on to the endpoint URL specified in the port.
* **Schemas using the XML Schema namespace to extend element types, but not importing the namespace explicitly**\
    Although some tools can work with invalid schemas like this, API Gateway requires them to be valid so it can run schema validation checks against the messages. The schema must be modified to import the namespace explicitly before you can import the schema in Policy Studio.
* **SOAP bodies with no children**\
    For a SOAP binding style of `"document"`, API Gateway does not support any operation whose request SOAP Body has no child element.
* **Input-only SOAP operations**\
    API Gateway does not currently support input-only SOAP operations, as these operations have no concept of a response message, and API Gateway has problems generating the **Web Service Filter**
    for these operations.

    When a **Web Service Filter**
    is generated as a result of virtualizing a web service, it handles both the request and response messages for the operations that are exposed by that service. Because input-only or notification-style SOAP operations have no concept of a response message, the **Web Service Filter**
    is not ideal, and a custom policy is recommended instead.
* **Output-only SOAP operations**\
    API Gateway does not currently support output-only SOAP operations, as API Gateway has problems generating the **Web Service Filter**
    for these operations.

    Similar to input-only operations, output-only or solicit-response operations cause difficulties for a generated **Web Service Filter**
    and a custom policy is recommended to process this type of request instead.
* **Multiple bindings with different WS-Policy requirements**\
    This results in multiple sets of security requirements that need to be configured by the user, and this is not currently supported by the **Import WSDL**
    wizard in Policy Studio.
* **WSDL messages that contain multiple parts**\
    API Gateway does not support importing a WSDL where the `<wsdl:message>`
    contains multiple parts. A workaround is to change the WSDL so that each `<wsdl:message>`
    contains a single `<wsdl:part>`
    that references a schema complex type that *wraps*
    the message parts currently defined in each `<wsdl:message>`.
* **Schemas without the schemaLocation attribute**\
    API Gateway requires schemas to import other schemas using both the namespace and schemaLocation attributes, except in the following circumstances:
    * The schemas are embedded in the WSDL. The schemaLocation attribute can be omitted from the schema import element and the schema resolver looks for the imported schemas in the WSDL itself.
    * The schemas import a schema from the system catalog (which comes preloaded with a number of common schemas, for example, the SOAP encoding schema). A schema from the system catalog can be imported into any schema using just the namespace attribute.

## Version and duplicate management

API Gateway manages the resources associated with any WSDL documents or XML schema documents stored in the global cache. This includes the WSDL or XML schema documents themselves, and any WSDL or XML schemas imported by those documents.

When a new resource is being added to the global cache (for example, when you import a WSDL or add a new XML schema), API Gateway compares each resource against the existing resources in the cache. API Gateway tracks two items for each resource:

* The location of the resource
* The contents of the resource at that location

By tracking these two items, API Gateway can identify when a resource is a new version of an existing resource at the same location, or when a resource is the same as an existing resource at a different location. In both cases a new version of the resource is created.

This means that resources that are shared by multiple web services are not duplicated in the global cache, and that web services can be updated easily if the WSDL defining the back-end web service changes.

A common example of this is where a vendor implements multiple web services that share common data structures, for example, an object that stores employee details. When the WSDL and schema files for these services are generated, they typically all import a common schema file that defines the employee details data structure.

On importing these WSDLs into Policy Studio, API Gateway recognizes that the services all share a common `employees`
schema, and avoids importing multiple copies of the schema into the cache. Instead, each imported web service *points*
at the common schema.

## Validate messages against XML schemas

The **Schema Validation** filter is used to validate XML messages against schemas stored in the cache. It can be configured to validate messages against schemas stored in the cache, and also against schemas embedded within WSDL stored in the cache. This filter is found in the **Content Filtering** category of filters in Policy Studio.

## Test a WSDL for WS-I compliance

Before importing a WSDL file, you can check it for compliance with the WS-I Basic Profile. The Basic Profile consists of a set of assertions and guidelines on how to ensure maximum interoperability between different implementations of web services. For example, there are recommendations on what style of SOAP to use (`document/literal`), how schema information is included in WSDL files, and how message parts are defined to avoid ambiguity for consumers of WSDL files.

Policy Studio uses the Java version of the WS-I testing tools to test imported WSDL files for compliance with the recommendations in the Basic Profile. A report is generated showing which recommendations have passed and which have failed. While you can still import a WSDL file that does not comply with the Basic Profile, there is no certainty that consumers of the web service can use it without encountering problems.

{{< alert title="Note" color="primary" >}}Before you run the WS-I compliance test, you must ensure that the Java version of the WS-I testing tools is installed on the machine on which Policy Studio is running. You can download these tools from [www.ws-i.org](http://www.ws-i.org/). {{< /alert >}}

To configure the location of the WS-I testing tools, select **Window** > **Preferences**
from the Policy Studio main menu. In the **Preferences** dialog, select **WS-I Settings**, and browse to the location of the WS-I testing tools. You must specify the *full path* to these tools (for example, `C:\Program Files\WSI_Test_Java_Final_1.1\wsi-test-tools`).

### Run the WS-I compliance test

 To run the WS-I compliance test on a WSDL file, perform the following steps:

1. Select **Tools** > **Run WS-I Compliance Test**
    from the Policy Studio main menu.
2. In the **Run WS-I Compliance Test**
    dialog, browse to the **WSDL File**
    or specify the **WSDL URL**.
3. Click **OK**. The WS-I analysis tools run in the background in Policy Studio.

The results of the compliance test are displayed in your browser in a **WS-I Profile Conformance Report**. The overall result of the compliance test is displayed in the **Summary**. The results of the WS-I compliance tests are grouped by type in the **Artifact:description** section. For example, you can access details for a specific port type, operation, or message by clicking the link in the **Entry List** table. Each **Entry** displays the results for the relevant WS-I test assertions.
