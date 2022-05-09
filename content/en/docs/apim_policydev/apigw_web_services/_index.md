{
"title": "Register and secure web services",
"linkTitle": "Register and secure web services",
"weight": 20,
"no_list": true,
"date": "2019-10-17",
"description": "Import a WSDL file to register and secure a web service, or expose a SOAP web service as a REST API."
}

Policy Studio provides the following features that enable you to register and secure web services:

* You can register a web service in Policy Studio by importing a WSDL file into the web service repository. For more information on registering web services, see [Manage web services](/docs/apim_policydev/apigw_web_services/general_ws_repository/).
* The **Import WSDL** wizard enables you to automatically generate the policies to protect web services. For more information on using the wizard, see [Configure policies from WSDL files](/docs/apim_policydev/apigw_web_services/general_policy_wsdl/).
* You can also manually configure policies to protect web services (for example, if a WSDL file is not available). For more information, see [Configure policies manually](/docs/apim_policydev/apigw_poldev/general_manual_policy/#configure-policies-manually).
* You can also invoke registered web services from policies, for example, when you expose a SOAP web service as a REST API, the REST API you define calls a policy to implement the API, which in turn invokes the SOAP web service. For more information, see [Expose a web service as a REST API](/docs/apim_policydev/apigw_web_services/mapper_soap_to_rest/).

## WSDL and XML schema cache

API Gateway maintains a global cache of WSDL and XML schema documents. For more information on adding, updating, and deleting WSDL and XML schema documents, and how API Gateway validates them during import, see [Manage WSDL and XML schema documents](/docs/apim_policydev/apigw_web_services/general_schema_cache/).

## JSON schema cache

API Gateway maintains a global cache of JSON schemas. JSON schemas define the structure of JSON data. For more details on JSON schemas, see [http://www.json-schema.org](http://www.json-schema.org/).

You can manage JSON schemas and schema containers under the **Resources > JSON Schemas** node in the Policy Studio tree.

To add a JSON schema, right-click the **JSON Schemas** node and select **Add Schema**. Browse to the location on the JSON file on disk and click **Open**.

You can also view or edit existing schemas. To view a schema, double-click it in the tree. The JSON is displayed on the right. You can edit the JSON in this view. Click **Save JSON** in the toolbar to save the changes.

Schema containers are used to group related schemas. To add a JSON schema container, right-click the **JSON Schemas** node and select **Add Schema Container**.

## WSDLs from a UDDI registry

WSDL documents can be imported from and published to a Universal Description, Discovery, and Integration (UDDI) registry. For more information, see [Work with a UDDI registry](/docs/apim_policydev/apigw_web_services/general_uddi/).

## Data maps

You can create data maps to map XML and JSON messages to other XML and JSON message formats using a graphical editor. For more information, see [Manage data maps](/docs/apim_policydev/apigw_web_services/resources_data_maps/).

## Policy Studio filters

The following Policy Studio filters are of interest when working with web services:

* The **Web Service** filter is the main filter generated when a WSDL file is imported into the web service repository. It contains all the routing information and links all the policies that are to be run on the request and response messages into a logical flow.
* The **Schema Validation** filter is used to validate XML messages against XML schemas stored in the global cache.
* The **JSON Schema Validation** filter is used to validate JSON messages against JSON schemas stored in the global cache.
* You can use the **Execute Data Map** filter to execute a mapping you defined in a data map as part of a policy.
