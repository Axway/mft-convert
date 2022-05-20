---
title: "Governance services"
linkTitle: "Governance services"
weight: 130
---{{< TransferCFT/axwayvariablesCompanyName  >}} Flow Manager and {{< TransferCFT/PrimaryCGorUM  >}} are two {{< TransferCFT/axwayvariablesPlatformorSuiteLongName  >}} management applications that provide a set of services for administering Transfer CFTs. From the Flow Manager UI, for example, you can configure, access logs, and perform operational tasks for your {{< TransferCFT/axwayvariablesComponentShortName  >}}. Flow Manager also simplifies the task of creating flows, and supports mass deployment of configuration changes to groups of {{< TransferCFT/axwayvariablesComponentShortName  >}}s.

## {{< TransferCFT/suitevariablesFlowManager  >}}

Flow Manager is an enterprise-scale MFT solution that provides a set of services and templates to configure and deploy file transfer patterns. Flow Manager also provides API services to extend the MFT functionality. Using Flow Manager you can deploy anywhere, control centrally, and empower IT and business users with self-service MFT capabilities. Flow Manager helps you to increase the efficiency of your data exchange ecosystem, reduce time to revenue, and streamline your business file transfer needs.

 

## {{< TransferCFT/PrimaryCGorUM  >}}

{{< TransferCFT/PrimaryCGorUM  >}} offers the following key features:

- Automatic recognition and registration of {{< TransferCFT/axwayvariablesComponentShortName >}}s on connection
- Centralized management of {{< TransferCFT/axwayvariablesComponentShortName >}} configurations, with processing capabilities for highly distributed environments
- Centralized start, stop, and log viewing for your {{< TransferCFT/axwayvariablesComponentShortName >}}s
- Global management of user and partner identities and rights
- Data flow repository, allowing you to create and supervise end-to-end data flow definitions
- Alert management for tracking {{< TransferCFT/axwayvariablesComponentShortName >}} and its data flow processing, including an alert notification option
- Out-of-the-box dashboards

The following diagram illustrates a simplified view of the {{< TransferCFT/PrimaryCGorUM  >}} architecture.

![High level view of Central Governance layer over a group of Transfer CFTs](/Images/TransferCFT/2013_g_CG_architecture_draft1.png)

You can deploy a single Central Governance instance with one computer per network. The system supports active/passive resiliency, and can bring another instance of Central Governance online if the primary fails.

> **Note**
>
> Flow Manager offers services that are similar to Central Governance, but with the addition of predefined templates and API functionality in a container setting.
