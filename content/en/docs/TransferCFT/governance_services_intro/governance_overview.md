---
title: "About governance services"
linkTitle: "About governance services"
weight: 150
--- In this guide, *flow* refers to the complete interaction between the source and target applications, more specifically Transfer CFT systems, to enable data exchanges between business applications or partners.

## Managed File Transfer services

Managed File Transfer services, using a blend of Axway products, can centralize flow definition and configuration deployment for {{< TransferCFT/axwayvariablesComponentLongName  >}} (file transfer) engines.

You can use {{< TransferCFT/suitevariablesFlowManager  >}} or Central Governance in your MFT architecture to easily create and deploy flows. You then trigger your flows at the system level.

![Multiple Transfer CFTs can send events from the data exchange environment towards Central Governance](/Images/TransferCFT/data_exchange_env.png)

> **Note**
>
> Connectivity may include connection to other or third- party products that are outside of the MFT reference solution.

### Additional documentation

- Axway Supported Platforms
- {{< TransferCFT/suitevariablesFlowManager >}} documentation
- {{< TransferCFT/suitevariablesCentralGovernanceName >}} documentation

## Governance exchanges

The following types of exchanges occur between {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} and the managed Transfer CFTs:

- Flow management
- Certificate management
- Configuration management
- Update management

See [Exchanges with Central Governance](../cg_postregister) for more information.

## Overview and practical considerations

Begin by planning your MFT architecture and deployment strategy. After installing {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}, the following steps occur:

- In the {{< TransferCFT/axwayvariablesComponentLongName >}} installation select the Central Governance connectivity option
- After installing, start the Transfer CFT Copilot server (the Transfer CFT server can be running, but this is optional)
- Registration occurs automatically on Copilot start up
- From {{< TransferCFT/suitevariablesCentralGovernanceName >}} or {{< TransferCFT/suitevariablesFlowManager >}} start the Transfer CFT(s)
- If you migrated or upgraded, you may want to reference the following sections:
    - [Manually activate Central Governance connectivity](../register_cg)
    - Parameter mapping between products

<span id="Feature"></span>

## Feature support and management

Transfer CFTs running under {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} can manage or have support for the following features.

| Feature  |  Manage using {{< TransferCFT/suitevariablesFlowManager  >}} or {{< TransferCFT/PrimaryCGorUM  >}}  | Supported but not configurable using Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}}  |
| --- | --- | --- |
| Folder monitoring  | yes  | yes  |
| Multi- node architecture  | no  | yes  |
| CRONJOB  | yes  | yes  |
| Exits  | no  | yes  |
| Network features  |   |   |
| IPv6  | yes  | yes  |
| pTCP (UNIX/Windows only)  | yes  | yes  |
| UDT (UNIX/Windows only)  | yes  | yes  |
| SOCKS  | no  | yes  |
| Heartbeat  | embedded  | yes  |
| Interoperability &lt;/th&gt;  |   |   |
| Secure Relay  | no  | yes  |
| TrustedFile (UNIX/Windows/and z/OS)  | no  | yes |
| PassPort AM  | embedded  | no (*)  |
| Sentinel  | embedded  | yes  |
| Protocols &lt;/th&gt;  |   |   |
| PeSIT  | yes  | yes  |
| ODETTE  | no  | yes  |
| SFTP  | yes  | yes  |

\* If you perform a migration or upgrade from a previous version, you must migrate your PassPort AM.

<span id="Legacy"></span>

## Legacy flows

Legacy flows refer to former flow definitions available in migrated {{< TransferCFT/axwayvariablesComponentLongName  >}} systems. Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}} can manage the following use cases:

- Via the Central Governance or {{< TransferCFT/suitevariablesFlowManager >}} user interface, you can add and manage partners, and use send and receive templates for a given Transfer CFT.
- You can migrate Transfer CFT flow definitions to the Central Governance or {{< TransferCFT/suitevariablesFlowManager >}} flow- management process.
