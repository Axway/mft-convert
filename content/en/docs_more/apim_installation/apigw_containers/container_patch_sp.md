---
title: Apply a patch, update, or service pack
linkTitle: Apply a patch or update
weight: 108
date: 2019-09-18T00:00:00.000Z
description: >-
  Apply a patch, update, or service pack (SP) to an API Gateway or API Manager
  container deployment.
---

In a container deployment, a patch or update is rolled out using an orchestration tool (for example, Kubernetes or OpenShift) after new Docker images containing the patch or update are pushed to the Docker registry. This enables you to perform a rolling zero downtime update of services.

## Apply a patch

To apply a patch, follow these steps:

{{< alert title="Note" color="primary" >}}Before you start, check the release notes of this patch for any specific instructions.{{< /alert >}}

1. Download the patch from Axway Support at [Axway Support](https://support.axway.com/).
2. Create a merge directory to contain the patch files and any custom configuration (for example, `/tmp/apigateway`). The merge directory must be called `apigateway` and must have the same directory structure as the `apigateway` directory of an API Gateway installation.
3. Unzip and extract the patch into the merge directory.
4. Add any custom configuration to the merge directory. For example, to add a custom `envSettings.props` file to your image, copy `envSettings.props` to `/tmp/apigateway/conf/`.
5. Create new Admin Node Manager and API Gateway images using the `--merge-dir` option to specify the merge directory containing the patch files and custom configuration.

    ```
    cd emt_containers-<version>
    ./build_anm_image.py --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --anm-username=gwadmin --anm-pass-file=/tmp/gwadminpass.txt --merge-dir=/tmp/apigateway
    ./build_gw_image.py --license=/tmp/api_gw.lic --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --merge-dir=/tmp/apigateway
    ```

## Apply an update

Use these instructions to apply an update (7.7.20200130 or later) or a service pack (7.7 SP1 or 7.7 SP2) to API Gateway version 7.7.

To apply an update or service pack, follow these steps:

{{< alert title="Note" color="primary" >}}Before you start, check the release notes of the update or service pack for any specific instructions.{{< /alert >}}

1. Download the latest API Gateway 7.7 Linux installer (which includes the update or service pack) from [Axway Support](https://support.axway.com/).
2. Create a new base image using the `--installer` option to build the image from the downloaded API Gateway installer.

    ```
    cd emt_containers-<version>
    ./build_base_image.py --installer=apigw-new-installer.run --os=centos7
    ```

3. Create a merge directory to contain any custom configuration (for example, `/tmp/apigateway`). The merge directory must be called `apigateway` and must have the same directory structure as the `apigateway` directory of an API Gateway installation.
4. Add any custom configuration to the merge directory. For example, to add a custom `envSettings.props` file to your image, copy `envSettings.props` to `/tmp/apigateway/conf/`.
5. Create new Admin Node Manager and API Gateway images using the `--merge-dir` option to specify the merge directory containing the custom configuration.

    ```
    cd emt_containers-<version>
    ./build_anm_image.py --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --anm-username=gwadmin --anm-pass-file=/tmp/gwadminpass.txt --merge-dir=/tmp/apigateway
    ./build_gw_image.py --license=/tmp/api_gw.lic --domain-cert=certs/mydomain/mydomain-cert.pem --domain-key=certs/mydomain/ mydomain-key.pem --domain-key-pass-file=/tmp/pass.txt --merge-dir=/tmp/apigateway
    ```
