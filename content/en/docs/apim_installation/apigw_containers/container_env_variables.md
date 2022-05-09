---
title: Runtime parameters
linkTitle: Runtime parameters
weight: 70
date: 2019-09-18
description: Customize a container's behavior at runtime.
---

When you start an API Gateway, Admin Node Manager, or API Gateway Analytics container, you can customize its behavior at runtime:

* You can pass one or more environment variables to the `docker run` command.
* You can add a file to a container at runtime, leveraging Docker data management capabilities, such as volumes or bind mounts.

## Environment variables reference

You can specify both externally managed topology (EMT) environment variables and core platform system variables when starting Docker containers.

Environment variables for controlling an EMT system are prefixed with `EMT_`.

| Variable Name | Description |
|------|-------|
| `EMT_ANM_HOSTS`             | Enables the API Gateway to communicate with the Admin Node Manager container through the port specified. Specify a comma-separated list of Admin Node Manager containers that a gateway should register with.  Sample usage: `-e EMT_ANM_HOSTS="anm-ds1:8090,anm-ds2:8090"` |
| `EMT_DEPLOYMENT_ENABLED`    | Enables deploying changes to a running container by way of Policy Studio only.  This parameter should only be used in a development environment. Deploying a new FED into a container does not update the associated image, and a new API Gateway container will continue to use the old FED. In production, the FED updates should be incorporated into a new image and deployed via a rolling update. Sample usage: `-e EMT_DEPLOYMENT_ENABLED=true`|
| `EMT_TOPOLOGY_LOG_DEST`     | Specifies the destination to which topology log records are written. Possible values: 1 (Topology log), 2 (Trace log), 3 (both). Defaults to 1.   |
| `EMT_TOPOLOGY_LOG_DIR`      | The directory into which topology logs are written. Defaults to `/opt/Axway/apigateway/logs`. If you mount this directory as a volume, you can set a location outside the container to write the logs. |
| `EMT_TOPOLOGY_LOG_ENABLED`  | Specifies whether topology logging is enabled or disabled. Possible values: `true`, `false`. Defaults to `false`.  |
| `EMT_TOPOLOGY_LOG_INTERVAL` | Time interval, in seconds, between successive log records for each gateway. Defaults to 60. Maximum value: 3600. For example, if set to 60, then a log record is written once a minute for each gateway, recording the number of transactions since the last log record was written.  |
| `EMT_PARENT_HOST`           | Identifies the host on which the API Gateway container is running.|
| `EMT_HEAP_SIZE_MB`          | Specifies the initial and maximum Java heap size for the Admin Node Manager, API Gateway, or API Gateway Analytics process. JVM processes within a Docker container sometimes set their heap size from the host machine's limits, thereby choosing a value that is too high. This parameter can be used to limit the JVM's memory footprint. Sample usage: `-e EMT_HEAP_SIZE_MB=4096` |
| `EMT_TRACE_LEVEL`           | Specifies the trace level for an Admin Node Manager or API Gateway container. This can be useful for debugging issues in the runtime environment  without having to build a new image. Possible values: INFO, DEBUG, DATA. Defaults to INFO. Sample usage: `-e EMT_TRACE_LEVEL=DEBUG` |
| `EMT_POLL_INTERVAL`         | The frequency, in seconds, with which an API Gateway container transmits registration requests to all Admin Node Managers in `EMT_ANM_HOSTS`. Admin Node Managers use these registration requests to dynamically build up a picture of the topology, which is then displayed in the API Gateway Manager UI. This parameter has default value 30, and should not normally need to be set explicitly. Sample usage: `-e EMT_POLL_INTERVAL=35`  |
| `EMT_TOPOLOGY_TTL`          | The time, in seconds, that an Admin Node Manager container will wait without receiving a registration request from a gateway before it deletes that gateway from the topology. When a gateway is removed from the topology, it disappears from the topology view in the API Gateway Manager UI. This could occur, for example, if the container orchestration framework scales down the number of gateway containers in response to dropping traffic levels. This parameter has default value 120, and should not normally need to be set explicitly. Sample usage: `-e EMT_TOPOLOGY_TTL=140`|
| `CASS_HOST`                 | Apache Cassandra host used to store the API Manager data.  |
| `METRICS_DB_URL`            | URL to connect with the metrics database. |
| `METRICS_DB_USERNAME`       | User name to connect with the metrics database. |
| `METRICS_DB_PASS`           | Password to connect with the metrics database.  |

## Add files to containers in a runtime environment

Many files can be added to images at build time.
Sometimes, files contain information that is only known in a runtime environment.
Such files can be added at container startup by using Docker bind mounts. For more details, see the `readme.md` included in the Docker scripts package.

For example, a mount point is `/merge/apigateway` for Admin Node Manager and API Gateway containers, and `/merge/analytics` for API Gateway Analytics containers.

```
docker run -v /tmp/apigateway:/merge/apigateway --name=anm ...
docker run -v /tmp/analytics:/merge/analytics --name=analytics ...
```
