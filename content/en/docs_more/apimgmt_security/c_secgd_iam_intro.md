{
"title": "Identity and access management",
"linkTitle": "Identity and access management",
"weight": 40,
"date": "2019-11-25",
"description": "Summary of the identity and access management features of API Gateway."
}

## API Gateway RBAC model

API Gateway uses Role-Based Access Control (RBAC) to restrict access to authorized users based on their assigned roles in a domain. Permissions to perform specific system operations are assigned to specific roles, and system users are granted permission to perform specific operations only through their assigned roles. This simplifies system administration because users do not need to be assigned permissions directly, but instead acquire them through their assigned roles.

For example, the default administrator user (for example, `admin`) has the following user roles:

* Policy Developer
* API Gateway Administrator
* KPS Administrator

The following diagram illustrates the RBAC model:

![RBAC model](/Images/docbook/images/rbac/rbac_overview.png)

The following is a description of the diagram:

* Clients initiate management service requests via the Admin Node Manager.
* The Admin Node Manager uses RBAC to determine if the user has permission to access the requested service.
* If the user has permission, the request can be completed. Otherwise, the request is denied.

For a more detailed explanation of RBAC in API Gateway, see [Configure Role-Based Access Control (RBAC)](/docs/apim_administration/apigtw_admin/general_rbac/).

## Available resources and actions

The Admin Node Manager component exposes a number of REST management services, which are all protected by RBAC. For example, the exposed services and the associated tools that use them include the following:

| Protected resource         | Tool                | Description                                                                  |
|----------------------------|---------------------|------------------------------------------------------------------------------|
| Traffic Monitoring Service | API Gateway Manager | Displays HTTP, HTTPS, JMS, and FTP message traffic processed by API Gateway. |
| Configuration Service      | API Gateway Manager | Adds and removes tags on API Gateway.                                        |
| Topology API               | API Gateway Manager | Accesses and configures API Gateway domains.                                 |
| Static Content Resources   | API Gateway Manager | Manages UI elements in a browser.                                            |
| Deployment API             | Policy Studio       | Deploys configurations to API Gateway.                                       |
| KPS Service                | Policy Studio       | Manages a Key Property Store.                                                |

## Predefined privileges and roles

User roles have specific tools and privileges assigned to them. These roles define who can use which tools to perform what tasks. The user roles provided with API Gateway assign the following privileges to administrator users with these roles:

| Role                      | Tool                | Privileges                                                                                    |
|---------------------------|---------------------|-----------------------------------------------------------------------------------------------|
| Policy Developer          | Policy Studio       | Download, edit, deploy, version, and tag a configuration.                                     |
| API Gateway Administrator | API Gateway Manager | Read/write access to API Gateway Manager.                                                     |
| API Gateway Operator      | API Gateway Manager | Read-only access to API Gateway Manager.                                                      |
| Deployer                  | Deployment scripts  | Deploy a new configuration.                                                                   |
| KPS Administrator         | KPS Web UI          | Perform create, read, update, delete (CRUD) operations on data in a Key Property Store (KPS). |
