---

    title: Migrate from Transfer CFT 2.5 or 2.6 to 3.9
    linkTitle: Migrating from Transfer CFT 2.5 or 2.6.x
    weight: 230

---
This topic describes how to migrate Transfer CFT 2.5 or 2.6 to version {{< TransferCFT/axwayvariablesComponentVersion  >}}. Before starting this migration procedure, review the prerequisites and information on [loading the environment](../). Additionally, you must have installed your new {{< TransferCFT/axwayvariablesComponentShortName  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}} and applied the most recent service pack.

## Migrate the configuration

### Migrating the main configuration

Migrate PARM, PART, IDF and other static configuration objects.

1. Load the former Transfer CFT (2.5 or 2.6) environment. See the <a href="../" class="MCXref xref">Migration prerequisites</a> for details.

<!-- -->

1. Export your static configuration objects using the command CFTUTIL CFTEXT. Enter:

```
<span class="code">`CFTUTIL CFTEXT type=all, fout=cft-extract.conf`</span>
```

1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} installation.

<!-- -->

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.

<!-- -->

1. Stop {{< TransferCFT/headerfootervariableshflongproductname >}} if you have not already done so.
1. Import your static configuration objects using the cftinit command.  
    Enter:

```
cftinit cft-extract.conf
```

### Migrating UCONF parameters

1. Load the former Transfer CFT (2.5 or 2.6) environment.

<!-- -->

1. Display your UCONF parameters using the CFTUTIL LISTUCONF command. Enter: <span class="code">`CFTUTIL LISTUCONF scope=user`</span>

<!-- -->

1. Select the UCONF parameters that you want to import into the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}}.

<!-- -->

1. Create a script file such as:

- UNIX: <span class="code">`uconf-import.sh`</span>

- Windows: <span class="code">`uconf-import.bat`</span>

1. For each parameter you select, add a line to the new script file in the format:

```
UCONFSET id=<parameter_id>, value=<value>
```

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.

<!-- -->

1. Import the selected UCONF parameters using the script file and the CFTUTIL command. Replace the &lt;script\_filename> with the new script file path:

```

<span class="code">`CFTUTIL <prefix_character><script_filename>`</span>

```

****Example****

- UNIX: CFTUTIL @uconf-import.sh

<!-- -->

- Windows: CFTUTIL #uconf-import.bat

### Migrating PKI certificates

For Transfer CFT 2.5, you must be at Transfer CFT 2.5.1 SP2 or higher before performing this procedure. For Transfer CFT 2.6.4, you must be at Transfer CFT 2.6.4 SP2 or higher before performing this procedure.

1. Load the former Transfer CFT environment (2.5 or 2.6).

<!-- -->

1. Export your PKI certificates using the command PKIUTIL PKIEXT: <span class="code">`PKIUTIL PKIEXT fout=pki-extract.conf`</span>

<!-- -->

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.

<!-- -->

1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;pki\_database\_filename> with the appropriate value: <span class="code">`PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’`</span>

- UNIX: $CFTPKU

- Windows: The absolute path value for the CFTPKU environment variable

1. Import your PKI certificates into the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} using the command PKIUTIL. Replace the &lt;script\_filename> with the new script file path: <span class="code">`PKIUTIL <prefix_character><script_filename>`</span>

****Example****

- UNIX: <span class="code">`PKIUTIL @pki-extract.conf`</span>

<!-- -->

- Windows: <span class="code">`PKIUTIL #pki-extract.conf`</span>

## Migrating the runtime environment

### Migrating the catalog

1. Load the former Transfer CFT (2.5 or 2.6) environment.

<!-- -->

1. Export the catalog using the command CFTMI240.

```
CFTMI240 MIGR type=CAT, direct=FROMCAT, ifname=<catalog_2.5_filename>, ofname=catalog_output.xml
```

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.

<!-- -->

1. Import the catalog using the command CFTMI. Replace the &lt;catalog\_filename\_new\_installation> with the corresponding environment variable:

- UNIX: \_CFTCATA
- Windows: $CFTCATA

<span class="autonumber"></span>Example

```
CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=<catalog_filename_new_installation>
```

### Migrating the communication media files

1. Load the former Transfer CFT (2.5 or 2.6) environment.

<!-- -->

1. Export the communication media file using command CFTMI240:

```
CFTMI240 MIGR type=COM, direct=FROMCOM, ifname=<com_2.5_filename>, ofname=com_output.xml
```

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion >}} environment.

<!-- -->

1. Import the communication media file using command CFTMI. Replace the &lt;com\_filename\_new\_installation> with the corresponding environment variable:

- UNIX: <span class="code">`_CFTCOM`</span>

<!-- -->

- Windows: <span class="code">`$CFTCOM`</span>

<span class="autonumber"></span>Example

```
CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_new_installation>
```
