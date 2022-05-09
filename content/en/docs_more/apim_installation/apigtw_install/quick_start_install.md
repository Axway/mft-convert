{
"title": "Quick start installation",
"linkTitle": "Quick start",
"weight":"2",
"date": "2019-10-07",
"description": "Perform a quick start installation of API Gateway on Linux."
}

A quick start installation is a simple, standard installation of API Gateway (for example, for a demonstration or proof of concept).

{{< alert title="Note" color="primary" >}}Windows is supported only for a limited set of developer tools, see [Install developer tools on Windows](/docs/apim_installation/apigtw_install/install_dev_tools/). API Gateway and API Manager do not support Windows.{{< /alert >}}

The API Gateway installer provides a default **Standard** installation option, which installs the following API Gateway components:

* API Gateway Server
* QuickStart tutorial
* API Gateway Analytics
* Policy Studio
* Configuration Studio
* Package and deployment tools

The **Standard** option also installs an external Apache Cassandra database, which is used to store API Gateway and API Manager data. For more details, see [Installation options](/docs/apim_installation/apigtw_install/installation/#installation-options).

## Before you begin

In preparation for a quick start installation, perform the following tasks:

1. Check that your target system meets the [system requirements](/docs/apim_installation/apigtw_install/system_requirements/).
2. Download the installation setup file for your target system.
3. Obtain the necessary license keys from your Axway Account Manager.

## Installation

Locate and run the installation setup file. The installer launches in GUI mode by default. Follow the instructions on each window, accepting the default selections at each step. For more information on starting the installer, see [Start installation](/docs/apim_installation/apigtw_install/installation/#start-installation).

When installation is complete, the Cassandra database, API Gateway instance, and Admin Node Manager processes are started, the QuickStart tutorial is launched in a browser window, and the Policy Studio desktop tool is started.

## Post-installation

You can use the QuickStart tutorial to invoke some example APIs and to monitor the API Gateway using API Gateway Manager.

You can use the Policy Studio desktop tool to virtualize APIs and develop policies (for example, to enforce security, compliance, and operational requirements). To begin developing policies in Policy Studio, you must first open or create a new project. For example, follow these steps to create a new project from a running API Gateway instance:

1. When Policy Studio starts up, select **File > New Project**.
2. In the New Project dialog, enter a name for the project and click **Next**.
3. Select **From a running API Gateway instance** and click **Next**.
4. In the Open Connection dialog, select the Admin Node Manager session to connect to, enter the administrator user name and password that you specified during installation and click **OK**.
5. In the Download Options dialog, select the group and the instance to download its configuration.
6. Click **Finish**.

For more information on using Policy Studio, see [Develop in Policy Studio](/docs/apim_policydev/).
