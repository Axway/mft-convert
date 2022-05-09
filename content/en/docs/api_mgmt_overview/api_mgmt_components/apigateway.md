{
"title": "Introduction to API Gateway",
"linkTitle": "API Gateway",
"weight":"20",
"date": "2020-04-14",
"description": "API Gateway manages, delivers, and secures enterprise APIs, applications, and consumers."
}

## API Gateway is core infrastructure

API Gateway does for APIs what the application server does for applications. This API Gateway role as core application infrastructure is shown as follows:

![API Gateway core application infrastructure](/Images/docbook/images/concepts/api_server_core_app.png)

The API Gateway can be seen as the API runtime environment, which provides core services such as the following:

* Security (for example, authentication and authorization)
* Connectivity with a range of different protocols
* Virtualization
* Scalability and elasticity
* High availability
* Manageability (for example, using API Gateway Manager)
* Development simplicity

Because the API Gateway provides this core API infrastructure, developers can focus on providing the application logic. They no longer need to build these services into their application, and can leverage the core infrastructure provided by the API Gateway.

Previously, the API was not treated as a first class citizen, and in many cases was part of the application interface. However, the API Gateway sees the API as a first class artifact, with its own particular constructs, and its own runtime environment. The API Gateway provides all of the same benefits for the API that the application server provides for the application. In this way, it is important to distinguish between the API and the application as two distinct entities.

The following overview diagram shows the range of transports and protocols supported by API Gateway on the left, and the services that it provides on the right:

![API Gateway features](/Images/docbook/images/concepts/api_server.png)

## API Gateway services

The main services supported by API Gateway are described in this section.

### API transformation

The API transformation features include the following:

* API virtualization and mediation
* Wide range of protocols, data formats, and standards
* Bi-directional transformation (for example, REST-to-SOAP, XML-to-JSON, and HTTP-to-JMS)

### API control and governance

The API control and governance features include the following:

* Service Level Agreement (SLA) monitoring and enforcement
* Quota management, traffic throttling, and load balancing
* Content-based routing, blocking, and processing
* Auditing of transactions

### API security

The API security features include the following:

* Protect APIs at all levels (interface, access, and data)
* Authentication and authorization
* Identity mediation and integration with IDM platforms
* Data monitoring, redaction, encryption, and signing
* Key and certificate management

### API monitoring

The API monitoring features include the following:

* Real-time API monitoring, with alerting based on errors, exceptions, and thresholds
* Configurable logging of API transaction data
* Analyze API use for insight and trends
* Automated generation and delivery of reports

### API development lifecycle

The API development features includes the following:

* Manage API lifecycle from creation to end-of-life
* Drag-n-drop policy creation with intuitive flow chart metaphor
* Extensive library of pre-built policy rules
* Interactive API testing tool
* Promotion between environments

### API administration

The API administration features include the following:

* Manage all aspects of the daily API operations
* Transaction management
* Tracing and debugging
* OAuth client management
* Managing JMS-based messaging

## API Gateway features

API Gateway provides a comprehensive platform for managing, delivering, and securing APIs. It provides integration, acceleration, governance, and security for Web API and SOA-based systems. This section describes the high-level functionality available in API Gateway.

### Integration

API Gateway provides the following integration features.

#### Identity management

API Gateway integrates with existing third-party Identity Management (IM) infrastructures to perform authentication and authorization of message traffic. For example, integration is provided with LDAP, Microsoft Active Directory, Oracle Access Manager, Computer Associates SiteMinder, Entrust GetAccess, IBM Tivoli Access Manager, RSA Access Manager, and other IM products. API Gateway also integrates with leading integration products and platforms (for example, Microsoft .NET, Oracle WebLogic, IBM WebSphere, and SAP NetWeaver).

#### Scalability

API Gateway is designed to offer a highly flexible and scalable solution architecture. Administrators can deploy new API Gateway instances as needed, and deploy the same or different policies across a group of API Gateway instances as required. This enables administrators to apply polices at any point in their system. Policy enforcement points can be distributed around the network, anywhere traffic is being passed.

#### Pluggable pipeline

The API Gateway internal message-handling pipeline is extensible, enabling extra access control and content-filtering rules to be added with ease. Customers do not have to wait for a full product release before receiving updates of support for emerging standards and for additional adapters.

#### REST APIs

The API Gateway REST support enables you to make enterprise application data and operations available using Web APIs. For example, you can convert a legacy SOAP service, and deploy it as a REST API to be consumed by mobile apps. REST-to-SOAP conversion is easy to achieve using the API Gateway. It can expose REST APIs that map to SOAP services, dynamically creating a SOAP request based on the REST API call.

#### Internationalization (i18n)

