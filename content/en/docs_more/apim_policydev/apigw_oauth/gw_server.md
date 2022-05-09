---
title: Set up API Gateway OAuth server
linkTitle: Set up API Gateway OAuth server
weight: 29
date: 2019-11-18
description:  Learn how to set up API Gateway as an OAuth server.
---

{{< alert title="Note" color="primary" >}}If you have installed API Manager, the OAuth server capabilities are already installed. You can skip this section. The OAuth client demo is not installed when you install API Manager.{{< /alert >}}

## Deploy OAuth services

To set up API Gateway as an OAuth authorization server and OAuth resource server, you must deploy the OAuth services. Perform the following steps:

1. Open Policy Studio and open or create a new project.
2. Select **File > Configure OAuth**.
3. If you do not have any Cassandra hosts configured, you must add a Cassandra host before you can continue:
    * Enter a name for the Cassandra server (for example, `local_cassandra`)
    * Enter the host name of the Cassandra host (for example, `localhost`)
    * Enter the port of the Cassandra host (for example, `9042`)
4. Click **Next**.
5. Select `all` or `authzserver` as the OAuth deployment type.
    * `all` – Deploys the OAuth 2.0 services listener and supporting policies, and the OAuth client demo. This deploys the OAuth server components on port 8089 and deploys the client demo on port 8088.

        This option does not register the sample client applications in the Client Application Registry. You must import them manually as detailed in [Import sample client applications](/docs/apim_policydev/apigw_oauth/gw_client_demo/#import-sample-client-applications).
    * `authzserver` – Select this option to deploy the OAuth server components only.
6. Click **Finish**.
7. Click **Deploy** in the toolbar to deploy the updated configuration to API Gateway.

{{< alert title="Note" color="primary" >}}In earlier versions of API Gateway, OAuth was configured using the `deployOAuthConfig.py` script provided in `INSTALL_DIR/apigateway/samples/scripts/oauth` directory. This script is still supported for backwards compatibility, but we recommend that you use Policy Studio to configure OAuth in this version. For more information on running the `deployOAuthConfig.py` script, run the script with the `--help` option. Apache Cassandra must be installed and running, and the Cassandra hosts configured in Policy Studio before you run the `deployOAuthConfig.py` script.{{< /alert >}}

## Enable OAuth endpoints

To enable the OAuth management endpoints on your API Gateway, perform the following steps:

1. In the Policy Studio tree, select **Environment Configuration > Listeners > API Gateway > OAuth 2.0 Services > Ports**.
2. Right-click the **OAuth 2.0 Interface** in the panel on the right, and select **Edit**.
3. Select **Enable Interface** in the dialog.
4. Click the **Deploy** button in the toolbar.
5. Enter a description and click **Finish**.

{{< alert title="Note" color="primary" >}}
On Linux-based systems, you must open the firewall to allow external access to port `8089`. If you need to change the port number, set the value of the `env.PORT.OAUTH2.SERVICES` environment variable.
{{< /alert >}}

The API Gateway provides the following endpoints used to manage OAuth 2.0 client applications:

| Description                                  | URL                                                 |
|----------------------------------------------|-----------------------------------------------------|
| Authorization Endpoint (REST API)            | `https://HOST:8089/api/oauth/authorize`               |
| Token Endpoint (REST API)                    | `https://HOST:8089/api/oauth/token`                 |
| Token Info Endpoint (REST API)               | `https://HOST:8089/api/oauth/tokeninfo`               |
| Revoke Endpoint (REST API)                   | `https://HOST:8089/api/oauth/revoke`                |
| Client Application Registry (HTML Interface) | `https://HOST:8089`                                  |
| Client Application Registry (REST API)       | `https://HOST:8089/api/kps/ClientApplicationRegistry` |

In this table, `HOST` refers to the machine on which API Gateway is installed.
