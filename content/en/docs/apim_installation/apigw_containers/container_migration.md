---
 title :  Migrate to container deployment 
 linkTitle :  Migrate to container deployment 
 weight: 100
 date :  2019-09-18 
 description :  Migrate an API Gateway or API Manager classic deployment to an elastic container deployment. 
---

## Prerequisites

* Your 7.7 classic deployment must be configured correctly and working as expected. The 7.7 deployment can be a system you upgraded from an earlier version or it can be a new installation.
* Apache Cassandra and any relational databases your system uses must be in the same network as your Docker host and accessible to your Docker host.
* You must have an appropriate license to run API Gateway or API Manager in a Docker container.

## Sample topology

The following is a sample topology used to demonstrate the steps you must perform to migrate from a classic deployment to a container deployment. Your own topology might be significantly different and you must adapt the steps appropriately.

In this example, the API Gateway 7.7 classic deployment being migrated has the following topology:

* One Admin Node Manager named ANM1
* API Gateway Group1 contains an API Gateway named GW1
    * API Manager is enabled
    * Apache Cassandra is configured for GW1
    * Metrics processing is enabled and a relational database is configured
    * GW1 is registered with ANM1
* API Gateway Group2 contains an API Gateway named GW2
    * API Manager is enabled
    * GW2 is registered with ANM1

## Migration steps

The steps are as follows.

### Step 1 – Generate domain certificates

Generate a new domain certificate and key as detailed in [Generate domain SSL certificates](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/#generate-domain-ssl-certificates).

Alternatively, you can reuse your existing system domain certificate and key. Domain certificates for a classic deployment are located in the `/INSTALL_DIR/apigateway/groups/certs/` directory. This can be useful if you have domain certificates signed by an external CA and you want to reuse them.

{{< alert title="Note" color="primary" >}}You must not use the same domain certificates for classic and container deployments that are running in parallel. Reusing the same certificates in this case can cause problems as the API Gateway containers might try to register with both the elastic container deployment and classic deployment Node Managers.{{< /alert >}}

### Step 2 – Export Admin Node Manager configuration

To export the Admin Node Manager configuration, follow these steps:

1. In Policy Studio, create a project from existing configuration. Select the Node Manager configuration from your classic deployment (for example, `/INSTALL_DIR/apigateway/conf/fed`).
2. Export the configuration to a `fed` file. Select **File > Save Package** and enter a file name (for example, `ANM.fed`). Use the same passphrase as you used in the classic deployment.

### Step 3 – Export API Gateway configuration

To export the API Gateway configuration, follow these steps:

1. In Policy Studio, create a project from a running API Gateway. Select the Group1 GW1 from your classic deployment. API Manager is already enabled. Leave the password blank for the API administrator user as this is already stored in Cassandra.
2. Export the configuration to a `fed` file. Select **File > Save Package** and enter a file name (for example, `Group1.fed`). Use the same passphrase as you used for Group1 in the classic deployment.
3. Repeat steps 1 to 3 for the Group2 GW2 and export the configuration to a `fed` file (for example, `Group2.fed`) using the same passphrase as you used for Group2 in the classic deployment.

### Step 4 – Export API Manager and KPS data

You can set up your container deployment to either use the existing Apache Cassandra keyspace from your classic deployment or you can create a new keyspace.

To point your API Gateway container to an existing Cassandra keyspace used by your classic deployment, follow these steps:

1. In **Server Settings > Cassandra > Keyspace** change the keyspace name from `x${DOMAINID}_${GROUPID}` to the actual keyspace name of this group in Cassandra (for example, `x5dca69a3_443d_4527_9893_3b4652c9ff6b_group_2`).

This enables the API Gateway container to use the same keyspace as the classic API Gateway.

Alternatively, to create a new keyspace for the API Gateway container, follow these steps:

1. Use `kpsadmin backup` to export the KPS data.
2. Copy the backup directory to the API Gateway container and run `kpsadmin restore` from the Admin Node Manager container.

For more details, see the `kpsadmin` section in the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).

{{< alert title="Note" color="primary" >}}You can also export your API Manager APIs as an API collection if you require a backup.{{< /alert >}}

### Step 5 – Prepare merge directory

You can use a merge directory to add custom configuration when building your Admin Node Manager and API Gateway Docker images. This directory is only for specifying custom configuration settings that are outside the `fed` (for example, `envSettings.props`, custom JARs, and so on).

To create a merge directory, follow these steps:

1. Create a new directory named `apigateway` for each API Gateway group. The structure of this directory must be similar to the `apigateway` directory in an API Gateway installation (with the exception of the `apigateway/groups` directory which has a different structure in a container deployment).

    The following is an example merge directory for Group1 and the Admin Node Manager:

    ```
    /tmp/mergedir/group1/apigateway
    ```

    The following is an example merge directory for Group2:

    ```
    /tmp/mergedir/group2/apigateway
    ```

2. Copy Admin Node Manager and API Gateway GW1 custom configuration files from your classic deployment to the `group1` directory (for example, copy `apigateway/conf/envSettings.props` and custom JARs from `apigateway/ext/lib` and copy `apigateway/groups/group-X/instance-X/conf/envSettings.props` to `apigateway/groups/emt-group/emt-service/conf/envSettings.props`).
3. Copy API Gateway GW2 custom configuration files from your classic deployment to the `group2` directory (for example, copy `apigateway/groups/group-X/instance-X/conf/envSettings.props` to `apigateway/groups/emt-group/emt-service/conf/envSettings.props`).

