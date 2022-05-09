{
"title": "Plan an API Gateway system",
"linkTitle": "Plan an API Gateway system",
"weight":"20",
"date": "2019-10-14",
"description": "Discover the most important factors to look at when architecting an API Gateway deployment."
}

One of the most important tasks when deploying an API Gateway system is confirming that the system is fit for purpose. Enterprise software systems are hugely valuable to the overall success of the business operation. For any organization, there are many implications of system downtime with important consequences to contend with.

## Policy development

The functional characteristics of any given policy run by an API Gateway can have a huge effect on the overall system throughput and latency times. Depending on the purpose of a particular policy, the demand on valuable processing power will vary. The following guidelines apply in terms of processing power:

* Threat analysis and transport-based authentication tasks are relatively undemanding.
* XML processing such as XML Schema and WS-Security user name/password authentication are slightly more intensive.
* Calling out to third-party systems is expensive due to network latency.
* Cryptographic operations like encryption and signing are processor intensive.

The key point is that API Gateway policy performance depends on the underlying requirements, and customers should test their policies before deploying them into a production environment.

### Policy development guidelines

Architects and policy developers should adhere to the following guidelines when developing API Gateway policies:

* Decide what type of policy you need to process your message traffic. Think in terms of functional requirements instead of technologies. Axway can help you to map the technologies to the requirements. Example functional requirements include the following:
    * Only trusted clients should be allowed send messages into the network.
    * An evidential audit trail should be kept.
* Think about what you already have in your architecture that could help to achieve these aims. Examples include LDAP directories, databases that already have replication strategies in place, and network monitoring tools.
* Create a policy to match these requirements and test its performance. Axway provides an integrated performance testing tool (API Tester) to help you with this process.
* Use the API Gateway Manager, API Gateway Analytics, Embedded Analytics, or third-party monitoring consoles to help identify what the bottlenecks are in your system. If part of the solution is slowing the overall system, try to find alternatives to meet your requirements.
* Test the performance capability of the back-end services.

### Example policy requirements

Supplier A is creating a service that will accept Purchase Order (PO) documents from customers. The PO documents are formatted using XML. The functional requirements are:

* The service should not accept anything that will damage the PO system.
* Incoming messages must to be authenticated against a customer database to make sure they come from a valid customer account.
* The supplier already has an LDAP directory and would like to use it to store the customer accounts.
* The supplier must be able prove that the message came from the customer.

These requirements can be achieved using a policy that includes processing for Threatening Content and checking the XML Signature, which verifies the certificate against the LDAP directory.

## Traffic analysis

In the real world, messages do not arrive in a continuous stream with a fixed size like a lab-based performance test. Message traffic distribution has a major impact on system performance. Some of the questions that need to be answered are as follows:

* Is the traffic smooth or does it arrive in bursts?
* Are the messages all of the same size? If not, what is the size distribution?
* Is the traffic spread out over 24 hours or only during the work day?

### Traffic analysis guidelines

You should adhere to the following guidelines when analyzing message traffic:

