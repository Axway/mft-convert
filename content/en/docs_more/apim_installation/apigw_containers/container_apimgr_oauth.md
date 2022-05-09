---
 title :  Deploy API Manager or OAuth in Docker containers 
 linkTitle :  Deploy API Manager or OAuth 
 weight: 80
 date :  2019-09-18 
 description :  Deploy API Manager or OAuth services in your API Gateway containers.
---

These steps are optional and only for users who wish to use API Manager or OAuth in a containerized environment.

## Deploy API Manager

To deploy API Manager in a Docker container, follow these steps.

### Configure API Manager in Policy Studio {#apimgrps}

Follow these steps:

1. Open Policy Studio and open or create a new project.
2. Select **File > Configure API Manager**.
3. If you do not have any Cassandra hosts configured, you must add a Cassandra host before you can continue:
    * Enter a name for the Cassandra server (for example, `container_cassandra`).
    * Enter the name of the Cassandra container as the host name (for example, `cassandra228`).
    * Enter the port of the Cassandra container (for example, `9042`).

4. Click **Next**.
5. Enter the appropriate API Manager settings. For full details, see [Enable API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_config/#enable-api-manager).

    {{< alert title="Note" color="primary" >}}The default API administrator user name and password set in Policy Studio are used only when creating the administrator account in Apache Cassandra. After the account has been created in Cassandra, you cannot change the credentials in Policy Studio. You must use API Manager to change the administrator credentials. You can also reset the administrator password by running the `setup-apimanager` script with the option `--resetPassword` inside the Admin Node Manager container. For details, see [Reset the default API administrator password](/docs/apim_installation/apigw_containers/container_troubleshoot/#reset-the-default-api-administrator-password).{{< /alert >}}

6. Click **Finish**.
7. Configure additional API Manager settings under **Server Settings > API Manager**. For example, you can specify custom policies that are called as traffic passes through API Manager.
8. Select **File > Export** and select a package to export the configuration as a package (`fed`, `pol`, or `env`).

### Deploy API Manager enabled API Gateway container {#apimgrdeploy}

Follow the steps in [Create and start API Gateway Docker container](/docs/apim_installation/apigw_containers/docker_script_gwimage/). When creating the API Gateway Docker image using `build_gw_image.py`, specify the deployment package you exported from Policy Studio. For an example, see [Create an API Manager or OAuth enabled API Gateway image](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-manager-or-oauth-enabled-api-gateway-image).

## Deploy OAuth services

To deploy OAuth services in a Docker container, follow these steps.

### Configure OAuth in Policy Studio {#oauthps}

Follow these steps:

1. Open Policy Studio and open or create a new project.
2. Select **File > Configure OAuth**.
3. If you do not have any Cassandra hosts configured, you must add a Cassandra host before you can continue:
    * Enter a name for the Cassandra server (for example, `container_cassandra`).
    * Enter the name of the Cassandra container as the host name (for example, `cassandra228`).
    * Enter the port of the Cassandra container (for example, `9042`).
4. Click **Next**.
5. Select the OAuth deployment type. For full details, see [Deploy OAuth configuration](/docs/apim_policydev/apigw_oauth/gw_server/#deploy-oauth-services).
6. Click **Finish**.
7. Select **File > Export** and select a package to export the configuration as a package (`fed`, `pol`, or `env`).

### Deploy OAuth-enabled API Gateway container {#oauthdeploy}

Follow the steps in in [Create and start API Gateway Docker container](/docs/apim_installation/apigw_containers/docker_script_gwimage/). When creating the API Gateway Docker image using `build_gw_image.py`, specify the deployment package you exported from Policy Studio. For an example, see [Create an API Manager or OAuth enabled API Gateway image](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-manager-or-oauth-enabled-api-gateway-image).
