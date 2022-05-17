---
    title: "About the PassPort CSD"
    linkTitle: "PassPort AM CSD"
    weight: 180
---This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

The PassPort CSD file provides resources and available actions for Transfer CFT. This CSD file must be accessible on your file system.

After installing Transfer CFT, access the CSD file at:

> `<Transfer CFT install directory>/home/distrib/am/csd_Transfer_CFT.xml`

****Available <span id="CSD description"></span>CSD actions and resources****


| Type of information  | Description  |
| --- | --- |
| Context | Context range is limited to date and time. |
| Resources | Resources are limited to MESSAGES, BASE, and SERVER. |
| Actions | Customize the actions that can be made on each resource. |
| Privileges | Customize the rights to perform one or more actions on a resource. |
| Roles | Customize the sets of privileges assigned to each role. |


For more information on customizing the CSD file, refer to the {{< TransferCFT/axwayvariablesCompanyName  >}} PassPort
AM documentation available at [support.axway.com]().

****Related topics****

- [About PassPort AM](../)
- [Configuring PassPort AM](../configure_passport_am)
- [Configuring PassPort AM SSL]()
