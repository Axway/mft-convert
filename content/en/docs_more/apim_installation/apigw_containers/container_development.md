---
 title :  Development and deployment with API Gateway containers 
 linkTitle :  Development and deployment
 weight: 95
 date :  2019-09-18 
 description :  Develop and test APIs and policies in a development environment, and promote and deploy them in other environments (for example, preproduction and production).
---

In a typical API Gateway container deployment:

* You create Docker images in your build or continuous integration (CI) environment from various artifacts checked out of your source control system. The resulting images are versioned and immutable.
* You start Docker containers from the immutable images in a specific runtime environment (for example, development, preproduction, or production).

Alternatively, you can create images and start containers in a single CI/CD pipeline, where artifact updates trigger a build that results in the automatic creation or refresh of an execution environment.

## Test in development environment

In a development environment, the policy developer works in a continuous cycle of iterative development, deployment, and testing. In this environment, it makes sense to keep all API Gateway configuration in a single deployment package (`.fed` file), which you can deploy or export from Policy Studio.

As a developer, you have three different options when testing in a development environment:

* Use an API Gateway that is running in classic (non-containerized) mode. In this case, to test changes you can deploy configuration changes directly from Policy Studio.
* Use a deployment-enabled API Gateway container. In this case, to test changes you can deploy directly from Policy Studio to the running container. Follow these steps:
    1. When starting the API Gateway Docker container, specify `EMT_DEPLOYMENT_ENABLED=true`. For an example, see [Start a deployment-enabled API Gateway container](/docs/apim_installation/apigw_containers/docker_script_gwimage/#start-a-deployment-enabled-api-gateway-container-in-a-development-environment).
    2. Make the configuration changes to be tested in Policy Studio and click **Deploy** in the toolbar to deploy the updated configuration to the running API Gateway container.
* Use an API Gateway container that does not have deployment enabled. In this case, to test changes you must export the configuration from Policy Studio and use it to generate a new API Gateway image and start a new container. Follow these steps:
    1. Make the configuration changes to be tested in Policy Studio and select **File > Export > Deployment Package** to export the configuration as a deployment package (`.fed`).
    2. When creating the API Gateway Docker image using `build_gw_image.py`, specify the deployment package you exported from Policy Studio. For an example, see [Create an API Gateway image using existing fed and customized configuration](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-gateway-image-using-existing-fed-and-customized-configuration).

## Promote to preproduction environment

After the configuration is deployed and tested in the development environment, you can promote it to the preproduction environment. This involves environmentalizing the configuration that will require environment-specific settings in upstream environments and exporting it as a policy package (`.pol` file) from Policy Studio, and creating an environment package (`.env` file) that is specific to the preproduction environment in Configuration Studio.

To promote a configuration to a preproduction environment, follow these steps:

1. Environmentalize the configuration in Policy Studio and select **File > Export > Policy Package** to export the configuration as a policy package (`.pol`).
2. Create the environment package for the preproduction environment in Configuration Studio and select **File > Save > Environment Package** to export the environment package (`.env`).
3. When creating the API Gateway Docker image using `build_gw_image.py`, specify the policy package and preproduction environment package you exported from Policy Studio and Configuration Studio. For reference, see the example [Create an API Gateway image using existing fed and customized configuration](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-gateway-image-using-existing-fed-and-customized-configuration), however, in this case you will need to specify a `.pol` and `.env` instead of a `.fed`.

For a detailed example of promoting configuration through environments, see [Example: Promote from development to testing environment](/docs/apigtw_devops/promotion_example/).

## Promote to production environment

After the configuration is deployed and tested in the preproduction environment, you can promote it to the production environment. This involves creating an environment package (`.env` file) that is specific to the production environment in Configuration Studio.

To promote a configuration to a production environment, follow these steps:

1. Create the environment package for the production environment in Configuration Studio and select **File > Save > Environment Package** to export the environment package (`.env`).
2. When creating the API Gateway Docker image using `build_gw_image.py`, specify the policy package and production environment package you exported from Policy Studio and Configuration Studio. For reference, see the example [Create an API Gateway image using existing fed and customized configuration](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-gateway-image-using-existing-fed-and-customized-configuration), however, in this case you will need to specify a `pol` and `env` instead of a `.fed`.

For a detailed example of promoting configuration through environments, see [Example: Promote from development to testing environment](/docs/apigtw_devops/promotion_example/).

## Continuous policy deployment

You can continuously deploy policy updates in a container environment, as part of your CI/CD process by scripting the steps detailed in [Create and start API Gateway Docker container](/docs/apim_installation/apigw_containers/docker_script_gwimage/) and specifying the latest policy package (`.pol`) exported from the development environment and the environment package (`.env`) for the production environment to the `build_gw_image.py` script.

## Promote APIs registered in API Manager

You can promote APIs registered in API Manager using the approaches described in [Promote managed APIs between environments](/docs/apim_administration/apimgr_admin/api_mgmt_promote/).
