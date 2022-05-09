{
    "title": "Create an API Gateway with Docker volumes",
    "linkTitle": "Create an API Gateway with Docker volumes",
    "weight": "30",
    "date": "2022-02-01",
    "description": "Configure an API Gateway Docker container to use a persisted volume for API Gateway configuration."
}

The primary documentation for [API Gateway in containers](/docs/apim_installation/apigw_containers/docker_script_gwimage) outlines the creation of images and their deployment to containers. This page describes how during the deployment of an image, you can use Docker volumes in a persisted store to update the configuration within the container. The main advantage of using Docker volumes is to allow you to update your configuration without having to rebake the Docker image.

The major steps to create and configure your API Gateway in containers with Docker volumes are:

1. Mount the new API Gateway configuration.
2. Start the API Gateway docker container.
3. Verify the container configuration.

This process applies to API Gateway configurations only, such as policy management, system property changes, environment settings, logging configurations, and son on.

## Mount API Gateway configuration

Docker volumes facilitate mounting API Gateway configuration files to the Docker container so that they are available to the API Gateway at runtime. These configuration files are stored in the host file system, which can be across any hardware or cloud service offering the client is utilizing.

To create your persisted API Gateway store, the expected mounted directory (mount point location) structure is as follows:

| Docker volume mount | Description |
| ------------------- | ----------- |
| /merge/fed | For an API Gateway policy configuration stored as a deployment package (.fed file).|
| /merge/yaml | For YAML based API Gateway policy configuration stored as a deployment package (.tar.gz file).|
| /merge/apigateway | For all other API Gateway policy configurations, for example, jvm.xml, envSettings.props, and so on. |
| /merge/mandatoryFiles | For the verification of mandatory configuration files. |

## Run API Gateway

After you create a persisted store, you must add the Docker volume configuration to the Docker `run` command to start the API Gateway container with the new runtime configuration. For example:

```bash
docker run -d --name=apimgr --network=api-gateway-domain \
           -p 8075:8075 -p 8065:8065 -p 8080:8080 \
           -v /home/user/apigw/fed/newFed.fed:/merge/fed \
           -v /home/user/apigw/mandatoryFiles.yaml:/merge/mandatoryFiles \
           -v /home/user/apigw/config:/merge/apigateway \
           -e ACCEPT_GENERAL_CONDITIONS=yes -e EMT_ANM_HOSTS=anm:8090 -e CASS_HOST=casshost1 \
           -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd \
           api-gateway-my-group:1.0
```

After the container is running, the files mounted within the Docker volumes are copied to the file system within the running container in its `/merge` directory. For example:

```bash
.
├── merge
│   ├── fed
│   ├──apigateway
│   │  └── groups
│   │      └── emt-group
│   │          └── emt-service
│   │              └── conf
│   │                  ├── envSettings.props
│   │                  └── jvm.xml
│   └── mandatoryFiles
└── opt
    └── Axway
        └── apigateway
```

The original factory configuration in the `/opt/Axway/apigateway` directory is then replaced with the hosted configuration in the `/merge` directory, and the new configuration is reflected in the running API Gateway instance.

## Verify the configuration of your images

All Docker images must be configured with default configuration files, there is no change in the image creation process. Only files that require updates in the container need to be mounted. To prevent default configuration in the image being loaded on startup, a mandatory file list must be maintained.

To verify that these API Gateway external configuration files, mounted on Docker volumes, have been successfully found by the Docker container at runtime, you must add the `mandatoryFiles.yaml` configuration file to the `/merge/mandatoryFiles` Docker volume. This file lists the external configuration files which should be mounted to the `/merge` directory in the running container. For example:

```bash
required:
    - /merge/apigateway/groups/emt-group/emt-service/conf/envSettings.props
    - /merge/apigateway/groups/emt-group/emt-service/conf/jvm.xml
    - /merge/apigateway/system/conf/log4j2.yaml2
    - /merge/fed
```

If a file listed is not available to the container's file system, API Gateway will fail to start.

## Sample API Gateway Docker container configurations

This section provides examples of how to configure an API Gateway Docker container for different configuration types.

### API Gateway policy configuration

API Gateway policy configuration stored as a deployment package (.fed file) can be added to the Docker container runtime configuration by adding the `fed` file to the `/merge/fed` Docker volume:

```bash
docker run -d --name=apimgr --network=api-gateway-domain \
           -p 8075:8075 -p 8065:8065 -p 8080:8080 \
           -v /home/user/apigw/fed/newFed.fed:/merge/fed \
           -e ACCEPT_GENERAL_CONDITIONS=yes -e EMT_ANM_HOSTS=anm:8090 -e CASS_HOST=casshost1 \
           -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd \
           api-gateway-my-group:1.0
```

### YAML Entity Store configuration

YAML based API Gateway policy configuration stored as a deployment package (.tar.gz file) can be added to the Docker container runtime configuration by adding the source directory to the `/merge/yaml` Docker volume:

```
docker run -d --name=apimgr --network=api-gateway-domain \
           -p 8075:8075 -p 8065:8065 -p 8080:8080 \
           -v /home/user/apigw/yaml.tar.gz:/merge/yaml \
           -e ACCEPT_GENERAL_CONDITIONS=yes -e EMT_ANM_HOSTS=anm:8090 -e CASS_HOST=casshost1 \
           -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd \
           api-gateway-my-group:1.0
```

### All other API Gateway configuration

All other API Gateway configuration, such as `jvm.xml` and `envSettings.props`, can be added to the Docker container runtime configuration by way of the `/merge/apigateway` Docker volume. You must store these configurations locally, in a directory structure which mirrors `/opt/Axway/apigateway` subfolder structure inside the Docker container. For example:

```bash
config
├── groups
│   └── emt-group
│       └── emt-service
│           └── conf
│               ├── envSettings.props
│               └── jvm.xml
└── system
    └── conf
        └── log4j2.yaml
```

This folder structure is then mounted to the `/merge/apigateway` directory of the container.

After that, you can start API Gateway Docker container as follows:

```
docker run -d --name=apimgr --network=api-gateway-domain \
           -p 8075:8075 -p 8065:8065 -p 8080:8080 \
           -v /home/user/apigw/config:/merge/apigateway \
           -e ACCEPT_GENERAL_CONDITIONS=yes -e EMT_ANM_HOSTS=anm:8090 -e CASS_HOST=casshost1 \
           -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd \
           api-gateway-my-group:1.0
```

## Further Information

For more information on how to build an API Gateway Docker image and start an API Gateway Docker container, see [Create and start API Gateway Docker container](/docs/apim_installation/apigw_containers/docker_script_gwimage).
