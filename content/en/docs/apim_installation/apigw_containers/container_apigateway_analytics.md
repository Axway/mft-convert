---
title:  Deploy API Gateway Analytics in Docker containers 
linkTitle:  Deploy API Gateway Analytics
weight: 85
date: 2019-09-18
description: Create an API Gateway Analytics Docker image and start an API Gateway Analytics Docker container.
---

These steps are optional and only for users who wish to use API Gateway Analytics monitoring and reporting in a containerized environment. In a containerized deployment, API Gateway Analytics runs as a standalone client of the metrics database.

## Create an API Gateway Analytics Docker image

To create an API Gateway Analytics Docker image, use the `build_aga_image.py`
script. This script builds an API Gateway Analytics Docker image using an API Gateway 7.7 Linux installer and a Docker image based on a standard or custom CentOS7 or RHEL7 operating system image.

{{< alert title="Caution" color="warning" >}}Docker automatically downloads the latest CentOS or RHEL7 image, which may potentially contain security vulnerabilities. Axway is not responsible for any third-party base OS images. You must ensure that all base OS images are up-to-date and apply any security patches if necessary.{{< /alert >}}

### API Gateway Analytics image script options

You must specify the following as options when using the `build_aga_image.py` script:

* API Gateway Analytics license
* API Gateway 7.7 Linux installer
* Operating system based on one of the following:
    * Standard CentOS7 Docker image downloaded from the [public Docker registry](https://store.docker.com/)
    * Standard RHEL7 Docker image downloaded from the [Red Hat Docker registry](https://access.redhat.com/containers)
    * If you specify a custom CentOS7 or RHEL7-based OS Docker image, Docker first tries to find the custom image in the local registry, and then tries to download it from a remote registry

You must have an RHEL7 license to build a base API Gateway image based on an RHEL7 OS.

This script also supports additional options when generating an API Gateway Analytics image. For example, you can:

* Build an image from existing API Gateway Analytics configuration by specifying an existing `.fed` file.
* Specify a merge directory to add to the API Gateway Analytics Docker image. This directory can include custom configuration, JAR files, and so on.

For the latest script usage and options, run the script with no options, or with the `-h` option.

```
cd emt_containers-<version>
./build_aga_image.py -h
```

The following examples show how you can use this script to build API Gateway Analytics Docker images.

### Create an API Gateway Analytics image for a development environment

The following example creates a simple API Gateway Analytics Docker image using a default administrator user and default connection for the metrics database. This approach is recommended in a development environment only.

Do not use default options on production systems. The `--default-user` option is provided only as a convenience for development environments.

Use the `--merge-dir` option to specify the `analytics` directory containing the JDBC driver JAR file for the specified metrics database in the `ext/lib` directory.

* The merge directory must be called `analytics` and must have the same directory structure as the `analytics` directory of an API Gateway Analytics installation.
* Copy the JAR file to a new directory `/tmp/analytics/ext/lib/` and specify `/tmp/analytics` to the `--merge-dir` option.

Use the metrics options to specify the URL, user name, and password for your metrics database. If not specified, the metrics options have the following default values:

* `--metrics-db-url`: Defaults to `${environment.METRICS_DB_URL}`
* `--metrics-db-username`: Defaults to `${environment.METRICS_DB_USERNAME}`
* `--metrics-db-pass-file`: Default value for password if password file not specified is `${environment.METRICS_DB_PASS}`

For example:

```
cd emt_containers-<version>
./build_aga_image.py --installer=apigw-installer.run --os=centos7 --license=/tmp/analytics_license.lic --default-user --merge-dir=/tmp/analytics --out-image=apigw-analytics:1.0
```

This example creates an API Gateway Analytics Docker image named `apigw-analytics` with a tag of `1.0`. This image has the following characteristics:

* Based on a standard CentOS7 Docker image
* Uses a default user name of `admin` and a default password for the API Gateway Analytics administrator user
* Uses default values for the metrics database
* Uses a specified merge directory (containing the JDBC driver JAR file for the metrics database) that is merged into the API Gateway Analytics image

### Create an API Gateway Analytics image for a production environment

The following example creates an API Gateway Analytics Docker image using a specified API Gateway Analytics administrator user and specified connection details for the metrics database. This is the recommended approach in a production environment.

Use the `--merge-dir` option to specify the `analytics` directory containing the JDBC driver JAR file for the specified metrics database in the `ext/lib` directory.

* The merge directory must be called `analytics` and must have the same directory structure as the `analytics` directory of an API Gateway Analytics installation.
* Copy the JAR file to a new directory `/tmp/analytics/ext/lib/` and specify `/tmp/analytics` to the `--merge-dir` option.

Use the metrics options to specify the URL, user name, and password for your metrics database. If not specified, the metrics options have the following default values:

* `--metrics-db-url`: Defaults to `${environment.METRICS_DB_URL}`
* `--metrics-db-username`: Defaults to `${environment.METRICS_DB_USERNAME}`
* `--metrics-db-pass-file`: Default value for password if password file not specified is `${environment.METRICS_DB_PASS}`

For example:

```
cd emt_containers-<version>
./build_aga_image.py --installer=apigw-installer.run --os=centos7 --license=/tmp/analytics_license.lic --analytics-username=user1 --analytics-pass-file=/tmp/pass.txt --metrics-db-url=jdbc:mysql://metricsdb:3306/metrics --metrics-db-username=db_user1 --metrics-db-pass-file=/tmp/dbpass.txt --merge-dir=/tmp/analytics --out-image=apigw-analytics:1.0
```

This example creates an API Gateway Analytics Docker image named `apigw-analytics` with a tag of `1.0`. This image has the following characteristics:

* Based on a standard CentOS7 Docker image
* Uses a user name of `user1` and a specified password for the API Gateway Analytics administrator user
* Uses a MySQL metrics database with specified connection details
* Uses a specified merge directory (containing the JDBC driver JAR file for the metrics database) that is merged into the API Gateway Analytics image

## Start the API Gateway Analytics Docker container

Use the `docker run` command to start the API Gateway Analytics container.

{{< alert title="Note" >}}API Gateway Analytics container **requires** you to enable the `ACCEPT_GENERAL_CONDITIONS` environment variable to acknowledge that you have read and accepted [Axway License, Support, and Service Agreement](https://cdn.axway.com/u/Axway_General_Conditions_version_april_2014_eng%20(France).pdf). {{< /alert >}}

```
docker run -it --name=analytics -p 8040:8040 --network=api-gateway-domain -v /tmp/reports:/tmp/reports -e ACCEPT_GENERAL_CONDITIONS=yes -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd apigw-analytics:1.0
```

This example performs the following:

* Starts an API Gateway Analytics container named `analytics` from an image named `apigw-analytics:1.0`. You must specify the name of the API Gateway Analytics Docker image that you created in [Create an API Gateway Analytics Docker image](#create-an-api-gateway-analytics-docker-image).
* Binds the port 8040 of the container to port `8040` on the host machine. This enables you to access the API Gateway Analytics web UI on port `8040` of your host machine.
* Mounts the host directory `/tmp/reports` inside the container to store API Gateway Analytics reports.
* Uses environment variables to specify connection details for the metrics database. The metrics database must be running as detailed in [Start external data stores](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/#start-external-data-stores).
* Sets the `ACCEPT_GENERAL_CONDITIONS` environment variable to `yes` to acknowledge that you have accepted [Axway License, Support, and Service Agreement](https://cdn.axway.com/u/Axway_General_Conditions_version_april_2014_eng%20(France).pdf).

To run the container in the background, use the `-d` option, for example:

```
docker run -d --name=analytics -p 8040:8040 --network=api-gateway-domain -v /tmp/reports:/tmp/reports -e ACCEPT_GENERAL_CONDITIONS=yes -e  METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd apigw-analytics:1.0
```
