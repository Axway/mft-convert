{
    "title": "Upgrade Transfer CFT IBM i",
    "linkTitle": "Upgrade Transfer CFT IBM i",
    "weight": "230"
}This page describes two upgrade procedures for Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} IBM i. The automatic upgrade procedure requires fewer user inputs than the manual procedure and is the procedure of choice. This upgrade procedure enables you to change the version of an installed Transfer CFT to a more recent version while conserving its current configuration. Additionally, you can use the [rollback](#) feature if a situation arises where you need to return to a previous version.

> **Note**
>
> Note: The UPGCFT procedure delivered in the installation package performs the same steps as those described in the manual upgrade procedure. However, we strongly recommend using the automatic upgrade procedure instead of the manual upgrade.

> **Note**
>
> Tip  
> CFTPGM is the standard name for the program library, and CFTPROD is the standard name for the library where the configuration files are usually located.

<span id="Upload"></span>

Before you start
----------------

Before beginning the `UPGCFT `procedure:

- Check that you can access the Transfer CFT upgrade pack available for download from Axway at [support.axway.com](https://support.axway.com/).
- Stop the Transfer CFT server and the Transfer CFT Copilot UI server. Enter:

```
CFTSTOP
COPSTOP
ENDSBS CFTSBS \*IMMED
```

Optionally, you can use the display command to check the version or product details prior to upgrading.

```
CFTUTIL about
```
<span id="Automati"></span>

Automatic upgrade procedure
---------------------------

### Uploading the installation package

Start the `UPGCFT `process by uploading the Transfer CFT installation package in binary mode to the IBM i system:

1. Log in with the `CFTINST `user.
1. Create a temporary library: `CRTLIB CFTTMP`  
    This is a temporary library containing the upgrade kit. You can delete it at the end of the procedure.
1. Create a SAVF file: `CRTSAVF FILE(CFTTMP/CFT39)`
1. Upload the installation package to the SAVF in binary mode using FTP:  
    ```
    binary
    cd CFTTMP
    put Transfer_CFT_os400.bin CFT39
    ```

<!-- -->

1. Restore the SAVF file in the temporary library:  
    ```
    RSTLIB SAVLIB(CFTPG) DEV(\*SAVF) SAVF(CFTTMP/CFT39) OPTION(\*NEW) RSTLIB(CFTTMP)
    ```

<!-- -->

1. After restoring the SAVF, you must add the library name in the first position of the library list for the user profile. Execute the command:  
    ```
    ADDLIBLE LIB(CFTTMP) POSITION(\*FIRST)
    ```

> **Note**
>
> Note: The user performing the upgrade requires the same rights as for a regular installation or update (\*JOBCTL, \*SPLCTL, and \*ALLOBJ).

### Executing the UPGCFT command

Enter the `UPGCFT `command and press F4 to display the Transfer CFT IBM i installation screen.

```
UPGRADE CFT (UPGCFT)
Extract lib for the CFT  . . . . CFTEXTLIB      __________
CFT Program Library  . . . . . . CFTPGM         __________
CFT Production Library . . . . . CFTPROD        __________
```

The following fields are mandatory; you should complete as per your system details:

- CFTEXTLIB: Enter the name of the temporary library used to process the upgrade. This can be existing or non-existent library, but its content is cleared during the process.
- CFTPGM: Enter the name of the library containing the binaries of your Transfer CFT to upgrade.
- CFTPROD: Enter the name of the library containing the working files of your Transfer CFT to upgrade.

### Check the new version

To check the Transfer CFT version, as well as the license key and system information, enter the command:

```
CFTUTIL about
```

### About the UPGCFT command

Running the `UPGCFT `command:

1. Creates an upgrade library, for example, CFTEXTLIB. It is advisable to keep this library as a backup of your {{< TransferCFT/axwayvariablesComponentLongName  >}}.  

    After completing the upgrade, this library contains the following elements:

    -   CFTPGM (\*SAVF): The backup of your CFTPGM library before upgrade (this file is also used by the restore procedure).
    -   CFTPROD (\*SAVF): The backup of your CFTPROD library before upgrade (this file is also used by the restore procedure).
    -   CAT (\*FILE): The extraction of the CFTCAT in XML format (this file is used during the upgrade to recreate the CAT file).
    -   COM (\*FILE): The extraction of the CFTCOM in XML format (this file is used during the upgrade to recreate the COM file).
    -   CFTEXT (\*FILE): The extraction of the CFT configuration (this file is used during the upgrade to import the entire Transfer CFT configuration).
    -   UCONFBACK (\*FILE): A backup of the CFTUCONF file.
    -   RSTCFT (\*CMD) and UPGRADE (\*PGM): The command and the program used for a restore procedure.

1. Extracts your configuration, COM file, CAT file into this temporary library.
1. Saves your IFS directories by moving them to new directories having the same name but which are suffixed by “_save”.
1. Saves your {{< TransferCFT/suitevariablesTransferCFTName  >}} libraries in the SAVF located in the temporary lib.
1. Replaces the binaries in your CFTPGM libraries by those of the new {{< TransferCFT/suitevariablesTransferCFTName  >}} version.
1. Replaces your IFS directories by those of the new {{< TransferCFT/suitevariablesTransferCFTName  >}} version.
1. Imports your configuration into your upgraded {{< TransferCFT/suitevariablesTransferCFTName  >}}.

As a result, your {{< TransferCFT/suitevariablesTransferCFTName  >}} binaries are upgraded, but their location is conserved.

### Upgrading multi-node installations

The `UPGCFT `procedure supports multi-node configurations, where your configuration remains strictly the same.
&lt;/p&gt;

### About IASP installations

The procedure supports the upgrade of {{< TransferCFT/suitevariablesTransferCFTName  >}} installed on an IASP. The process remains the same, the UPGCFT procedure itself detects if the {{< TransferCFT/suitevariablesTransferCFTName  >}} is an installation on ASP, and then keeps the {{< TransferCFT/suitevariablesTransferCFTName  >}} in the same ASP.

> **Note**
>
> Note: The UPGCFT procedure does not support upgrades from a non-IASP CFT to an IASP Transfer CFT or vice versa. In the same spirit, an upgrade from one type of IASP to another is not supported.

<span id="Manual"></span>

Manual upgrade procedure
------------------------

This section explains how to upgrade an existing Transfer CFT IBM i from 2.7.1, 3.0.1, 3.1.3, 3.3.2, 3.4-3.8 to Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} for either a [single installation](#Manually) or a [multi-node installation](#Manually2).

<span id="Manually"></span>

### Manually upgrade a single installation

The manual upgrade procedure is similar to the migration procedure.

1. Load the former Transfer CFT CFT 2.7.1, 3.0.1, or 3.1.3 environment.
1. Stop Transfer CFT.
1. Create a backup library to save your configuration:

```
CRTLIB LIB(CFTUPGLIB)
```

1. Export your configuration.
    -   Export your static configuration objects using the command CFTUTIL CFTEXT:

    ```
    CALL PGM(CFTUTIL) PARM(‘CFTEXT’ ‘type=all, fout=CFTUPGLIB/EXTCONF’)
    ```
    -   Export your PKI certificates using the PKIUTIL PKIEXT command. Enter:

    ```
    CALL PGM(PKIUTIL) PARM(‘PKIEXT’ ‘fout=CFTUPGLIB/EXTPKI’)
    ```
    -   Export the catalog using the CFTMI command. Enter:

    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=FROMCAT, ifname=CFTPROD/CAT, ofname=CFTUPGLIB/EXTCAT’)
    ```
    -   Export the communication media file using the CFTMI command:

    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=COM, direct=FROMCOM, ifname=CFTPROD/COM, ofname=CFTUPGLIB/EXTCOM’)
    ```
    -   Additionally, you must export any procedures that are specific to your production, sample APIs, exits, and execs to your backup library.

1. Rename the INSTALL directory, the RUNTIME directory, and the Transfer CFT libraries.
    ```
    STRQSH CMD('mv /home/cft/install /home/cft/install_save')
    STRQSH CMD('mv /home/cft/runtime /home/cft/runtime_save')
    RNMOBJ OBJ(QSYS/CFTPROD) OBJTYPE(\*LIB) NEWOBJ(CFTPRODSAV)
    RNMOBJ OBJ(QSYS/CFTPGM) OBJTYPE(\*LIB) NEWOBJ(CFTPGMSAV)
    ```
1. You can now install the new Transfer CFT version. See [About the INSTALL command](../../install_intro_ibmi/use_install_command).
1. Import the configuration.

    -   Import your static configuration objects using the command:

    ```
    CFTINIT FILES('CFTUPGLIB/EXTCONF')
    ```

    -   Import your PKI certificates using the PKIUTIL command:

    ```
    CALL PGM(PKIUTIL) PARM('PKIFILE' 'fname=CFTPROD/PKIBASE, mode=CREATE')
    PKIUTIL LIBRARY(EXTLIB) FILE(EXTPKI) MEMBER(EXTPKI)
    ```

    -   Import the catalog using the CFTMI command:

    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=TOCAT, ifname=CFTUPGLIB/EXTCAT, ofname=CFTPROD/CAT’)
    ```

    -   Import the communication media file using the CFTMI command:

    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=COM, direct=TOCOM, ifname=CFTUPGLIB/EXTCOM, ofname=CFTPROD/COM’)
    ```

    -   Import specific procedures to your production, for example APIs, EXITs, and execs from the CFTUPGLIB backup library.

    > **Note**
    >
    > You must recompile these after upgrading.

