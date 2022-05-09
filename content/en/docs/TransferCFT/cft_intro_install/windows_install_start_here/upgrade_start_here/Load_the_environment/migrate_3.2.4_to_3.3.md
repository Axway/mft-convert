{
    "title": "Migrating Transfer CFT  3.2.x, 3.3.2 or 3.4 to 3.10",
    "linkTitle": "Migrating from Transfer CFT 3.2.x or higher",
    "weight": "270"
}This topic describes how to migrate Transfer CFT 3.2.x, 3.3.2, or 3.4 to version {{< TransferCFT/axwayvariablesComponentVersion  >}}. It is divided in 2 sections, the first section describes migration for a single node architecture, and the second section multi-node architecture. Lastly there are instructions explaining what would be needed to migrate from single node architecture to multi node architecture.

Single node architecture
------------------------

Stop Transfer CFT and the Copilot server before starting the migration.

### Migrating the configuration

#### Migrating the main configuration and UCONF parameters

Migrate PARM, PART, IDF, other static configuration objects and UCONF parameters as follows:

1. Load the former Transfer CFT 3.x environment. See the [Migration prerequisites](../../../../unix_install_start_here/upgrade_start_here/load_the_environment) for details.
1. Export your static configuration objects using the command CFTUTIL CFTEXT. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```
1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} installation.
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Stop {{< TransferCFT/headerfootervariableshflongproductname  >}} if you have not already done so.
1. Import your static configuration objects using the cftinit command. Enter:  
    ```
1. cftinit cft-extract.conf

### Migrating PKI certificates

Stop Transfer CFT and the Transfer CFT Copilot server before starting.

1. Load the former Transfer CFT 3.x environment.
1. Export your PKI certificates using the command PKIUTIL PKIEXT. Enter:  
    ```
1. PKIUTIL PKIEXT fout=pki-extract.conf
1. Copy all files that are referenced in the pki-extractconf file (INAME OR IKNAME) to the folder where you are going to execute the PKI command on the new version. In the following example, copy ROOT0001 to the new folder in Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}.
1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;`pki_database_filename`&gt; with the appropriate value: $CFTPKU for UNIX, the absolute path value for the CFTPKU for Windows. Enter:
1. Import your PKI certificates into Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} using the command PKIUTIL. Replace the &lt;`prefix_character`&gt; based on your system, @ for UNIX and \# for Windows.  
    Enter:  
    ```
    PKIUTIL <prefix_character>pki-extract.conf
    ```

### Migrating the runtime environment

Stop Transfer CFT and the Transfer CFT Copilot server before starting.

#### Migrating the catalog

1. Load the former Transfer CFT 3.x environment.
1. Export the catalog using the command `CFTMI`. Replace the &lt;c`atalog_filename` &gt; with the corresponding environment variable, _CFTCATA for UNIX or $CFTCATA for Windows. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_former_cft>, ofname=catalog_output.xml
    ```
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Import the catalog using the command CFTMI. Replace the &lt;catalog_filename &gt; with the corresponding environment variable, _CFTCATA for UNIX or $CFTCATA for Windows. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=<catalog_filename_new_cft >
    ```

#### Migrating the communication media files

1. Load the former Transfer CFT 3.x environment.
1. Export the communication media file using command CFTMI. Replace the &lt;com_filename &gt; with the corresponding environment variable, _CFTCOM for UNIX, or $CFTCOM for Windows. Enter:  
    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_former_cft>, ofname=com_output.xml
    ```
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Import the communication media file using command CFTMI. Replace the &lt;com_filename &gt; with the corresponding environment variable, _CFTCOM for UNIX or $CFTCOM for Windows. Enter:  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_new_cft >
    ```

#### Executables and binaries

Remember that you can copy your post-processing scripts directly from the runtime/exec to the new version ({{< TransferCFT/axwayvariablesReleaseNumber  >}}). When you copy files from the exec folder, be certain to modify any paths that point to the former version (3.x in this case). However, you must rebuild APIs and EXITS (binaries).

Migrate a multi-node to multi-node architecture
-----------------------------------------------

### Migrating the configuration

Stop Transfer CFT and the Transfer CFT Copilot server before starting the migration.

#### Migrating the main configuration and UCONF parameters

Migrate PARM, PART, IDF, other static configuration objects and UCONF parameters as follows:

1. Load the former Transfer CFT 3.x environment.
1. Export your static configuration objects using the command CFTUTIL CFTEXT. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```
1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} installation.
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Import your static configuration objects using the cftinit command. Enter:  
    ```
    cftinit cft-extract.conf
    ```

### Migrating PKI certificates

Stop Transfer CFT and the Transfer CFT Copilot server before starting.

1. Load the former Transfer CFT 3.x environment.
1. Export your PKI certificates using the command PKIUTIL PKIEXT. Enter:  
    ```
    PKIUTIL PKIEXT fout=pki-extract.conf
    ```
1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;pki_database_filename&gt; with the appropriate value, $CFTPKU for UNIX or the absolute path value for the CFTPKU for Windows. Enter:  
    ```
    PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’
    ```
1. Import your PKI certificates into Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} using the command PKIUTIL. Replace the &lt;prefix_character&gt; based on your system, @ for UNIX and \# for Windows. Enter:  
    ```
    PKIUTIL <prefix_character>pki-extract.conf
    ```

### Migrating the runtime environment

Stop Transfer CFT and the Transfer CFT Copilot server before starting.

#### Migrating the catalog

1. Load the former Transfer CFT 3.x environment.
1. Export all catalogs (one per node, named as cftcataXX, where XX is the node number with range from 00 to &lt;number of nodes - 1&gt;) using the command CFTMI. For each catalog. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_former_cft_node_<node>>, ofname=catalog_output_<node>.xml
    ```
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Import all catalogs using the command CFTMI for each of them. Use the same node number on both &lt;node&gt; on command. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output_<node>.xml, ofname=<catalog_filename_new_cft_node_<node>>
    ```

#### Migrating the communication media files

1. Load the former Transfer CFT 3.x environment.
1. Export all communication media files (cftcom and cftcomXX, where XX is the node number with range from 00 to &lt;number of nodes - 1&gt;) using the command CFTMI.  
    For each communication media file, enter:
1. For each node, enter:
1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Import all communication media files using the CFTMI command.  
    For the manager, enter:
1. For each node, enter:` `

Single-node to multi-node architecture migration
------------------------------------------------

The only difference between migrating from single node to multi-node architecture and migrating from single-node to single-node architecture is the catalog migration step. Since there is no catalog named `cftcata `in multi-node, import the catalog exported from single-node architecture to the catalog of any of the nodes in the multi-node architecture.

Be certain to stop Transfer CFT and the Transfer CFT Copilot server before performing the procedure.
