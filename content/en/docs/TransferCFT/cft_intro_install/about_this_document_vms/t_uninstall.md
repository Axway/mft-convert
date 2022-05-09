{
    "title": "Uninstall Transfer CFT",
    "linkTitle": "Uninstall ",
    "weight": "200"
}This topic describes how to uninstall Transfer CFT. If you uninstall a Transfer CFT, you will lose the complete Transfer CFT configuration. To avoid this, save your environment (sample, exit, …) before removing the Transfer CFT OpenVMS.

There is no automatic procedure to uninstall {{< TransferCFT/axwayvariablesComponentLongName  >}}; you must perform system commands to delete all files as described here.

1. Identify the Transfer CFT directories to delete it with the command:

    `sh log D$CFT_INST`

    `sh log D$CFT_RUN`

      
    ```
    "D$CFT_INST" = "DKB200:[CFT.HOME.]"
    "D$CFT_RUN" = " DKB400:[CFT.SHARED.CFT.RUNTIME.]"
    ```

1. Use and repeat the delete command to remove all {{< TransferCFT/axwayvariablesComponentLongName  >}} files.  
    ```
    delete DKB200:[CFT.HOME…]\*.\*;\*
    delete DKB400:[CFT.SHARED.CFT.RUNTIME…]\*.\*;\*
    ```
1. After you have deleted all files, delete the home and runtime directories:  
    ```
    delete DKB200:[CFT]HOME.DIR;\*
    delete DKB400:[CFT.SHARED.CFT]RUNTIME.DIR;\*
    ```
1. Check that all directories are removed.
1. Delete the Transfer CFT user for this installation if no longer need.
