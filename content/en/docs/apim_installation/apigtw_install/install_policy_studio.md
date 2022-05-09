{
"title": "Install Policy Studio",
"linkTitle": "Install Policy Studio",
"weight":"16",
"date": "2019-10-02",
"description": "Policy Studio is a graphical IDE that allows you to virtualize APIs and develop policies."
}

You can install Policy Studio on both Linux and Windows.

{{< alert title="Note" color="primary" >}}Windows is supported only for a limited set of developer tools, see [Install developer tools on Windows](/docs/apim_installation/apigtw_install/install_dev_tools). API Gateway and API Manager do not support Windows.{{< /alert >}}

## Prerequisites

Ensure that all of the prerequisites detailed in [prerequisites](/docs/apim_installation/apigtw_install/system_requirements) are met.

## Install Policy Studio

To install Policy Studio in GUI mode, perform an installation following the steps described in [Installation](/docs/apim_installation/apigtw_install/installation), using the following selections:

* Select the **Custom** setup type. This screen is omitted on Windows.
* Select to install the Policy Studio component.

To install Policy Studio in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following example shows how to install the Policy Studio component in unattended mode on Linux:

```
./APIGateway_7.7_Install_linux-x86-32-BN<n>.run --mode unattended --setup_type advanced  
--enable-components policystudio --disable-components nodemanager,apigateway,qstart,apitester,analytics,configurationstudio,apimgmt,cassandra,packagedeploytools
```

{{< alert title="Note" color="primary" >}}
To install Policy Studio on Windows without administrator privileges set the following environment variable in a command-line window:

```
set __COMPAT_LAYER=RunAsInvoker
```

You must start the installer from the same window.
{{< /alert >}}

## Start Policy Studio

It is against security best practices to run Policy Studio using privileged accounts, such as `root` in UNIX environments or `LOCALSYSTEM` in Windows environments.

When applications are run with privileged accounts, the processes and the code being executed on top of these processes run with all of the rights of these users. Code will execute with the authority of the privileged account, thus increasing the possible damage from a hypothetical vulnerability exploit. This risk is not present in any application running with minimal permissions.

If you did not select to launch Policy Studio after installation, perform the following steps:

1. Ensure both the Admin Node Manager and the API Gateway instance are running. For more details, see [Start API Gateway](/docs/apim_installation/apigtw_install/install_gateway#start-api-gateway)
2. Open a command prompt.
3. Change to your Policy Studio installation directory (for example, `INSTALL_DIR/policystudio`).
4. Run `policystudio`.
5. When Policy Studio starts up, select **File > New Project**.
6. In the New Project dialog, enter a name for the project and click **Next**.
7. Select **From a running API Gateway instance** and click **Next**.
8. In the Open Connection dialog, select the Admin Node Manager session to connect to, enter the administrator user name and password you specified when you installed API Gateway, and click **OK**.
9. In the Download Options dialog, select a group and an API Gateway instance to download its configuration.
10. If a passphrase has been set, enter it in the **Passphrase** field, and click **Finish**. Alternatively, if no passphrase has been set, click **Finish**.