API Gateway includes support for multi-byte message data and a wide range of international languages and character sets. For example, this includes requests in languages such as Chinese, German, French, Spanish, Danish, Serbian, Russian, Japanese, Korean, Greek, Arabic, Hebrew, and so on. The API Gateway supports character sets such as `UTF-8`, `KO-I8`, `UTF-16`, `UTF-32`, `ISO-8859-1`, `EUC-JP`, `US-ASCII`, `ISO-8859-7`, and so on.

### Performance

API Gateway accelerates performance as follows.

#### Processing offload

You can use API Gateway to offload the heavy lifting of XML from application servers, and on to the network. This frees up resources on application servers and enables applications to run faster.

#### Acceleration engine

The core acceleration engine is integrated into API Gateway to accelerate the essential XML security primitives. This engine provides XML processing at faster levels than those performed by common JAXP implementations in application servers and other applications that sit downstream from API Gateway. The acceleration engine performs Document Object Model (DOM) processing, XPath, JSON Path, XSLT conversion, and validation of XML and JSON.

#### Data enrichment

API Gateway can automatically populate content in XML and JSON documents from sources such as databases. By putting this functionality on to the network infrastructure, data is automatically populated in messages before they reach the consuming services. This simplifies and accelerates applications in ESBs and application servers.

### Governance

API Gateway provides the following governance features.

#### Ease of deployment

API Gateway includes many features that speed up deployment. For example, certificates and private keys, necessary for XML security functions, are issued on board. API Gateway has a *deny-by-default* defense posture, to detect and block unauthorized deployments of services. Policies can be re-applied across multiple endpoints using simple menus. Policies can also be imported and exported as XML files. This minimizes time needed to replicate policies across multiple API Gateways, or to move from a staging system to production environment.

#### Centralized management

The Policy Studio tool enables administrators to add security and management policies to the API Gateway, and to manage policy versions across multiple API Gateways. This enables enterprise policy management to be brought under centralized control, rather than be managed separately on each API Gateway.

#### Monitoring

Web-based system management dashboards provide centralized control of API Gateways in your domain:

* API Gateway Manager includes monitoring and traffic logging to monitor messages sent through API Gateways. All monitoring data can be aggregated across multiple gateway instances in a group or domain, and can be used to perform root cause analysis and generate alerts.
* API Manager also includes monitoring of APIs and client applications in a metrics database.
* Embedded Analytics enables you to monitor and analyze key metrics in your system. For example, this includes API health, infrastructure health, API usage, and client application health.

#### Reporting

The API Gateway Analytics console provides auditing and reporting on usage across all entry points and creates comprehensive reports to meet operational and compliance requirements. API Gateway Analytics also provides root cause analysis by identifying common failure points in multi-service transactions. If a service fails, and impacts the transaction as a whole, API Gateway Analytics can detect this and generate alerts.

#### Traffic throttling

API Gateway protects services from unanticipated traffic spikes by smoothing out traffic. It also limits clients to agreed service consumption levels in accordance with service usage agreements. This enables Axway customers to charge their clients for different levels of service usage.

### Security

API Gateway includes the following security features.

#### Identity mediation

Through its support for a wide range of security standards, API Gateway enables identity mediation between different identity schemes. For example, the API Gateway can authenticate external clients by user name and password, but then issue SAML tokens that are used for identity propagation to application servers.

#### API management

API Gateway enables you to secure Web APIs against attack and abuse. It also enables you to govern and meter access to and usage of Web APIs. API Gateway provides support for API management security standards such as OAuth. This enables you to share private resources with third-party websites without needing to provide credentials.

#### Application-level networking

API Gateway routes data based on sender identity, content, and type. This enables messages to be sent to the appropriate application in a secure manner. It also enables *service virtualization*, where services are exposed to clients with virtual addresses to mask their actual addresses for security and application delivery. In this way, the API Gateway acts as an important control point for network traffic by shielding endpoint services from direct access.

#### Audit trail

API Gateway satisfies audit requirements by enabling service transactions to be archived in a tamper-proof store for subsequent audit. It also facilitates privacy compliance support by allowing sensitive information, such as customer names, to be encrypted or stripped out of message traffic.

### Form factors

API Gateway is available as software on Linux, or container deployment on Docker. A limited set of developer tools is available on Windows, but the API Gateway server does not support Windows.

## API Gateway tools

API Gateway provides powerful easy-to-use tools that enable you to develop, deploy, and manage API solutions.

![API Gateway tools](/Images/docbook/images/concepts/api_server_tools_rs.png)

The central API Gateway core component is described as follows:

* Provides the runtime environment for exposing virtualized APIs and executing policies
* Implemented using a combination of native code for performance and Java for extensibility
* Deployed and managed in a distributed environment of multiple servers providing scalability and availability

In enterprise organizations, the API Gateway is typically deployed in the DMZ between the public Internet and private intranet.

For more details on the other tools API Gateway provides, see [API Gateway tools](/docs/api_mgmt_overview/api_mgmt_components/tools/).