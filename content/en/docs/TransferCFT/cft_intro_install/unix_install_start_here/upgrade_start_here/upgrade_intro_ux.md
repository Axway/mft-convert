---
    title: "Upgrade  Transfer CFT "
    linkTitle: "Upgrade Transfer CFT"
    weight: 160
---This section explains how to upgrade an existing Transfer CFT from 3.1.3 or higher to{{< TransferCFT/axwayvariablesComponentShortName  >}} {{< TransferCFT/PrimaryTransferCFTversionlong  >}}. It begins by detailing the prerequisites for a standalone (non multi-node) upgrade. For details on upgrading a multi-node installation, see [Upgrade a Transfer CFT multi-node installation](../upgrade_multinode_ux#top).

## About upgrades

As of {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.4 there is no separate upgrade package, you use the installation package to perform an upgrade procedure as described in the sections below.

All passwords stored in the UCONF dictionary, or in the {{< TransferCFT/axwayvariablesComponentLongName  >}} databases (for example, CFTPART, CFTPARM) are cyphered using the key generated at installation. If you are performing an upgrade, all passwords are cyphered using a hard-coded key. We recommend that you generate an encryption key.

> **Note**
>
> See also silent mode for details on using the silent installation method.

<span id="Before"></span>

## Before you start

Before beginning the upgrade procedure, you should:

- Download the Transfer CFT installation kit, available  at [support.axway.com](https://support.axway.com/).

<!-- -->

- Stop the Transfer CFT server and the Transfer CFT Copilot server, by entering:
    -   cft stop
    -   copstop -f

## Limitations

During an upgrade, if the CFTCOM file path is greater than 64 characters the COM file is not migrated, and you must migrate it manually.

When upgrading from {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 to 3.3.2 or higher, check that the PKIPASSW length value in the CFT 3.1.3 version (source) is not greater than 8 characters. If it is, truncate the password as described in [Migration or upgrade impact and considerations](../../../mig_impact_considerations)

## Use {{< TransferCFT/PrimaryCGorUM  >}} to upgrade {{< TransferCFT/suitevariablesTransferCFTName  >}}

<span id="testing"></span>You can perform {{< TransferCFT/suitevariablesTransferCFTName  >}} upgrades using {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. However, from the {{< TransferCFT/PrimaryCGorUM  >}} interface you cannot remove service packs or patches, and can only upgrade Transfer CFT as of Transfer CFT 3.1.3 with last service pack (to the latest version). You cannot perform a {{< TransferCFT/axwayvariablesComponentLongName  >}} migration via {{< TransferCFT/PrimaryCGorUM  >}}.

Please refer to the [Central Governance documentation](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/AxwayStartPage.htm) for details.

<span id="Upgrade"></span>

## Upgrade options

You can use the following installer options for {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}} when performing an upgrade:

**--architecture &lt;architecture>**: Installation architecture (single or cluster mode).

- Default: single
- Allowed: single first_host additional_host

**--runtimedir &lt;runtimedir>**: Shared Runtime Directory. On LEGACY upgrades, you must specify the installation’s shared directory instead of the runtime. For example:` /mnt/Axway_Shared `or` Z:\Axway_Shared`

- Only used when architecture=additional_host

**--installdir &lt;installdir>**: Directory where the Transfer CFT is installed/upgraded. On LEGACY upgrades, this is the directory where the Axway Installer was installed.

- Not used when architecture=additional_host
- Default:&lt;Current Drive>:\\axway\\cft

**--conf-file &lt;conf-file>**: File used to personalize installation of Transfer CFT

- In this type of installation only 2 parameters are used:
    -   \- architecture and installdir (if architecture = single/first_host), or
    -   \- architecture and runtimedir (if architecture = additional_host)

You can set these using command line or the configuration file. The values passed in command line take precedence over the values in the configuration file.

## Upgrade Transfer CFT 3.1.3, 3.2.x, or 3.3.2 to {{< TransferCFT/PrimaryTransferCFTversionlong  >}}

> **Note**
>
> Remember to update the product key between versions.

**Step 1: Upgrade to the latest Transfer CFT 3.1.3, 3.2.x, or 3.3.2 Service Pack**

Run the Axway Installer in update mode.

1. Launch the Axway Installer

1. Apply the Transfer_CFT_3.x.y_SP\*\*\*\*\*.zip

    Where \*\*\*\*\* represents the SP level and the platform

    Example: Transfer_CFT_3.1.3_SP3_aix-power-64_BN8712000.zip

> **Note**
>
> In this step you are working with a zip file (not a jar as in earlier Installer versions). Do NOT unzip/uncompress the zip file.

****Step 2: Upgrade to Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}****

> **Note**
>
> Caution  
> This operation removes the old Axway Installer and all its content, so no rollback is available. You should backup the content of your installation directory if you want to have the option of undoing this operation.

1. Uncompress the Transfer CFT installation kit.
1. From the Transfer CFT installation kit, enter:  
    ```
    ./Transfer_CFT_ 3.10
    _Install_<OS>_<BN>.run [<options>]
    ```
1. Accept the license and the appropriate installation mode (for example, single installation).
1. When prompted for the installation directory, enter the path to the existing Transfer CFT installation directory.

****Available options****

The following available options are described in detail in [Upgrade options](#Upgrade):

- --architecture &lt;architecture>
- --runtimedir &lt;runtimedir> (only available when architecture = additional_hosts)
- --installdir &lt;installdir>
- --conf-file &lt;conf-file>
- --help
- --debuglevel
- --mode

<span id="Upgrade"></span>

## Post upgrade

After completing the upgrade procedure, your Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}}, exec scripts are operational. However, you must rebuild your programs that use C and COBOL APIs and exits.

After performing an upgrade, all passwords are cyphered using a hard-coded key. We recommend that you generate an encryption key as described in [Generate an encryption](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Security/cipher_key.htm).

### Set the profile

Once you complete an upgrade from 3.7 or lower, you must execute the profile before running Transfer CFT 3.8 or higher.

### Check the new version

To check the {{< TransferCFT/axwayvariablesComponentShortName  >}} version, as well as the license key and system information, enter the command:

```
CFTUTIL ABOUT
```
