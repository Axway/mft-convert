---
title: "Migrate from Transfer CFT 3.0.1 through 3.5 to 3.10"
linkTitle: "Migrate from Transfer CFT 3.0.1 through 3.5 to 3.6"
weight: 270
--- This section describes how to migrate Transfer CFT 3.0.1 or 3.1.3 to version {{< TransferCFT/PrimaryTransferCFTversionlong  >}}. It is divided in 2 sections, the first section describes migration for a single- node architecture, and the second section describes migration for a multi- node architecture. Lastly, there are instructions explaining what would be needed to migrate from a single- node architecture to multi node architecture.

****Be sure to first read the [Before you start](../vms_migrate_before_you_start).****

> **Note**
>
> When migrating from 3.0.1 to 3.3.x, you must install SP10 P6 on all Transfer CFTs 3.0.1 that inter- operate with any Transfer CFTs 3.3.x prior to migrating.

## Single node architecture

### Migrate the configuration

#### Migrate the main configuration and UCONF parameters

Migrate the PARM, PART, IDF, other static configuration objects, and UCONF parameters as follows:

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment. See the [Before you start](../vms_migrate_before_you_start) for details.
1. Export your static configuration objects using the `CFTUTIL CFTEXT` command. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft- extract.conf
    ```
1. Open the extracted configuration files, `cft- extract.conf`, and update the file paths with those of the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} installation.
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Import your static configuration objects using the `cftinit `command. Enter:

### Migrate the PKI certificates

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Export your PKI certificates using the `PKIUTIL PKIEXT` command. Enter:  
    ```
    PKIUTIL PKIEXT fout=pki- extract.conf
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Create a new PKI internal datafile using the `PKIUTIL PKIFILE` command.  
    ```
    PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
    ```
1. Import your PKI certificates into Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} using the `PKIUTIL` command. Replace the &lt;prefix_character> with `@` for OpenVms. Enter:  
    ```
    PKIUTIL <prefix_character>pki- extract.conf
    ```

### Migrate the runtime environment

#### Migrate the catalog

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Export the catalog using the `CFTMI `command. Enter:  
    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=CFTCATA, ofname=catalog_output.xml
    ```
1. Load Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Import the catalog using the `CFTMI `command.

#### Migrate the communication media files

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Export the communication media file using the `CFTMI `command.  
    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=CFTCOM, ofname=com_output.xml
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Import the communication media file using the `CFTMI `command.  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=CFTCOM
    ```

## Multi node architecture

### Migrate the configuration

#### Migrate the main configuration and UCONF parameters

Migrate the PARM, PART, IDF, other static configuration objects, and UCONF parameters as follows:

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Export your static configuration objects using the `CFTUTIL CFTEXT` command. Enter:  
    ```
    CFTUTIL CFTEXT type=all, fout=cft- extract.conf
    ```
1. Open the extract configuration files, `cft- extract.conf`, and update the file paths with those of the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} installation.
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Import your static configuration objects using the `cftinit `command. Enter:  
    ```
    cftinit cft- extract.conf
    ```

### Migrate the PKI certificates

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Export your PKI certificates using the `PKIUTIL PKIEXT` command. Enter:  
    ```
    PKIUTIL PKIEXT fout=pki- extract.conf
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Create a new PKI internal datafile using the `PKIUTIL PKIFILE` command. Replace &lt;pki_database_filename> with the value `CFTPKU` for OpenVMS. Enter:  
    ```
    PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’
    ```
1. Import your PKI certificates to Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} using the `PKIUTIL `command. Replace the &lt;prefix_character> with `@` for OpenVMS. Enter:  
    ```
    PKIUTIL <prefix_character>pki- extract.conf
    ```

### Migrate the runtime environment

#### Migrate the catalog

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Use the `CFTMI `command to export all catalogs, one per node (named cftcataXX, where XX is the node number having a range from 00 to the &lt;number of nodes - 1>). For each catalog, enter:  
    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_former_cft_for_node_<node>>, ofname=catalog_output_<node>.xml
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong >}} environment.
1. Import all catalogs using the CFTMI command for each. Use the same node number on both &lt;node> elements in the command. Enter:  
    ```
    CFTM MIGR type=CAT, direct=TOCAT, ifname=catalog_output_<node>.xml, ofname=<catalog_filename_new_cft><node>
    ```

#### Migrate the communication media files

1. Load the former Transfer CFT 3.0.1 or 3.1.3 environment.
1. Use the CFTMI command to export all communication media files (cftcom and cftcomXX, where XX is the node number having a range from 00 to &lt;number of nodes - 1>).

- For each communication media file, enter:  
    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_for_node_manager_on_former_cft>, ofname=com_output.xml
    ```
- For each node, enter:  
    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=<com_filename_for_node_<node>_on_former_cft>, ofname=com_output_<node>.xml
    ```

Load Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.

Import all communication media files using the `CFTMI` command for each of them. Use the same node number for both &lt;node> in the command.

- Enter:  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_for_node_manager_on_new_cft>
    ```
- For each node, enter:  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput_<node>.xml, ofname=<com_filename_for_node_<node>_on_new_cft>
    ```

## Single node to multi- node architecture

The only difference between migrating from a single node to multi- node architecture, and migrating from single node to single node architecture is the catalog migration step. Since there is no CFTCATA catalog in multi- node, you should import the catalog exported from the single node architecture to each of the node catalogs in your multi- node architecture.
