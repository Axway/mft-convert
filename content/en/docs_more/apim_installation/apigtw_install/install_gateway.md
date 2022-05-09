{
"title": "Install the API Gateway server",
"linkTitle": "Install the API Gateway server",
"weight":"10",
"date": "2019-10-02",
"description": "The API Gateway server is the main runtime environment consisting of an API Gateway instance and a Node Manager."
}

{{< alert title="Note" color="primary" >}}Windows is supported only for a limited set of developer tools, see [Install developer tools on Windows](/docs/apim_installation/apigtw_install/install_dev_tools). API Gateway and API Manager do not support Windows.{{< /alert >}}

## Prerequisites

* Ensure that all of the [prerequisites](/docs/apim_installation/apigtw_install/system_requirements) are met.
* If you are using Apache Cassandra, before starting API Gateway, you must first ensure that Cassandra is installed and running.

### Axway license file

You must have a valid Axway license file to install the API Gateway server. Also, if you intend to run API Gateway in FIPS-compliant mode, ensure that your license file allows this. To obtain an evaluation trial license or a full license, contact your Axway Account Manager.

## Install the API Gateway server

To install the API Gateway server in GUI mode, perform an installation following the steps described in [Installation](/docs/apim_installation/apigtw_install/installation), using the following selections:

* Select the **Custom** setup type.
* Select to install the API Gateway server component.

To install the API Gateway server in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following example shows how to install the API Gateway server component in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type advanced --enable-components apigateway --disable-components nodemanager,qstart,policystudio,analytics,apitester,configurationstudio,apimgmt,cassandra,packagedeploytools --licenseFilePath mylicense.lic
```

## Before you start API Gateway

Before you can start API Gateway, you must first use the `managedomain` script to create a new domain that includes an API Gateway instance. If you installed the QuickStart tutorial, a sample API Gateway domain is automatically configured in your installation. Otherwise, you must first create a new domain. For more details, see [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway/).

If you installed the QuickStart tutorial, the QuickStart server and Admin Node Manager start automatically. Otherwise, you must start them manually.

## Start API Gateway

To start API Gateway manually, follow these steps:

1. Open a command prompt in the following directory:

    ```
    INSTALL_DIR/apigateway/posix/bin
    ```

2. Ensure that the `startinstance` has execute permissions, and run the `startinstance` command, for example:

    ```
    startinstance -n "Server1" -g "Group1"
    ```

3. To manage and monitor your gateway, you must ensure that the Admin Node Manager is running. Use the `nodemanager` command to start the Admin Node Manager from the same directory.
4. To launch API Gateway Manager, enter the following address in your browser:

    ```
    https://HOST:8090/
    ```

    `HOST` refers to the host name or IP address of the machine on which API Gateway is running (for example, `https://localhost:8090/`).

5. Enter the administrator user name and password. This is the administrator user name and password you entered during installation.

{{< alert title="Note" color="primary" >}}You can encrypt all sensitive API Gateway configuration data with an encryption passphrase. For example, you can specify this passphrase in your gateway configuration file, or on the command line when the gateway is starting up. For more details, see [Configure an encryption passphrase](/docs/apim_administration/apigtw_admin/general_passphrase/). {{< /alert >}}

### Start as a service

You can also run the gateway instances and Node Managers as services. For more information, see [Set up services](/docs/apim_installation/apigtw_install/post_overview/#set-up-services).
