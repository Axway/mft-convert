{
"title": "Configure API Gateway high availability",
"linkTitle": "Configure API Gateway HA",
"weight":"38",
"date": "2019-10-15",
"description": "Recommended architecture for an API Gateway HA production environment."
}

System administrators can configure High Availability (HA) in the API Gateway environment to ensure that there is no single of point of failure in the system. This helps to eliminate any potential system downtime in production environments. Typically, the API Gateway platform is deployed in the Demilitarized Zone (DMZ) to provide an additional layer of security for your back-end systems.

This section includes recommendations on topics such as load balancing, commonly used transport protocols, caching, persistence, and connections to external systems.

## HA in production environments

The following diagram shows an overview of an API Gateway platform running in an HA production environment:

![API Gateway High Availability](/Images/APIGateway/admin_ha_config.png)

The architecture shown in the diagram is described as follows:

* An external client application makes inbound calls sending business traffic over a specific message transport protocol (for example, HTTP, JMS, or FTP) to a load balancer.
* A standard third-party load balancer performs a health check on each gateway instance, and distributes the message load to the listening port on each API Gateway instance (default is `8080`).
* Each gateway instance has External Connections to third-party systems. For example, these include databases such as Oracle and MySQL, and Authentication Repositories such as CA SiteMinder, Oracle Access Manager, Lightweight Directory Access Protocol (LDAP) servers, and so on.
* Caching is replicated between each API Gateway instance using a distributed caching system based on Ehcache.
* Each gateway instance has Remote Host interfaces that specify outbound connections to back-end API systems, and which can balance the message load based on specified priorities for Remote Hosts.
* Each gateway instance connects to an external Apache Cassandra database used by certain features for persistent data storage, and which has its own HA capabilities.
* Each gateway instance contains an embedded Apache ActiveMQ messaging system, which can be configured for HA in a shared file system.
* Each back-end API is also replicated to ensure there is no single point of failure at the server level.
* Management traffic used by the Admin Node Manager, API Gateway Manager, and Policy Studio is handled separately on different port (default is `8090`).

{{< alert title="Note" color="primary" >}}For simplicity, the diagram shows two gateway instances only. However, for a resilient HA system, a minimum of at least two active gateway instances at any time, with a third and fourth in passive mode, is recommended.{{< /alert >}}

## Load Balancing

In this HA production environment, the load balancer performs a health check and distributes message load between gateway instances. The API Gateway supports a wide range of standard third-party load balancing products (for example, F5) without any special requirements. Multiple gateway instances can be deployed active/active (hot standby) or active/passive (cold standby) modes as required.

The load balancer polls each gateway instance at regular intervals to perform a health check on the message traffic port (default `8080`). The load balancer calls the API Gateway Health Check policy, available on the following default URL:

```
http://GATEWAY_HOST:8080/healthcheck
```

The health check returns a simple `<status>ok</status>` message when successful. In this way, if one gateway instance becomes unavailable, the load balancer can route traffic to an alternative gateway instance.

Both transparent load balancing and non-transparent load balancing are supported. For example, in transparent load balancing, the gateway can see that incoming messages are sent from specific client and load balancer IP addresses. The gateway can also extract specific client details from the HTTP header as required (for example, the SSL certificate, user credentials, or IP address for Mutual or 2-Way SSL Authentication). In non-transparent load balancing, the gateway sees only the virtual service address of the load balancer.

In addition, API Gateway can also act as load balancer on the outbound connection to the back-end APIs. For more details, see [Remote Hosts](#remote-hosts).

## Java Message System

API Gateway supports integration with a wide range of third-party Java Message System (JMS) products. For example, these include the following:

* IBM WebSphere MQ
* Progress SonicMQ
* Tibco Rendezvous
* Fiorano
* OpenJMS
* JBoss Messaging

API Gateway can act as a JMS client (for example, polling messages from third-party JMS products or sending message to them). For details on configuring the gateway client connections to JMS systems, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/). For details on configuring HA in supported third-party JMS systems, see the user documentation available from your JMS provider.

API Gateway also provides an embedded Apache ActiveMQ server in each API Gateway instance. For more details, see [Embedded Apache ActiveMQ](#embedded-apache-activemq).

## File Transfer Protocol

API Gateway supports the following protocols:

* File Transfer Protocol (FTP)
* FTP over SSL (FTPS)
* Secure Shell FTP (SFTP)

When using FTP protocols, the gateway writes to a specified directory in your filesystem. In HA environments, when the uploaded data is required for subsequent processing, you should ensure that the filesystem is shared across your API Gateway instances—for example, using Storage Area Network (SAN) or Network File System (NFSv4).

## Remote Hosts

You can use the **Remote Host Settings** in Policy Studio to configure how API Gateway connects to a specific external server or routing destination. For example, typical use cases for configuring Remote Hosts with the gateway are as follows:

* Mapping a host name to a specific IP address or addresses (for example, if a DNS server is unreliable or unavailable), and ranking hosts in order of priority.
* Specify load balancing settings (for example, whether load balancing is performed on a simple round-robin basis or weighted by response time).
* Allowing the gateway to send HTTP 1.1 requests to a destination server when that server supports HTTP 1.1, or resolving inconsistencies in how the destination server supports HTTP.
* Setting timeouts, session cache size, input/output buffer size, and other connection-specific settings for a destination server. For example, if the destination server is particularly slow, you can set a longer timeout.

## Distributed caching

In an HA production environment, caching is replicated between each gateway instance using a distributed caching system. In this scenario, each gateway has its own local copy of the cache, but registers a cache event listener that replicates messages to the caches on other gateway instances. This enables the put, remove, expiry, and delete events on a single cache to be replicated across all other caches.

In the distributed cache, there is no master cache controlling all caches in the group. Instead, each cache is a peer in the group that needs to know where all the other peers in the group are located. Peer discovery and peer listeners are two essential parts of any distributed cache system.

For more details on configuring distributed cache settings, see [Configure caching](/docs/apim_policydev/apigw_poldev/general_cache/). API Gateway distributed caching system is based on Ehcache. For more details, see <http://ehcache.org/>.

## External Connections

You can use **External Connections** settings in Policy Studio to configure how the gateway connects to specific external third-party systems. For example, this includes connections such as the following:

* Authentication Repositories (LDAP, CA SiteMinder, Oracle Access Manager, and so on)
* Databases (Oracle, DB2, MySQL, and MS SQL Server)
* JMS Services
* SMTP Servers
* ICAP Servers
* Kerberos
* Tibco
* Tivoli
* Radius Clients

For details on how to configure **External Connections** in Policy Studio, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/). For details on how to configure HA for any external third-party systems, see the product documentation provided by your third-party vendor.

