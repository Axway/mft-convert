{
    "title": "API Manager REST APIs",
    "linkTitle": "REST APIs",
    "weight": "40",
    "date": "2019-09-17",
    "description": "Perform create, read, update, and delete (CRUD) operations on API Manager data."
}

The API Manager REST APIs enable you to perform create, read, update, and delete (CRUD) operations on API Manager data. For example, this includes configured APIs, users, organizations, applications, quotas, metrics, alerts and events related to API Manager.

The API Manager REST APIs are available from the following locations:

* `INSTALL_DIR/apigateway/samples/swagger`
* [Product APIs](https://docs.axway.com/category/api) page on the Axway Documentation portal
    {{< alert title="Note" color="primary" >}}When viewing REST APIs on the Axway Documentation portal, the `consumes` field is not displayed if you are using `formData` type parameters in an API, due to limitations in the Swagger UI version.{{< /alert >}}

## Import the API Manager REST API

You can import the API Manager REST API Swagger 2.0 (OAS2) or OAS3 definitions into API Manager in the same way that you import any other APIs. For example:

1. Click the **API Registration** > **Backend API** view in API Manager.
2. Click **New API** and select **Import Swagger API**.
3. In the **Import API** dialog, complete the following:
    * **Source**: Select Swagger definition file.
    * **File** or **URL**: Click the browse button to select the definition file. For example:
        `INSTALL_DIR/apigateway/samples/swagger/api-manager-V_1_3-swagger.json`
    * **API Name**: Enter a user-friendly name for the API. The default is **api-manager-V_1_3-swagger.json**.
    * **Organization**: Select the organization from the list (for example, **API Development**).
4. Click **Import** to import the API Manager API.

The following image shows the details of the API Manager v1.3 Swagger 2.0 back-end API.

![API Manager REST API Swagger 2.0 Definition](/Images/docbook/images/api_mgmt/api_mgmt_rest_api.png)

For more details, see [Register REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/).
