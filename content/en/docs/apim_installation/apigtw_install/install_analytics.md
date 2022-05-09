{
"title": "Install API Gateway Analytics",
"linkTitle": "Install API Gateway Analytics",
"weight":"24",
"date": "2019-10-02",
"description": "API Gateway Analytics is a server runtime and web UI that allows you to analyze API usage over extended periods of time."
}

## Prerequisites

Ensure that all of the [prerequisites](/docs/apim_installation/apigtw_install/system_requirements) are met.

### Axway license file

You must have a valid Axway license file to install API Gateway Analytics. To obtain an evaluation trial license or a full license, contact your Axway Account Manager.

## Install API Gateway Analytics

To install API Gateway Analytics in GUI mode, perform an installation following the steps described in [Installation](/docs/apim_installation/apigtw_install/installation), using the following selections:

* Select the **Custom** setup type.
* Select to install the API Gateway Analytics component.

To install API Gateway Analytics in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following example shows how to install the API Gateway Analytics component in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type advanced --enable-components analytics --disable-components nodemanager,apigateway,qstart,policystudio,apitester,configurationstudio,apimgmt,cassandra,packagedeploytools --analyticsLicenseFilePath myanalyticslicense.lic
```

## Configure your metrics database for API Gateway Analytics

Before starting API Gateway Analytics, you must perform the following steps:

1. Create a database instance to store metrics for API Gateway Analytics. Alternatively, if you already have an existing database, skip to the next step.
2. Update your API Gateway Analytics configuration with the database details using the `configureserver` script.
3. Configure the database tables using the `dbsetup` script.
4. Enable writing of metrics from your API Gateway instance to the database using the `managedomain` tool.

For more details on how to perform these steps, see the [API Gateway Analytics User Guide](/docs/apimanager_analytics/).

## Start API Gateway Analytics

When you have configured your metrics database and API Gateway Analytics, you can start up API Gateway Analytics.

### Start as a service

You can also run API Gateway Analytics as a service. For more information, see [Set up services](/docs/apim_installation/apigtw_install/post_overview#set-up-services).