{{< alert title="Note" color="primary" >}}When configuring connections to server hosts, it is recommended that you specify server host names instead of IP addresses, which are less stable and are subject to change in a Domain Name System (DNS) environment.{{< /alert >}}

## External Apache Cassandra database

You can configure a gateway instance to use an external Apache Cassandra database for persistent data storage. This Cassandra database is required by the following gateway features:

* API Manager
* OAuth tokens and codes
* Client Application Registry
* API Keys
* KPS collections that you create

If your gateway system uses any of these features, you must configure an external Apache Cassandra database for HA. All API Gateways in a group can share the same external Cassandra data source. In a production environment, you must configure the gateway group to use the same Cassandra data source to provide HA.

All nodes in a Cassandra cluster must run on the same operating system.

For more details on installing and configuring an external Cassandra database for HA, see the [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install/).

## Embedded Apache ActiveMQ

API Gateway provides an embedded Apache ActiveMQ broker in each gateway instance. In a HA production environment, multiple ActiveMQ brokers can work together as a network of brokers in a group of API Gateways. This requires setting up a shared directory that is accessible from all gateway instances—for example, using Storage Area Network (SAN) or Network File System (NFSv4).

When setting up a shared directory, do not use OCFS2, NFSv3, or other software where exclusive file locks do not work reliably. For details, see [Apache ActiveMQ Shared File System Master Slave](http://activemq.apache.org/shared-file-system-master-slave.html).

In this shared network of ActiveMQ brokers, each API Gateway can start a local embedded ActiveMQ broker, which listens on a configured TCP port (`61616` by default). This port is accessible from both the local gateway and remote clients using the load balancer.

For details on how to configure and manage embedded Apache ActiveMQ, see the following:

* [Embedded ActiveMQ settings](/docs/apim_reference/general_activemq_settings)
* [Manage embedded ActiveMQ messaging](/docs/apim_administration/apigtw_admin/admin_messaging)

For more details on Apache ActiveMQ, see <http://activemq.apache.org/>.

## Admin Node Manager high availability

API Gateway provides an active/active high availability solution for the Admin Node Manager that supports multiple DMZ deployment patterns. Multiple Admin Node Managers in a domain are supported in an active/active configuration, in which each Admin Node Manager can perform management operations with the same shared topology and configuration. This supports the following DMZ deployment patterns:

* All nodes in the DMZ run Admin Node Managers
* Only nodes behind the internal firewall run Admin Node Managers
* DMZ has multiple zones, and only nodes behind the firewall run Admin Node Managers

You must configure at least two Admin Node Managers in a domain for high availability and security. For details on how to configure and secure multiple Admin Node Managers in a domain, see [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr).

**Scenario 1—Admin Node Managers in DMZ**:

In this deployment pattern, all nodes in the DMZ run an Admin Node Manager in active/active mode. For example:

![Admin Node Managers in DMZ](/Images/APIGateway/admin_node_mngr_ha1.png)

Multiple gateway instances are deployed on separate nodes in the DMZ, and all nodes can communicate with each other. Each node runs an Admin Node Manager in an active/active configuration, and there are no Node Managers. This means that the API Gateway management infrastructure is deployed in the DMZ.

**Scenario 2—Admin Node Managers behind firewall**:

In this deployment pattern, only nodes behind the internal firewall run Admin Node Managers in active/active mode. For example:

![Admin Node Managers behind firewall](/Images/APIGateway/admin_node_mngr_ha2.png)

Similar to scenario 1, multiple gateway instances are deployed on separate nodes in the DMZ, and all nodes can communicate with each other. However, the management infrastructure cannot be deployed in the DMZ, and must be deployed behind the internal firewall.

All nodes in the DMZ run Node Managers instead. There are two nodes deployed behind the internal firewall running Admin Node Managers in an active/active configuration. Both Admin Node Managers can manage any Node Manager in the DMZ.

**Scenario 3—Multi-zoned DMZ with Admin Node Managers behind firewall**:

In this deployment pattern, all nodes in the DMZ run an Admin Node Manager in active/active mode. For example:

![Multi-zoned DMZ with Admin Node Managers in DMZ](/Images/APIGateway/admin_node_mngr_ha3.png)

This deployment pattern is a refinement of scenario 2. The DMZ is divided into multiple zones with no inter-zone communication. Multiple gateway instances are deployed on separate nodes in each zone, and all nodes in the zone can communicate with each other.

All nodes in the DMZ run Node Managers. There are two nodes deployed behind the internal firewall running Admin Node Managers in an active/active configuration. Both Admin Node Managers can manage any Node Manager in any zone.
