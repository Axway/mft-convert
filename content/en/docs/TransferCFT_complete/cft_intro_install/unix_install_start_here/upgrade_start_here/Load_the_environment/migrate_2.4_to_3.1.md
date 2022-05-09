{
    "title": "Migrating from Transfer CFT 2.4 to 3.10",
    "linkTitle": "Migrating from Transfer CFT 2.4.x",
    "weight": "210"
}This topic describes how to migrate from Transfer CFT 2.4 to version {{< TransferCFT/axwayvariablesComponentVersion  >}}. Before starting this migration procedure, review the prerequisites and information on [loading the environment](../). Additionally, you must have installed your new {{< TransferCFT/axwayvariablesComponentShortName  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}} and applied the most recent service pack.

Migrating the configuration
---------------------------

### Migrating the main configuration

Migrate PARM, PART, IDF and other static configuration objects.

1. Load the Transfer CFT 2.4 environment. See the [Migration prerequisites](../) for details.

<!-- -->

1. Export your static configuration objects using the command CFTUTIL CFTEXT.  
    Enter: `CFTUTIL CFTEXT type=all, fout=cft-extract.conf`

<!-- -->

1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} installation.

<!-- -->

1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.
1. Stop {{< TransferCFT/headerfootervariableshflongproductname  >}} if you have not already done so.

<!-- -->

1. Import your static configuration objects using the cftinit command. Enter:

```
cftinit cft-extract.conf
```

### Migrating trkapi.cfg file parameters

Migrate the parameters from the Transfer CFT 2.4 trkapi.cfg file.

1. In the trkapi.cfg file, select the parameters you want to import in {{< TransferCFT/axwayvariablesComponentVersion  >}}.

<!-- -->

1. Create a script file, for example:

- UNIX:` trkapi-import.sh`
- Windows:` trkapi-import.bat`

1. For each parameter you select, add a UCONF command line to your new script file using the format:

```
UCONFSET id=<parameter_id>, value=<value>
```

Use the parameter mapping between trkapi and UCONF, as listed in the following table, to specify the correct parameter id.

Parameter mapping between the trkapi.cfg file and UCONF


| Parameter in trkapi.cfg | Parameter names in UCONF |
| --- | --- |
| TRACE | sentinel.trktrace |
| TRKGMTDIFF | sentinel.trkgmtdiff |
| TRKIPADDR_BKUP | sentinel.trkipaddr_bkup |
| TRKIPPORT | sentinel.trkipport |
| TRKIPPORT_BKUP | sentinel.trkipport_bkup |
| TRKLOCALADDR | sentinel.trklocaladdr |
| TRKPRODUCTNAME | sentinel.trkproductname |
| XFB.BufferSize | sentinel.xfb.buffer_size |
| XFB.Log (UNIX) | sentinel.xfb.log |
| XFBLOG (Windows) | sentinel.xfb.log |
| XFB.Sentinel | sentinel.xfb.enable |
| XFB.Trace | sentinel.xfb.trace |
| XFB.Transfer | sentinel.xfb.transfer |


1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.

<!-- -->

1. Import the selected UCONF parameters using the command CFTUTIL. Replace &lt;script_filename&gt; with the new script file path.

```
CFTUTIL <prefix_character><script_filename>
```

****Example****

- UNIX: CFTUTIL @trkapi-import.sh
- Windows: CFTUTIL \#trkapi-import.bat

### Migrating copconf.ini parameters

Migrate parameters from the Transfer CFT 2.4 copconf.ini file.

1. From the copconf.ini file, select the parameters you want to import into version {{< TransferCFT/axwayvariablesComponentVersion  >}}.

<!-- -->

1. Create a script file, for example:

- UNIX: copconf-import.sh
- Windows: copconf-import.bat

1. For each selected parameter add a UCONF command line in your new script file using the format:

`UCONFSET id=<parameter_id>, value=<value>`

Use the parameters mapping between copconf and UCONF as listed in the following table to specify the correct parameter id.

