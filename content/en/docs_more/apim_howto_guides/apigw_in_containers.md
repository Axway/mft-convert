{
    "title": "Best practices for running API management in Docker containers",
    "linkTitle": "Running in Docker containers",
    "weight": "20",
    "date": "2020-06-26",
    "description": "A collection of tips and tricks for deploying and operating AMPLIFY API Management in Docker containers."
}

## Reduce Docker image size

The API Gateway Docker image created with the standard Docker sample scripts has a considerable size. Follow these steps to reduce the size of the image by around 200 MB.  

The scripts are provided as an example and you can adjust them as needed for your requirements.

### Reduce the size of base images

To remove files and folders which are not required in the base images:  

1. Download the Docker samples (`APIGateway_7.7.YYYYMMDD-<bn>_DockerScripts.tar.gz`) and extract the package.  
2. In the extracted package open the file `Dockerfiles/gateway-base/scripts/runInstall.sh`.
3. Add a line starting with `rm -rf` to remove any files you do not require from the image. For example:

    ```bash
    # removes some sample McAfee DAT files  
    /opt/Axway/apigateway/conf/plugin
    # removes the IBM-MQ driver, which might not be required in your environment  
    /opt/Axway/apigateway/system/lib/ibmmq
    # Database setup scripts not required as part of the Docker container  
    /opt/Axway/apigateway/system/conf/sql
    # Not required MIB description  
    /opt/Axway/apigateway/system/conf/snmp
    # Policy configuration templates not required as part of the Docker container  
    /opt/Axway/apigateway/system/conf/templates/*
    # Sample SpiderLabs Mod-Security pattern  
    /opt/Axway/apigateway/system/conf/threat-protection/SpiderLabs-owasp-modsecurity-crs-2.2.9-7-g3e6782b.zip
    # Gateway Metrics plugin for Oracle Enterprise Manager also not required  
    /opt/Axway/apigateway/system/conf/oracle-em
    # Configuration migration artifacts  
    /opt/Axway/apigateway/system/conf/migrate
    # Provided samples not required as part of the Docker container  
    /opt/Axway/apigateway/samples
    # If you don't need the SDK-Generator you can remove it as well  
    /opt/Axway/apigateway/tools/sdk-generator
    ```

    Download [`runInstall.sh`](/samples/apimanagement/howto/docker/runInstall.sh) for a complete example.

    It is best to version control any changes you make in the file.

After these changes, the size of the generated Docker base images is reduced from 947MB to 747MB.

### Optimize images for Node Manager and API Gateways

In addition, you can create a layered [multi-stage](https://docs.docker.com/develop/develop-images/multistage-build/) image for Admin Node Manager and the API Gateways and in these images you can remove additional folders that are not required.

1. Open the file `Dockerfiles/emt-nodemanager/Dockerfile`.
2. Change the line `FROM $PARENT_IMAGE` to `FROM $PARENT_IMAGE as base`.
3. Add the following line at the end before the `CMD` instruction:

    ```
    FROM centos:7
    COPY --from=base /opt/Axway/apigateway /opt/Axway/apigateway
    ```

4. Remove the folders that you do not require. Download the following complete example files to learn which folders you can remove in each image:
    * [`anm-dockerfile`](/samples/apimanagement/howto/docker/anm-dockerfile)
    * [`gw-dockerfile`](/samples/apimanagement/howto/docker/gw-dockerfile)

This reduces the size of the API Gateway image to 715 MB and the Admin Node Manager image to 708 MB.