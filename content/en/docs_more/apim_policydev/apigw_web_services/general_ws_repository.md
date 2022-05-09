{
"title": "Manage web services",
"linkTitle": "Manage web services",
"weight": 2,
"date": "2019-10-17",
"description": "Learn how to manage web services and groups, and understand what happens when you import a WSDL file to register a web service."
}

The **Web Service Repository** stores information about web services whose definitions have been imported using Policy Studio. The WSDL files that contain these web services definitions are stored together with their related XML schemas. Clients of the web service can then query the repository for the WSDL file, which they can use to build and send messages to the web service using API Gateway.

The web service repository enables you to register web services by importing a WSDL file, and to group related web services into groups. When you register a web service in the web service repository, Policy Studio auto-generates a **Service Handler** that is used to control and validate requests to the web service and responses from the web service. If a web service is updated after initial import, you can resynchronize with it to import new versions of the WSDL and XML schemas, and the **Service Handler** is regenerated to reflect the updates.

## Manage web services and groups

The **Web Service Repository** is displayed under the **APIs** node in the Policy Studio tree. WSDL files are imported into web service groups, which provide a convenient way of keeping groups of related web service definitions together. To import a WSDL file into the default group, right-click the **Web Services** node, and select **Register Web Service**.

Alternatively, to add a new web service group, right-click the default **Web Services** group, or the **Web Service Repository** node, and select **Add a new web services group**. When the new group is added, you can right-click it in the tree, and select **Register Web Service**.

## Register a web service

Registering a web service involves importing the WSDL file that contains the definitions for the web service. Policy Studio provides an **Import WSDL** wizard to make registering a web service a simple process that requires minimal manual intervention. For more information on registering a web service by importing a WSDL file, see [Configure policies from WSDL files](/docs/apim_policydev/apigw_web_services/general_policy_wsdl/).

The **Import WSDL** wizard auto-generates policies and service handlers for each web service imported. These are automatically configured wherever possible, based on the imported WSDL. This means that only a small number of fields need to be configured manually.

After a web service has been registered using the **Import WSDL** wizard, the WSDL for the service is *published*
by API Gateway. Clients can specify `WSDL` on a request query string to retrieve the WSDL file (for example, `http://localhost:8080/HelloWorldService/HelloWorld?WSDL`).

