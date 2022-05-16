---

    title: Migration or upgrade impact and considerations 
    linkTitle: Migration/upgrade impact and considerations
    weight: 80

---
You should be aware of the following features or parameters, which were modified since their inception, and how these changes may impact your upgrade or migration.

## Impact of features and parameters

QQQ\_QQQ\_QQQ


| Key element | Originating versions  | Updated versions  | Description  |
| --- | --- | --- | --- |
| API and Exits  | all versions  | to any version  | You must recompile any API or Exit programs that are used by Transfer CFT.  |
| Configuration cache feature<br/> (cft.server.parm.cache_size) | Lower than 3.8  | 3.8 and higher  | <span id="parmcache"></span>The default value is now 5000 instead of zero, making the cache feature active by default.<br/> This means that updates no longer occur dynamically; you can execute <span ><code>RECONFIG </code></span>type<span ><code>=PARMCACHE </code></span>or wait for a cache timeout as defined in <span ><code>cft.server.parm.cache_timeout (60 seconds).</code></span> |
| cft.listcat_compat = No<br/> cft.state_compat = No | Lower than 3.8  | 3.8 and higher  | Modified the default value for the <span ><code>cft.listcat_compat </code></span>(lstcompat) and <span ><code> cft.state_compat </code></span>(stacompat) parameters from YES to NO. |
| Amazon S3  | Lower than 3.8  | 3.8 and higher  | When using Amazon S3, the default setting FACTION=VERIFY is no longer ignored.<br/> If you would like to continue to have the same behavior of overwriting the file, please use FACTION=DELETE. Note, though, that the file is not available during the transfer. |
| CFTUIPREF  | Lower than 3.8  | 3.8 and higher  | After an upgrade you may need to check user privileges for creating filters in the CFTUIPREF object.  |
| Copilot Java applet  | Lower than 3.8  | 3.8 and higher  | The Copilot Java applet was removed from the product. Users are invited to use the Transfer CFT UI or Flow Manager for a graphical UI experience.  |
| SQLite database  | 3.7 and lower  | 3.8 and higher  | The CFTPARM object's PARTFNAM and PKIFNAME fields are obsolete for Windows, UNIX, and HP NonStop. |
| FTYPE=T<br/> *Windows only* | 3.2.4 to 3.7 without appropriate patch or SP*  | 3.8 and higher  | On Windows systems, note the following difference when FTYPE=T.<br/> • For versions 3.2.4 to 3.7 without the patch, an empty line terminated by a 1A character is transmitted.<br/> • Prior to 3.2.4 and for the versions with the SP or patch applied, an empty line terminated by a 1A character is not transmitted.<br/> *3.7 SP1 (patch), 3.3.2 SP8, 3.6 SP3, 3.8 |
| Visual C++ Redistributable Package for Visual Studio 2019 &lt;/td&gt;  | 3.6 and lower  | 3.7 and higher  | Transfer CFT on Windows requires the **Visual C++ Redistributable Package for Visual Studio 2019** for proper functioning. This provides the necessary library files (DLL) for Transfer CFT.<br/> You must install <code>vcredist_x64.exe</code> prior to installing or upgrading Transfer CFT.<br/> **Issue**<br/> If you perform an upgrade without first installing the Redistributable package, the runtime is not imported and Transfer CFT will not operate correctly. The following information displays in the <code>&lt;installdir&gt;/install.log</code> file:<br/> <code>Script stderr:</code><br/> <code>child killed: unknown signal</code><br/> <code> </code><br/> <code>Fail to import RUNTIME data.</code><br/> <code>Problem running post-install step. Installation may not complete correctly</code><br/> <code>Fail to import RUNTIME data.</code><br/> **Corrective action**<br/> • Install the Redistributable package.<br/> • From the <span ><code>cmd </code></span>console, load the profile.<br/> • Import the runtime data by running the import command to complete the upgrade.<br/> • Check that the script executed correctly. |
| LISTPKI  | 3.6 and lower  | 3.7 and higher  | To use the new LISTPKI format, copy the <code>dspcnf.xml</code> model file from <code>&lt;installdir&gt;/distrib/template/conf</code> to the <code>&lt;runtimedir&gt;/conf.</code>  |
| CFTACCNT  | 3.5 and lower  | 3.6 and higher  | Updated the documentation for the account file in v24 format. Please note the changes in field length as described in the CFTACCNT list. |
| SORTBY  | 3.5 and lower  | 3.6 and higher  | Catalog records are no longer displayed by IDTU. To have the same display as in previous versions, use the SORTBY parameter as follows:<br /> <code>listcat sortby=idtu</code> |
| EBICS  | 3.5 and lower  | 3.6 and higher  | Use the Axway EBICS client. Please refer to the <a href="https://docs.axway.com/bundle/EBICSClient_10_allOS_en_HTML5/page/ebics_client_documentation_home.html">EBICS client documentation</a> for product details. |
| BUFSIZE<br/> FBUFSIZE | 3.3.2 and lower<br/> *Unix and IBM i only* | 3.4, 3.6 SP2 and lower, 3.7 and 3.8 | A BUFSIZE or FBUFSIZE value greater than 32 kiB may lead to Transfer CFT failing to exchange messages between CFTTPRO and CFTTFIL. If you have set a value higher than 32 kiB, please decrease it to 32768.<br/> <blockquote> **Note**<br/> As of 3.6 SP3, 3.8 SP1, and 3.9, the internal value limit is 32768.<br/> </blockquote>  |
| PKIFNAME  | 3.4 and lower  | 3.5 and higher  | You can no longer reference a certificate with the PKIFNAME format (<span ><code>CFTPARM:PKIFNAME=TXT://certificate</code></span>). Previously, when implementing an integrated PKI, the PKIFNAME parameter could indicate a flat-file database (<span ><code>PKIFNAME=TXT://certificate</code></span>). If you were using this kind of file and then migrate, you must manually import all certificates into the PKI database. |
| CFTCRON  | Lower than 3.4  | 3.4 and higher  | An upgrade from a version lower than Transfer CFT 3.4 to 3.4 or higher may fail due to an incorrect time syntax because the CFTCRON time syntax is checked when creating or editing a CFTCRON object.  |
| PKIPASSW  | 3.3.2 or lower  | 3.4 and higher  | Removed the PKIPASSW parameter from PKI commands (still available for CFTPARM).<br/> <blockquote> **Note**<br/> In earlier versions of Transfer CFT, the PKIPASSW parameter was used for encryption in the multiple PKI commands. This functionality is now replaced by the UCONF crypto.key_fname parameter.<br/> </blockquote> <span >****Impact****</span><br/> If you are using PKIEXT to export keys during a manual migration, you must use the same PKIPASSW (CFTPARM object) as was originally used to import the key. Using the same logic, to re-import a key that you extracted using PKIEXT, you require the same CFTPARM <a href="../../c_intro_userinterfaces/command_summary/parameter_intro/pkipassw">PKIPASSW</a>.<br/> For information on exporting keys, please refer to <a href="../../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext">Using PKIEXT</a>. |
| 32-bit releases  | 3.3.2 and lower  | 3.4 and higher  | End of 32-bit version deliveries.  |
| Some default values  | 3.3.2 and lower  | 3.4 and higher  |   |
| cft.server.processing_scripts_variables_blacklist  | 3.3.2 SP3 and lower  | 3.3.2 SP4 and higher  | POSIX Regular Extended expression that defines forbidden characters.  |
| TLS  | 3.2.x and higher  | not applicable | When migrating to 3.2.x or higher, SSL transfers may fail with a DIAGP e105s86 or e75s89 when performing transfers with the versions listed below (with the error occurring on the remote {{< TransferCFT/suitevariablesTransferCFTName  >}}).<br/> Affected versions:<br/> • All 3.1.3 SP7 and lower<br/> • All 3.0.1 SP3 and lower<br/> On even older versions, we recommend setting the CFTPROT:CONCAT parameter to No. |
| CA certificate chains  | 3.1.3 and lower  | 3.2.2 and higher  | In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and lower, you can perform a SSL transfer even if the certificate chain is not complete (not signed by a ROOT CA).<br/> **Impact**<br/> In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher, the certificate chain must be complete for a transfer to succeed.<br/> For more information, see <a href="../../troubleshoot_intro/admin_troubleshooting_server/troubleshoot_security#Unknown" >Unknown CA leads to a failed certificate verification</a> |
| PKIPASSW  | 3.1.3 and lower  | 3.3.2 and higher  | When upgrading from 3.1.3 to 3.3.2, first check that the PKIPASSW length value is not greater than 8 characters.<br/> If the value is 8 or less, you can proceed with the upgrade.<br/> If the PKIPASSW value in the CFTPARM command is greater than 8 characters, perform the steps in the solution below.<br/> <span >****Solution**** </span><br/> Prior to migration you must truncate the password on the Transfer CFT 3.1.3:<br/> • Export the CFTPARM.<br /> <code>CFTUTIL cftext type=parm, fout=file_parm.out</code><br/> • Modify the PKIPASSW in the file. For example, if the old value was <span ><code>PKIPASSW=12345678910</code></span>, replace it with <span ><code>PKIPASSW=12345678.</code></span><br/> • Reimport:<br /> <span ><code>CFTUTIL config type=input,fname=file_parm.out</code></span><br/> • Continue the Transfer CFT 3.3.2 upgrade process. |
| Copilot client  | 3.1.3 or lower | 3.2.2 and higher  | The Copilot application changed from a Java applet to a Java Web Start program.<br/> <span >****Impact****</span><br/> Copilot requires Java 7 or higher. |
| ROOTCID=NONE  | 3.1.3  | 3.2.2 and higher  | Non authentication method was available in 3.1.3 and lower (anonymous TLS connection).<br/> <span >****Impact****</span><br/> This support has been removed in {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher. You must update the ROOTCID parameter. |
| TLS  | 3.1.3 or lower  | 3.2.2 and higher  | To comply with security standards, as of Transfer CFT version 3.2.2 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively.<br/> <span >****Impact****</span><br/> This means that if some of your partners use a version of Transfer CFT lower than 3.2.2 that does not support TLS 1.2, and you are using ciphers 59, 60 and 61, which requires TLS 1.2 in version 3.2.2 and higher, you must add another cipher in the cipher list and remove ciphers 59, 60, 61 from the partner's cipher list.<br/> <blockquote> **Note**<br/> You do not have to remove ciphers 59, 60, 61 in the partner cipher list if you apply the Transfer CFT patch 3.0.1 SP11.<br/> </blockquote>  |
| Rotate the log  | 3.0.1 or lower  | 3.1.3 and higher  | Changed the switch log feature behavior.<br/> In version 3.0.1 or lower, there were two files that automatically alternated.<br/> <span >****Impact****</span><br/> In version 3.1.3 and higher if you want to continue this functionality, you must set the alternate log file's uconf value <span ><code>cft.cftlog.afname</code></span> to the alternate file path (for example, <span ><code>$CFTRUNTIME/log/cftloga</code></span>). |
| Demo certificates  | 3.0.1 or lower  | 3.1.2 and higher  | Axway no longer delivers the template certificates used in the Transfer CFT SSL.<br/> ****Impact****<br/> If you were using the demo certificates, import your proper certificates and replace in the PKI database as the Demo certificates are expired. |
| CFTPARM <br/> key parameter | 2.7.1 or lower  | 3.0.1 and higher  | If you had the CFTPARM key parameter set directly to a value, you must modify this so that key parameter points to an indirection file containing the license key.  |


## Impact of default values

When migrating from 3.3.2 and lower to 3.4 and higher, be aware that some default values are subject to change.

Updated default values of the following parameters to optimize and standardize among platforms.

QQQ\_QQQ\_QQQ

### Default values relating to CFTPARM


| Parameter  | Old default  | New default  |
| --- | --- | --- |
| MAXTRANS | 128 (Win), 256 (os400, unix, vms), 990 (z/OS) | 256 |
| MAXTASK | 1 (Win), 16 (os400, unix, vms), 400 (z/OS) | 8 |
| TRANTASK | 14 (z/OS), 16 (os400, unix, vms), 128 (win) | 3 |
| WAITTASK | 1441 | 10 |
| SSLMTASK | 1 (Win), 16 (os400, unix, vms), 64 (z/OS) | 8 |
| SSLTTASK | 14 (z/OS), 16 (os400, unix, vms), 128 (win) | 3 |
| SSLWTASK | 1441 | 10 |


QQQ\_QQQ\_QQQ

### Default values relating to CFTNET


| Parameter  | Old default  | New default  |
| --- | --- | --- |
| type | x25 | TCP |
| maxcnx | 32 | 384 |


QQQ\_QQQ\_QQQ

### Default values relating to CFTPROT type=PeSIT prof=ANY


| Parameter  | Old default  | New default  |
| --- | --- | --- |
| concat | no | yes |
| multart | no | yes |
| segment | no | yes |
| rpacing | 36 | 32767 |
| spacing | 36 | 32767 |
| rrusize | 4056 | 32750 |
| srusize | 4056 | 32750 |
| disctc | 90 | 60 |
| disctd | 120 | 10 |
| disctr | 45 | 45 |
| discts | 165 | 60 |
| rchkw | 2 | 3 |
| schkw | 2 | 3 |
| rcomp | 10 | 0 |
| scomp | 10 | 0 |
| sserv  | PESIT  | GSIT  |


QQQ\_QQQ\_QQQ

### Default values relating to CFTPROT type=ODETTE


| Object  | Parameter  | Old default  | New default  |
| --- | --- | --- | --- |
| **CFTPROT type=ODETTE**  | tcp  | CFT  | OFTP  |


QQQ\_QQQ\_QQQ

### Default values relating to CFTTCP


| Parameter  | Old default  | New default  |
| --- | --- | --- |
| retryw | 7 | 1 |
| retryn | 6 | 4 |
| retrym | 12 | 12 |
| cnxinout | 2 | 4 |


**Impact**

Check the use in your flows and modify according.
