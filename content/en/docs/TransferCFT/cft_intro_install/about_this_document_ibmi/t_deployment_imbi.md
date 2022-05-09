{
    "title": "Create an Express Package",
    "linkTitle": "Create an Express Package",
    "weight": "180"
}A product deployment package in Transfer CFT is called an Express Package. For the iSeries platform, you can create a deployment package for Transfer CFTs to be used with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, or for standalone Transfer CFTs.

This section describes how to create a reusable and distributable {{< TransferCFT/axwayvariablesComponentShortName  >}} package to simplify and ease the task of installing and configuring {{< TransferCFT/axwayvariablesComponentShortName  >}}s on multiple servers of the same architecture.

> **Note**
>
> Note: You can only install a Transfer CFT Express Package on the same platform as the one on which it was generated.

Create a deployment package for {{< TransferCFT/suitevariablesTransferCFTName  >}}s used with Central Governance
---------------------------------------------------------------------------------------------------------------------

Perform the following steps:

1. Create a user profile using the command: `CRTUSRPRF`
1. Create a temporary library, for example:  
    ```
    CRTLIB CFTTMP
    ```
1. Create a save file (\*SAVF) in the CFTTMP library, for example:  
    ```
    CRTSAVF FILE(CFTTMP/CFT32XL) TEXT('Transfer CFT Distribution save file')
    ```
1. Use FTP in binary mode to send the save file to an iSeries system. Open an FTP session, and enter:  
    ```
    Set Transfer Mode to bin
    cd CFTTMP
    put Transfer_CFT_os400.bin CFT32XL
    quit
    ```
1. Restore the Transfer CFT save file, for example:  
    ```
    RSTLIB SAVLIB(CFTPG) DEV(\*SAVF) SAVF(CFTTMP/CFT32XL) RSTLIB(CFTTMP)
    ```
1. Install a Transfer CFT 3.2.4 with Central Governance (you must use this command for all iSeries Transfer CFT deployments).

- See the example and options described in the INSTALL section and customize to suit your business needs. Details on [Silent installation](../install_intro_ibmi/perform_auto_installation).

> **Note**
>
> Note: If you want to add or modify the installation parameters, you must run the INSTALL command after selecting F4. Answer the prompted questions to configure the product for your production. At the end of your first installation, press F9 to execute the recall command. This command must be used for all your deployments.

1. Use Central Governance to deploy and configure your Transfer CFTs as needed.

Create a {{< TransferCFT/axwayvariablesComponentShortName  >}} deployment package for standalone usage
-----------------------------------------------------------------------------------------------------------

In this procedure, you must first create a SAVF file that contains all of your necessary configurations for your deployment including:

- Static configuration, such as protocols (CFTPROT), networks (CFTNET), UCONF parameters, and so on
- Partners (CFTPART, CFTTCP)

> **Note**
>
> Note: If you create partners to export, DO NOT use the NSPART parameter in the CFTPART definition. The target Transfer CFT instead uses the CFTPARM PART/NPART values.

- Flows (CFTSEND and CFTRECV)
- SSL certificates
- Processing scripts and EXITs
- Additional Axway components that you use with Transfer CFT such as Sentinel, PassPort, etc.

### Procedure

On the local machine where you have {{< TransferCFT/suitevariablesTransferCFTName  >}} installed:

1. Create a temporary library that will contain all the items you want to deploy, for example:  
    ```
    CRTLIB CFTCONF
    ```
1. Copy all the configuration elements you want to deploy into this library, for example:  
    ```
    CPYF FROMFILE(CFTPROD/UTIN) TOFILE(CFTCONF/UTIN) FROMMBR(CGPARAM) TOMBR(TCPPARAM)
    ```
1. Create a backup for your library CFTCONF, for example:  
    ```  
     1. CRTSAVF FILE(CFTCONF/CFTCONFSVF) 2. SAVLIB LIB(CFTCONF) DEV(\*SAVF) SAVF(CFTCONF/CFTCONFSVF) 3. Get the CFTCONFSVF.savf (in binary mode)
    ```

On the other machines, where you want to deploy {{< TransferCFT/suitevariablesTransferCFTName  >}}:

1. Create a temporary library, for example:  
    ```
    CRTLIB CFTTMP
    ```
1. Create two save file (\*SAVF) in the CFTTMP library, for example:  
    ```  
     1. CRTSAVF FILE(CFTTMP/CFT32XL) TEXT('Transfer CFT Distribution') 2. CRTSAVF FILE(CFTTMP/CFTCONFSVF) TEXT('CFT configuration')
    ```
1. Use FTP in binary mode to send the save file to an Iserie system. Open an FTP session, and enter:  
    ```
    Set Transfer Mode to bin
    cd CFTTMP
    put Transfer_CFT_os400.bin CFT32XL (SAVF with Transfer CFT)
    put CFTCONFSVF.savf CFTCONFSVF (SAVF with CFT configuration)
    quit
    ```
1. Restore the Transfer CFT save file, for example:  
    ```
    RSTLIB SAVLIB(CFTPG) DEV(\*SAVF) SAVF(CFTTMP/CFT32XL) RSTLIB(CFTTMP)
    ```
1. Installing a Transfer CFT 3.2.4 without Central Governance  
    ```
    INSTALL
    ```

> **Note**
>
> Note: When you run INSTALL without parameters, you run the default installation with the CFTPGM and CFTPROD libraries.

> **Note**
>
> Note: As of Transfer CFT 3.3.2, you can define the user for the Transfer CFT installation. This user can be different from the current user. From the INSTALL command, select F4 (Prompt) and modify the USERINST value. This user must exist on the machine; if it does not, you can use the CRTUSRPRF command to create it.

1. Restore the Transfer CFT configuration save file, for example:  
    ```
    RSTLIB SAVLIB(CFTCONF) DEV(\*SAVF) SAVF(CFTTMP/CFTCONFSVF) RSTLIB(CFTPROD)
    ```
1. Apply your configuration to your new environment, for example:  
    ```  
     1. CFTUTIL PARAM('\#CFTPROD/<CFTCONF>) 2. PKIUTIL PARAM('\#CFTPROD/<PKICONF>')
    ```

Limitations
-----------

- Transfer CFT Express Package does not support cluster mode installations.
- Transfer CFT Express Package cannot embed a Transfer CFT upgrade pack.
