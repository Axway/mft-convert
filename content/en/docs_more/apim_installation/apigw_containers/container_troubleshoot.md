---
title: Troubleshoot container deployments
linkTitle: Troubleshooting
weight: 110
date: 2019-09-18
description: Problems you might encounter when running API Gateway and API Manager in Docker containers, and possible solutions. 
---

## Reset the default API administrator password

Problem
: You have forgotten the password for the default API administrator user in API Manager and you cannot log in to the API Manager UI.

Solution
: After the default API administrator account has been created in Apache Cassandra, you cannot change the password in Policy Studio. If you cannot log in to the API Manager UI to change the password, you must use the `setup-apimanager` script with the `--resetPassword` option to reset it. Follow these steps:

1. Use the `docker exec` command to connect to the running Admin Node Manager Docker container.

    ```
    docker exec -it <anm-container-id> bash
    ```

2. Change to the `bin` directory.

    ```
    cd /opt/Axway/apigateway/posix/bin
    ```

3. Run the `setup-apimanager` script with the `--resetPassword` option.

    ```
    ./setup-apimanager --resetPassword
    ```

## Manage KPS with kpsadmin

Problem
: You need to perform administrative or management of a key property store (KPS) in a container deployment.

Solution
: To run the `kpsadmin` tool in a container deployment, you must connect to the Admin Node Manager container and run the tool from there. Follow these steps:

1. Use the `docker exec` command to connect to the running Admin Node Manager Docker container.

    ```
    docker exec -it <anm-container-id> bash
    ```

2. Change to the `bin` directory.

    ```
    cd /opt/Axway/apigateway/posix/bin
    ```

3. Run the `kpsadmin` tool.

    ```
    ./kpsadmin
    ```

For more information on using `kpsadmin`, see the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).

## Logs do not persist when container stops

Problem
: Traffic monitor data and trace logs do not persist when a container stops.

Solution
: You can redirect the trace and traffic logs to `stdout` instead of to separate files, which allows the logs to be read directly from each container by an external logging service. For more information, see [Redirect logs to stdout](/docs/apim_installation/apigw_containers/configure_log_streaming/).

## Use Apache Cassandra as a distributed data store

Problem
: Distributed Ehcache is not supported in a container deployment.

Solution
: You can use Apache Cassandra as a distributed data store. This involves using the KPS scripting API, which enables you to perform CRUD operations and interact directly with a KPS. For details, see the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).
