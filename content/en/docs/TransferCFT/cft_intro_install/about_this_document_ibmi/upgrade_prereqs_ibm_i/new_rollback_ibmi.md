---

    title: Roll back an upgrade
    linkTitle: Roll back an upgrade
    weight: 240

---
In the event that after upgrading a version you need to revert back to the previous version, you can perform the steps in this section using either the [automatic](#Automati) procedure or the [manual](#Manually) roll back and restore catalog procedure.

> **Note**
>
> If you executed transfers in Transfer CFT the new version that use parameters or metadata not available in the previous version, when you rollback the new metadata is lost.

> **Note**
>
> Tip  
> CFTPGM is the standard name for the program library, and CFTPROD is the standard name for the library where the configuration files are usually located.

## Before you start

Before beginning the rollback procedure:

- Check that you can access the Transfer CFT installation or upgrade packages for download from Axway at [support.axway.com](https://support.axway.com/).
- Stop the Transfer CFT server and the Transfer CFT Copilot (UI) server. Enter:

```
CFTSTOP
COPSTOP
```

Optionally, you can use the display command to check the version or product details prior to upgrading.

```
CFTUTIL about
```
<span id="Automati"></span>

## Automatically roll back

### Uploading the installation package

Start the rollback process by uploading the Transfer CFT installation package in binary mode to the IBM i system:

1. Log in with the `CFTINST `user.
1. Create a temporary library: `CRTLIB CFTTMP`
1. Create a SAVF file: `CRTSAVF FILE(CFTTMP/CFT37)`
1. Upload the installation package to the SAVF in binary mode using FTP:  
    ```
    binary
    cd CFTTMP
    put Transfer_CFT_os400.bin CFT37
    ```

<!-- -->

1. Restore the SAVF file in the temporary library:  
    ```
    RSTLIB SAVLIB(CFTPG) DEV(\*SAVF) SAVF(CFTTMP/CFT37) OPTION(\*NEW) RSTLIB(CFTTMP)
    ```

<!-- -->

1. After restoring the SAVF, you must add the library name in the first position of the library list for the user profile. Execute the command:  
    ```
    ADDLIBLE LIB(CFTTMP) POSITION(\*FIRST)
    ```

> **Note**
>
> The user performing the upgrade requires the same rights as for a regular installation or update (\*JOBCTL, \*SPLCTL, and \*ALLOBJ).

1. Call the UPGRADE command for your {{< TransferCFT/suitevariablesTransferCFTName >}}. Applying an UPGRADE of a version older than the version of your {{< TransferCFT/suitevariablesTransferCFTName >}} rolls it back to this older version, but keeps your configuration.

In rollback mode, the UPGRADE command prompt resembles the following screen:

```
UPGRADE CFT (UPGRADE)
Extract lib for the CFT  . . . . CFTEXTLIB      __________
CFT Program Library  . . . . . . CFTPGM         __________
CFT Production Library . . . . . CFTPROD        __________
Are you rolling back? . . . . . .ROLLBACK       '1'   
Lib of the SAVF  . . . . . . . . LIBSAVF        '\*LIBL'  
SAVF of the version to apply . . SAVF           CFT37X      
```

The following fields are mandatory; you should complete as per your system details:

- CFTEXTLIB: Enter the name of the temporary library used to process the upgrade. This can be existing or non-existent library, but its content is cleared during the process.
- CFTPGM: Enter the name of the library containing the binaries of your Transfer CFT to upgrade.
- CFTPROD: Enter the name of the library containing the working files of your Transfer CFT to upgrade.
- ROLLBACK: '1': Enables the rollback mode (YES).  
    ‘2’: Indicates that you are NOT rolling back to a previous {{< TransferCFT/suitevariablesTransferCFTName >}} version (NO).
- SAVF: This field only displays when you enter '1' in the ROLLBACK field. In this case, enter the name of the SAVF for the version you want to apply. The default value is the name of SAVF for the version that you downloaded.

> **Note**
>
> Caution  
> When performing a rollback, the default value of the SAVF field MUST match the version you want to roll back to. Please determine the name of the SAVF corresponding to the version you want to roll back to, as shown below:


| Transfer CFT version | SAVF name |
| --- | --- |
| 3.1.3 | CFT31X |
| 3.2.4 | CFT32X |
| 3.3.2 | CFT33X |
| 3.6 | CFT36X |


> **Note**
>
> Tip  
> The UPGRADE command is available as of Transfer CFT 3.7.

## Roll back to a version that predates the UPGRADE command

The UPGRADE command was not available in Transfer CFT 3.6 and lower; however you can still execute the UPGRADE command to roll back to those versions. Use FTP in binary mode for the various upload steps to your system.

To roll back to version 3.6 or lower:

1. Follow the [Upload instructions](#Upload) to upload the rollback version SAVF to a temporary library, for example CFTTMP.
1. Restore the SAVF in CFTTMP.
1. Repeat the [Upload instructions](#Upload) to upload the most recent {{< TransferCFT/suitevariablesTransferCFTName >}} version SAVF to a second temporary library, for example CFTTMP2.
1. Add the CFTTMP2 temporary library in the first position of your library list.

```
ADDLIBLE LIB(CFTTMP2) POSITION(\*FIRST)
```

1. Call the UPGRADE command, and then press F4 to fill the fields. Remember to enable the rollback mode by changing the ROLLBACK value. The command to execute, again for example for version 3.6, should resemble the following:

```
UPGRADE CFTEXTLIB(CFTEXTLIB) CFTPGM(CFTPGM) CFTPROD(CFTPROD) ROLLBACK('1') LIBSAVF(CFTTMP) SAVF(CFT36X)  
```
<span id="Manually"></span>

## Manually roll back

### Prerequisites

- Save all the CFTPGM and CFTPROD before installing Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber >}}.
- Install to upgrade from {{< TransferCFT/headerfootervariableshflongproductname >}} 3.4, 3.5, 3.6 (3.x) to {{< TransferCFT/headerfootervariableshflongproductname >}} {{< TransferCFT/axwayvariablesReleaseNumber >}}.

### Manual procedure

Perform the following tasks (we use 3.x in this example step procedure to indicate the version to roll back to):

1. Migrate the Transfer CFT catalog file. On the IBM i system: 

    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=FROMCAT, ifname=CFTPROD/CAT, ofname=CFTUPGLIB/CAT35’)

    Where <span style="font-family: 'Courier New';">CFTUPGLIB</span> is a temporary backup library.

1. Downgrade from Transfer CFT 3.6 to Transfer CFT 3.x version:
    1.  Rename CFTPGM and CFTPROD.
    2.  Install Transfer CFT3.x.

1. Restore your EXEC procedures and the CFTPROD library template.

1. Create a new Transfer CFT catalog file.

1. Import the saved cftcat\_35 to the new Transfer CFT catalog file:

    <span style="font-family: 'Courier New';">CFTMI migr type=cat,direct=tocat,ifname=cftcat\_35,ofname=$CFTCATA</span>

1. On the IBM i system:

    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=TOCAT, ifname=CFTUPGLIB/CAT35, ofname=CFTPROD/CAT’)
