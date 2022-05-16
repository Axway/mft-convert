---

    title: Migrating Transfer CFT 3.0.1 or 3.1.x to 3.9
    linkTitle: Migrate from Transfer CFT 3.0.1 
    weight: 270

---
This topic describes how to migrate Transfer CFT 3.0.1 or 3.1.2 to version {{< TransferCFT/axwayvariablesComponentVersion  >}}. It is divided in 2 sections, the first section describes migration for a single node architecture, and the second section multi-node architecture. Lastly there are instructions explaining what would be needed to migrate from single node architecture to multi node architecture.

> **Note**
>
> When migrating from 3.0.1 to 3.3.x, you must install SP10 P6 on all Transfer CFTs 3.0.1 that inter-operate with any Transfer CFTs 3.3.x prior to migrating.

## Single node architecture

### Migrating the configuration

#### Migrating the main configuration and UCONF parameters

Migrate PARM, PART, IDF, other static configuration objects and UCONF parameters as follows:

1. Load former Transfer CFT 3.0.1 or 3.1.x environment. See the <a href="../../../../unix_install_start_here/upgrade_start_here/load_the_environment" class="MCXref xref">Migration prerequisites</a> for details.
1. Export your static configuration objects using the command CFTUTIL CFTEXT. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```

<!-- -->

1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} installation.
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Stop {{< TransferCFT/headerfootervariableshflongproductname >}} if you have not already done so.
1. Import your static configuration objects using the cftinit command. Enter:  
    ```
1. cftinit cft-extract.conf

### Migrating PKI certificates

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export your PKI certificates using the command PKIUTIL PKIEXT. Enter:  
    ```
1. PKIUTIL PKIEXT fout=pki-extract.conf

<!-- -->

1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;pki\_database\_filename> with the appropriate value: $CFTPKU for UNIX, the absolute path value for the CFTPKU for Windows. Enter:  
    ```
    <span class="code">`PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’`</span>
    ```

<!-- -->

1. Import your PKI certificates into Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} using the command PKIUTIL. Replace the &lt;prefix\_character> based on your system, @ for UNIX and # for Windows.  
    Enter:  
    ```
    <span class="code">`PKIUTIL <prefix_character>pki-extract.conf`</span>
    ```

### Migrating the runtime environment

#### Migrating the catalog

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export the catalog using the command CFTMI. Replace the &lt;catalog\_filename > with the corresponding environment variable, \_CFTCATA for UNIX or $CFTCATA for Windows. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_former_cft>, ofname=catalog_output.xml
    ```

<!-- -->

1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Import the catalog using the command CFTMI. Replace the &lt;catalog\_filename > with the corresponding environment variable, \_CFTCATA for UNIX or $CFTCATA for Windows. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=<catalog_filename_new_cft >
    ```

#### Migrating the communication media files

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export the communication media file using command CFTMI. Replace the &lt;com\_filename > with the corresponding environment variable, \_CFTCOM for UNIX, or $CFTCOM for Windows. Enter:  
    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_former_cft>, ofname=com_output.xml
    ```

<!-- -->

1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Import the communication media file using command CFTMI. Replace the &lt;com\_filename > with the corresponding environment variable, \_CFTCOM for UNIX or $CFTCOM for Windows. Enter:  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_new_cft >
    ```

#### Executables and binaries

Remember that you can copy your post-processing scripts directly from the runtime/exec to the new version ({{< TransferCFT/axwayvariablesReleaseNumber  >}}). When you copy files from the exec folder, be certain to modify any paths that point to the former version (for example, 3.0.1, 3.1.3, or 3.2.x). However, you must rebuild APIs and EXITS (binaries).

## Multi-node architecture

### Migrating the configuration

#### Migrating the main configuration and UCONF parameters

Migrate PARM, PART, IDF, other static configuration objects and UCONF parameters as follows:

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export your static configuration objects using the command CFTUTIL CFTEXT. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```

<!-- -->

1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} installation.
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Import your static configuration objects using the cftinit command. Enter:  
    ```
1. <span class="code">`cftinit cft-extract.conf`</span>

### Migrating PKI certificates

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export your PKI certificates using the command PKIUTIL PKIEXT. Enter: <span class="code">`PKIUTIL PKIEXT fout=pki-extract.conf`</span>

<!-- -->

1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace <pki_database_filename> with the appropriate value, $CFTPKU for UNIX or the absolute path value for the CFTPKU for Windows. Enter:  
    ```
1. <span class="code">`PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’`</span>

<!-- -->

1. Import your PKI certificates into Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} using the command PKIUTIL. Replace the &lt;prefix\_character> based on your system, @ for UNIX and # for Windows. Enter:  
    ```
    PKIUTIL <prefix_character>pki-extract.conf
    ```

### Migrating the runtime environment

#### Migrating the catalog

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export all catalogs (one per node, named as cftcataXX, where XX is the node number with range from 00 to &lt;number of nodes - 1>) using the command CFTMI. For each catalog. Enter:  
    ```
1. <span class="code">`CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_former_cft_for_node_<node>>, ofname=catalog_output_<node>.xml`</span>

<!-- -->

1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Import all catalogs using the command CFTMI for each of them. Use the same node number on both <node> on command. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog\_output\_&lt;node>.xml, ofname=&lt;catalog\_filename\_new\_cft>&lt;node>
    ```

#### Migrating the communication media files

1. Load former Transfer CFT 3.0.1 or 3.1.2 environment.
1. Export all communication media files (cftcom and cftcomXX, where XX is the node number with range from 00 to &lt;number of nodes - 1>) using the command CFTMI. For each communication media file.
    -   Enter: <span class="code">`CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_for_node_manager_on_former_cft>, ofname=com_output.xml`</span>
    -   For each node, enter: <span class="code">`CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_for_node_<node>_on_former_cft>, ofname=com_output_<node>.xml`</span>
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.
1. Import all communication media files using command CFTMI for each of them. Use the same node number on both &lt;node> on command.
    -   Enter: <span class="code">`CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_for_node_manager_on_new_cft> `</span>
    -   For each node, enter: <span class="code">`CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput_<node>.xml, ofname=<com_filename_for_node_<node>_on_new_cft> `</span>

## Single-node to multi-node architecture migration

The only difference between migrating from single node to multi-node architecture and migrating from single-node to single-node architecture is the catalog migration step. Since there is no catalog named cftcata in multi-node, import the catalog exported from single-node architecture to the catalog of any of the nodes in the multi-node architecture.