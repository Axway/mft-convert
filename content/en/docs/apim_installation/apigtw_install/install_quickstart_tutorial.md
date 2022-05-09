{
"title": "Install the QuickStart tutorial",
"linkTitle": "Install the QuickStart tutorial",
"weight":"10",
"date": "2019-10-02",
"description": "The QuickStart tutorial demonstrates the main API Gateway features and tools, and lets you invoke example APIs and monitor your API Gateway."
}

The QuickStart tutorial is automatically installed as part of a default **Standard** or **Complete** setup.

## Prerequisites

* The QuickStart tutorial is dependent on the API Gateway Server. You cannot install the QuickStart tutorial without the API Gateway Server.
* Ensure that all of the prerequisites detailed in [Prerequisites](/docs/apim_installation/apigtw_install/system_requirements/) are met.

## Install the QuickStart tutorial

To install the API Gateway Server and the QuickStart tutorial in GUI mode, perform an installation following the steps described in [Installation options](/docs/apim_installation/apigtw_install/installation#select-setup-type), using the following selections:

* Select the **Custom** setup type.
* Select to install the API Gateway Server, Admin Node Manager, and QuickStart tutorial components.

To install the API Gateway Server, Admin Node Manager, and QuickStart tutorial in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended/).

The following example shows how to install the API Gateway Server component and the QuickStart tutorial in unattended mode:

```
APIGateway_7.7_Install_linux-x86-32_BNyyyyMMdd.run --mode unattended --setup_type advanced --enable-components apigateway,nodemanager,qstart --disable-components policystudio,configurationstudio,apimgmt,cassandra,packagedeploytools --licenseFilePath mylicense.lic
```

## QuickStart domain configuration

When the QuickStart tutorial is installed, a sample gateway domain is automatically configured in your installation. This includes a `QuickStart Server` gateway instance that runs in a `QuickStart Group` group. The QuickStart server and Admin Node Manager start automatically when installation is complete.

## Start the QuickStart tutorial

The QuickStart tutorial launches automatically in your browser when installation is complete. Follow the instructions in your browser to perform the steps in the tutorial.

For example, the following screen shows invoking a sample API in the tutorial:

![QuickStart Overview](/Images/APIGateway/quickstart_api.png)

You can click the **Try it** button to invoke the sample API. This displays a JSON list of available products. You can click the **Show Me** button to view the traffic monitored by API Gateway in API Gateway Manager.

## Restart the QuickStart tutorial

At any point, if you need to restart the QuickStart tutorial, perform the following steps:

1. Open a command prompt in the following directory:

    ```
    INSTALL_DIR/apigateway/posix/bin
    ```

2. Ensure that the `startinstance` has execute permissions, and run the `startinstance` command, for example:

    ```
    startinstance -n "QuickStart Server" -g "QuickStart Group"
    ```

3. To manage and monitor the gateway, you must ensure that the Admin Node Manager is running. Use the `nodemanager` command to start the Admin Node Manager from the same directory.
4. To launch API Gateway Manager, enter the following address in your browser:

    ```
    https://127.0.0.1:8090/
    ```

5. Enter the administrator user name and password. This is the administrator user name and password you entered during installation.
6. To launch the QuickStart tutorial, enter the following address in your browser:

    ```
    http://127.0.0.1:8080/quickstart/index.html?mgr=8090
    ```
