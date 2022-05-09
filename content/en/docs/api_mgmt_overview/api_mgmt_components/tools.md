{
"title": "Amplify API Management tools",
"linkTitle": "Tools",
"weight":"35",
"no_list": "true",
"date": "2019-11-07",
"hide_readingtime": "true",
"description": "Amplify API Management tools to develop, deploy, and manage API solutions."
}

## Policy Studio

Policy Studio is graphical tool that enables you to virtualize APIs and develop policies (for example, to enforce security, compliance, and operational requirements). It includes the following features:

* Flow-chart style visualization for easy development and maintenance
* Graphical drag-and-drop user interface that enables you to drag filters (processing rules) on to the policy canvas and configure them
* Extensive library of filters to build powerful policies

The following example shows the policy canvas at the center and the filter library on the right.

![Policy Studio](/Images/docbook/images/concepts/policy_studio.png)

A *filter* is an executable rule that performs a specific type of processing on a message. For example, the **Message Size** filter rejects messages that are greater or less than a specified size.

There are many categories of message filters available with the API Gateway (for example, Authentication, Authorization, Content Filtering, Conversion, Trust, and so on). In Policy Studio, a filter is displayed as a block of business logic that forms part of an execution flow known as a policy.

A *policy* is a network of filters in which each filter is a modular unit that processes a message. A message can traverse different paths through the policy, depending on which filters succeed or fail. For example, this enables you to configure policies that route messages that pass a **Schema Validation** filter to a back-end system, and route messages that pass a different **Schema Validation** filter to a different system.

A policy can also contain other policies, which enables you to build modular reusable policies. In Policy Studio, the policy is displayed as a path through a set of filters, as shown in the previous example.

Policy Studio is available also on Windows.

To learn more, see [Get started with Policy Studio](/docs/apim_policydev/apigw_poldev/gs_concepts/).

## Configuration Studio

Configuration Studio is a graphical tool used to promote API Gateway configuration from development environments to upstream environments (for example, testing or production).

Configuration Studio enables API Gateway administrators to take configuration prepared by policy developers, and to create environment-specific configuration for deployment. Configuration Studio is designed for the skills of upstream administrators, and does not assume expertise in policy development and policy configuration.

![Configuration Studio](/Images/docbook/images/concepts/config_studio.png)

Configuration Studio enables administrators to perform tasks such as the following:

* Open a policy package (`.pol`) received from a development environment.
* Specify values for environment-specific settings selected in a development environment (for example, policy, listener, and external connections).
* Import or create environment-specific certificates and keys.
* Define environment-specific users and user groups.
* Export the environment package to a file on disk. The environment package is implemented as an `.env` file.

Configuration Studio is available also on Windows.

## API Gateway Manager

API Gateway Manager is a web-based administration console that enables you to perform operational monitoring, management, and troubleshooting.

![API Gateway Manager](/Images/docbook/images/concepts/vordel_mngr.png)

API Gateway Manager includes the following features:

* Dashboard displaying the distributed topology with a real-time overview of message traffic by domain, group, and API Gateway
* Real-time monitoring of message traffic and content, enabling easy identification of exceptions and drilling into message details
* Real-time monitoring of performance metrics by API service, system, and remote host
* Aggregated view of audit, alert, and SLA alert messages across the domain
* Centralized viewing of audit and debug logs of each API Gateway instance
* Managing dynamic system settings
* Managing user roles assigned in the domain

To learn more about API Gateway Manager, see:

* [Monitoring and metrics](/docs/apim_administration/apigtw_admin/monitor_service/)
* [Manage API Gateway deployments](/docs/apim_administration/apigtw_admin/deploy_get_started/)

## API Manager CLI

You can use the API Manager CLI (`apim-cli`) to integrate APIs automatically in API Manager without the API Manager web UI. This enables you to easily manage APIs using a command-line interface or CI/CD pipeline.

{{< alert title="Note" color="primary" >}}`apim-cli` is not supported directly by Axway as part of Amplify API Management, but it is the de-facto standard for managing API Manager from the command line. To learn more about `apim-cli`, see [`apim-cli` on GitHub](https://github.com/Axway-API-Management-Plus/apim-cli/blob/develop/README.md) and [`apim-cli` wiki](https://github.com/Axway-API-Management-Plus/apim-cli/wiki).{{< /alert >}}

API Manager CLI includes the following features:

* Import, export, and update of APIs
* Management of the entire API lifecycle
* System and application default quota management
* API security configuration
* Documentation, API image
* Organization permissions
* Application subscriptions

Watch this video to learn how to manage APIs using the CLI.

{{< youtube GWAm1VxKUgo >}}

## Key Property Store

The API Gateway Key Property Store (KPS) is used to store configuration parameters that are dynamically passed into policies at runtime. This enables policy configuration data to be managed directly by business or operational users at runtime, and allows dynamic change of policy behavior.

![Key Property Store](/Images/docbook/images/concepts/kps.png)

The KPS includes the following features:

* Policies look up configuration data in the KPS at runtime to dynamically determine behavior
* Policies developed in the Policy Studio use a selector syntax to specify context-sensitive lookup of policy configuration data at runtime from the KPS (for example, `${kps.CustomerProfiles[JoeBloggs].age}` obtains the age of the specified customer)
* Provides a cached read-frequently, write occasionally cache with backing stores
* Policy-specific UIs can be developed for business or operational users to manage the policy configuration data in the KPS

To learn more, see [KPS overview](/docs/apim_policydev/apigw_kps/introduction/).

## Embedded Apache ActiveMQ

API Gateway can act as a native Java Message Service (JMS) provider by embedding Apache ActiveMQ. This enables the API Gateway to integrate external facing REST APIs and SOAP Web services with back-end systems and applications using reliable, asynchronous messaging.

For internal integration and ESB-style projects, API Gateway provides a messaging and mediation solution to route and transform messages flowing between applications and services. In addition, JMS queues hosted on the embedded ActiveMQ can be used by API Gateway policies to provide asynchronous policy behavior.

An ActiveMQ broker is embedded in each API Gateway instance, with brokers organized by API Gateway groups. An active/active deployment is supported to ensure high availability of the messaging infrastructure, with an external shared file system used for the persistent message store.

Queue and topic management is integrated into the API Gateway Manager web console, which enables the API administrator to view queues and topics, messages on queues, and individual message contents.

![ActiveMQ message](/Images/docbook/images/concepts/admin_messaging_content.png)

The API Gateway installation includes the ActiveMQ Java JMS 1.1 client library, which applications can use to send and receives message to and from the queues and topics hosted on the embedded ActiveMQ broker. In addition, ActiveMQ clients that use the OpenWire protocol (ActiveMQ default transport protocol) can interact with the embedded broker. For more details, see [Apache ActiveMQ OpenWire documentation](http://activemq.apache.org/openwire.html).

Learn how to [set up ActiveMQ in API Gateway](/docs/apim_administration/apigtw_admin/admin_messaging/) and how to  [read and write with JMS](/docs/apim_policydev/apigw_polref/routing_jms/).
