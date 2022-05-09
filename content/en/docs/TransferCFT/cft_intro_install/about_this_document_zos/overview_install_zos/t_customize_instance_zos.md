{
    "title": "Common installation steps (A03PARM and A12OPTSP)",
    "linkTitle": "Common installation steps (A03PARM and A12OPTSP)",
    "weight": "180"
}This section applies to either an SMP/E or non-SMP/E installation.

<span id="kanchor29"></span>

Customize parameters in the JCL
-------------------------------

- [Set standard JCL parameters A03PARM](#Modifying_A03PARM)
- [Configure the SGINSTAL using UCONF or A12OPTSP](#Selectin)

Execute the JOB:

1. Adapt the customization parameters in the [A03PARM](#Modifying_A03PARM) member.
1. Submit the JOB.
1. Quit both EDIT and the library.
1. Wait for the JOB to execute.

When you modify the A03PARM member, respect the following keyword conventions:

- Enter keywords in lower case
- Enter keyword values between quotes ' '
- Refer to the Keyword description table for details, and adapt bold parameters to suit your environment

<span id="Modifying_A03PARM"></span>

Set standard JCL parameters A03PARM
-----------------------------------

Before submitting JCL A00CUSTO to customize the **Target.INSTALL** library JCL, modify the Target.INSTALL (A03PARM) member.

**<span id="Keyword descriptions zOS JCL "></span>Keyword descriptions**

When modifying the A03PARM member, adapt any parameters displayed in bold text to suit your environment. If you used the installer to install, the starred (\*) parameters are already customized.

<span id="Password"></span>

### Password encryption (CFTGNKEY)

Before you can customize the instance environment, you require a password to generate a key to use for internal encryption. The password you enter must be at least 8 characters long, contain upper and lower case characters as well as numeric and special characters (\*\#$!?+-@).

The password is temporarily stored in the '`pswfname`' file, with the syntax `--pass <password>`, and is then removed after installation. See [pswfname](#pswfname) for details.

### Environment customization


| Keyword  | Default  | Description  |
| --- | --- | --- |
| jobname | '&amp;%strip(substr(userid(),1,6))&quot;I&quot;''  | Name of the JOB for submitting the installation JCLs **(*)**. |
| userid | '&amp;SYSUID'  | Name of user authorized to submit the installation JCLs. |
| account | 'valacc'  | Account number with which the installation JCLs are submitted **(*)**. |
| msgclass | 'MSGCLASS=R,'  | Message output class. To add other JOB statement parameter use, for example: 'MSGCLASS=R,REGION=0M,' |
| class | 'CLASS=P,'  | JCL installation execution class **(*)**.<br/> Keyword and value with a comma. The keyword can be omitted by using (two) single quotation marks ''. |
| jobparm | '//*JOBPARM ROOM=CFTx'  | Parameters relating to JES2 installation JOBs **( //* if none)**. |
| sysout | '*'  | Execution report output class. |
| sysda | 'VIO'  | UNITNAME for the LINK-EDIT temporary files (VIO/SYSDA). |
| lepfx | 'CEE'  | LE Language Environment libraries alias. |
| cpfx | 'CBC’  | C/C++ libraries alias. |
| isppfx | 'ISP'  | ISPF libraries prefix. |
| asmcmp | 'ASMA90'  | High Level Assembler. |
| lcobol | 'IGY'  | COBOL data set prefix that is used in JCL I91APICP. |
| acmp  | 'yes'  | Compile/link ASM EXITs and APIs.  |
| cobcmp  | 'yes'  | Compile/link COBOL EXITs and APIs.  |
| ccmp  | 'yes'  | Compile/link C EXITs and APIs.  |
| cftvol | 'volsert'  | Name of the volume for Transfer CFT z/OS file creation **(*)**. |
| cftunit | '3390'  | Disk unit type. |
| cftv2 | '&amp;&amp;TARGET'  | Transfer CFT z/OS file creation alias **(*)**. |
| hostadd  | '&amp;&amp;HOSTBYADDR'  | Host address (or '&amp;&amp;HOSTID')  |


#### Transfer CFT loadlib management

{{< TransferCFT/axwayvariablesComponentLongName  >}} z/OS allows you to concatenate two libraries, a user library and the product library. The user library is not mandatory, but is strongly advised, and should be positioned first.

> **Note**

- A `..USER.LOAD` load is created during the installation.
- If the Transfer CFT LOAD is an APF, the USER load must also be an APF.
- Set `cftuload '&&TARGET".LOAD"'` to manage only one LOAD.


| Keyword  | Default  | Description  |
| --- | --- | --- |
| cftuload  | '&amp;&amp;TARGET&quot;.USER.LOAD&quot;'  | First loadlib in steplib / joblib  |
| cftload  | '&amp;&amp;TARGET&quot;.LOAD&quot;'  | Second loadlib in steplib / joblib  |
| syslmods  | '&amp;%$cftuload'  | SYSLMOD (link output) for SGINSTAL Or $cftload  |
| syslmodx  | '&amp;%$cftuload'  | SYSLMOD for Exit(s) and/or API(s) or $cftload  |


#### Transfer CFT parameters customization


| Keyword  | Default  | Description  |
| --- | --- | --- |
| <span id="pswfname"></span>pswfname  | '&amp;&amp;TARGET&quot;.UPARM(GENKEY)&quot;'  | The password, required to generate the key for installation, is temporarily stored in the 'pswfname' file (and is removed after installation).<br/> **Syntax**: <code>--pass &lt;password&gt;</code> |
| keyfname  | '&amp;&amp;TARGET&quot;.CRYPKEY&quot;'  | File in which the generated key is stored.  |
| sltfname  | '&amp;&amp;TARGET&quot;.CRYPSALT&quot;'  | File in which the computed salt is stored.  |
| stacompat  | 'NO'  | Catalog state compatibility: cft.state_compat  |
| lstcompat  | 'NO'  | Catalog state compatibility: cft.listcat_compat  |
| pesitany  | '1761'  | Port id defined in the protocol PeSIT ANY parameter.  |
| pesitssl  | '1762'  | Protocol PeSIT SSL port.  |
| sftpprot  | '1763'  | Protocol SFTP port.  |
| apisp | '1765'  | Synchronous API TCP/IP port (The address is 127.0.0.1 in* ..SAMPLE(CFTPARM) cftcom). |
| idparm  | 'IDPARM0'  | CFTPARM identifier: (cft.idparm).<br/> • This parameter is set during installation.<br/> • JCL CFTMAIN uses this parameter, where MNRMAIN (PARM=). |
| cftinst  | '&amp;%&quot;Z11&quot;$pesitany'  | The Transfer CFT instance ID, CFTPARM partner (value size &lt;= 24). This value identifies the Transfer CFT and must be unique (cft.instance_id).<br/> <code>If Composer is enabled, the naming conventions differs:</code><br/> • Value size &lt;= 8<br/> • First alphabetic character<br/> • Naming convention: the same as the PDS’s member<br/> The sentence '&amp;%Mvsvar(&quot;SYSNAME&quot;)&quot; &quot;$pesitany' is replaced with the result of the REXX function Mvsvar(&quot;SYSNAME&quot;)&quot; concatenated with the value of the previously customized pesitany field.<br/> &quot;Z11&quot; represents the z/OS partition’s name. For example, $pesitany corresponds to the value assigned to keyword 'pesitany'. |
| cftgroup  | 'Production.zos'  |  Transfer CFT instance GROUP  |


#### Transfer CFT {{< TransferCFT/suitevariablesCopilotName  >}} server customization


| Keyword  | Default  | Description  |
| --- | --- | --- |
| copenable | 'yes'  | Enable the {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/suitevariablesCopilotName  >}} server. |
| copladdr | '&amp;&amp;HOSTBYADDR'  | Transfer CFT {{< TransferCFT/suitevariablesCopilotName  >}} server TCP/IP address. The key word '&amp;&amp;HOSTBYADDR' is substituted by the result of the REXX function socket (&quot;GETHOSTBYADDR&quot;). (copilot.general.serverhost) |
| coplport | '1766'  | Transfer CFT {{< TransferCFT/suitevariablesCopilotName  >}} listening port (copilot.general.serverport). |
| coplsslp  | '1767'  | Copilot server SSL listening port. *Mandatory for {{< TransferCFT/PrimaryCGorUM  >}} (copilot.general.ssl_serverport). |
| restenable  | 'yes'  | Enable Copilot REST API (Yes/No).  |
| restport  | '1768'  | Copilot REST API server port  |


#### Transfer CFT Heartbeat for Sentinel Dashboards


| Keyword  | Default  | Description  |
| --- | --- | --- |
| jobcft  | 'S:&amp;:1.8SJOBNAME'  | Transfer CFT job name.  |
| jobcop  | 'N:COPP'  | Transfer CFT Copilot server port.<br/> * 'COPP' is a keyword, and the asterisk value * is found via the uconf file. |


#### Certificates management


| Keyword  | Default  | Description  |
| --- | --- | --- |
| xpkiopt  | 'YES'  | Exit PKI used (YES or NO)<br/> Note that xpkiopt=YES is mandatory when **pkitype**=system, and that you should use the YES option when possible, even if using pkitype=**cft** or pkitype=**passport**. |
| csflib  | 'CSF.SCSFMOD0'  | CSF library, mandatory if xpkiopt=yes  |
| pkitype  | 'cft'  | Possible values:<br/> • cft: certificates are stored in a PKI internal datafile<br/> • passport: manages certificates via PassPort<br/> • system: PKI system is activated |
| pasaddr | 'pasaddr'  | If pkitype=**passport** then the PassPort server TCP/IP listening address. |
| pasport | '7000'  | PassPort server port (7000). |
| exitpki  | 'YES'  |   |


#### Sentinel installation customization


| Keyword  | Default  | Description  |
| --- | --- | --- |
| snenable  | 'no'  | Enable Sentinel services.  |
| sntlload  | '&amp;&amp;TARGET&quot;.LOAD&quot;'  | Location of the Universal Agent (TRKUTIL Load library).  |
| sntllocad | '127.0.0.1'  | Local TCP/IP address for Sentinel. |
| sntladdr | 'sntl.srv.xx'  | Sentinel server or Event Router address (TCP/IP). |
| sntlp | '1305'  | Sentinel server or Event Router port (TCP/IP). |
| sntlgstr | 'CFTLG30X'  | LOGGER file identification with a maximum length of 26 characters, or '', available exclusively with the Event Router. |


#### Parameters for RACF (or SAF enabled) control of Transfer CFT

Use these parameters only with the {{< TransferCFT/axwayvariablesComponentShortName  >}} z/OS security setup described in [Setting up RACF Security]().


| Keyword  | Default  | Description  |
| --- | --- | --- |
| <code>grpcft </code> | <code>'grpcft' </code> | <code>Transfer CFT administrator SAF group.</code> |
| grpmon  | 'grpmon'  | Transfer CFT SAF group.  |
| grpaprm  | 'grpaprm'  | All parameters access SAF group.  |
| grpfprm  | 'grpfprm'  | PARM and PART access SAF group.  |
| grpdesk  | 'grpdesk'  | Transfer CFT help desk SAF group.  |
| grptrf  | 'grptrf'  | Transfer CFT transfer SAF group.  |
| userdef  | 'userdef'  | Default CFTRECV userid.  |
| safcftcl  | 'safcftcl'  | SAF class for Transfer CFT profiles.  |


#### Default identification values for Transfer CFT files

If you modify the following values, you must un-comment them in the JCL \* CFT$SET corresponding steps CUSTOM3 and resubmit CFT$SET.


| Keyword  | Default  | Description  |
| --- | --- | --- |
| cftenv | 'CFTENV'  | Id member included in each JCL:<br/> // INCLUDE MEMBER=CFTENV<br/> This member contains the command SET for the variables used in the JCL (except for CFTMAIN, and COPRUN). |
| icftcat  | 'CATALOG'  | Transfer CFT catalog file identifier  |
| <code>icftcom</code> | <code> 'COM' </code> | <code>Transfer CFT com file identifier</code> |
| icftparm  | 'PARM'  | Transfer CFT parameter file identifier  |
| icftpart  | 'PART'  | Transfer CFT partner file identifier  |
| icftpki  | 'PKIFILE'  | Transfer CFT PKI file identifier  |
| <code>cftloga</code> | <code>'LOG1'</code> | <code>Transfer CFT log file identifier</code> |
| <code>cftlogb</code> | 'LOG2'  | Transfer CFT log alternate file identifier  |
| cftacca  | 'ACCNT1'  | Transfer CFT account file identifier  |
| cftaccb  | 'ACCNT2'  | Transfer CFT account alternate file identifier  |
| cftuconf  | 'UCONF'  | Unified configuration file  |
| cftucrun  | 'UCONFRUN'  | Unified configuration file runtime identifier  |
| cftuparm  | 'UPARM'  | The unified configuration file definition (member DEFAULT)  |
| secini  | 'SECINI'  |  Security files identifier  |
| secact  | 'SECACT'  | Security actions  |
| secobj  | 'SECOBJ'  | Security objects  |


#### {{< TransferCFT/PrimaryCGorUM  >}}


| Keyword  | Default  | Description  |
| --- | --- | --- |
| cgenable  | 'no'  | Enables exchanges with the {{< TransferCFT/PrimaryCGorUM  >}} server. (yes &#124; no)  |
| cghost  | 'cghost'  | {{< TransferCFT/PrimaryCGorUM  >}} server host address.  |
| cgport  | 'cgport' ('12553')  | Transfer CFT’s port for registering with Central Governance.  |
| cgsecret  | 'cgsecret'  | {{< TransferCFT/PrimaryCGorUM  >}} shared secret.  |
| amsusers  | '&amp;%userid()'  | AM superuser(s) for {{< TransferCFT/PrimaryCGorUM  >}}.  |


#### {{< TransferCFT/suitevariablesSecureRelayName  >}}


| Keyword  | Default  | Description  |
| --- | --- | --- |
| srenable  | 'no'  | Enable/disable {{< TransferCFT/suitevariablesSecureRelayName  >}}.  |
| srmapath  | '/home/AXWAY/CFT32X/inst'  | USS directory for Secure Relay Master Agent (/xsr is automatically added). &lt;/p&gt;<br/> <blockquote> **Note**<br/> Note: Read only, you can share the directory with other Transfer CFTs.<br/> </blockquote>  |
| srmarun  | '/home/AXWAY/CFT32X/runtime/xsr'  | Runtime directory for Secure Relay Master Agent; one per instance, with Read/Write rights for {{< TransferCFT/axwayvariablesComponentShortName  >}}.  |
| srmacopo  | 'srmacopo'  | Secure Relay Master Agent communication port.  |
| srrahost  | 'srrahost'  | Secure Relay Router Agent host.  |
| srraadpo  | '6810'  | Secure Relay Router Agent administration port.  |
| srracopo  | '6811'  | Secure Relay Router Agent communication port, with one port per Transfer CFT instance.  |
| srrasap  | 'srrasap'  | Transfer CFT/Secure Relay configuration listening port on Router Agent side.  |
| srrassap  | 'srrassap'  | SSL listening port on Router Agent side.  |


#### SAML


| Keyword  | Default  | Description  | Example  |
| --- | --- | --- | --- |
| saml_enable  | no  | Enable SAML as the authentication method for this Transfer CFT (the UCONF am.type=saml).  |   |
| saml_client_id  | '$(cft.instance_id)'  | Specify the Client_ID value to use as issuer for SAML requests. This should match the Identity Provider configuration.  |   |
| authserver_host  | ' '  | Specify the SAML endpoint for AuthnRequest (HTTP-Redirect binding).<br/> If Keycloak is the Identity Provider, this should resemble: <code>https://authserver.host/auth/realms/\{realm-name}/protocol/saml. authserver_host ' '</code> | 'https://aa.bb.cc.int:8443'  |
| saml_idp_signonservice  | ' '  | Specify the SAML endpoint for SignonRequest (HTTP-Redirect binding).<br/> If Keycloak is the Identity Provider, this should resemble:<br/> <code>https://authserver.host/auth/realms/\{realm-name}/protocol/saml.</code><br/> <code>saml_idp_signonservice ' '</code> | '/auth/realms/synapses/protocol/saml'  |
| saml_idp_logoutservice  | ' '  | Specify the endpoint for SAML LogoutRequest (HTTP-Redirect binding).<br/> If Keycloak is the Identity Provider, this should resemble:<br/> <code>https://authserver.host/auth/realms/\{realm-name}/protocol/saml.</code><br/> <code>saml_idp_logoutservice ' '</code> | '/auth/realms/synapses/protocol/saml'  |
| saml_idp_certificate_path  | ' '  | Specify the path to the certificate that verifies the SAML Identity Provider server's signatures. This certificate is stored in the internal PKI database.  |   |


#### File prefixes

You can customize specific prefixes for the following Transfer CFT files.


| Keyword  | Default  | Description  |
| --- | --- | --- |
| pfx_cat  | '*'  | Catalog file  |
| pfx_com  | '*'  | Communication file  |
| pfx_parm  | '*'  | Parameter file  |
| pfx_part  | '*'  | Partner file  |
| pfx_pki  | '*'  | PKI file  |
| pfx_log  | '*'  | Log file  |
| pfx_acc  | '*'  | Account fie  |
| pfx_sec  | '*'  | Security files  |
| pfx_uconf  | '*'  | Uconf runtime file (1)  |


> **Note**
>
> Note: (1) The UCONF and UCONFRUN files are created at the same time as the instance environment, so you must manually re-create when a specific prefix is used (recfm= VB, lrecl=2048).

<span id="Selectin"></span>

Configure the SGINSTAL using UCONF or A12OPTSP
----------------------------------------------

As of {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.2.4 SP2, you are no longer required to submit the JOB A12OPTS to generate the SGINSTAL executable in the LOAD library or the USER.LOAD.

If the executable is not present in the LOAD library, the default values are used in the executables of Transfer CFT: CFTMAIN, CFTCOPL, CFTUTIL, etc. Additionally, you can configure the SGINSTAL macro parameters as UCONF variables.

****Syntax****

See the table below for possible keywords and values.

```
UCONFSET id=cft.mvs.sginstal.<keyword>,value=<value>
```

****Example****

```
UCONFSET id=cft.mvs.sginstal.sdsfopt,value=’monitor’
```

For continued compatibility, you can generate the Transfer CFT z/OS options tables. You can modify the parameters in the A12OPTSP member.


| Parameters that can be used as the &lt;keyword&gt;=&lt;value&gt;  | Description  |
| --- | --- |
| SYST = MVS | Transfer CFT operating system support type. |
| [ARM = {<span >YES</span> &#124; NO}] | Transfer CFT transfer support of the Automatic Restart Manager component:<br/> • YES (default value): Transfer CFT is authorized (APF) and registers with the ARM component.<br /> Transfer CFT uses an element named Xidparm, where idparm is the startup parameter for Transfer CFT. For details, refer to Using Services &gt; Automatic Restart Manager.<br/> • NO: Transfer CFT does not register with ARM.<br/> <blockquote> **Note**<br/> Note: The delivered sample uses the value 'ARM=NO'.<br/> </blockquote>  |
| [BLKSIZE = {27920 &#124; n}] {4100…32760} | Maximum value used to calculate the BLKSIZE for files created by Transfer CFT, when this information is absent.<br/> You may reduce the default value by 32 if you want to create DF/SMS managed EXTENDED or LARGE format data sets.<br/> <blockquote> **Note**<br/> Note: The delivered sample uses the value 'BLKSIZE=27998'.<br/> </blockquote>  |
| [ALLPRIM = {100 &#124; n}]{1…255}  | The % factor applied to the primary allocation computed to create a single volume file.  |
| [ALLSEC = {10 &#124; n}]{1…255}  | The % of the primary space used as the secondary allocation when creating a single volume file.  |
| [VOLNUM = {20 &#124; n}]{1…127}  | The maximum number of volumes in a multi-volume allocation.  |
| [ALLONE = {100 &#124; n}]{1…255}  | The % factor applied to the primary allocation computed to create a multi-volume file.<br/> Use extreme care when changing this parameter. |
| [ALLNEXT= {100 &#124; n}]{1…255}  | The % factor applied to the secondary allocation when creating a multi-volume file.<br/> Use extreme care when changing this parameter. |
| [DSNTYPE = { <span >NONE</span> &#124; EXTPREF/EXTREQ/LARGE}  | Values other than NONE (default value) are added to create DF/SMS managed EXTENDED or LARGE files. You can also control the DSNTYPE via DF/SMS customization.  |
| [BLKPDS = {150 &#124; n}]{1…32760} | Number of PDS blocks allocated while creating a partitioned file by Transfer CFT. |
| [DESC = { Value of the DESCRIPTOR CODE field of the WTO &#124; n} ] | The left bit corresponds to 1. The right bit corresponds to 16. |
| [EMCSOPT = { <span >USER</span> &#124; MONITOR &#124; IGNORE}]  | How Transfer CFT processes a MODIFY command issued from a program using SVC 34:<br/> • USER (default value): the left 8 bytes of EMCS console name are used as the user id issuing the command.<br/> • MONITOR: the USERID associated with the monitor is always used.<br/> • IGNORE: MODIFY commands issued from SVC 34 are ignored.<br/> *See Note. |
| [FORMATVB = {YES &#124; <span >NO</span>}] |  • YES: Active. You receive a file with a V-type RECFM, and the file is created with the type VB.<br/> • NO: Default value. |
| [HSMASYNC = {YES &#124; <span >NO</span>}] |  • NO (default value): For all Transfer CFT handled files, the HSM recall is performed synchronously. <br/> • YES: If a file to be sent is migrated, the HSM recall is performed asynchronously, and the transfer is delayed until HSM completes the request. Transfer CFT will always wait for the HSM recall of received files. <br/> <blockquote> **Note**<br/> Note: If a Transfer CFT EXEC procedure is HSM migrated, it is not executed. An error message indicating this is displayed in the Transfer CFT LOG: CFTS02E migrated.<br/> </blockquote> <blockquote> **Note**<br/> Note: The delivered sample uses the value ‘HSMASYNC=YES’.<br/> </blockquote>  |
| [MAXAB = {15 &#124; n}]{1…255} | Number of ABENDs allowed by Transfer CFT before shutting down. |
| [MAXDUMP = {2 &#124; n}] {1…255}<br/>  | Number of ABENDs that are the object of a Transfer CFT requested DUMP.<br/> Additional ABENDs do not provoke a DUMP. |
| [MAXTRACE = {80000 &#124; n}] | Size limit for SGTRACE. |
| [MCSOPT = { <span >CHECK</span> &#124;MONITOR}]  | How Transfer CFT adds a user id to a z/OS MODIFY command:<br/> • CHECK (default value): The console name is checked for a valid security definition, and is used if yes. Else the user id associated with the monitor is used. SAF checking applies only if Transfer CFT is running APF authorized.<br/> • MONITOR: The USERID associated with the monitor is always used.<br/> *See Note. |
| PDSESHARING [ <span >NO</span> &#124; YES]  |  • NO (default) = Do not allow others to write to PDSE in sharing mode.<br/> • YES = Allow simultaneous writing to a PDSE file type. Other intervening applications must also use the shared mode option though for sharing to occur.<br/> <blockquote> **Note**<br/> Note: On a shared SYSPLEX you must customize the following z/OS system parameter, either:<br/> </blockquote> • NORMAL: SYSn.PARMLIB member IGDSMSxx to specify PDSESHARING, or<br/> • EXTENDED: SYSn.PARMLIB member IGDSMSxx to specify PDSESHARING<br/> <blockquote> **Note**<br/> Note: The delivered sample uses the value ‘PDSESHARING=YES’.<br/> </blockquote>  |
| [ROUTCDE = {Value of the ROUTCODE CODE field of the WTO &#124; n}] | The left bit corresponds to 1. The right bit corresponds to 16. The default value ROUTCDE=X‘0008’ corresponds to ROUTCDE=(13).<br/> This value is used with the ‘OPERMSG’ option of the ‘CFTLOG’ parameter.<br/> For the options DESC and ROUTCE, refer to the IBM document Supervisor services and macros, which explains the use of DESCRIPTOR CODES and ROUTCODES. |
| [SDSFOPT = { <span >USER</span> &#124; MONITOR &#124; IGNORE}]  | How Transfer CFT processes a MODIFY command issued from SDSF:<br/> • USER (default value): the console name defined in SDSF options is used as the user id issuing the command.<br/> • MONITOR: the USERID associated with the monitor is used.<br/> • IGNORE: MODIFY commands issued from SDSF are ignored.<br/> *See Note. |
| [SHARECAT = { **YES** &#124; NO &#124; INACT }] | <br/> • YES (default value): The catalog is cached in a common dataspace that is shared with the Copilot user interface, the CFTUTIL utility, and so on. This parameter improves catalog reading, especially from the Copilot user interface, when enabled. • If the catalog is full and an extension is created (using either the <code>cft.cftcat.auto_expand_*</code> parameters or <code>RECONFIG TYPE=CAT</code>), new records are stored in the extension and not in the cache. Accessing records in the extension may negatively impact performance.<br/><br/> • Do not use YES when implementing a multi-node architecture.<br/> <br/> • NO: The catalog is cached in a dataspace, but the dataspace is not shared.<br/> • INACT: The catalog is not cached, and no dataspace is created.<br/> NO: The catalog is cached in a dataspace, but the dataspace is not shared.<br/> INACT: The catalog is not cached, and no dataspace is created.<br/> <blockquote> **Note**<br/> Note: When the Transfer CFT is an APF-authorized program (Authorized Program Facility), specify if the catalog dataspace cache is available to be read by other Transfer CFT applications.<br/> </blockquote> <blockquote> **Note**<br/> Note: The delivered sample uses the value ‘SHARECAT=YES’.<br/> </blockquote>  |
| [SGTRACE = {0 &#124; n}] {1…65535} | Initial value of the SGTRACE trace file.<br/> A value other than 0 may be used if requested by Transfer CFT customer support.<br/> Possible combinations are:<br/> • 1: Network actions (TCP)<br/> • 2: Erroneous actions<br/> • 4: File manager actions<br/> • 8: Read/write to files<br/> • 16: C functions<br/> • 32: Long messages<br/> • 64: Inter-task communication actions<br/> • 128: Program calls and return messages<br/> • 256: Interactive interface actions<br/> • 512: User exit calls<br/> <blockquote> **Note**<br/> Note: When you use the SGTRACE options with the Transfer CFT interface under VTAM, the non-encrypted passwords are listed in the trace records.<br/> </blockquote>  |
| [SUBOPT = { 0 &#124; 1 &#124; 3 } ]  | Manages 2 statistic lines for a job submitted by Transfer CFT:<br/> • 0: Default. Generates 2 lines at the end of JCL //* SUBMITTED BY:jobname AT DD/MM/YY hh:mm:ss ,USERID=username ,CARDS= 0000000 // .<br/> • 1: Generates only the statistic line at the end of the JCL and no card ‘//’ //* SUBMITTED BY:jobname AT DD/MM/YY hh:mm:ss ,USERID=username ,CARDS= 0000000 .<br/> • 3: Does not generate a line at the end of the JCL. |
| [TRACE = { <span >128</span> &#124; n}] {4…16383} | Transfer CFT internal trace size (in Kb). |
| [TAPE = {NOTSUP &#124; <span >UPDATE</span> &#124; OUTPUT}] |  • NOTSUP: Forbids access to tape files for all Transfer CFT programs.<br/> • UPDATE: Default. Enables writing of tape files protected by means of a retaining date or tape library management software.<br/> • OUPUT: The transfer fails in ABDEND 713 if the tape is write-protected by tape library management software, or an expiration date.<br/> <blockquote> **Note**<br/> Note: The delivered sample uses the value ‘TAPE=OUTPUT’.<br/> </blockquote>  |
| [TSOEDIT = { <span >NO</span> &#124; YES}] | File support with sequence number in columns 73 to 80:<br/> • YES: If the ISPF editor with the ‘NUMBER ON’ option creates them, then the input files read by CFTUTIL can contain an eight-digit sequence number in columns 73 to 80. This sequence number is then ignored by Transfer CFT z/OS.<br/> • NO: Input files are read without changes from CFTUTIL.<br/> <blockquote> **Note**<br/> Note: The delivered sample uses the value ‘TSOEDIT=YES’.<br/> </blockquote>  |
| [VSAMSUFS = { <span >SHORT</span> &#124; LONG }]  |  • SHORT: VSAM component suffixes are created with .D for KSDS and ESDS, and .I for KSDS.<br/> • LONG: VSAM component suffixes are created with .DATA for KSDS and ESDS, and .INDEX for KSDS.<br/> Alternatively, you can set this parameter using: <code>UCONFSET ID=cft.mvs.sginstal.vsamsufs,value=x</code> |


> **Note**
>
> Note: \*MCSOPT, SDSFOPT, EMCSOPT: A user id is added only to CFTUTIL commands. The z/OS PAUSE command is interpreted as a CFTUTIL SHUT FAST=YES command. Transfer CFT diagnosis commands are not associated with a used id, for example MODIFY cft or ECHO.

When you start Transfer CFT, all parameters are printed in the transfer CFT LOG, for example:

```
CFTI18I+Installation options (macro SGINSTAL)
CFTI18I+ Macro date - 05/29/17 15.45 (MM/DD/YY hh.mm) (c)
CFTI18I+ SHARECAT=NO
CFTI18I+ TAPE=UPDATE
CFTI18I+ BLKSIZE=27998
CFTI18I+ BLKPDS=200
CFTI18I+ HSMASYNC=YES
CFTI18I+ ALLPRIM=100,ALLSEC=10,VOLNUM=20,ALLONE=100,ALLNEX=100
CFTI18I+ …
```

When Transfer CFT starts, the CFTI18I message and DATE macro display in one of four formats:

1. CFTI18I and DATE macro - 06/16/17 14.10 (MM/DD/YY hh.mm) (c) SGINSTAL default model.  
    The SGINSTAL executable does not exist.  
    This was not configured with the cft.mvs.sginstal.xxxx variables.
1. CFTI18I and DATE macro - 06/05/17 15.30 (MM/DD/YY hh.mm) (c) SGINSTAL load.  
    The SGINSTAL executable exists.  
    This was not configured with the cft.mvs.sginstal.xxxx variables.
1. CFTI18I and DATE macro - 06/16/17 14.10 (MM/DD/YY hh.mm) (c) SGINSTAL default model modified by UCONF.  
    The SGINSTAL executable does not exist.  
    This was configured with a variable(s) of the cft.mvs.sginstal.xxxx type.
1. CFTI18I and DATE macro - 06/05/17 15.30 (MM/DD/YY hh.mm) (c) SGINSTAL load modified by UCONF.  
    The SGINSTAL executable exists.  
    A variable of the type cft.mvs.sginstal.xxxx.

To generate parameters from the SGINSTAL executable as UCONF variables:

```
//\* STEP 1 : GENERATE
//CFTSGIGN EXEC PGM=CFTSGIGN
//STEPLIB DD DISP=SHR,DSN=&CFTLOAD (LIBRARY contains SGINSTAL)
//UCONFGEN DD UNIT=SYSDA,DISP=(NEW,PASS),
// DCB=(RECFM=VB,LRECL=256,BLKSIZE=2560),
// SPACE=(TRK,(1)),
// DSN=&&TMP
//\* STEP 2 : UPDATE UCONF
//UCONF EXEC PCFTUTL,PARM=''
//CFTIN DD DISP=(OLD,DELETE),
// DSN=&&TMP
Delivered JCL INSTALL(MIGRSGI) extract.
```

****Related topics****

[Customize JCL installation files](../installation_parameters_to_customize)
