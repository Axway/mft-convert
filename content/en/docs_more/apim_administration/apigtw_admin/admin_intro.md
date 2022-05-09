{
"title": "Introduction to API Gateway administration",
"linkTitle": "Introduction",
"weight":"10",
"date": "2019-10-14",
"description": "Introduces the main issues involved in API Gateway administration."
}

## API Gateway form factors

The API Gateway is available in software and hardware form factors. The available platforms are as follows:

* Software installation
* Container deployment in Docker containers

## Who owns the API Gateway platform and how is it administered?

The API Gateway platform is administered by the following groups:

* Operations—Runtime management of message traffic, logs and alerts, and high availability is performed by Operations staff.
* Architecture—Design-time policy definition, which determines the behavior of the API Gateway platform, is performed by Security Architects and Systems Architects.

### Operations team

Operations staff are responsible for making sure that the gateway platform is running correctly. They are concerned with the following problems:

* System status and health
* Network connectivity
* Security alerts
* System security
* Backups and recovery
* Maintenance of logs

The gateway platform provides a web-based console named API Gateway Manager that is dedicated to the Operations team. The API Gateway Manager includes the following features:

* Dashboard for monitoring overall system health and network topology.
* Real-time monitoring and metrics for all messages processed by the API Gateway.
* Traffic monitoring to quickly isolate failed or blocked message transactions and provide detailed information about each transaction, payload, and so on.
* API Gateway logs, trace, events, and alerts.
* Messaging based on Java Message System (JMS). This includes managing queues, topics, subscribers, consumers, and messages in Apache ActiveMQ.
* Dynamic system settings, user roles, and credentials.

As well as providing operational management functionality of its own, the API Gateway also interoperates with third-party network operations tools such as HP OpenView, BMC Control, and CA UniCenter. Finally, all functionality available in the API Gateway Manager is also available as a REST API.

### Architecture team

System architects and security architects have an overarching view of enterprise IT infrastructures, and so are more concerned with using the gateway to help integrate and secure existing enterprise systems. Architects wish to create API policies and integrate with third-party systems.

Policy Studio is a rich API Gateway policy development tool that enables architects and policy developers to create and control the gateway policies. You can use Policy Studio to visually define policy workflows in a drag-n-drop environment. This means that configuration is performed at the systems architecture level, without needing to write code.

## Where do you deploy an API Gateway?

API Gateways can be deployed in the Demilitarized Zone (DMZ) or in the Local Area Network (LAN) depending on policies or requirements, as shown in the following diagram:

![API Gateway network location](/Images/APIGateway/admin_gw_location.png)

The following guidelines help you to decide where to deploy the gateway:

* If you are processing only traffic from external sources, consider locating the gateway in the DMZ. If the gateway is also processing internal traffic, consider locating it in the LAN.
* If you are processing traffic internally and externally, a combination of gateways in the DMZ and internally on the LAN is considered best practice. The reason for this is that different policies should be applied to traffic depending on its origin.
* Both internal and external traffic should be checked for threats and to make sure that they contain the correct parameters for REST API requests, or correspond to Web service definitions.
* External traffic carries a greater potential risk and should be scanned by the gateway located in the DMZ to make sure that it does not in any way affect the performance of internal applications.
* Internal traffic and pre-scanned external traffic should then be processed by the gateway located in the LAN. This type of checking includes:
    * Checking API service level agreements and enforcing throttle threshold levels
    * Integration with a wide range of third-party systems
    * Web service standards support

## Where do you deploy API Gateway Analytics?

Although you can select the API Gateway Analytics component in the API Gateway installer, it is good practice to install API Gateway Analytics on a separate host from your gateway installations. You should ensure that API Gateway Analytics runs on a dedicated host, or on a host that is not a running an API Gateway instance or Node Manager.

You can deploy API Gateway Analytics on any supported host platform depending on its availability in your architecture.

{{< alert title="Note" color="primary" >}}API Gateway Analytics supports a range of databases for storing historic reports and metrics (for example, Oracle, DB2, MySQL, and Microsoft SQL Server). You should not install the database used for API Gateway Analytics in the DMZ. You should install this database in the LAN on a separate host from your API Gateway installations.{{< /alert >}}

You can secure the connection to the API Gateway Analytics database by dedicating it to one IP address. For more details on configuring the API Gateway Analytics database, see the [API Gateway Analytics User Guide](/docs/apimanager_analytics/).

## Secure the last mile

