---
title: Create base Docker image
linkTitle: Create base Docker image
weight: 40
date: 2019-09-18
description: Create a base Docker image containing an operating system and an API Gateway installation.
---

## Create a base Docker image

To create a base Docker image containing an operating system and an API Gateway installation, use the `build_base_image.py` script. This script builds a base API Gateway Docker image using an API Gateway 7.7 Linux installer and a Docker image based on a standard or custom CentOS7 or RHEL7 operating system image.

{{< alert title="Caution" color="warning" >}}Docker automatically downloads the latest CentOS or RHEL7 image, which may potentially contain security vulnerabilities. Axway is not responsible for any third-party base O/S images. You must ensure that all base O/S images are up-to-date and apply any security patches if necessary.{{< /alert >}}

### Base image script options

You must specify the following as options when using the `build_base_image.py` script:

* API Gateway 7.7 Linux installer
* Operating system based on one of the following:
    * Standard CentOS7 Docker image downloaded from the [public Docker registry](https://store.docker.com/)
    * Standard RHEL7 Docker image downloaded from the [Red Hat Docker registry](https://access.redhat.com/containers)
    * If you specify a custom CentOS7 or RHEL7-based OS Docker image, Docker first tries to find the custom image in the local registry, and then tries to download it from a remote registry

{{< alert title="Note" color="primary" >}}You must have an RHEL7 license to build a base API Gateway image based on an RHEL7 OS.{{< /alert >}}

This script also supports additional options when generating a base image. For the latest script usage and options, run the script with no options, or with the `-h` option.  

```
cd emt_containers-<version>
./build_base_image.py -h
```

See [Best practices for running API management in Docker containers](/docs/apim_howto_guides/apigw_in_containers/) for additional information.  

The following examples show how you can use the script to build base Docker images:

### Create a base image based on standard CentOS7

The following example creates a base Docker image using a standard CentOS7 image.

```
cd emt_containers-<version>
./build_base_image.py --installer=apigw-installer.run --os=centos7
```

This example creates a base Docker image named `apigw-base` with a tag of `latest`. The image is based on a standard CentOS7 Docker image.

### Create a base image based on standard RHEL7

The following example creates a base Docker image using a standard RHEL7 image.

```
cd emt_containers-<version>
./build_base_image.py --installer=apigw-installer.run --os=rhel7 --out-image=my-gw-base:1.0
```

This example creates a base Docker image named `my-gw-base` with a tag of `1.0`. The image is based on a standard RHEL7 Docker image.

### Create a base image based on custom CentOS7/RHEL7

The following example creates a base Docker image using a custom RHEL7 image.

```
cd emt_containers-<version>
./build_base_image.py --installer=apigw-installer.run --parent-image=my-custom-rhel:2.0 --out-image=my-custom-gw-base:1.0
```

This example creates a base Docker image named `my-custom-gw-base` with a tag of `1.0`. The image is based on the specified `my-custom-rhel:2.0` Docker image.
