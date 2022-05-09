{
    "title": "Restore a previous version",
    "linkTitle": "Roll back an upgrade",
    "weight": "240"
}In the event that after upgrading a version you need to revert back to the previous version, you can perform the steps in this section using either the [automatic](#Automati) procedure or the [manual](#Manually) restore procedure.

> **Note**
>
> Note: If you executed transfers in Transfer CFT the newer version that use parameters or metadata not available in the previous version, when you restore a version that new metadata is lost.

> **Note**
>
> Tip  
> CFTPGM is the standard name for the program library, and CFTPROD is the standard name for the library where the configuration files are usually located.

Before you start
----------------

Before beginning either restore procedure:

- Check that you have access to the CFTEXTLIB library created during the previous upgrade procedure.
- Stop the Transfer CFT server and the Copilot UI server. Enter:

```
CFTSTOP
COPSTOP
ENDSBS CFTSBS \*immed
```

Optionally, use the display command to check the version or product details.

```
CFTUTIL about
```
<span id="Automati"></span>

Automatically restore a version
-------------------------------

1. Log in with the `CFTINST `user.
1. In the production, add the library you created during the upgrade (CFTEXTLIB in the following example). Use the command syntax:

<!-- -->

1. ```
    ADDLIBLE LIB(CFTEXTLIB) POSITION(\*FIRST)
    ```
1. Call the `RSTCFT `command for your {{< TransferCFT/suitevariablesTransferCFTName  >}}. The `RSTCFT `command prompt resembles the following screen:

- CFTEXTLIB: Enter the name of the library used during the upgrade procedure. This library must contain the two SAVFs (CFTPGM and CFTPROD) needed for the restore procedure.
- CFTPGM: Enter the name of the library containing the binaries of your Transfer CFT to restore.
- CFTPROD: Enter the name of the library containing the working files of your Transfer CFT to restore.

<span id="Manually"></span>

Manually restore a version
--------------------------

1. Log in with the `CFTINST `user.
1. In the production environment, add the library you created during the upgrade (`CFTEXTLIB `in this example) using the following syntax:  
    ```
    ADDLIBLE LIB(CFTEXTLIB) POSITION(\*FIRST)
    ```
1. Export the current catalog, which you will use later in the restore procedure:   
    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=FROMCAT, ifname=CFTPROD/CAT, ofname=CFTEXTLIB/CAT39’)
    ```
1. Restore the CFTPGM and CFTPROD libraries using the `SAVF `created during the upgrade:
    -   Restore CFTPGM:
    -   ```
        CLRLIB CFTPGM
        RSTLIB SAVLIB(CFTPGM) DEV(\*SAVF) SAVF(CFTEXTLIB/CFTPGM) RSTLIB(CFTPGM) OPTION(\*ALL) MBROPT(\*ALL) ALWOBJDIF(\*ALL)
        ```
    -   Restore CFTPROD:
    -   ```
        CLRLIB CFTPGM
        RSTLIB SAVLIB(CFTPROD) DEV(\*SAVF) SAVF(CFTEXTLIB/CFTPROD) RSTLIB(CFTPROD) OPTION(\*ALL) MBROPT(\*ALL) ALWOBJDIF(\*ALL)
        ```
1. Import the {{< TransferCFT/axwayvariablesComponentLongName  >}} catalog that you saved in Step 3:  
    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=TOCAT, ifname=CFTEXTLIB/CAT39, ofname=CFTPROD/CAT’)
    ```
