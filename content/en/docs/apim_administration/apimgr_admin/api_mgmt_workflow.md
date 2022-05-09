{
    "title": "Get started with API Manager",
    "linkTitle": "Get started",
    "weight": "40",
    "date": "2019-09-17",
    "description": "Quick summary of the steps required to register and virtualize an API in API Manager."
}

{{< alert title="Note" color="primary" >}}
Before you can register APIs in API Manager, you must first enable an organization for API registration and development. The API Manager welcome screen prompts you to automatically create an `API Development` organization, which is enabled for API development by default. For more details, see [Register REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/).
{{< /alert >}}

## Register a back-end REST API in API Manager

To register a back-end API in API Manager, perform the following steps:

1. In API Manager, select **API Registration** > **Backend API**.
2. Click **New API**, and select one of the following:
    * **Import API from Topology**: Import a REST API deployed on an API Gateway.
    * **Import Swagger API**: Import a REST API in JSON or YAML format.
    * **Import WADL API**: Import a REST API in WADL format.
    * **Import WSDL API**: Import a web service in WSDL format.
3. Specify the API details (for example, location, name, and organization), and click **Import**.
4. When the API is imported, click **OK**.

For more details, see [Register REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/).

Alternatively, if you do not have a Swagger or WADL file to import for an existing API, see [Manually register a new back-end REST API](/docs/apim_administration/apimgr_admin/api_mgmt_register_web/#manually-register-a-new-back-end-rest-api).

## Virtualize a front-end REST API in API Manager

To virtualize a front-end API in API Manager, perform the following steps:

1. In API Manager, select **API Registration** > **Frontend API**.
2. Click **New API**, and select **New API from backend API**.
3. Select the existing back-end API, and click **OK**.
4. Select an **Inbound security**  device from the list. The most commonly used security devices are as follows:

    * **API key**: Enables API Manager to control and monitor client applications that can access APIs by requiring users to authenticate with an API key.
    * **Pass through**: API Manager does not control and monitor access to the API, and does not use its client registry for the API, which is effectively public. However, the backend API may have its own authentication mechanism.

5. Specify the settings for the security device in the dialog, and click **OK**.
6. If the back-end API is accessed using HTTPS, click the **Trusted Certificates** tab, and click the plus icon on the left. In the dialog, you can specify the URL to valid back-end content, and authentication parameters (if required). For example, you can use the URL for the Swagger or WADL file that you already used to import the back-end API.
7. When finished, click **Save**.

For more details, see [Virtualize REST APIs in API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/). The following example shows an existing Swagger-based back-end API virtualized as a front-end API:

![Virtualized API details in the web console](/Images/docbook/images/api_mgmt/api_mgmt_frontend_api_edit.png)
