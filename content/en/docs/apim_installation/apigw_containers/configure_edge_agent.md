---
title: Configure usage tracking
linkTitle: Configure usage tracking
weight: 65
date: 2020-02-05T00:00:00.000Z
description: >-
  Enable usage tracking for on-premise API management products purchased on a
  subscription basis.
---
Usage tracking is how Axway measures the subscription services you use on a monthly basis. Axway measures your usage to make sure it is within the prescribed thresholds specified in your subscription, and to determine whether overages require billing adjustments. For more information, see [About subscription usage tracking](https://docs.axway.com/bundle/subusage_en/page/about_subscription_usage_tracking.html).

You can log in to the [Amplify Platform](https://platform.axway.com/) and look up information about your subscriptions and service entitlements. If you are unsure whether usage tracking applies to you, contact your Axway representative or [Axway Support](https://support.axway.com/).

The Amplify Edge Agent collects data from your on-premise API management products and uploads usage reports to the Amplify Platform. You must connect your API Gateway to the Edge Agent to enable automatic upload of usage reports to the Amplify Platform.

{{< alert title="Note" color="primary" >}}Topology logging and subscription-based billing are restricted to container-based API Gateway installations that are running in EMT mode.{{< /alert >}}

## Configure API Gateway to connect with the Edge Agent

Before you start, see [Deploy the agent](https://docs.axway.com/bundle/subusage_en/page/deploy_the_agent.html).

Upload the following two configuration files to the Edge Agent to enable API Gateway to send usage data to the Edge Agent:

* An [input configuration file](https://support.axway.com/en/documents/download/id/1443964) that defines the type of data the Edge Agent collects from API Gateway.
* A [report configuration file](https://support.axway.com/en/documents/download/id/1443965) the Edge Agent uses to aggregate the data to upload to the platform.

API Gateway communicates with the agent over Lumberjack protocol using Filebeat.

### Start an Admin Node Manager container with topology logging enabled

Topology logging contains usage statistics that can be used for billing. It records the number of API Gateways that are running and the number of transactions (for example: HTTP, JMS, FTP, and directory scanner) processed by each. This can be useful for tracking the distribution of API Gateways across hosts.

The Admin Node Manager is responsible for writing the topology log, which is configured by way of environment variables. Topology logging is disabled by default.

The following example shows how to start an Admin Node Manager container with topology logging enabled:

```
docker run -d -p 8090:8090 --name=anm --network=api-gateway-domain -v /tmp/events:/opt/Axway/apigateway/events -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=db_user1 -e METRICS_DB_PASS=my_db_pwd -e EMT_TRACE_LEVEL=DEBUG -e EMT_TOPOLOGY_LOG_ENABLED=true -e EMT_TOPOLOGY_LOG_INTERVAL=30 -e EMT_TOPOLOGY_LOG_DEST=3 -e EMT_TOPOLOGY_LOG_DIR=/tmp/topology-logs admin-node-manager:1.0
```

This example uses the following topology logging environment variables:

`EMT_TOPOLOGY_LOG_ENABLED`
: Starts an Admin Node Manager with topology logging enabled

`EMT_TOPOLOGY_LOG_INTERVAL`
: Sets the time interval to write log records

`EMT_TOPOLOGY_LOG_DEST`
: Sets the destination of the log records

`EMT_TOPOLOGY_LOG_DIR`
: Specifies a directory to write log records

For information on the other options used in this example, see [Start the Admin Node Manager container](/docs/apim_installation/apigw_containers/docker_script_anmimage/#start-the-admin-node-manager-docker-container).

### Start an API Gateway container with topology logging enabled

If you have started an Admin Node Manager with topology logging enabled, you must specify the API Gateway host identity in the `EMT_PARENT_HOST` environment variable to identify the host of the log records, and restart the host.

The following example shows how to start an API Gateway container and set the parent host in the topology log records.

```
docker run -d --name=apimgr --network=api-gateway-domain -p 8075:8075 -p 8065:8065 -p 8080:8080 -v /tmp/events:/opt/Axway/apigateway/events -e EMT_ANM_HOSTS=anm:8090 -e CASS_HOST=casshost1 -e METRICS_DB_URL=jdbc:mysql://metricsdb:3306/metrics?useSSL=false -e METRICS_DB_USERNAME=root -e METRICS_DB_PASS=my_db_pwd -e EMT_TRACE_LEVEL=DEBUG -e EMT_PARENT_HOST=acme-DC1-server2 api-gateway-my-group:1.0
```

This example uses the following topology logging environment variables:

`EMT_PARENT_HOST`
: Sets the name of the parent host for the API Gateway

{{< alert title="Note" color="primary" >}}If you do not specify this variable, the topology log records for the API Gateway will contain the value `<UNKNOWN>` for the parent host.{{< /alert >}}

For information on the other options used in this example, see [Start the API Gateway container](/docs/apim_installation/apigw_containers/docker_script_gwimage/#start-the-api-gateway-docker-container).

## Review subscriptions and usages on the platform

After you have completed the configuration and have enabled uploads of usage reports, you can log in to the [Amplify Platform](https://platform.axway.com/) and access and review your subscriptions and usage data. For more information, see [Review subscriptions and usages on the platform](https://docs.axway.com/bundle/subusage_en/page/review_subscriptions_and_usages_on_the_platform.html).
