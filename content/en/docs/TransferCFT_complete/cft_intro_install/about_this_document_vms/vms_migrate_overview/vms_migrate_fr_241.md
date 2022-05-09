{
    "title": "Migrate from Transfer CFT 2.4.1",
    "linkTitle": "Migrate from Transfer CFT 2.4.1",
    "weight": "250"
}This section describes how to migrate from Transfer CFT 2.4 to version {{< TransferCFT/PrimaryTransferCFTversionlong  >}}. Before starting the migration procedure you must perform the steps described in [Before you start](../vms_migrate_before_you_start#Importan).

Migrate the configuration
-------------------------

### Migrate the main configuration

Migrate the PARM, PART, IDF and other static configuration objects.

1. Load the Transfer CFT 2.4 environment. See the [Before you start](../vms_migrate_before_you_start) for details.
1. Export your static configuration objects using the CFTUTIL CFTEXT command. Enter:  
      
    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```
1. Open the extract configuration files, cft-extract.conf, and update the file paths with those of the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} installation.
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import your static configuration objects using the cftinit command. Enter:  
    ```
    cftinit cft-extract.conf
    ```

### Migrating trkapi.cfg file parameters

Migrate the parameters from the Transfer CFT 2.4 trkapi.cfg file.

1. In the trkapi.cfg file, select the parameters you want to import in {{< TransferCFT/PrimaryTransferCFTversionlong  >}}.
1. Create a script file, for example:`  trkapi-import.com`
1. For each parameter you select, add a UCONF command line to your new script file using the format:  
    ```
    UCONFSET id=<parameter_id>, value=<value>
    ```
1. 
| Parameter in trkapi.cfg | UCONF parameter |
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


1. Load the Transfer CFT 3.2.1 environment.
1. Import the selected UCONF parameters using the `CFTUTIL` command. Replace &lt;script_filename&gt; with the new script file path.  
    ```
    CFTUTIL <prefix_character><script_filename>
    ```
1. ```
    CFTUTIL @trkapi-import.com
    ```

### Migrate copconf.ini parameters

Migrate parameters from the Transfer CFT 2.4 copconf.ini file.

1. From the copconf.ini file, select the parameters you want to import to version {{< TransferCFT/PrimaryTransferCFTversionlong  >}}.
1. Create a script file, for example: `copconf-import.com`
1. For each selected parameter add a UCONF command line in your new script file using the format:  
    ```
    UCONFSET id=<parameter_id>, value=<value>
    ```
      

    Use the parameters mapping between copconf and UCONF as shown in the following table to specify the correct parameter id.

      

    Copconf file to UCONF parameter mapping

      

    
| Parameter in copconf.ini | UCONF parameter |
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


1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import the selected UCONF parameters using the `CFTUTIL `command. Replace the &lt;script_filename&gt; with the new script file path.  
    ```
    CFTUTIL <prefix_character><script_filename>
    ```
1. ```
    CFTUTIL @copconf-import.com
    ```

### Migrate PKI certificates

You must be at Transfer CFT 2.4.1 SP5 or higher before performing this procedure.

1. Load the Transfer CFT 2.4 environment.
1. Export your PKI certificates using the `PKIUTIL PKIEXT` command:  
    ```
    PKIUTIL PKIEXT fout=pki-extract.conf
    ```
1. Load the new Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Create a new PKI internal datafile using the command PKIUTIL PKIFILE. Replace &lt;pki_database_filename&gt; with the variable `CFTPKU`.  
    ```
    PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
    ```
1. If an error occurs when importing your certificate, for example:  
    ```
    PKIU20I 
    DDecode_Cert : Failure, Unable to get info about the certificate
    PKIU26E PKICER   _ Error ( PKI decipher error {15026/0} (pkider(): Failed to decode DER certificate) )
    PKIU00I PKICER   _ Failed  (ID='CAXMP',ROOTCID='CAXMP',ITYPE='ROOT',COMMENT='S
    PKIU00I                     AMPLE CA',INAME='ROOT0001.',IFORM='DER',MODE='REPL
    PKIU00I                     ACE')
    PKIU00I RETURN   _ Correct (CODE=8)
    PKIU20I Number of Command(s) 1
    ```
1. ```
    SET FILE/ATTRIB=(LRL:4096,RFM:UDF) <mycertificate>
    ```
1. ```
    SET FILE/ATTRIB=(LRL:4096,RFM:UDF)  USER\*.\*
    SET FILE/ATTRIB=(LRL:4096,RFM:UDF)  ROOT\*.\*
    ```
1. Import your PKI certificates to Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} using the PKIUTIL command. Replace the &lt;script_filename&gt; with the new script file path.  
    ```
    PKIUTIL <prefix_character><script_filename>
    ```
1. ```
    PKIUTIL @pki-extract.com
    ```

Migrate the runtime environment
-------------------------------

### Migrate the catalog

1. Load the Transfer CFT 2.4 environment.
1. Define the symbolic variable:  
    ```
    CFTMI240 == "$CFT_EXE:CFTMI240"
    ```
1. Export the catalog using the `CFTMI240` command:  
    ```
    CFTMI240 MIGR type=CAT, direct=FROMCAT, ifname=CFTCATA, ofname=catalog_output.xml
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import the catalog using the `CFTMI` command. The `ofname `should be the corresponding environment variable, in this case `CFTCATA`.  
    ```
    CFTMI MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml, ofname=CATA
    ```

### Migrating the communication media files

1. Load the Transfer CFT V2.4 environment.
1. Define the symbolic name:  
    ```
    CFTMI240 == "$CFT_EXE:CFTMI240"
    ```
1. Export the communication media file using the `CFTMI240` command:  
    ```
    CFTMI240 MIGR type=COM, direct=FROMCOM, ifname=CFTCOM, ofname=com_output.xml
    ```
1. Load the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} environment.
1. Import the communication media file using the `CFTMI` command. Use the corresponding environmental variable as the `ofname`, in this case `CFTCOM.` Enter:  
    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml, ofname=CFTCOM
    ```
