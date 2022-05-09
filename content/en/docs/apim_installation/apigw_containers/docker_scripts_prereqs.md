---
title: Set up Docker environment
linkTitle: Set up Docker environment
weight: 30
date: 2019-09-18T00:00:00.000Z
description: Prerequisites and steps you must follow to set up your Docker environment.
---
## Before you start

Your system must meet the following prerequisites before you can run the scripts to build and deploy API Gateway in Docker containers.

### Set up your Docker environment

You must have the following installed on your local system:

* Docker CE version 18.06 or later on CentOS 7
* Python version 3.x or later
* OpenSSL version 1.1 or later

{{< alert title="Note" color="primary" >}}Axway supports Red Hat Enterprise Linux 7 and CentOS Linux version 7 as the base image for Docker containers. Axway supports deployment on any host operating system, cloud provider, or container orchestration system supported by your Docker version. {{< /alert >}}

For more details on Docker system requirements, see [Docker](https://docs.docker.com/engine/installation/) documentation.

### Set up API Gateway Docker scripts

You must download the following from [Axway Support](https://support.axway.com).

* API Gateway Linux installer
* Latest Docker sample scripts package for your API Gateway version. Choose the appropriate package from the [list of Docker scripts on Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/Docker/ipp/50/product/324/subtype/47).

#### Acceptance of General Conditions for license and subscription services

To run API Gateway, API Manager, Admin Node Manager, or API Gateway Analytics containers you must accept Axway General Terms and Conditions:

_“You hereby accept that the Axway Products and/or Services shall be governed exclusively by the [Axway General Terms and Conditions](https://www.axway.com/en/legal/contract-documents), unless an agreement has been signed with Axway in which case such agreement shall apply.”_

To accept them, set the environment variable `ACCEPT_GENERAL_CONDITIONS` to `yes` when running each container.

#### API Gateway licenses

You must have specific API Gateway licenses to run the following:

* API Gateway container
* Admin Node Manager or API Gateway container in Federal Information Processing Standard (FIPS) mode
* API Manager-enabled API Gateway container
* API Gateway Analytics container

#### Install the Docker scripts

Extract files from the Docker scripts package that you downloaded from [Axway Support](https://support.axway.com/)

```
tar -xvf APIGateway_7.7.YYYYMMDD-<bn>_DockerScripts.tar.gz
```

The extracted package includes the following:

* Python scripts (`*.py`)
* Docker files
* Quickstart demo

#### Quickstart demo

The Quickstart demo `quickstart.sh` script enables you to quickly deploy a demo of API Gateway in Docker containers. A `readme.md` is also provided, that describes the Quickstart demo and includes a topology diagram.

This script builds a base API Gateway Docker image using an API Gateway Linux installer and a Docker image based on a standard CentOS7 operating system image.

{{< alert title="Caution" color="warning" >}}Docker tries to download the latest CentOS image from a remote registry, which may potentially contain security vulnerabilities. Axway is not responsible for any third-party base O/S images. You must ensure that all base O/S images are up-to-date and apply any security patches if necessary. See [Create a base image based on custom CentOS7/RHEL7](/docs/apim_installation/apigw_containers/docker_script_baseimage/#create-a-base-image-based-on-custom-centos7-rhel7) for details.{{< /alert >}}

For the Quickstart help, run:

```
./quickstart.sh -h
```

The `quickstart.sh` script is intended to simplify deployment of API Gateway in containers, especially for development environments and evaluation use. However, you can also use it as a starting point for customization and modify it to suit your own environment.

## Create a Docker network

You must run the `docker network` command to create a Docker network for the API Gateway domain. This enables all of the containers in the domain to communicate with each another easily (for example, the API Gateway container and Admin Node Manager container). A containerized API Gateway domain must include one Admin Node Manager container and one or more API Gateway containers.

Run the `docker network` command:

```
docker network create api-gateway-domain
```

This example creates a Docker network called `api-gateway-domain`. For more details on the `docker network` command, see the [Docker user documentation](https://docs.docker.com).

### Example API Gateway domain

The example Quick Start API Gateway domain topology provided with the API Gateway Docker scripts includes the following Docker containers:

![Example containerized API Gateway domain](/Images/ContainerGuide/container_gw_domain.png)

This simple API Gateway domain topology is described as follows:

* Container 1 runs an Apache Cassandra database that stores API Manager data.
* Container 2 runs a MySQL database that stores API Gateway metrics data.
* Container 3 runs an API Gateway that writes transaction event logs, and includes API Manager is an optional component. For more details, see [Deploy API Manager or OAuth in Docker containers](/docs/apim_installation/apigw_containers/container_apimgr_oauth).
* Container 4 runs an Admin Node Manager that generates metrics from transaction event logs, which are then read from the metrics database by API Manager and API Gateway Analytics.
* Container 5 runs API Gateway Analytics, which is an optional standalone client of the metrics database. For more details, see [Deploy API Gateway Analytics in Docker containers](/docs/apim_installation/apigw_containers/container_apigateway_analytics).

{{< alert title="Note" color="primary" >}}The example Quick Start domain topology is suitable for a development environment only. For more details, see the readme file provided with the API Gateway Docker scripts.{{< /alert >}}

## Start external data stores

If you are using any external data stores, such as Apache Cassandra for API Manager, or a metrics database for API Manager or API Gateway Analytics, you must start these data stores.

### Start Apache Cassandra

Deploying a Cassandra container is only recommended for development environments. In a production environment, you must configure Cassandra for high availability (HA) as detailed in
[Configure a Cassandra HA cluster](/docs/cass_admin/cassandra_config/).

For details on starting Apache Cassandra in a Docker container, see [Docker](https://hub.docker.com/_/cassandra) documentation.

### Start the metrics database

If you are using a metrics database, you can use the `docker run` command to start a database container.

```
cd emt_containers-<version>
cp quickstart/mysql-analytics.sql /tmp/sql
docker run -d --name metricsdb --network=api-gateway-domain -v /tmp/sql:/docker-entrypoint-initdb.d -e MYSQL_ROOT_PASSWORD=root01 -e MYSQL_DATABASE=metrics mysql:5.7
```

The above `docker run` command performs the following:

* Downloads a MySQL 5.7 Docker image from the public Docker registry.
* Mounts the host directory `/tmp/sql` (containing a MySQL metrics database creation script) inside the container.
* Uses environment variables `MYSQL_ROOT_PASSWORD` and `MYSQL_DATABASE` to specify the database root password and the database name.
* Starts a MySQL container named `metricsdb`.

## Generate domain SSL certificates

To secure the communications between the Admin Node Manager and API Gateways, you can use the `gen_domain_cert.py` script to generate a self-signed CA certificate for a domain, or a Certificate Signing Request (CSR) to be signed by an external Certificate Authority (CA).

{{< alert title="Note" color="primary" >}}If you already have a domain certificate (for example, from another API Gateway installation) you can skip this section. You can specify your existing domain certificate (certificate and private key in .pem format) to the `build_anm_image.py` and `build_gw_image.py` scripts. You cannot use one domain certificate for both a container deployment and a classic deployment if they are running in parallel.{{< /alert >}}

### Generate domain certificates script options

You must specify the following as options when using the `gen_domain_cert.py` script:

* Domain identifier
* Passphrase for the domain private key

This script also supports additional options when generating certificates. For example:

* Specify a signing algorithm (SHA256, SHA384, or SHA512)
* Generate a CSR
* Specify custom values for the fields in the domain certificate (for example, organization)

For the latest script usage and options, run the script with no options, or with the `-h` option.

```
cd emt_containers-<version>
./gen_domain_cert.py -h
```

### Generate a default certificate and key

The following example creates a certificate and private key using default values.

{{< alert title="Caution" color="warning" >}} Do not use default options on production systems. The `--default-cert` option is provided only as a convenience for development environments.{{< /alert >}}

```
cd emt_containers-<version>
./gen_domain_cert.py --default-cert
```

This example creates a default certificate and private key:

* The certificate uses a domain identifier of `DefaultDomain`
* The certificate and key are stored in the `certs/DefaultDomain` directory
* The certificate uses a default passphrase

### Generate a self-signed certificate and key

The following example creates a self-signed certificate and private key.

```
cd emt_containers-<version>
./gen_domain_cert.py --domain-id=mydomain --pass-file=/tmp/pass.txt
```

This example creates a self-signed certificate and private key:

* The certificate uses a domain identifier of `mydomain`.
* The certificate and key are stored in the `certs/mydomain` directory
* The certificate uses a specified passphrase

### Generate a CSR

The following example creates a certificate signing request (CSR).

* You must send the generated CSR to a CA for signing.
* When running the scripts to build Admin Node Manager or API Gateway images, specify the certificate and private key returned from the CA, and not the CSR.

```
cd emt_containers-<version>
./gen_domain_cert.py --domain-id=mydomain --pass-file=/tmp/pass.txt --out=csr --O=MyOrg
```

This example creates a CSR that:

* Uses a domain identifier of `mydomain`
* Is stored in the `certs/mydomain` directory
* Uses a specified passphrase and organization
