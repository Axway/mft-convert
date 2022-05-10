---
    "title": "Rollback upgrade and restore the catalog",
    "linkTitle": "Rollback upgrade and restore catalog",
    "weight": "230"
---
{{% TransferCFT/snippets/rollback_intro%}}

Roll back to a previous version
-------------------------------

You can rollback an upgrade if needed the same as if you [Uninstall a service pack](#Uninstal). However, rolling back an upgrade takes Transfer CFT back to the previous version before the upgrade, so all transfers and configuration modifications that were performed since the upgrade are lost.

Roll back and restore catalog
-----------------------------

Under the described conditions, you can restore your current catalog as explained below.

### Prerequisites

To perform a version downgrade, you must have already upgraded from Transfer CFT 3.4 or higher to Transfer CFT 3.6.

### Procedure

1. Enter the following command to export the Transfer CFT catalog file, perfoming the command from the `<installation directory>`:  
    ```
    CFTMI migr type=cat,direct=fromcat,ifname=$CFTCATA,ofname=cftcat_36
    ```
1. Uninstall the Transfer CFT 3.6. Please see [Uninstall]() Transfer CFT for details.
1. Create a Transfer CFT catalog file. See [Manually create internal data files](../../../admin_intro/admin_commands_intro/cftfile) for details. For example:  
    ```
    CFTUTIL cftfile type=cat,mode=replace,fname=$CFTCATA,recnb=5000
    ```
1. Import the saved `cftcat_36 `to the catalog file `<logical_filename>` you created in the previous step.  
    ```
    CFTMI migr type=cat,direct=tocat,ifname=cftcat_36,ofname=$CFTCATA
    ```
1. Start Transfer CFT to verify the procedure.