Parameter mapping between copconf file and UCONF


| Parameter in copconf.ini | Parameter name in UCONF |
| --- | --- |
| BatchList | copilot.batches |
| CFTCOM | copilot.cft.com |
| CFTMEDIACOM | copilot.cft.mediacom |
| ChildProcessTimeout | copilot.misc.childprocesstimeout |
| HttpRootDir | copilot.http.httprootdir |
| MinNbProcessReady | copilot.misc.minnbprocessready |
| NbProcessToStart | copilot.misc.nbprocesstostart |
| NBWAITCFTCATA | copilot.cft.nbwaitcftcata |
| ServerHost | copilot.general.serverhost |
| ServerPort | copilot.general.serverport |
| SslCertFile | copilot.ssl.sslcertfile |
| SslCertPassword | copilot.ssl.sslcertpassword |
| SslKeyFile | copilot.ssl.sslkeyfile |
| SslKeyPassword | copilot.ssl.sslkeypassword |
| TcpTimeout | copilot.misc.tcptimeout |
| TIMERWAITCFTCATA | copilot.cft.timerwaitcftcata |
| TrcMaxLen | copilot.trace.trcmaxlen |
| TrcType | copilot.trace.trctype |
| wlogComment | copilot.batches.wlog.comment |
| wlogParams | copilot.batches.wlog.params |
| WsiComplience | copilot.webservices.wsicomplience |


1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.

<!-- -->

1. Import the selected UCONF parameters using the command CFTUTIL. Replace the &lt;script_filename&gt; with the new script file path.

```
CFTUTIL <prefix_character><script_filename>
```

****Example****

- UNIX: CFTUTIL @copconf-import.sh

<!-- -->

- Windows: CFTUTIL \#copconf-import.bat

### Migrating PKI certificates

You must be at Transfer CFT 2.4.1 SP5 or higher before performing this procedure.

1. Load the Transfer CFT 2.4 environment.

<!-- -->

1. Export your PKI certificates using the command PKIUTIL PKIEXT:

```
PKIUTIL PKIEXT fout=pki-extract.conf
```

1. Load the new Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.

<!-- -->

1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;pki_database_filename&gt; with the appropriate variable:

- UNIX: $CFTPKU

<!-- -->

- Windows: The absolute path value for the CFTPKU environment variable

```
PKIUTIL PKIFILE fname=<pki_database_filename>, mode='CREATE’
```

1. Import your PKI certificates into Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} using the command PKIUTIL. Replace the &lt;script_filename&gt; with the new script file path.

```
PKIUTIL <prefix_character><script_filename>
```

****Example****

- UNIX: PKIUTIL @pki-extract.conf

<!-- -->

- Windows: PKIUTIL \#pki-extract.conf

Migrating the runtime environment
---------------------------------

### Migrating the catalog

1. Load the Transfer CFT 2.4 environment.

<!-- -->

1. Export the catalog using the command CFTMI240:

```
CFTMI240 MIGR type=CAT, direct=FROMCAT, ifname=<catalog_2.4_filename>, ofname=catalog_output.xml
```

1. Load the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.

<!-- -->

1. Import the catalog using the command CFTMI. Replace the &lt;catalog_filename_new_installation&gt; with the corresponding environment variable:

- UNIX: _CFTCATA

<!-- -->

- Windows: $CFTCATA

```
CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=<catalog_filename_new_installation>
```

### Migrating the communication media files

1. Load the Transfer CFT V2.4 environment.

<!-- -->

1. Export the communication media file using command CFTMI240:

```
CFTMI240 MIGR type=COM, direct=FROMCOM, ifname=<com_2.4_filename>, ofname=com_output.xml
```

1. Load Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} environment.

<!-- -->

1. Import the communication media file using command CFTMI. Replace &lt;com_filename_new_installation&gt; with the corresponding environment variable:

- UNIX: _CFTCOM

<!-- -->

- Windows: $CFTCOM

```
CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=<com_filename_new_installation>
```