Securing the last mile refers to preventing internal users from directly accessing services without going through the gateway. This can be achieved in multiple ways. You should carefully choose which option is best for your use case, taking into account the security level you want to achieve, and the impact on performance the solution will have. You should choose from the following approaches:

* **Controlling traffic at the network level**: Services can only be accessed if the traffic is coming from pre-approved IP addresses. This is the simplest solution to put in place, is very secure, and has no impact on performance or existing applications.
* **Establishing a mutual SSL connection between API Gateways and services**: This solution is the easiest to put in place and has little to no impact on existing applications. However, it does have a non-negligible impact on latency.
* **Passing authentication tokens from API Gateways to back-end services**: This involves passing authentication tokens for WS-Security, Security Assertion Markup Language (SAML), and so on. This solution has a low impact on latency but requires some development because the target service container must validate the presence and the contents of the token.

For more details on configuring mutual SSL, and configuring WS-Security and SAML authentication tokens, see the [API Gateway Policy Developer Guide](/docs/apim_policydev/).

## API Gateway administration lifecycle

The main stages in the overall API Gateway administration lifecycle are as follows:

1. Planning an API Gateway system. This is described in [Plan an API Gateway system](/docs/apim_administration/apigtw_admin/admin_planning/).
2. Installing API Gateway components. See the [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/).
3. Configuring a domain. This is described in [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway/).
4. Operating and managing the API Gateway. This is described in the rest of this guide.
5. Upgrading an API Gateway. See the [API Gateway Upgrade Guide](/docs/apim_installation/apigw_upgrade/).

## Interaction with existing infrastructure

As part of API Gateway policy execution, the gateway needs to interact with various components of the existing infrastructure. For example, these include external connections to systems such as databases, LDAP servers, third-party identity management products, and so on.

### Databases

The gateway interoperates with a range of commonly used databases for a wide variety of purposes. For example, this includes managing access control credentials, authorization attributes, identity and role information, monitoring metrics, and reporting. The gateway uses Java Database Connectivity (JDBC) to manage database connections. The following databases are supported:

* MySQL
* Oracle
* DB2
* Microsoft SQL Server
* MariaDB

API Gateway also supports Apache Cassandra for internal data storage. For details on supported database versions, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install/).

### Anti-virus

The gateway supports Anti-Virus (AV) scanning using virus scanning engines from third-party vendors such as McAfee, Sophos, and ClamAV. Depending on the engine, this scanning can happen in-process or remotely using a client SDK or Internet Content Adaptation Protocol (ICAP). It also takes message attachments, passes them to the AV scanner, and then acts on the decision. The following conditions apply:

* AV signature distribution and updates are performed using mechanisms of the anti-virus vendor.
* Licensing for the AV engine is performed through the AV vendor distribution channels.

### Operations and management

The gateway has a number of different options for operations and management:

* Detailed transaction logs can be sent to syslog (UDP), relational databases, or flat files. These contain detailed records of processed messages, their contents, how long processing took, and decisions taken during message processing. This type of logging can also include information alerts about policy execution failures and breached Service Level Agreements (SLAs), and information about critical events such as connection or disk failures. Logging levels can be controlled for a gateway or policy as a whole or on a filter-by-filter basis.
* Auditing information can be viewed in real time in the gateway Manager console, and can be pushed to your metrics database (for example, for monitoring in API Gateway Analytics, API Manager, or a third-party tool), or pushed to Embedded Analytics dashboards.
* The gateway Manager console is used to provide a current snapshot of how the gateway is behaving. For example, it displays how many messages are being processed, and what services are under the most load. The gateway Manager displays what is happening now on gateway instances, and can be viewed by pointing a browser at an Admin Node Manager. For more details, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service).
* Flexible alerts can be sent out to email, SNMP, OPSEC, syslog, Twitter, and Windows Event Log based on a condition being met in a policy. An example might be to email a service owner for every 1000 failures or to generate an alert if a service is processing more than 10000 messages per second. They can also be used to generate alerts on client usage.
* Service Level Agreement filters can be used to perform a statistical measure of the services quality of service. They are used to make sure that the amount of network connection errors, response times and server errors are below a certain threshold.

### Network firewalls

When deployed in a Demilitarized Zone (DMZ) or perimeter network, the API Gateway sits behind network firewalls. Network Address Translation (NAT) firewall functionality is used on the network firewall to provide the gateway with a publicly routable address in the DMZ. This allows the gateway to route traffic internally to a local IP address range. API Gateways may be dual-homed to pass messages between the DMZ and the internal trusted network.

