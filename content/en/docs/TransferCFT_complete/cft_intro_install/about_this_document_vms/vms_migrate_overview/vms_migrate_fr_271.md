{
    "title": "Migrate from Transfer CFT 2.7        ",
    "linkTitle": "Migrate from Transfer CFT 2.7.1",
    "weight": "260"
}This section describes how to migrate Transfer CFT 2.7 to version {{< TransferCFT/PrimaryTransferCFTversionlong  >}}.

> **Note**
>
> Note: Be sure to first read the Before you start.

Migrate the main configuration and UCONF parameters
---------------------------------------------------

You can migrate the PARM, PART, IDF, other static configuration objects, and UCONF parameters as follows:

1. Load the former Transfer CFT environment. See [Migration prerequisites](../vms_migrate_prereq) for details.
1. Export your static configuration objects using the `CFTUTIL CFTEXT` command. Enter:
    &lt;/li&gt;
1. ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```
1. Open the extract configuration files, `cft-extract.conf`, and update the file paths with those of the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} installation.
1. Load the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import your static configuration objects using the `cftinit `command. Enter:  
    ```
    cftinit cft-extract.conf
    ```

Migrate the PKI certificates
----------------------------

1. Load the former Transfer CFT 2.7 environment.
1. Export your PKI certificates using the command PKIUTIL PKIEXT. Enter:  
    ```
    PKIUTIL PKIEXT fout=pki-extract.conf
    ```
1. Load the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
    &lt;/li&gt;
1. Create a new PKI internal datafile using the `PKIUTIL PKIFILE` command. Replace &lt;pki_database_filename&gt; with the OS appropriate value, `CFTPKU`.  
    ```
    PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
    ```
1. Convert your certificate with the following command.  
    ```
    SET FILE/ATTRIB=(LRL:4096,RFM:UDF) <mycertificate>
    ```
1. Import your PKI certificates in the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} using the `PKIUTIL` command. Replace the &lt;script_filename&gt; with the new script file path.
    &lt;/li&gt;
1. ```
    PKIUTIL <prefix_character><script_filename>
    ```
1. ```
    PKIUTIL @pki-extract.conf
    ```

Migrate the runtime environment
-------------------------------

### Migrate the catalog

1. Load the former Transfer CFT 2.7 environment.
1. Define the symbolic variable:  
    ```
    CFTMI240 == "$CFT_EXE:CFTMI240"
    ```
1. Export the catalog using the `CFTMI240` command.  
    ```
    CFTMI240 MIGR type=CAT, direct=FROMCAT, ifname=CFTCATA, ofname=catalog_output.xml
    ```
1. Load the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import the catalog using the command CFTMI. Replace the &lt;catalog_filename_new_installation&gt; as the `ofname `with the corresponding environment variable `CFTCATA`.
1. ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=CFTCATA
    ```

### Migrate the communication media files

1. Load the former Transfer CFT 2.7.0 environment.
1. Export the communication media file using the `CFTMI240` command.  
    ```
    CFTMI240 MIGR type=COM, direct=FROMCOM, ifname=<com_2.7.0_filename>, ofname=com_output.xml
    ```
1. Load the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
    &lt;/li&gt;
1. Import the communication media file using the `CFTMI` command. Use the corresponding environment variable for the `ofname`, in this case `CFTCOM`.
1. ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=CFTCOM
    ```
