{
"title": "Install the Admin Node Manager",
"linkTitle": "Install the Admin Node Manager",
"weight":"14",
"date": "2019-10-02",
"description": "Install an Admin Node Manager component in isolation without an API Gateway license."
}

## Prerequisites

Ensure that all of the prerequisites detailed in [Prerequisites](/docs/apim_installation/apigtw_install/system_requirements) are met.

## Install the Admin Node Manager

To install the Admin Node Manager in GUI mode, perform an installation following the steps described in [Installation options](/docs/apim_installation/apigtw_install/installation), using the following selections:

* Select the **Custom** setup type.
* Select to install the Admin Node Manager component.

To install the Admin Node Manager in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following example shows how to install the Admin Node Manager component in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type advanced --enable-components apigateway --disable-components qstart,policystudio,configurationstudio,analytics,apitester,apimgmt,cassandra,packagedeploytools
```

## Start the Admin Node Manager

For more information on starting the Admin Node Manager, see [Start API Gateway](/docs/apim_installation/apigtw_install/install_gateway#start-api-gateway).
