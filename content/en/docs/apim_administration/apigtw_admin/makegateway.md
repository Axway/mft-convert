{
"title": "Configure an API Gateway domain",
"linkTitle": "Configure an API Gateway domain",
"weight":"30",
"date": "2019-10-14",
"description": "Use the `managedomain` command to configure a managed API Gateway."
}

This section describes the minimum steps required to configure an API Gateway domain:

* Register a host in a new domain
* Create a new API Gateway instance

{{< alert title="Note" color="primary" >}}If you installed the QuickStart tutorial, an example domain was created automatically. If you did not install QuickStart, you must configure a domain using `managedomain`.{{< /alert >}}

A single API Gateway installation supports a single API Gateway domain only. If you wish to run gateways in different domains on the same host, you need separate installations for each domain.

You can also use the topology view in the web-based API Gateway Manager tool to manage a newly created domain. For example, you can create or delete API Gateway groups and instances, and start or stop instances. For more details, see [Manage domain topology in API Gateway Manager](/docs/apim_administration/apigtw_admin/managetopology).

## Managedomain script

When configuring a domain, the `managedomain` script enables you to perform tasks such as the following:

* Host management (registering and deleting hosts, or changing Admin Node Manager credentials)
* API Gateway management (creating and deleting API Gateway instances, or adding Linux services)
* Group management (editing or deleting API Gateway groups)
* Topology management (viewing topologies)
* Deployment (deploying to a group, listing deployments, creating or downloading deployment archives, and editing group passphrases)
* Domain SSL certificates (regenerating SSL certificates on localhost)

### Managedomain options

For details on selecting specific options, enter the `managedomain` command in the following directory, and follow the instructions at the command prompt:

```
INSTALL_DIR/apigateway/posix/bin
```

{{< alert title="Note" color="primary" >}}To register an API Gateway instance as a service on Linux, you must run the `managedomain` command as `root`.{{< /alert >}}

For more details on `managedomain` options, see [Managedomain command reference](/docs/apim_reference/managedomain_ref/).

## Register a host in a domain

Before registering multiple hosts in a domain, you must first ensure that a licensed API Gateway is installed on each host machine. Then to register each host, you must select option `1` on each host machine.

To register a host in a managed domain, perform the following steps:

1. Change to the following directory in your API Gateway installation:

    ```
    INSTALL_DIR/apigateway/posix/bin
    ```

2. Enter the following command:

    ```
    managedomain --menu
    ```

3. Enter **`1`** to register your host, and follow the instructions when prompted. For example, if this is the first host in the domain, enter **`y`** to configure an Admin Node Manager on the host. Alternatively, to add the host to an existing domain, enter **`n`** to configure a local Node Manager that connects to the Admin Node Manager in the existing domain.
4. Enter **`q`** to quit when finished.
5. Enter the following command to start the Admin Node Manager or local Node Manager on the registered host:

    ```
    nodemanager
    ```

You must ensure the Admin Node Manager is running in the domain to enable monitoring and management of the gateway instances.

## Create an API Gateway instance

To create an API Gateway instance, perform the following steps:

1. Open a new command window.
2. Change to the following directory in your API Gateway installation:

    ```
    INSTALL_DIR/apigateway/posix/bin
   ```

3. Enter the following command:

    ```
    managedomain --menu
    ```

4. Enter **`5`** to create a new API Gateway instance, and follow the instructions when prompted. You can repeat to create multiple API Gateway instances on local or remote hosts.
5. Enter **`q`** to quit when finished.
6. Use the `startinstance` command to start API Gateway, for example:

    ```
    startinstance -n "my_server" -g "my_group"
    ```

* You can add an API Gateway instance on any registered host in the domain, not just the local host. However, if you are creating Linux services for the gateway, you must run `managedomain` on the same host.
* You must run `startinstance` on the host on which you intend to start the instance.
* You must ensure that the `startinstance` file has execute permissions. Running `startinstance` without any arguments lists all API Gateway instances available on the host.

## Test the health of an API Gateway instance

You can test the connection to the new API Gateway instance by connecting to the Health Check service. For example, enter the following default URL in your browser:

```
http://HOST:8080/healthcheck
```

This should display a simple `<status>ok</status>` message.

You can view the newly created gateway instance on the API Gateway Manager dashboard. For example, the default URL is as follows:

```
https://HOST:8090
```

The port numbers used to connect depend on those entered when configuring the domain using `managedomain`, and are available from the localhost only.
