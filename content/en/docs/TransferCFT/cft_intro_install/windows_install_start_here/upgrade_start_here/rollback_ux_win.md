---
    title: "Roll back to a previous version "
    linkTitle: "Roll back to a previous version"
    weight: 200
---This section describes the procedure to revert to the previous version if, for whatever reason, you need to roll back an upgrade. However, if you executed transfers in the upgraded version that used parameters or metadata that are not available in the earlier version, this metadata is lost when you roll back.

## Prerequisites

- Download the installation package you want to downgrade to, noting that you can only downgrade to a version from which you have previously upgraded. For example, if you upgraded from version 3.4 directly to 3.8, you can only downgrade to version 3.4 and not to an intermediary version 3.6.
- You must have already upgraded from {{< TransferCFT/headerfootervariableshflongproductname >}} 3.4 or higher to {{< TransferCFT/headerfootervariableshflongproductname >}} {{< TransferCFT/axwayvariablesReleaseNumber >}}

> **Note**
>
> On Unix systems, do not set the profile prior to upgrading. If you are in a session and the profile is set, exit the bash session before running the upgrade.

## Procedure

**Unix and Windows**

Execute the following steps:

1. Run the installation program that corresponds to the version you want to roll back. When prompted, enter the installation directory of the {{< TransferCFT/suitevariablesTransferCFTName >}} instance to roll back.
1. Start {{< TransferCFT/suitevariablesTransferCFTName >}} to verify the procedure and check the version.
