---
    title: "Upgrade a Transfer CFT multi-node installation"
    linkTitle: "Upgrade a multi-node installation"
    weight: 190
---This section describes how to upgrade from a Transfer CFT 3.1.3, 3.2.x, 3.3.2, or 3.4 multi-node, multihost installation to Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}.

As of {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.4 there is no separate upgrade package, you use the installation package to perform an upgrade procedure as described in the sections below.

<span id="Before"></span>

## Before you start

Before beginning the upgrade procedure, download the Transfer CFT installation kit available at [support.axway.com](https://support.axway.com/).

> **Note**
>
> Remember that Transfer CFT clusters must be fully stopped while performing an upgrade.

## Limitation

During an upgrade, if the CFTCOM file path is greater than 64 characters the COM file is not migrated, and you must migrate it manually.

## Procedure

For details on shared disks, node commands, and other multi-node considerations, refer to the *Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} User Guide &gt; Manage multi-node architecture*. For more information on Upgrading Transfer CFT, see [Upgrade Transfer CFT](../upgrade_intro_win) for Windows.

### Upgrade all hosts

1. Stop Copilot. This command stops Copilot as well all cftnodes running on that machine.  
    ```
    copstop -f
    ```

1. Connect to the first machine and execute the following command:

1. ```
    .\\Transfer_CFT_ 3.10
    _Install_win-x86-64_<BN>.exe --architecture first_host --installdir <installdir>
    ```

     

1. For each additional host, connect to the machine and execute the following command:  
    ```
    .\\Transfer_CFT_ 3.10
    _Install_win-x86-64_<BN>.exe --architecture additional_host --runtimedir <runtimedir>
    ```

- Use the two following parameters, depending on if this is the first host or an additional host:
    -   `architecture `and `installdir `(first_host), *or*
    -   `architecture `and `runtimedir `(additional_host)
- Where:
    -   --architecture &lt;architecture>: Installation architecture (first_host or additional_host).
    -   --installdir &lt;installdir>: For a legacy upgrade, this is the directory where the Axway Installer was installed. When this parameter is assigned, it overwrites any reference in the configuration file (first_host).
    -   --runtimedir &lt;runtimedir>: For a legacy upgrade, this is the shared data directory where the Axway Installer was installed. When this parameter is assigned, it overwrites any reference in the configuration file (additional_host).

### Restart the upgraded Transfer CFT multihost multi-node environment

1. Launch the Transfer CFT profile from the Transfer CFT runtime directory on the shared disk.  
    ```
    cd /<shared_disk>/<CFTdir>/Transfer_CFT/runtime
    profile.bat
    ```
1. Check the new version using the following command:  
    ```
    CFTUTIL ABOUT
    ```
1. Start Copilot (start each of the Copilots in the multi-node environment).  
    ```
    copstart
    ```
1. After restarting the Copilots, restart the Transfer CFT server:  
    ```
    cft restart
    ```
1. Check the upgraded Transfer CFT multi-node multihost system.  
    ```
    CFTUTIL listnode
    ```
    -   All of the Copilots should be started

    <!-- -->

    -   All of the Transfer CFT nodes must be started

Once Transfer CFT has been upgraded on a host you can start that instance, there is no need to wait until Transfer CFT is upgraded on every host.

## Post upgrade

After completing the upgrade procedure, your Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}}, exec scripts are operational. However, you must rebuild your programs that use APIs and exits.

After performing an upgrade, all passwords are cyphered using a hard-coded key. We recommend that you generate an encryption key as described in [Generate an encryption key](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Security/cipher_key.htm).