{{< alert title="Tip" color="primary" >}}You can also register a web service using a script. For more information, see [Use scripts to manage web services](#use-scripts-to-manage-web-services).{{< /alert >}}

## Results of registering a web service

Service handlers and policies are auto-generated when you register a web service by importing a WSDL file in Policy Studio. The specific policies that are created depends on whether you configured a policy to enforce security between the client and API Gateway, and whether the imported WSDL file contained any WS-Policy assertions. The following list summarizes what is created by the wizard:

### Web service

A new web service tree node is created in the **Web Service Repository** tree for each web service that you selected to expose operations from in the imported WSDL. The web service is created in the **Web Services** group by default.

Click the new web service node to list the WSDL file for the service and any imported WSDL files and XML schemas. To view the source for a WSDL or XML schema file, click the file and a read-only view of the source is displayed.

{{< alert title="Note" color="primary" >}}It is important to realize that the WSDL displayed in this view represents the WSDL that is exposed to the client. Therefore, it contains only those operations that were selected during the import process, together with any recipient WS-Policy that has been applied to the virtualized web service. You can view the WSDL in its original form under the **Resources > WSDL Document Bundles** node.{{< /alert >}}

You can add, remove, or edit recipient WS-Policies for the service from this node. For example, to edit an existing recipient WS-Policy, right-click the web service node, and select **WS-Policy > Edit Recipient WS-Policy**. If the imported WSDL file included WS-Policy assertions, you can also edit the initiator WS-Policy (for example, to change the signing key). Right-click the web service node and select **WS-Policy > Edit Initiator WS-Policy**
.

### WSDL resources

A new WSDL document bundle is created for the imported WSDL file under the **Resources > WSDL Document Bundles**
tree node. This document bundle contains the original WSDL document for the back-end web service, as retrieved at the time of the WSDL import. If multiple versions of this WSDL have been imported, each version is available here. The document bundle also contains the original versions of any resources imported by the WSDL, such as other WSDL files or XML schemas.

Click the WSDL document bundle to list the versions that have been imported. Click a particular version to list the WSDL file and any imported WSDL files and XML schemas, as they appeared for that version. To view the source for a WSDL or XML schema file, click the file and a read-only view of that version of the source is displayed.

### Policy container

A container for the newly generated policies is created under the **Generated Policies** node in the **Policies**
tree. The new container is named after the service (for example, `Web Services.HelloWorldService`).

### Policy

A policy for the web service is created in the generated policy container. The policy name is the name of the service (for example, `HelloWorldService`). This policy contains the **Service Handler** for the service. The service handler is an auto-configured **Web Service Filter**.

### Service handler

The **Service Handler** is used to control and validate requests to the web service and responses from the web service. The **Service Handler** is named after the web service (for example, `Service Handler for 'HelloWorldService'`). The **Service Handler** is a **Web Service Filter**, and is used to control the following:

* Routing
* Validation
* Message request/response processing
* WSDL options
* Monitoring options

### Security policies

If you configured a WS-policy to enforce security between the client and API Gateway (as described in [Configure a security policy](/docs/apim_policydev/apigw_web_services/general_policy_wsdl/)), or if the imported WSDL file contained WS-Policy assertions, a number of additional policies are automatically created in a generated policy container named `WSPolicy`. Any recipient policies are created in a container named `Recipient` and any initiator policies are created in a container named `Initiator`. These generated policies include the filters required to generate and validate the relevant security tokens (for example, SAML tokens, WS-Security `UsernameToken` elements, and WS-Addressing headers). These policies perform the necessary cryptographic operations (for example, signing/verifying and encryption/decryption) to meet the security requirements of the specified policies.

## Export a web service

To export a web service, right-click on the web service node under the **Web Service Repository** and select **Export Web Service**.

The following items are exported:

* All circuit containers for the web service, including policies and containers that were generated as part of WS-Policy configuration.
* The current version of the WSDL and its schemas. If multiple versions of the WSDL are available, only the current version is exported. The complete history of the WSDL is *not* exported.
* Optionally, other configuration used by the policies can be exported (for example, remote host configuration).

## Update a web service

Over the lifetime of a web service, the service definition for the web service can change. You can manage the lifecycle of a web service easily, by using the **Resynchronize Web Service** option in Policy Studio. This enables you to update the web service definition for a service by revisiting the location of the WSDL.

To update a web service, right-click the web service node, and select **Resynchronize Web Service**. You are prompted for the location of the latest version of the WSDL. This defaults to the location from which the WSDL was originally loaded. API Gateway compares the contents of the WSDL and any of its imported schemas to those stored in its cache. If the contents are different, it creates a new version of the WSDL.

Examples of possible updates to the WSDL or XML schemas that trigger creation of a new version are as follows:

* **Service name (local part)**
    If the service name has changed in the WSDL, the import exits with the error `"Couldn't find target Service in this WSDL"`. You can import this WSDL definition as a new web service using the **Register Web Service** option.
* **WSDL namespace**
    If the namespace has changed in the WSDL, the import continues with a warning. Namespace changes result in a new node being created under the **Resources > WSDL Document Bundles** tree. Any subsequent resynchronizations of the service result in new versions being added to this node (assuming no further namespace changes).
* **WSDL 2.0**
    If the WSDL has changed to use the WSDL 2.0 specification, import exits with an error. API Gateway supports WSDL 1.1 only, it does not support WSDL 2.0.
* **Operation added**
    If a new `operation` has been added to a `portType` in the WSDL, import succeeds. API Gateway regenerates the configuration, using older operations as a template for the WS-Policy settings, if attached. The service handler for the web service is updated to include a resolver for the new operation.
* **Operation modified**
    If an `operation` has been modified on a `portType` in the WSDL (for example, changes to the `input`, `output`, or `fault`), import succeeds. API Gateway regenerates the configuration and updates the service handler for the web service.
* **SOAPAction**
    If the `SOAPAction` has changed for an `operation` in the WSDL, import succeeds. API Gateway regenerates the configuration and updates the service handler for the web service.
* **Operation removed**
    If an `operation` has been removed from a `portType` in the WSDL, import succeeds. API Gateway removes the operation from the configuration. The service handler for the web service is updated to remove the resolver for the removed operation.
* **XML schema (when WSDL validation is in use)**
    If you are using the WSDL to validate incoming messages (this is the default option when you import a WSDL file), and the schemas in the WSDL have been updated, API Gateway retrieves the updated schemas.

Updating a web service is *not* possible for the following types of WSDL or XML schema changes:

* Port, binding, or operation changes that require user intervention for WS-Policy configuration.
* Port change (new endpoint location).
* Port added (SOAP flavor).
* Port change (new binding).
* Binding style or use change.
* WS-Policy added.
* WS-Policy modified.
* WS-Policy removed.
* Operation message part changes (for example, reference to an element changes to a type, part name changes, and so on).
* XML schema changes (when custom schema validation is in use).

## Change the operations exposed by a web service

When you register a web service by importing a WSDL file, you are prompted to select the operations that are to be exposed to the client. You can change the operations that are exposed after import by using the **Select Exposed Operations** option in Policy Studio. For example, if you imported a web service containing two operations, `foo`
and `bar`, and you only selected the `foo` operation at import time, you can use this feature to also expose the `bar`
operation after import.

To change the operations exposed by a web service, right-click the web service node, and select **Select Exposed Operations**. All of the operations defined in the WSDL file are displayed, and the operations that are currently exposed are selected. Select the operations to be exposed and deselect the operations that are not to be exposed and click **OK**.

API Gateway regenerates the configuration. The service handler for the web service is updated to include resolvers for any newly exposed operations, and to remove resolvers for any removed operations. The WSDL exposed to clients of the virtualized service is updated to reflect the changes.

## Delete a web service

To delete a web service, right-click on the web service node under the **Web Service Repository** and select **Delete**.

{{< alert title="Note" color="primary" >}}To delete the WSDL document or XML schemas associated with a web service, see [Delete cached WSDL or XML schema documents](/docs/apim_policydev/apigw_web_services/general_schema_cache/#delete-cached-wsdl-or-xml-schema-documents).{{< /alert >}}

## Use scripts to manage web services

You can also use scripts to manage web services. The following sample scripts are provided in the `INSTALL_DIR/apigateway/samples/scripts/ws` directory:

* `listWebServices.py`
    – List web services
* `registerWebService.py`
    – Register a web service
* `removeWebService.py`
    – Delete a web service

You could also create your own custom scripts, for example, to update a web service. However, it would only be possible to script the same type of WSDL or XML schema updates that are supported by the Policy Studio **Resynchronize Web Service** option.

## Publish the WSDL

When the WSDL has been imported into the web service repository, it can be retrieved by clients. In effect, by importing the WSDL into the repository, you are *publishing* the WSDL. In this way, consumers of the services defined in the WSDL can learn how to communicate with those services by retrieving the WSDL for those services. However, to do this, the location of the service must be changed to reflect the fact that API Gateway now sits between the client and the defined service.

For example, assume that the WSDL file states that a particular service resides at `http://www.example.com/services/myService`:

```
<service name="myService">
  <port binding="SoapBinding" name="mySample">
    <wsdl:address location="http://www.example.com/services/myService"/>
  </port>
</service>
```

When deployed behind API Gateway, this URL is no longer accessible to consumers of the service. Because of this, clients must send SOAP messages through API Gateway to access the service. In other words, they must now address the machine hosting API Gateway instead of that directly hosting the service.

When returning the WSDL to the client, API Gateway dynamically changes the value of the `location` attribute in the `service` element in the WSDL file to point to the machine on which API Gateway resides. API Gateway is responsible for routing messages on to the machine hosting the service.

Assuming that API Gateway is running on port `8080` on a machine called `SERVICES`, the location specified in the exported WSDL file is changed to the following:

```
<service name="myService">
  <port binding="SoapBinding" name="mySample">
    <wsdl:address location="http://SERVICES:8080/services/myService"/>
  </port>
</service>
```

When the client retrieves this modified WSDL file, it routes messages to the machine hosting API Gateway instead of attempting to directly access the web service.

### Access the WSDL

For the client to access this modified WSDL file, Policy Studio provides a WSDL retrieval facility whereby clients can query the web service repository for the WSDL file for a particular web service. To do this, the client must specify `WSDL` on a request query string to the relative path mapped to the policy for this web service.

For example, if the policy is deployed under `http://SERVICES:8080/services/getQuote`,the client can retrieve the WSDL for this web service by sending a request to `http://SERVICES:8080/services/getQuote?WSDL`. When the client has a copy of the updated WSDL file, it knows how to create correctly formatted messages for the service, and more importantly, it knows to route messages to API Gateway rather than to the web service directly.

### Publish to UDDI

For details on how to publish a WSDL file registered in the web service repository to a UDDI registry, see [Publish WSDL files to a UDDI registry](/docs/apim_policydev/apigw_web_services/general_uddi/#publish-wsdl-files-to-a-uddi-registry).
