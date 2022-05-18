---
title: "Update Transfer CFT"
linkTitle: "Update"
weight: 220
--- This section describes how to perform the following Transfer CFT tasks in an OpenVMS environment:

- [Apply a service pack](#Apply)
- [Remove a service pack](#Remove)
- [Apply a patch](#Apply2)
- [Remove a patch](#Remove2)

<span id="Apply"></span>

## Apply a service pack

1. Download the service pack from Axway support at [support.axway.com](https://support.axway.com/) to the machine you want to update.
1. Stop the servers that you want to update.
1. Transfer the service pack to the host in binary mode.
1. Extract the zip file using the following command, where you replace the SP number with the most recent SP:  
    ```
    unzip Transfer_CFT_ 3.10
    _SP1_vms- ia64.zip
    ```
1. Load the Transfer CFT profile.
1. Run the system command @SYS$UPDATE:VMSINSTAL:  
    ```
    SET DEF [.Transfer_CFT_V 3.10
    _vms- ia64.install]
    @SYS$UPDATE:VMSINSTAL CFT032

    ###############################################
    ### Upgrading of an existing CFT account... ###
    ###############################################
    ```
1. In the Installation options screen, enter **1** to update {{< TransferCFT/axwayvariablesComponentLongName >}}:  
    ```
    Installation options - Installing the Service Pack : 1 - Uninstalling the Service Pack : 2 - Link product : 3 \* Option selected [1
    ]:
    ```
1. Reconnect to the Transfer CFT profile.
1. Use the `CFTUTIL ABOUT` command to check that {{< TransferCFT/axwayvariablesComponentLongName >}} was successfully upgraded.

> **Note**
>
>  

When you install a service pack, the contents of the home directory are updated, but the runtime directory remains untouched. This is so that your customizations, such as APIs, are not overwritten.

<span id="Remove"></span>

## Remove a service pack

This section describes how to uninstall a service pack, which you can do using the console mode in OpenVMS environments.

1. Stop Transfer CFT.
1. Load the Transfer CFT profile.
1. Run the system command: `@SYS$UPDATE:VMSINSTAL`
1. In the Installation options screen, enter 2 to uninstall the service pack:  
    ```
    Installation options
      Installing the Service Pack : 1
      Uninstalling the Service Pack : 2
      Link product : 3 \* Option selected [2
    ]:
    ```
1. Reconnect to the Transfer CFT profile.
1. Use the` CFTUTIL ABOUT` command to check that the service pack was successfully uninstalled.

<span id="Apply2"></span>

## Apply a patch

1. Download the product update from Axway support at [support.axway.com](https://support.axway.com/) to the machine you want to update.
1. Stop the servers that you want to update.
1. Transfer the patch to the host in binary mode.
1. Extract the zip file using the following command, where you replace the SP and Patch number with the most recent SP and patch numbers. Notice that the `- a` option converts the text file to a VMS format.  
    ```
    unzip - a Transfer_CFT_ 3.10
    - SP1_Patch1_vms- ia64.zip
    ```
1. Load the Transfer CFT profile.
1. Execute the `KITUPDATE.COM` script to apply the patch:  
    ```
    SET DEF [.install]
    @KITUPDATE.COM INSTALL
    ```
1. Reconnect to the Transfer CFT profile.
1. Use the `CFTUTIL ABOUT` command to check that the patch was successfully applied.

<span id="Remove2"></span>

## Remove a patch

This section describes how to uninstall a patch using the console mode in OpenVMS.

1. Stop Transfer CFT.
1. Load the Transfer CFT profile.
1. Run the system command:  
    ```
    SET DEF [.install]
    @KITUPDATE.COM UNINSTALL
    ```
1. Reconnect to the Transfer CFT profile.
1. Use the` CFTUTIL ABOUT` command to check that the patch update was removed.