* Use the **Traffic** tab in the API Gateway Manager web console to analyze message traffic. For more details, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service).
* Use the [API Gateway Analytics](/docs/apimanager_analytics/) web console to analyze historical message traffic.
* Use the [Embedded Analytics](https://docs.axway.com/bundle/EmbeddedAnalyticsAPIM_allOS_en_HTML5/) web dashboards to analyze API, infrastructure, and client application health and API usage.
* Take traffic distribution into account when calculating performance requirements.
* Take message size distribution into account when running performance tests.
* If traffic bursts cause problems for service producers, consider using the API Gateway to smooth the traffic (for example, using the **Throttling** filter).

## Load balancing and scalability

The API Gateway scales well both horizontally and vertically. Customers can scale horizontally by adding more API Gateways to a cluster and load balancing across it using a standard load balancer. API Gateways being load balanced run the same configuration to virtualize the same APIs and execute the same policies. If multiple API Gateway groups are deployed, load balancing should be across groups also.

For example, the following diagram shows load balancing across two groups of API Gateways deployed on two hosts:

![Group-based load balancing](/Images/APIGateway/lb_groups.png)

The API Gateway imposes no special requirements on load balancers. Loads are balanced on a number of characteristics including the response time or system load. The execution of API Gateway policies is stateless, and the route through which a message takes on a particular system has no bearing on its processing. Some items such as caches and counters are held on a distributed cache, which is updated on a per message basis. As a result, API Gateways can operate successfully in both sticky and non-sticky modes.

The distributed state poses a number of questions in terms of active/active and active/passive clustering. For example, if the counter and cache state is important, you must design your overall system so that at least one API Gateway is active at all times. This means that for a resilient HA system, a minimum of at least two active API Gateways at any one time, with a third and fourth in passive mode is recommended.

The API Gateway ensures zero downtime by implementing configuration deployment in a rolling fashion. For example, while each API Gateway instance in the cluster or group takes a few seconds to update its configuration, it stops serving new requests, but all existing in-flight requests are honored. Meanwhile, the rest of the cluster or group can still receive new requests. The load balancer ensures that requests are pushed to the nodes that are still receiving requests.

### Load balancing guidelines

Axway recommends the following guidelines for load balancing:

* Use the Admin Node Manager and API Gateway groups to maintain the same policies on load-balanced API Gateways.
* Configure alerts to identify when gateways and back-end services are approaching maximum capacity and need to be scaled.
* Use the API Gateway Manager console to see which parts of the system are processing the most traffic.

## SSL termination

Secure Socket Layer (SSL) connections can be terminated at the load balancer or API Gateway level. These options are described as follows:

* **SSL connection terminated at load balancer**:
    * The SSL certificate and associated private key are deployed on the load balancer, and not on the gateway. The subject name in the SSL certificate is the fully qualified  domain name (FQDN) of the server (for example, `axway.com`).
    * The traffic between the load balancer can be in the clear or over a new SSL connection. The disadvantage of a new SSL connection is that it puts additional processing load on  the load balancer (SSL termination and SSL establishment).
    * If mutual (two-way) SSL is used, the load balancer can insert the client certificate into the HTTP header. For example, the F5 load balancer can insert the entire client  certificate in `.pem` format as a multi-line HTTP header named `XClient-Cert` into the incoming HTTP request. It sends this header to theAPI Gateway, which uses it for  validation and authentication.

* **Load balancer configured for SSL pass-through, all traffic passed to API Gateway**:

    With SSL pass-through, the traffic is encrypted so the load balancer cannot make any layer seven decisions (for example, if HTTP 500 is returned by the gateway, route to the HA gateway) To avoid this problem, you can configure the gateway so that it closes external ports on defined error conditions. In this way, the load balancer is alerted to switch to the HA gateway.

## High Availability and failover

API Gateways are used in high value systems, and customers typically deploy them in High Availability (HA) mode to protect their investments. The API Gateway architecture enables this process as follows:

* The Admin Node Manager is the central administration server responsible for performing all management operations across an API Gateway domain. It provides policy synchronization by ensuring that all API Gateways in an HA cluster have the same policy versions and configuration.
* API Gateway instances are stateless by nature. No session data is created, and therefore there is no need to replicate session state across API Gateways. However, gateways can maintain cached data, which can be replicated using a peer-to-peer relationship across a cluster of API Gateways.
* API Gateway instances are usually deployed behind standard load balancers which periodically query the state of the API Gateway. If a problem occurs, the load balancer redirects traffic to the hot stand-by machine.
* If an event or alert is triggered, the issue can be identified using API Gateway Manager, API Gateway Analytics,Embedded Analytics, or third-party monitoring consoles, and the active gateway can then be repaired.

![High availability and failover](/Images/APIGateway/admin_ha.png)

## HA stand-by systems

High Availability can be maintained using hot, cold, or warm stand-by systems. These are described as follows:

* **Cold stand-by**: System is turned off.
* **Warm (passive) stand-by**: System is operational but not containing state.
* **Hot (active) stand-by**: System is fully operational and with current system state.

### HA and failover guidelines

Axway recommends the following guidelines for HA stand-by systems:

* For maximum availability, use an API Gateway in hot stand-by for each production gateway.
* Use gateways to protect against malicious attacks that undermine availability.
* Limit traffic to back-end services to protect against message flooding. This is particularly important with legacy systems that have been recently service-enabled. Legacy systems may not have been designed for the traffic patterns to which they are now subjected.
* Monitor the network infrastructure carefully to identify issues early. You can do this using API Gateway Manager, API Gateway Analytics, Embedded Analytics, or third-party monitoring consoles. Interfaces are also provided to standard monitoring tools such as syslog and Simple Network Management Protocol (SNMP).

## Backup and recovery

Most customers have a requirement to keep a mirrored backup and disaster recovery site with full capacity to be able to recover from any major incidents. These systems are typically kept in a separate physical location on cold stand-by until the need arises for them to be brought into action.

### Disaster recovery guidelines

The following applies for backup and recovery:

* The backup and disaster site must be a full replica of the production site (for example, with the same number of API Gateway instances).
* The Admin Node Manager helps get backup and recovery sites up and running fast by synchronizing the API Gateway policies in the backup solution.
* Remember to also include any third-party systems in backup and recovery solutions.

For more details, see [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations#api-gateway-backup-and-disaster-recovery).

## Development staging and testing

The most common reason for system downtime is change. Customers successfully alleviate this problem through effective change management as part of a mature software development lifecycle. A software development lifecycle controls change by gradually pushing it through a series of stages until it reaches production.

Each customer will have their own approach to staging depending on the value of the service and the importance of the data. Staging can be broken into a number of different milestones. Each milestone is intended to isolate a specific type of issue that could lead to system downtime. For example:

* The development stage is where the policy and service are created.
* Functional testing makes sure the system works as intended.
* Performance testing makes sure the system meets performance requirements.
* System testing makes sure the changes to the system do not adversely affect other parts.

In some cases, each stage is managed by a different group. The number of API Gateways depends on the number of stages and requirements of each of these stages. The following diagram shows a typical environment topology that includes separate API Gateway domains for each environment:

![API Gateway environment topology](/Images/APIGateway/topology.png)

### Staging and testing guidelines

The following guidelines apply to development staging and testing:

* Use API Gateway configuration packages (`.fed`, `.pol`, and `.env`) to control the migration of policies from development through to production.
* Keep an audit trail of all system changes.
* Have a plan in place to roll back quickly in the event of a problem occurring.
* Test all systems and policy updates before promoting them to production.
* Test High Availability and resiliency before going into production.

## Hardening—secure the API Gateway

The API Gateway platform is SSL-enabled by default, so you do not need to SSL-enable each gateway component. However, in a production environment, you must take additional precautions to ensure that the gateway environment is secure from external and internal threats.

### Hardening guidelines

The following guidelines apply to securing your API Gateway:

* You must change all default passwords (for example, change the password for Policy Studio and API Gateway Manager). For more details, see [Manage users](/docs/apim_administration/apigtw_admin/manage_user_access).
* The default X.509 certificates used to secure the gateway components are self-signed (for example, the certificate used by the API Gateway Manager on port `8090`). You can replace these self-signed certificates with certificates issued by a Certificate Authority (CA). For more details, see [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr).
* By default, the API Gateway management ports bind on all HTTP interfaces (IP address set to `*`). You can change these ports or restrict them to bind on specific IP addresses instead.
* By default, API Gateway configuration is unencrypted. You can specify a passphrase to encrypt API Gateway instance configuration. For more details, see [Configure an API Gateway encryption passphrase](/docs/apim_administration/apigtw_admin/general_passphrase).
* You can configure user access at the following levels:
    * Policy Studio, API Gateway Manager, and Configuration Studio users; API Gateway users. See [Manage users](/docs/apim_administration/apigtw_admin/manage_user_access)
    * Role-based access. See [Configure Role-Based Access Control (RBAC)](/docs/apim_administration/apigtw_admin/general_rbac).

## Capacity planning example

The following is an example formula for deciding how big your API Gateway deployment will need to be. This example is purely intended for illustration purposes only:

```
numberOfGateways = (ceiling (requiredThroughput x factorOfSafety / testedPerformance) x HA) x (1 + numDR) + staging + eng))
```

This formula is described as follows:

* `factorOfSafety` has a value of 2 for normal.
* Typically High Availability (`HA`) has a value of 2
* `ceiling` implies that the number is rounded up. It is very important that this is not under provisioned by accident.
* Typically customers have a single backup and disaster recovery site (`numDR`=1).
* There is a large variation in performance between the gateways depending on the policy deployed so it important to performance test policies prior to final capacity planning.
* `staging` is the amount of stage testing licenses. Customers with very demanding applications should have a mirror of their production system. This is the only way to be certain.

### Example required throughput

For example, a software system is being designed that will connect an automobile manufacturer's purchasing system with a supplier. The system is designed to meet the most stringent load and availability requirements. An API Gateway system is being used to verify the integrity of the purchase orders.

The system as a whole is required to process 10,000,000 transactions per day. Each of these transactions will result in 8 individual request/responses. 90% of the load will happen between 9 am in the morning and 5 pm in the evening. The customer wishes to have a factor of safety of 2 for load spikes.

```
Overall throughput = (8 x 10,000,000 x 0.9)/8/60/60= 2,500 transactions per second (tps)
```

### Example development process

The development process for the example system should be as follows:

1. A Signature Verification policy is developed by one of the system engineers using a software license on his PC. When he is satisfied with the policy, he pushes it forward to the test environment.
2. Test engineers deploy the policy to the interoperability testing environment. They make sure that all of the systems work correctly together, and test the system as a whole. The API Gateway must show it can process the signature and integrate with the service back-end and PKI systems. When the system as a whole is working correctly, the testing moves to system testing.
3. System testing will prove that the system as a whole is reliable and has the capacity to meet the performance requirements. It will also make sure that the system interoperates with monitoring software and behaves properly during failover and recovery. During system test the API Gateway is found to be capable of processing 1,000 messages per second for this policy.
4. The customer will have the following infrastructure:
    * One backup and disaster site which is a full replica of the production site
    * A full HA system on hot standby
    * Two gateways for testing on staging
    * A factor of safety on the load of 2
5. The total number of gateways across all environments (development, staging, production) should be as follows:

    ```
    ((ceiling(2500/1000)*2)*2)*(1+1)+2+1 = 27
    ```

6. The resulting architecture provisioned should be 27 API Gateways made up of the following:
    * 12 production licenses (6 live, 6 standby)
    * 12 backup and disaster recovery licenses
    * 2 staging licenses
    * 1 engineering license