### Step 6 – Build Admin Node Manager and API Gateway Docker images

To build images, follow these steps:

1. Build a base image as detailed in [Create base Docker image](/docs/apim_installation/apigw_containers/docker_script_baseimage). For example:

    ```
    ./build_base_image.py --installer=apigw-installer.run --os=rhel7 --out-image= apigw-base:1.0
    ```

2. Build an Admin Node Manager image as detailed in [Create an Admin Node Manager Docker image](/docs/apim_installation/apigw_containers/docker_script_anmimage/#create-an-admin-node-manager-docker-image). For example:

    ```
    ./build_anm_image.py --parent-image=apigw-base:1.0 --out-image my-domain-anm:1.0 --merge-dir /tmp/mergedir/group1/apigateway --fed ANM.fed --domain-cert ClassicDomainCert.pem --domain-key ClassicDomainKey.pem --domain-key-pass-file /tmp/domainpass.txt
    ```

3. Build the Group1 API Gateway image as detailed in [Create an API Gateway Docker image](/docs/apim_installation/apigw_containers/docker_script_gwimage/#create-an-api-gateway-docker-image). For example:

    ```
    ./build_gw_image.py --parent-image=apigw-base:1.0 --out-image my-gw-group1:1.0 --license LicenseWithoutHostname.lic --domain-cert ClassicDomainCert.pem --domain-key ClassicDomainKey.pem --domain-key-pass-file /tmp/domainpass.txt --fed Group1.fed
    --fed-pass-file /tmp/fedpass.txt --merge-dir /tmp/mergedir/group1/apigateway --group-id=Group1
    ```

4. Build the Group2 API Gateway image. For example:

    ```
    ./build_gw_image.py --parent-image=apigw-base:1.0 --out-image my-gw-group2:1.0 --license LicenseWithoutHostname.lic --domain-cert ClassicDomainCert.pem --domain-key ClassicDomainKey.pem --domain-key-pass-file /tmp/domainpass.txt --fed Group2.fed
    --fed-pass-file /tmp/fedpass.txt --merge-dir /tmp/mergedir/group2/apigateway --group-id=Group2
    ```

### Step 7 – Start Docker containers from the Admin Node Manager and API Gateway images

To start the Admin Node Manager and API Gateway Docker containers, follow these steps:

{{< alert title="Note" >}}API Gateway Analytics container **requires** you to enable the `ACCEPT_GENERAL_CONDITIONS` environment variable to acknowledge that you have read and accepted [Axway License, Support, and Service Agreement](https://cdn.axway.com/u/Axway_General_Conditions_version_april_2014_eng%20(France).pdf). {{< /alert >}}

1. Start the Admin Node Manager container as detailed in [Start the Admin Node Manager Docker container](/docs/apim_installation/apigw_containers/docker_script_anmimage/#start-the-admin-node-manager-docker-container). For example:

    ```
    docker run -it -p 8090:8090 --name=ANM1 -e ACCEPT_GENERAL_CONDITIONS=yes --network=my_network my-domain-anm:1.0
    ```

    Alternatively, if metrics are enabled:

    ```
    docker run -it -p 8090:8090 --name=ANM1 -e ACCEPT_GENERAL_CONDITIONS=yes --network=my_network -v /tmp/gw-events:/opt/Axway/apigateway/events my-domain-anm:1.0
    ```

    You can now log in to API Gateway Manager at `https://docker_host:8090` with the same administrator credentials as you used for your classic deployment.

2. Start the Group1 API Gateway container as detailed in [Start the API Gateway Docker container](/docs/apim_installation/apigw_containers/docker_script_gwimage/#start-the-api-gateway-docker-container). For example:

    ```
    docker run -it -p 8075:8075 -e EMT_ANM_HOSTS= ANM1:8090 -e ACCEPT_GENERAL_CONDITIONS=yes --network=my_network my-gw-group1:1.0
    ```

3. Start the Group2 API Gateway container. For example:

    ```
    docker run -it -e EMT_ANM_HOSTS=ANM1:8090 -e ACCEPT_GENERAL_CONDITIONS=yes--network=my_network my-gw-group2:1.0
    ```

    Alternatively, if metrics are enabled start GW1 and GW2 as follows:

    ```
    docker run -it -p 8075:8075 -p 8065:8065 -e EMT_ANM_HOSTS=ANM1:8090 -e ACCEPT_GENERAL_CONDITIONS=yes --network=my_network -v /tmp/gw-events:/opt/Axway/apigateway/events my-gw-group1:1.0
    docker run -it -e EMT_ANM_HOSTS=ANM1:8090 -e ACCEPT_GENERAL_CONDITIONS=yes --network=my_network -v /tmp/gw-events:/opt/Axway/apigateway/events my-gw-group2:1.0
    ```

    You can now log in to API Manager UI of Group1 at `https://docker_host:8075` with the same API administrator credentials as you used for your classic deployment.