### Check the new version

To check the Transfer CFT version, as well as the license key and system information, enter:

```
CFTUTIL about
```
<span id="Manually2"></span>

### Manually upgrade a Transfer CFT 2.7.1 to 3.8 multi-node installation

The multi-node procedure is similar to the single instance upgrade procedure. ****However, when exporting CAT and COM, you must export your configuration for each node.****

1. Load the former Transfer CFT 2.7.1 to 3.8 environment.
1. Stop Transfer CFT IBM i.
1. Create a temporary library for your exported configuration:

```
CRTLIB LIB(CFTUPGLIB)
```

1. Export your configuration.
    -   Export your static configuration objects using the command:

    ```
    CALL PGM(CFTUTIL) PARM(‘CFTEXT’ ‘type=all, fout=CFTUPGLIB/EXTCONF’)
    ```
    -   Export your PKI certificates using the PKIUTIL PKIEXT command:

    ```
    CALL PGM(PKIUTIL) PARM(‘PKIEXT’ ‘fout=CFTUPGLIB/EXTPKI’)
    ```
    -   Export the catalog using the CFTMI command:

    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_ former_cft_for_node_<node>>, ofname=catalog_output_<node>.xml
    ```
    -   Export the communication media file using the command:
        -   For each communication media file, enter:
        -   For each node, enter:

    <!-- -->

    -   You must also export any procedures that are specific to your production, such as APIs, exits, execs, etc. That is, copy your API, exit sources, exec, and specific scripts.

1. Change the hostname for each Transfer CFT.
    Rename the INSTALL and RUNTIME directories, and the Transfer CFT libraries:  
    ```
    STRQSH CMD('mv /home/cft/install /home/cft/install_save')
    STRQSH CMD('mv /home/cft/runtime /home/cft/runtime_save')
    RNMOBJ OBJ(QSYS/CFTPROD) OBJTYPE(\*LIB) NEWOBJ(CFTPRODSAV)
    RNMOBJ OBJ(QSYS/CFTPGM) OBJTYPE(\*LIB) NEWOBJ(CFTPGMSAV)
    ```
1. You can now install the new Transfer CFT version. See [About the INSTALL command](../../install_intro_ibmi/use_install_command).
1. Import the configuration that you saved in the temporary directory created in Step 3:
    ```
    CFTINIT FILES('CFTUPGLIB/EXTCONF')
    ```
    -   Import the PKI certificates using the PKIUTIL command:

    ```
    CALL PGM(PKIUTIL) PARM('PKIFILE' 'fname=CFTPROD/PKIBASE, mode=CREATE')
    PKIUTIL LIBRARY(EXTLIB) FILE(EXTPKI) MEMBER(EXTPKI)
    ```
    -   Import the catalog using the CFTMI command:

    ```
    CALL PGM(CFTMI) PARM(‘MIGR’ ‘type=CAT, direct=TOCAT, ifname=CFTUPGLIB/EXTCAT, ofname=CFTPROD/CAT’)
    ```
    -   Import the communication media file using the CFTMI command:
        -   For each communication media file, enter:
        -   For each node, enter:
    -   Import specific procedures to your production, for example APIs, EXITs, and execs from the CFTUPGLIB backup library. &lt;blockquote&gt;&lt;html&gt;&lt;body&gt;&lt;p&gt;&lt;b&gt;Note&lt;/b&gt; &lt;/p&gt;&lt;/body&gt;&lt;/html&gt;You must recompile these after upgrading.&lt;/blockquote&gt;&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;

### Check the new version

To check the Transfer CFT version, as well as the license key and system information, enter the command:

```
CFTUTIL about
```