The gateway can then perform security processing on the incoming message traffic. For example, this includes the following:

* Looking for known attack patterns.
* Checking the validity and structure of the message for anomalies.
* Looking for traffic patterns that suggest a Denial-of-Service (DoS) attack.

#### Advantages over traditional application firewalls

API Gateways have a number of advantages over traditional application firewalls for message processing:

* Can understand the structure of the traffic, and can detect subtle attack mechanisms such as entity expansion attacks and external reference attacks.
* Can consume information in the messages such as security and platform-specific tokens.
* Can use well understood standards such as JSON or XML Schema, JSON Path or XPath, WSDL, and OAuth to properly content filter the traffic.

If an attack or unusual traffic pattern is detected, the gateway can notify the firewall to block the traffic at source using OPSEC or some other notification mechanism.

#### Firewall modes

You can configure the gateway to act as a network endpoint or a network proxy, or both in tandem.

* Block unidentified traffic. This is the default setting. If there is no policy configured for this traffic, block it and, depending on configuration, raise an alert. In this way, rogue message traffic is detected and blocked.
* Pass unidentified traffic.

### Application servers

Application servers are the infrastructure alongside which API Gateways are most commonly deployed. For example, gateways are often deployed in production systems with IBM WebSphere, Oracle WebLogic Server, Microsoft Biztalk Server, Fiorano SOA Platform, TIBCO ActiveMatrix BusinessWorks, and many others.

Gateways interact with application servers in a number of modes, for example:
For increased integration with such application server systems, the gateway supports numerous transport protocols including HTTP, HTTPS, JMS, email (SMTP/POP), FTP, and so on.

* Intercepting application server traffic by acting as a proxy.
* Offloading processing handed over by the application server (for example, to offload message transformation, encryption, or signature validation).

### Enterprise Service Buses

API Gateways and Enterprise Service Buses (ESBs) have similar functionality and complement each other very well. Example ESB systems include Oracle Enterprise Service Bus, IBM WebSphere ESB, Progress Sonic ESB, Software AG WebMethods, and JBoss ESB. Gateways are primarily used to perform the following tasks:

* Protect ESBs and downstream systems from traffic surge, potential DoS attacks, and threats.
* Offload expensive operations such as message validation and cryptography operations from ESBs.

#### Similarities between API Gateways and ESBs

API Gateways and ESBs typically both perform the following tasks:

* Protocol mediation
* Message routing and transformation
* Service composition
* Message processing

#### Differences between API Gateways and ESBs

The main differences between API Gateways and ESBs are as follows:

API Gateways:

* can be used for simple composite services (chained), but do not support Business Process Execution Language (BPEL), and are not suitable for long duration composite services.
* are stateless and cannot maintain transaction state.
* are targeted at performance and application acceleration.
* have been designed to provide superior security capabilities, without impacting on performance.

ESBs are usually delivered with backend adapters for systems such as CICS, IMS, Siebel, or SAP.

### Directories and user stores

API Gateway supports a wide variety of user stores including LDAP, Active Directory, and access control products such as CA SiteMinder and Oracle Access Manager. User stores contain some of the most valuable information in an organization. For example, this includes private identity information such as phone numbers, addresses, email addresses, medical plan IDs, user names and passwords, certificates, organization structures, and so on. API Gateways must be able to interact with user stores without compromising them.

The API Gateway can use LDAP directories to retrieve user information such as the following:

* Authentication using username/password.
* Retrieve certificates and checking signatures.
* Authorization of clients based on attribute values.
* Retrieval of attributes for placing into SAML assertions.
* Checking certificate validity using Certificate Revocation Lists (CRL) retrieved from user stores.

#### Simple inline user store deployment

The following diagram shows a simple inline user store deployment:

![Inline user store deployment (DMZ)](/Images/APIGateway/admin_inline_user_store.png)

**Advantages**:

This architecture has the following advantages:

* This is simple and easy to set up.
* There is a single entry point through the DMZ for all message traffic.
* This is the least expensive option.

**Disadvantages**:

This architecture has the following disadvantages:

* Exposing important user information in the DMZ is a potential security risk.
* The LDAP server is only being used for external traffic.

### API Gateway in DMZ—LDAP in LAN

This is a very common setup where the API Gateway is located in the DMZ, and where it communicates with a user store located in the LAN:

