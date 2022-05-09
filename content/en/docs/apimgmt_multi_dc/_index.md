{
"title": "Configure API Manager in multiple datacenters",
"linkTitle": "Configure API Manager in multi-DC",
"weight":"40",
"no_list": "true",
"date": "2019-10-02",
"description": "Recommended multi-datacenter configuration that applies to the various types of API Management data in storage. For each data type, it describes how data is replicated across the datacenter, the recommended configuration, and expected behavior in case of failover."
}

## Multi-datacenter deployment

This section describes the infrastructure required for API Management multi-datacenter deployment, and the various types of API Management data. For example, this includes API catalog, client registry, OAuth tokens, quota, Key Property Store (KPS), and so on. It also describes where the data is stored. For example, this includes files on disk, Apache Cassandra database, Ehcache, or Relational Database Management System (RDBMS). For details on supported database versions, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements).

### Multi-datacenter deployment architecture

The following diagram shows the minimum infrastructure required for API Management multi-datacenter deployment.

![Multi-datacenter overview](/Images/APIGateway/multi-dc_overview.png)

This deployment architecture is described as follows:

* Each datacenter includes the same components deployed in active/active mode. Each datacenter can handle all of the traffic load and can scale in the same way. The API Gateway instances and Cassandra nodes in each datacenter are all highly available.
* API Gateway group configuration is shared across both datacenters. This means that all API Gateway instances in a group are managed as a single unit, and run the same configuration to virtualize the same APIs and execute the same policies.
* You must have at least two API Gateway instances per datacenter, at least one of which is an Admin Node Manager. However, you can configure multiple Admin Node Managers per datacenter for high availability.
* The API Gateway group in the internal zone is responsible for API management. The Admin Node Manager is the central administration server responsible for all management operations. For example, this enables API Gateway administrators to perform monitoring in the API Gateway Manager web console.
* The API Gateway group in the internal zone also hosts the API Manager web console used by API administrators.
* The API Gateway group in the outer DMZ is responsible for securing traffic. The Node Manager manages local API Gateways instances on that host only.
* The Cassandra database cluster is required to store data for API Manager or API Gateway client registry (API key or OAuth). You can use Cassandra as an option to store custom KPS data and OAuth tokens. You can also use an RDBMS to store custom KPS, OAuth tokens, or metrics for API Manager or API Gateway Analytics.
* Caching is replicated between API Gateway instances using the Ehcache distributed caching system. For more details, see [Global distributed cache settings](/docs/apim_policydev/apigw_poldev/general_cache/#global-distributed-cache-settings).

### API Management data storage

This section describes what API Management data can be persisted and where.

#### API Gateway data

* **API Gateway configuration**: files are stored on disk.

    * API Gateway instance: `INSTALL_DIR/apigateway/groups/group-n/instance-n/conf/fed`
    * Node Manager/Admin Node Manager: `INSTALL_DIR/apigateway/conf/fed`
  
    Alternatively, you can use a deployment archive (`.fed` file).

* **API Gateway logs**: Files are stored on disk.

    * API Gateway instance: `INSTALL_DIR/apigateway/groups/group-n/instance-n/logs`
    * Node Manager/Admin Node Manager: `INSTALL_DIR/apigateway/logs`

* **API Gateway traffic monitoring**: files are stored on disk.

    * API Gateway instance: `INSTALL_DIR/apigateway/groups/group-n/instance-n/conf/opsdb.d`
    * Node Manager/Admin Node Manager: `INSTALL_DIR/apigateway/conf/opsdb.d`

* **API Gateway KPS custom tables**: Files are stored on Cassandra or RDBMS.

* **API Gateway OAuth token store**: Files are stored on Ehcache, Cassandra, or RDBMS.

* **API Gateway throttling counters**: Files are stored onEhcache.

* **API Gateway custom cache**: Files are stored on Ehcache.

#### API Manager data

* **API Manager catalog, client registry, web-based settings**: Files are stored on Cassandra.
* **API Manager quota counters**: Files are stored in memory, Cassandra, or RDBMS.
* **API Manager metrics**: Files are stored on RDBMS.
