{
"title": "Unattended installation",
"linkTitle": "Unattended installation",
"weight":"6",
"date": "2019-10-02",
"description": "Run the API Gateway installer or the developer tools installer in unattended mode."
}

This section explains how to run the API Gateway installer in unattended mode on Linux and the developer tools installer in unattended mode on Windows. It also describes each of the available command options.

{{< alert title="Note" color="primary" >}}Windows is supported only for a limited set of developer tools, see [Install developer tools on Windows](/docs/apim_installation/apigtw_install/install_dev_tools/). API Gateway and API Manager do not support Windows.{{< /alert >}}

## Run the installer in unattended mode

You can run the API Gateway installer in unattended mode on the command line. Perform the following steps:

1. Change to the directory where the setup file is located.
2. Run the setup file with the `--mode unattended` option.

### Standard setup without API Manager

The following example shows how to install all available API Gateway components excluding API Manager in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type standard --licenseFilePath my_license.lic --analyticsLicenseFilePath my_analytics_license.lic --prefix /opt/Axway-7.7 --cassandraInstalldir opt/db/cassandra --cassandraJDK opt/jre --startCassandra 1
```

The components are installed in the background, in the directory specified by the `--prefix` option.

### Complete setup with API Manager

The following example shows how to install all API Gateway components, including API Manager, in unattended mode:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type complete --licenseFilePath my_license.lic --analyticsLicenseFilePath my_analytics_license.lic --apimgmtLicenseFilePath my_mgmt_license.lic --prefix /opt/Axway-7.7 --cassandraInstalldir /opt/db/cassandra --cassandraJDK /opt/jre --startCassandra 1
```

The components are installed in the background, in the directory specified by the `--prefix` option.

### Custom setup

The pages on installing each gateway component show how to use the `-setup_type advanced` option to install a custom setup in unattended mode. For example, see [Install the API Gateway server](/docs/apim_installation/apigtw_install/install_gateway/).

## Unattended mode options

For a description of all the available command-line options and their default settings, run the setup file with the `--help` option. This outputs the help text in a separate console. For example:

**Linux**:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --help
```

**Windows**:

```
APIGateway_7.7_Client_Tools_Install_win-x86-32_BN<n>.exe --help
```

The following summarizes some of the more common options:

* `--help`: Display available options and default settings.
* `--mode`: Specify an installation mode.
* `--setup_type`: Specify a setup type (`standard`, `complete`, or `advanced`) (on Linux only).
* `--enable-components`: Specify a comma-separated list of components to enable.
* `--disable-components`: Specify a comma-separated list of components to disable.
* `--prefix`: Specify an installation directory.
* `--unattendedmodeui`: Specify different levels of user interaction when installing on a Linux system with X-Windows or on Windows.  
* `--cassandraInstalldir`: Specify the Apache Cassandra installation directory, for example, `opt/db/cassandra`.
* `--cassandraJDK`: Specify the location of your Java Runtime Environment for Apache Cassandra. The default value is `INSTALL_DIR/apigateway/Linux.x86_64/jre/bin`
* `--startCassandra`: Specify whether the Apache Cassandra server starts after the installer completes. Set to `1` to start Cassandra after installation, or set to `0` if you do not * want Cassandra to start.
* `--optionfile`: Specify options in a properties file. For more information on option files, see [BitRock InstallBuilder User Guide 20](http://installbuilder.bitrock.com/docs/installbuilder-userguide.html).