![User store in LAN](/Images/APIGateway/admin_internal_user_store.png)

**Advantages**:

This architecture has the following advantages:

* The user store is protected from external access.
* This option is only moderately expensive.

**Disadvantages**:

This architecture has the following disadvantages:

* Administrators must maintain two entry points into the LAN from the DMZ for a single application.
* The user store is addressable from the DMZ, which is contrary to the security policies enforced by many organizations.

#### Split deployment between DMZ and LAN

In this scenario, two API Gateways are used to split the security checks across the DMZ and the LAN. For example, threats and JSON and XML schema validity are performed in the DMZ, while authentication and/or authorization is performed in the LAN:

![User store split deployment](/Images/APIGateway/admin_split_user_store.png)

**Advantages**:

This architecture has the following advantage:

* This is the most secure deployment available.
* By separating threats from access control, it is easy to run separate security policies for internal and external traffic.

**Disadvantages**:

This architecture has the following disadvantages:

* This is a more expensive option involving multiple API Gateways.

### Access control

API Gateway interoperates with third-party Access Control and Identity Management products at a number of different levels. For example:

![Example access control options](/Images/APIGateway/admin_access_control.png)

The options shown in the diagram are described as follows:

* API Gateway can directly connect into the Identity Management system as an agent. This solution is currently available for third-party products such as Oracle Access Manager, Oracle Entitlements Server, RSA Access Manager, CA SiteMinder, and IBM Tivoli Access Manager. The Identity Management policy is defined in the Identity Management product to which the gateway delegates the authentication and authorization.
* API Gateway can connect to the Identity Manager using the XML Access Control Markup Language (XACML) and Security Assertion Markup Language (SAML) protocols. API Gateway can request an authorization decision from a SAML Policy Decision Point (PDP) for an authenticated client using the SAML Protocol (SAMLP), which is defined in XACML. In such cases, the API Gateway presents evidence to the PDP in the form of user credentials, such as the Distinguished Name of a client X.509 certificate, or a username/password combination.
* The PDP decides whether a user is authorized to access the requested resource. It then creates an authorization assertion, signs it, and returns it to the gateway in a SAMLP response. The API Gateway can then perform a number of checks on the response, such as validating the PDP signature and certificate, and examining the assertion. It can also insert the SAML authorization assertion into the message for consumption by a downstream service. This enables propagation of the access control decision to occur.

### Public Key Infrastructure

API Gateway supports Secure Sockets Layer (SSL) and Transport Layer Security (TLS) for transport-based authentication, encryption, and integrity checks. The gateway can interact with Public Key Infrastructure (PKI) systems in the following ways: Certificates and keys can be stored in the gateway keystore, or in a network or an optional Hardware Security Module (HSM). For more details, see [Manage certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates/).

* Connecting to Online Certificate Status Protocol (OCSP) and XML Key Management Specification (XKMS) to query certificate status online.
* Using a Certificate Revocation List (CRL) retrieved from a directory, file, or LDAP to check certificate status.
* Checking a certificate chain.

### Registries and repositories

API Gateway includes the following support for service registries and repositories:

* Architects and policy developers can use Policy Studio to pull Web service definitions (WSDL) from UDDI directories or HTTP-based repositories. These WSDL files are then used to generate the required security policies. The gateway can update UDDI registries with updated WSDL files or can serve them directly to the client.
* When the API Manager product is installed, architects and policy developers can use Policy Studio to register REST APIs with the gateway. API administrators can then use API Manager to make these APIs available to client application developers in a Client Application Registry. For more details, see the [API Manager User Guide](/docs/apim_administration/apimgr_admin/).

### Software Configuration Management

The API Gateway does not include its own built-in Software Configuration Management or Source Code Management (SCM) features. It is recommended that you use your existing SCM tools to manage your gateway configuration packages (`.fed`, `.pol`, and `.env` files) and API Gateway policy or general configuration exports (`.xml` files). For example, commonly used source code management tools include Concurrent Version System (CVS), Subversion (SVN), and Git.

In addition, you can use the properties metadata associated with the gateway configuration packages (`.fed`, `.pol`, and `.env` files) to associate version information with the packages that are provided to upstream environments. You can also use the gateway Compare/Merge features to determine changes between different configuration packages.

For more details on API Gateway configuration packages and their properties metadata, see the [API Gateway DevOps Deployment Guide](/docs/apigtw_devops/).
