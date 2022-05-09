---
title: Create and start Admin Node Manager Docker container
linkTitle: Create and start Admin Node Manager Docker container
weight: 50
date: 2019-09-18
description: Steps to build an Admin Node Manager Docker image and start an Admin Node Manager container.
---

## Create an Admin Node Manager Docker image

Use the `build_anm_image.py` script to create an Admin Node Manager Docker image.

To create an Admin Node Manager Docker image, use the `build_anm_image.py` script. This script builds an Admin Node Manager Docker image using the base image you created in [Create base Docker image](/docs/apim_installation/apigw_containers/docker_script_baseimage).

### Admin Node Manager image script options

You must specify the following as options when using the `build_anm_image.py` script:

* Domain certificate, private key, and password.
* User name and password for the administrator user. You can use this user name and password to log in to the API Gateway Manager web console.

This script also supports additional options when generating an Admin Node Manager image. For example:

* Enable API metrics processing in the Admin Node Manager Docker image. This enables you to monitor APIs and applications in API Manager.
* Reuse the same configuration in multiple domains by specifying a `.fed` file containing Admin Node Manager configuration to include in the Admin Node Manager Docker image.
* Specify a merge directory to add to the Admin Node Manager Docker image. This directory can include custom configuration, JAR files, and so on.
* Enable FIPS mode for the Admin Node Manager Docker image.

For the latest script usage and options, run the script with no options, or with the `-h` option.

```
cd emt_containers-<version>
./build_anm_image.py -h
```

See [Best practices for running API management in Docker containers](/docs/apim_howto_guides/apigw_in_containers/) for additional information.  

### Create a metrics-enabled Admin Node Manager image

The following example creates an Admin Node Manager Docker image with a specified domain certificate that runs with metrics processing enabled. The Admin Node Manager container processes event logs from API Gateway containers and writes them to a specified metrics database. This is the recommended option and is suitable for a production environment.

Use the `--merge-dir` option to specify the `apigateway` directory containing the JDBC driver JAR file for the metrics database in the `ext/lib` directory:

* The merge directory must be called `apigateway` and must have the same directory structure as in an API Gateway installation.
* Copy the JAR file to a new directory `/tmp/apigateway/ext/lib/` and specify `/tmp/apigateway` to the `--merge-dir` option.

When running the Admin Node Manager and API Gateway Docker containers, use the `docker run -v` option to mount a volume for the API Gateway events directory:

