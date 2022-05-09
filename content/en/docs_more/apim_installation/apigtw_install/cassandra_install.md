{
"title": "Install an Apache Cassandra database",
  "linkTitle": "Install Apache Cassandra",
  "weight": "8",
  "date": "2019-10-02",
  "description": "Install Apache Cassandra to store data for API Manager or API Gateway client registry (API key and OAuth)."
}

Apache Cassandra is required to store data for API Manager (for example, API catalog, quotas, and client registry) or API Gateway client registry (API key and OAuth). In addition, Cassandra is optional to store data for the following API Gateway components:

* Custom KPS table definitions and data
* OAuth token stores

{{< alert title="Note" color="primary" >}}You must ensure that Cassandra is installed and running to use API Manager or API Gateway client registry.{{< /alert >}}

### Supported Cassandra versions

API Gateway supports Apache Cassandra version 2.2.12. For more details, see [Apache Cassandra](http://cassandra.apache.org/) home page.

For details on upgrading your Cassandra version, see [Upgrade Apache Cassandra](/docs/apim_installation/apigw_upgrade/upgrade_cassandra/).

### Upgrade from earlier API Gateway versions

API Gateway version 7.5.3 and later include the Datastax Cassandra client, which uses a default port of `9042` to communicate with Cassandra over the Cassandra native protocol. Earlier API Gateway versions included the Hector Cassandra client, which used a default port of `9160` to communicate with Cassandra over the Apache Thrift protocol.

In API Gateway version 7.5.1 or later, Cassandra runs externally to the API Gateway process. In earlier API Gateway versions, Cassandra was embedded in the API Gateway process.

For details on upgrading from an earlier API Gateway version, see the [API Gateway Upgrade Guide](/docs/apim_installation/apigw_upgrade/).

## Cassandra prerequisites

This section describes Cassandra-specific prerequisites in addition to the general API Gateway [Prerequisites](/docs/apim_installation/apigtw_install/system_requirements).

### Production environment requirements

API Gateway supports the following in a production environment:

* **Operating systems**:
    * All supported Linux platforms
* **Cassandra**:
    * Cassandra version 2.2.12
    * 64-bit OpenJDK JRE or Oracle JRE version 8

For details on requirements for high availability, see [Configure a Cassandra HA cluster](/docs/cass_admin/cassandra_config/).

### JRE requirements and recommendations

The default API Gateway installation includes a 64-bit OpenJDK JRE (`apigateway/Linux.x86_64/jre/bin`). You can configure Cassandra to use the API Gateway JRE (for example, in a demo environment), but it is recommended that you install a separate JRE (OpenJDK or Oracle) for use with Cassandra. When using a separate JRE, use the same version (or at least the same major version) as the API Gateway uses.

### Cassandra hardware

Cassandra is designed to run on commodity distributed drives, and therefore it is strongly recommended not to use a storage area network (SAN) for Cassandra deployments.

For more information on Cassandra hardware choice recommendations, see [Hardware choices]( https://cassandra.apache.org/doc/latest/operating/hardware.html).

## Install Apache Cassandra

{{< alert title="Note" color="primary" >}}Apache Cassandra 2.2.12 is installed by default in an API Gateway standard or Complete setup.{{< /alert >}}

### Install Cassandra in GUI mode

In GUI mode, to install Apache Cassandra only, use the steps described in [Installation](/docs/apim_installation/apigtw_install/installation) with the following selections:

* **Setup Type**: Select **Custom**.
* **Select Components**: Select **Cassandra**.
* **Cassandra configuration**: Enter your Cassandra **Installation Directory** and your **JRE Location**.

## Install Cassandra in unattended mode

To install Apache Cassandra using the API Gateway installer in unattended mode, follow the steps described in [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended).

The following command is an example of how to install Apache Cassandra in unattended mode on Linux:

```
./APIGateway_7.7_Install_linux-x86-32_BN<n>.run --mode unattended --setup_type advanced --enable-components cassandra --disable-components apigateway,qstart,policystudio,analytics,configurationstudio,apitester,apimgmt,packagedeploytools --cassandraInstalldir /opt/db/cassandra --cassandraJDK /opt/jre --startCassandra 0
```

### Keep Cassandra installation after API Gateway is uninstalled

To keep your Cassandra installation after API Gateway is uninstalled, you must ensure that you first install Cassandra only. For example, perform the following steps:

1. Run the API Gateway installer, and select Cassandra only.
2. Run the API Gateway installer, and select API Gateway components to install.

If API Gateway is uninstalled, Cassandra remains installed.

For more details on Apache Cassandra, see the following:

* [Start installation](http://cassandra.apache.org/)
* [Installation options](http://docs.datastax.com/en/cassandra/2.2/)