* Run the API Gateway container with a volume mounted for the events directory (for example, `-v /tmp/events:/opt/Axway/apigateway/events` writes API Gateway event logs to `/tmp/events` on the host machine).
* Run the Admin Node Manager container with the same volume mounted (for example, `-v /tmp/events:/opt/Axway/apigateway/events` enables the Admin Node Manager to read API Gateway event logs from `/tmp/events` on the host machine). For details, see [Start a metrics-enabled Admin Node Manager container](#start-a-metrics-enabled-admin-node-manager-container).

Use the metrics options to specify the URL, user name, and password for your metrics database. If not specified, the metrics options have the following default values:

* `--metrics-db-url`: Defaults to `${environment.METRICS_DB_URL}`
* `--metrics-db-username`: Defaults to `${environment.METRICS_DB_USERNAME}`
* `--metrics-db-pass-file`: Default value for password if password file not specified is `${environment.METRICS_DB_PASS}`

{{< alert title="Note" color="primary" >}}When running in a multi-node system, you must mount a shared network volume that is accessible from the Admin Node Manager and from all API Gateways.{{< /alert >}}

```
cd emt_containers-<version>
./build_anm_image.py --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --anm-username=gwadmin --anm-pass-file=/tmp/gwadminpass.txt --parent-image=my-gw-base:1.0 --out-image=my-metrics-admin-node-manager:1.0 --metrics --merge-dir=/tmp/apigateway
```

This example creates an Admin Node Manager Docker image named `my-metrics-admin-node-manager` with a tag of `1.0`. This image has the following characteristics:

* Based on the `my-gw-base:1.0` image
* Uses a specified domain certificate, key, and password
* Uses a specified user name of `gwadmin` and a specified password for the administrator user
* Runs with metrics processing enabled
* Uses a specified merge directory (containing the JDBC driver JAR file for the metrics database) that is merged into the API Gateway image

### Other options for creating Admin Node Manager Docker images  

The following are additional examples of using the `build_anm_image.py` script to build Admin Node Manager Docker images:

#### Create an Admin Node Manager image using existing fed and customized configuration

The following example creates an Admin Node Manager Docker image using an existing Admin Node Manager deployment package (`.fed` file) and customized configuration from an existing API Gateway installation.

Ensure that your `.fed` contains the following:

* Admin Node Manager configuration. You can open the `.fed` in Policy Studio and verify that it is identified as a Node Manager configuration in the navigation pane.
* Only IP addresses that are accessible at runtime. For example, the `.fed` cannot contain IP addresses of container-based Admin Node Managers and API Gateways, because IP addresses are usually dynamically assigned in a Docker network.

Use the `--merge-dir` option to add more files and folders to the `apigateway` directory inside the image:

* The merge directory must be called `apigateway` and must have the same directory structure as in an API Gateway installation.
* For example, to add an optional custom `envSettings.props` file to your image, copy `envSettings.props` to a new directory named `/tmp/apigateway/conf/`, and specify `/tmp/apigateway` to the `--merge-dir` option.
* To add custom JAR files to your image, copy the JAR files to a new directory named `/tmp/apigateway/ext/lib/`, and specify `/tmp/apigateway` to the `--merge-dir` option.

{{< alert title="Note" color="primary" >}}`envSettings.props` specifies settings such as the port the Admin Node Manager listens on (default of `8090`), and the session timeout for API Gateway Manager (default of 12 hours). `envSettings.props` must contain only IP addresses and host names that are accessible at runtime. It cannot contain IP addresses of container-based Admin Node Managers and API Gateways because these are usually dynamically assigned in a Docker network.{{< /alert >}}

```
cd emt_containers-<version>
./build_anm_image.py --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --anm-username=gwadmin --anm-pass-file=/tmp/gwadminpass.txt --parent-image=my-gw-base:1.0 --out-image=my-fed-admin-node-manager:1.0 --fed=my-anm-fed.fed --fed-pass-file=/tmp/anmfedpass.txt --merge-dir=/tmp/apigateway
```

This example creates an Admin Node Manager Docker image named `my-fed-admin-node-manager` with a tag of `1.0`. This image has the following characteristics:

* Based on the `my-gw-base:1.0` image
* Uses a specified certificate and key
* Uses a specified user name of `gwadmin` and a specified password for the administrator user
* Uses a specified `.fed` that contains Admin Node Manager configuration
* Uses a specified merge directory that is merged into the Admin Node Manager image

#### Create a FIPS-enabled Admin Node Manager image

The following example creates an Admin Node Manager Docker image that runs in FIPS-compliant mode.

You must have a valid FIPS-compliant mode API Gateway license file to create an image that can run in FIPS-compliant mode.

```
cd emt_containers-<version>
./build_anm_image.py --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --anm-username=gwadmin --anm-pass-file=/tmp/gwadminpass.txt --parent-image=my-gw-base:1.0 --out-image=my-fips-admin-node-manager:1.0 --fips --license=/tmp/api_gw_fips.lic
```

This example creates an Admin Node Manager Docker image named `my-fips-admin-node-manager` with a tag of `1.0`. This image has the following characteristics:

* Based on the `my-gw-base:1.0` image
* Uses a specified domain certificate, key, and password
* Uses a specified user name of `gwadmin` and a specified password for the administrator user
* Runs in FIPS-compliant mode

#### Create an Admin Node Manager image for a development environment

The following example creates a simple Admin Node Manager Docker image suitable for a development environment only using default certificates and a default administrator user.

Do not use default options on production systems. The `--default-cert` and `--default-user` options are provided only as a convenience for development environments.

```
cd emt_containers-<version>
./build_anm_image.py --default-cert --default-user
```

This example creates an Admin Node Manager Docker image named `admin-node-manager` with a tag of `latest`. This image has the following characteristics:

* Based on the `apigw-base:latest` image
* Uses a default domain certificate and key (generated from running `./gen_domain_cert.py --default-cert`)
* Uses a default user name of `admin` and a default password for the administrator user

## Start the Admin Node Manager Docker container

Use the `docker run` command to start the Admin Node Manager container. The following example shows how to run a metrics-enabled Admin Node Manager container in the background on a specific port:

{{< alert title="Note" >}}API Gateway Analytics container **requires** you to enable the `ACCEPT_GENERAL_CONDITIONS` environment variable to acknowledge that you have read and accepted [Axway License, Support, and Service Agreement](https://cdn.axway.com/u/Axway_General_Conditions_version_april_2014_eng%20(France).pdf). {{< /alert >}}

```
docker run -d -p 8090:8090 --name=anm --network=api-gateway-domain -v /tmp/events:/opt/Axway/apigateway/events -e ACCEPT_GENERAL_CONDITIONS=yes -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd -e EMT_TRACE_LEVEL=DEBUG admin-node-manager:1.0
```

This example performs the following:

* Starts an Admin Node Manager container named `anm` from an image named `admin-node-manager:1.0`. You must specify the name of the image that you created in [Create an Admin Node Manager Docker image](#create-an-admin-node-manager-docker-image).
* Binds the default management port `8090` of the container to port `8090` on the host machine. This enables you to access the API Gateway Manager web console on port `8090` of your host machine.
* Runs the container in the background using the `-d` option.
* Mounts the `/tmp/events` host directory in the container, which contains API Gateway transaction event logs. For best practice, you can parametrize this directory by way of the `quickstart.sh` script included in the Docker scripts package.
* Uses `METRICS_DB_URL`, `METRICS_DB_USERNAME` and `METRICS_DB_PASS` environment variables to specify connection details for the metrics database.
* Uses an environment variable `EMT_TRACE_LEVEL` to set a trace level inside the container. In the above example a trace level switches from INFO to DEBUG level during container startup.
* Sets the `ACCEPT_GENERAL_CONDITIONS` environment variable to `yes` to acknowledge that you have accepted [Axway License, Support, and Service Agreement](https://cdn.axway.com/u/Axway_General_Conditions_version_april_2014_eng%20(France).pdf).

### Further information

For more details on the `docker run` command, see the [Docker user documentation](https://docs.docker.com/ "https://docs.docker.com/").

For more information on the environment variables that you can specify at runtime, see [Environment variables reference](/docs/apim_installation/apigw_containers/container_env_variables/#environment-variables-reference).

For more information on usage tracking for on-premise products purchased on a subscription basis, see [Configure usage tracking](/docs/apim_installation/apigw_containers/configure_edge_agent).
