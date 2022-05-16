---

    title: Transfer CFT command guide and syntax 
    linkTitle: Command guide and parameters
    weight: 130

---
This topic provides a useful list of {{< TransferCFT/axwayvariablesComponentShortName  >}} commands,
syntax, and parameters. For a more detailed description of the Transfer
CFT commands, refer to the link displayed below each command
syntax.

The {{< TransferCFT/axwayvariablesComponentShortName  >}} commands are presented in alphabetical order in this
summary. Each
command is presented with possible parameters and default values.

See also, [Syntax
conventions and symbolic variables.](#Syntax_conventions)

<span id="ABOUT"></span>

#### ABOUT: Displays product/host information

Syntax

`[ COMMENT   = string ]`

`[ TYPE   = { ALL   | HOST | CFT } ]`

`[ KEY = { FIRST | ALL } ]`

[ABOUT details](../about_cftutil/about_command)

<span id="ACT"></span>

#### ACT: Reactivates a partner

Syntax

`ID = identifier `

`[ TYPE   = { PART   | TRK | CRON | FOLDER } ]`

`[ MODE   = { BOTH   | REQUESTER|   SERVER } ]`

[ACT details](../about_cftutil/reactivate_an_object_cl)

<span id="CFTACCNT"></span>

#### CFTACCNT: Defines transfer accounting records

Syntax

CFTACCNT TYPE = FILE

`TYPE   = FILE`

`FNAME   = filename `

`ID   = identifier `

`[ AFNAME   = filename ]`

`[ COMMENT   = string ]`

`[ EXEC   = filename ]`

`[ LANGUAGE   = { COBOL   | C } ]`

`[ MAXREC   = { 0   | n } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ SWITCH   = { 00000000   | time } ]`

`[ FORMAT   = { V23   | 23 | V24 | 24} ]`

 

CFTACCNT TYPE = SYST

`TYPE   = SYST`

`ACCID   = n `

`ID   = identifier `

`[ COMMENT   = string ]`

`[ FORMAT   = { V23   | 23 | V24 | 24} ]`

`[ LANGUAGE   = { COBOL   | C } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

[CFTACCNT details](../web_copilot_ui/conf_intro/cftaccnt)

[Transfer
accounting records](../../admin_intro/admin_config_commands/cftaccnt_concepts)

<span id="CFTAPPL"></span>

#### CFTAPPL: Defines a transfer owner

Syntax

`CFTAPPL   MODE = REPLACE`

`ID   = identifier `

`USERID   = string`

`[ COMMENT   = string ]`

`[ GROUPID   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

CFTAPPL MODE = DELETE

`ID   = identifier `

`USERID   = string`

`DIRECT   = { BOTH   |  SEND   | RECV }`

`[ COMMENT   = string ]`

`[ GROUPID   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

[CFTAPPL details](../web_copilot_ui/flow_def_intro/cftappl)

[Assign a transfer owner]()

#### CFTAUTH: Defines an Authorized Flow Definition list

Syntax

`FNAME = filename `

`ID = identifier`

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

` `

Or

 

`IDF = (identifier | mask, identifier | mask, …)`

`ID = identifier`

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

<span style="font-weight: bold;">**** ****</span>

<span id="CFTCAT"></span>

#### CFTCAT: Defines Catalog attributes

Syntax

`FNAME   = filename `

`ID   = identifier  `

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ RH   = { 10   | n }  ]`

`[ RKERROR   = { KEEP   | DELETE } ]`

`[ RT   = { 10   | n } ]`

`[ RX   = { 10   | n } ]`

`[ SH   = { 10   | n } ]`

`[ ST   = { 10   | n } ]`

`[ SX   = { 10   | n } ]`

`[ TIMEP   = { 23595999   | HHMMSSCC } ]`

`[ TLVCEXEC   = { n } ]`

`[ TLVCLEAR   = { TLVWARN-10 | n } ]`

`[ TLVWEXEC   = { n } ]`

`[ TLVWRATE   = { 60 | n } ]`

`[ TLVWARN   = { 80 | n } ]`

`[ UPDAT   = { 256   | n } ]`

`[ WSCAN   = { 5   | n } ]`

[CFTCAT details](../web_copilot_ui/conf_intro/cftcat)

[Catalog attributes](../../admin_intro/admin_config_commands/catalog_parameter_concepts)

<span id="CFTCOM"></span>

#### CFTCOM: Defines parameters related to the communication between applications and {{< TransferCFT/axwayvariablesComponentShortName  >}}

Syntax

#### CFTCOM TYPE = FILE

`TYPE   = FILE`

`ID   = identifier `

`NAME   = filename  `

`[ COMMENT   =  string   ]`

`[ MODE   = { REPLACE | CREATE | DELETE } ]`

`[ TLVCEXEC   = { n } ]`

`[ TLVCLEAR   = { TLVWARN-20 | n } ]`

`[ TLVWEXEC   = { n } ]`

`[ TLVWRATE   = { 60 | n } ]`

`[ TLVWARN   = { 70 | n } ]`

`[ WSCAN   = { 60   | n } ]`

` `

#### CFTCOM TYPE = TCPIP

`ID   = identifier `

`HOST   = string `

`PORT   = number`

`PROTOCOL   = { XHTTP }`

`[ ADDRLIST   = ( string, string, ...) ]`

`[ COMMENT   =  string   ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

<span style="font-weight: bold;">**** ****</span>[CFTCOM](../web_copilot_ui/conf_intro/cftcom)

[Communication
media](../../admin_intro/admin_config_commands/communication_media_concepts)

<span id="CFTCRON"></span>

#### CFTCRON: Define {{< TransferCFT/axwayvariablesComponentShortName  >}} cron jobs

Syntax

`ID = identifier`

`CRONTAB   = string`

`EXEC   = filename`

`EXECPOLICY        = [ INSTANCE |ALLNODES ]`

`TIME   = { string | @shutdown | @startup } [FOR   DETAILS: CFTCRON   time syntax]`

`[ PARM   = string ]`

`[ COMMENT   = string ]`

`[ STATE = { ACTIVE | NOACTIVE } ]`

`[ TYPE   = { EXEC   | CFTUTIL } ]`

`[ USERID   = { CFT   server "userid"   | string } ]`

[Define script execution](../web_copilot_ui/flow_def_intro/cftcron)

<span id="CFTDEST"></span>

#### CFTDEST: Definition of a list of partners 

Syntax

CFTDEST FNAME

`ID = identifier `

`FNAME   = filename `

`[ EXEC   = { DEST   | PART | CHILDREN} ]`

`[ EXECA = { DEST   | PART | CHILDREN} ]`

`[ EXECPRE = { DEST   | PART | CHILDREN} ]`

`[ COMMENT   = string ]`

`[ FOR   = { BOTH   | COMMUT |   LOCAL } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NOPART   = { ABORT   | CONTINUE | IGNORE } ]`

####  

CFTDEST PART

`ID = identifier `

`PART   = (identifier, identifier, ...) `

`[ EXEC   = { DEST   | PART } ]`

`[ COMMENT   = string ]`

`[ FOR   = { BOTH   | COMMUT |   LOCAL } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NOPART   = { ABORT   | CONTINUE | IGNORE } ]`

[CFTDEST details](../web_copilot_ui/flow_def_intro/cftdest)

[Broadcast
list]()

<span id="CFTEXIT"></span>

#### CFTEXIT: Activate an exit task 

Syntax

#### CFTEXIT TYPE = FILE

`ID   = identifier `

`TYPE   = FILE`

`[ COMMENT   = string ]`

`[ FORMAT   = { V23   | 23 | V24 | 24 } ]`

`[ LANGUAGE   = { COBOL   | C } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ PARM   = string ]`

`[ PROG   = { CFTEXIT   | string } ]`

`[ RESERV   = { 16384   | n } ]`

`[ WAITTASK   = { 1441   | n } ]`

 

#### <span style="font-weight: normal;">CFTEXIT TYPE = { FILE | ACCESS | EXEC | BOT}</span>

`ID   = identifier `

`TYPE   = { FILE | ACCESS |  EXEC | BOT } `

`[ COMMENT   = string ]`

`[ FORMAT   = { V23   | 23 | V24 | 24 } ]`

`[ LANGUAGE   = { COBOL   | C } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ PARM   = string ]`

`[ PROG   = { CFTEXIT   | string } ]`

`[ RESERV   = { 1024   | n } ]`

`[ WAITTASK   = { 1441   | n } ]`

<span style="font-weight: bold;">**** ****</span>CFTEXIT details

[Exit
tasks](../../app_integration_intro/managing_exits)

<span id="CFTEXT"></span>

#### CFTEXT: Extract data from the parameter and partner files 

Syntax

`[ TYPE   = { ALL   | ACCNT | APPL | AUTH | CAT | COM | CRON | DEST |  EXIT | IDF | LOG   |  NET | PARM | PART | PROT | RECV | SEND | SSL |  TCP |  XLATE } ]`

`[ CONTENT = { FULL | BRIEF } ]`

`[ FOUT   = filename ]`

`[ FPARM   = filename ]`

`[ FPART   = filename ]`

`[ ID   = { *   | identifier | mask } ]`

[Export
configuration](../about_cftutil/configuring_cft_start_here/cftext_command)

<span id="CFTFILE"></span>

#### CFTFILE: Create or delete {{< TransferCFT/axwayvariablesComponentShortName  >}} files

Syntax

`TYPE   = { PARM | PART }`

`FNAME   = filename  `

`[ HABFNAME   = filename ] `

`[ FBLKSIZE   = { 0   |n } ]`

`[ FSPACE   = n ]`

`[ FSPACEX   = n ]`

`[ MODE   = { CREATE   | REPLACE | DELETE } ]`

` `

#### CFTFILE { CAT | COM }

`TYPE   = {  CAT   | COM }`

`FNAME   = filename `

`[ RECNB   = n ]`

`[ FBLKSIZE   = { 0   |n } ]`

`[ FSPACE   = { 0   | n } ]`

`[ FSPACEX   =  { 0   | n } ]`

`[ HABFNAME   = filename ]`

`[ MODE   = { CREATE   | REPLACE | DELETE } ]`

`[ NODE = { 0   | n } ] available for TYPE=CAT`

` `

#### CFTFILE { ACCNT | LOG }

`TYPE   = { ACCNT | LOG }`

`FNAME   = filename `

`[ FBLKSIZE   = 0   | n ]`

`[ FSPACE   = 0   |n ]`

`[ FSPACEX   = 0   |n ]`

`[ MODE   = { CREATE   | REPLACE | DELETE } ]`

[Manually create internal datafile files](../../admin_intro/admin_commands_intro/cftfile)

#### CFTFOLDER

See [CFTFOLDER](../web_copilot_ui/flow_def_intro/cftfolder) for additional parameter details.

`IDF = string         </code></p>`

`PART = string         </code></p>`

`SCANDIR         = string</code></p>`

`WORKDIR = string         </code></p>`

`[ ARCHIVEDIR = string ]`

`[ ENABLESUBDIR = { YES | NO } ]`

`[ FILEIDLEDELAY = n ]`

`[ METHOD = { FILE | MOVE }] `

`[ STATE = { ACTIVE | } ]`

`[ INTERVAL = n ]`

`[ FILECOUNT = n ]`

`[ FILESIZEMIN = n ]`

`[ FILESIZEMAX = n ]`

`[ INCLUDEFILTER = string ]`

`[ EXCLUDEFILTER = string ]`

`[ RESUBMITCHANGED { YES | NO }]`

`[ FILTERTYPE ]`

`[ GROUPID = string ]`

`[ RENAMEMETHOD ]`

`[ RENAMESEPARATOR = string ] `

`[ USEFSEVENTS = { YES | NO } ]`

`[ USERID = string ]`

<span id="CFTIDF"></span>

#### CFTIDF ID = identifier: Correspondence between the network identifier and the local identifier of a transferred model file 

Syntax

`NIDF   = string `

`PART   = identifier `

`TYPE   = { RECV   | SEND } `

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

<span style="font-weight: bold;">**** ****</span>[CFTIDF details](../web_copilot_ui/flow_def_intro/cftidf)

[File
template/virtual file association](../../concepts/cft_configuration_concepts_start_here/network_file_identifier_concepts)

<span id="CFTLOG"></span>

#### CFTLOG FNAME = filename: Log file management parameters 

Syntax

`ID   = identifier `

`[ AFNAME   = filename ]`

`[ COMMENT   = string ]`

`[ CONTENT   = { FULL   | BRIEF } ]`

`[ EXEC   = filename ]`

`[ FORMAT   = { V23   | 23 | V24 | 24 } ]`

`[ LENGTH   = { 160   | n } ]`

`[ MAXREC   = { 0   | n } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NOTIFY   = identifier ]`

`[ OPERMSG   = n ]`

`[ SWITCH   = { 00000000   | time } ]`

[CFTLOG details]()

[Transfer
Log file]()

<span id="CFTNET"></span>

#### CFTNET: Local network resources

Syntax

#### CFTNET TYPE = TCP

`HOST   = { string | INADDR_ANY } `

`ID   = { identifier | *identifier } `

`[ CALL   = { INOUT   | IN | OUT } ]`

`[ CLASS   = { 1 | n } ]`

`[ MAXCNX   = { 384   | n } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ SRCHOST=   { hostname1 | n} ]`

`[ SRCPORTS= { string } ]`

` `

#### CFTNET TYPE = SR

`HOST   = { string | INADDR_ANY } `

`ID   = { identifier | *identifier } `

`[ RECALLHOST = { string } ]`

`[ PORT = {0 ...65535 } ]`

`[ SRCHOST    = { string } ] `

`[ SSLTERM           { YES | NO } ]`

#### PROTOCOL = GENERIC

`HOST   = string `

`ID   = identifier `

`INET   = identifier`

`PORT   =  n `

`[ CALL   = { INOUT   | IN | OUT } ]`

`[ CLASS   = { 1 | n } ]`

`[ MAXCNX   = { 384   | n } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ PARM   = string ]`

`[ SRCHOST=   { hostname1 | n} ]`

` `

#### CFTNET TYPE = TCP

#### PROTOCOL = SOCKS4, SOCKS5

`HOST   = string  `

`ID   = identifier `

`INET   = identifier`

`PORT   =  n `

`[ CALL   = { INOUT   | IN | OUT } ]`

`[ CLASS   = { 1 | n } ]`

`[ MAXCNX   = { 32   | n } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ SRCHOST=   { hostname1 | n} ]`

`[ USER   = string ]`

 [CFTNET details](../web_copilot_ui/conf_intro/cftnet)

[Network
resources](../../admin_intro/admin_config_commands/network_resource_concepts)

[About proxies and SOCKS](../../protocols_start_here/ipv6/use_proxy_and_socks_protocol)

<span id="CFTPARM"></span>

#### CFTPARM: General {{< TransferCFT/axwayvariablesComponentShortName  >}} environment parameters

Syntax

`CAT   = identifier `

`COM   = ( identifier ,  identifier   , ...) `

`ID   = identifier `

`KEY   = {string | #filename } `

`NET   = ( identifier ,  identifier   ,...) `

`PART   = identifier  `

`PARTFNAM   = filename  `

`PROT   = ( identifier ,  identifier   , ...) `

`[ ACCNT   = identifier  ]`

`[ BUFSIZE   =  { 4096   | n } ]`

`[ COMMENT   = string ]`

`[ CRONTABS   = (crontab, crontab, …) ]`

`[ DEFAULT   = { DEFAUT   | identifier } ]`

`[ EXECRE   = filename ]`

`[ EXECRF   = filename ]`

`[ EXECRM   = filename ]`

`[ EXECSE   = filename ]`

`[ EXECSF   = filename ]`

`[ EXECSFA   = filename ]`

`[ EXECSM   = filename  ]`

`[ EXECSMA   = filename ]`

`[ EXITBOT   = identifier  ]`

`[ EXITEOT   = identifier  ]`

`[ FBUFSIZE   = { 0   |65535 } ]`

`[ LENAPPL   = { 32   | 1 } ]`

`[ LOG   = identifier  ]`

`[ MAXTASK   = { 8   | n }  ]`

`[ MAXTRANS   =  { 256   | 1 } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE }  ]`

`[ NPART   = string ]`

`[ PKIPASSW   = { PKIPASSW | string } ]`

`[ RCVALLER   = { STOP   | CONTINUE } ]`

`[ SECFNAME   = filename ]`

`[ SSLMTASK   = { 8   | n } ]`

`[ SSLTTASK   =  {3   | n } ]`

`[ SSLWTASK   = { 10   |n } ]`

`[ SSLWRESP   = { 60   | n } ]`

`[ TRACE   = string ]`

`[ TRANTASK   = { 3   | n } ]`

`[ TRKPART   =  { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ TRKRECV   =  { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ TRKSEND   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ USERCTRL   = { NO   | YES } ]`

`[ WAITRESP   = { 60   | n } ]`

`[ WAITTASK   = { 10   | n } ] `

[CFTPARM details](../web_copilot_ui/conf_intro/cftparm)

[Transfer CFT general
parameters](../../admin_intro/admin_config_commands/cftparm_general_parameters)

<span id="CFTPART"></span>

#### CFTPART ID = identifier: Partner definition parameters  

Syntax

`PROT   = { (identifier | mask , identifier | mask , .... ) } `

`[ COMMENT   = string  ]`

`[ COMMUT   = { YES   | NO | SERVER }   ]`

`[ CTRLPART   = { IGNORE   | ALL | RPART | SPART } ]`

`[ FPREFIX   = string ]`

`[ GROUP   = identifier ]`

`[ IDF   = identifier  ]`

`[ IMAXTIME   = { 23595999   | time } ]`

`[ IMINTIME   = { 0   | time } ]`

`[ IPART   = identifier ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NACK = { YES | NO | ANY } ]`

`[ NRPART   = string ]`

`[ NRPASSW = string  ]`

`[ NSPART   = string  ]`

`[ NSPASSW   = string  ]`

`[ OMAXTIME   = { 23595999   | time } ]`

`[ OMINTIME   = { 0   | time } ]`

`[ RAUTH   = { *   | identifier } ]`

`[ SAP   = (string, string, …) ]`

`[ SAUTH   = { *   | identifier } ]`

`[ SSL   = identifier ]`

`[ STATE   = {ACTIVEBOTH   | ACTIVEREQ | ACTIVESERV | NOACTIVE } ]`

`[ SYST   = { ‘   ‘ | GCOS7 | GCOS8 | GUARD |  MVS |  OS400 |   UNIX | VM | VMS |  VSE | WINNT | BS2000 } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ XLATE   = identifier ]`

[CFTPART details]()

[Partner
attribute concepts]()

<span id="CFTPROT"></span>

#### CFTPROT: Transfer protocol

Syntax

CFTPROT TYPE = ODETTE

`ID   = identifier `

`NET   = identifier `

`[ DISCTD   = { 20   | n } ]`

`[ DISCTS   = { 65   | n } ]`

`[ DYNAM   = identifier  ]`

`[ EERP   = { 91   | 86 } ]`

`[ EXITA   = identifier ]`

`[ IDF   = string  ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ PAD   = { NO   | YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCOMP   =  { 0   | 15 } ]`

`[ RCREDIT   = { 4   | n } ]`

`[ RESTART   = { 5   | n } ]`

`[ RESYNC   = { NO   | YES } ]`

`[ REVERSE   = { YES   | NO } ]`

`[ RRUSIZE   = { 2048   | n } ]`

`[ RTO   = { 260   | n } ]`

`[ SAP   = string ]`

`[ SCOMP   = { 0 | 1 |15 } ]`

`[ SCREDIT   = { 4   | n } ]`

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SRUSIZE   = { 2048 | n } ]`

`[ SSL   = identifier ]`

`[ TCP   = { CFT   | OFTP} ]`

` `

CFTPROT TYPE = PESIT

`PROF   = ANY `

`ID   = identifier  `

`NET   = identifier `

`[ CONCAT   = { NO   | YES } ] `

`[ DISCTC   = { 60   | n } ] `

`[ DISCTD   = { 10   | n } ] `

`[ DISCTR   = { 45   | n } ] `

`[ DISCTS   = { 60   |n } ] `

`[ DYNAM   = identifier ] `

`[ EXITA   = identifier ] `

`[ HIDE99   = { NO   | YES } ] `

`[ IDF   = string ] `

`[ LOGON   = { YES   | NO } ] `

`[ MODE   = { REPLACE   | CREATE | DELETE } ] `

`[ MULTART   = { NO   | YES } ] `

`[ NACK = { YES | NO | ANY} ]`

`[ PAD   = { NO   | YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCHKW   = { 3   | n } ] `

`[ RCOMP   = { 0    | 15 } ] `

`[ RESTART   = { 5   | n } ] `

`[ RESYNC   = { NO   | YES } ] `

`[ REVERSE   = { NO   | YES } ] `

`[ RPACING   = { 32767   | n } ] `

`[ RRUSIZE   = { 32750   |n } ] `

`[ RTO   = { 260   | n } ] `

`[ SAP   = string ] `

`[ SCHKW   = { 3   | n } ] `

`[ SCOMP   = { 0 | 15} ] `

`[ SEGMENT   = { NO   | YES } ] `

`[ SPACING   = { 32767   | n } ] `

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ] `

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ] `

`[ SRUSIZE   = { 32750   | n } ] `

`[ SSERV   = { GSIT   | string } ] `

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT  

`PROF   = CFT`

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   | YES } ]`

`[ DISCTC   = { 90   | n }  ]`

`[ DISCTD   =  { 20   | n } ]`

`[ DISCTR   = { 45   | n } ]`

`[ DISCTS   = { 65   | n } ]`

`[ DYNAM   = identifier  ]`

`[ EXITA   = identifier  ]`

`[ HIDE99   = { NO   |YES } ]`

`[ IDF   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ MULTART   = { NO   | YES } ]`

`[ PAD   = { NO   | YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCHKW   = { 2   | n }  ]`

`[ RCOMP   = { 0 |15} ]`

`[ RESTART   = { 5   | n } ]`

`[ RESYNC   = { NO   | YES } ]`

`[ REVERSE   = { NO   | YES } ]`

`[ RPACING   = { 36   | n } ]`

`[ RRUSIZE   = { 4056   | n } ]`

`[ RSERV   = string ]`

`[ RTO   = { 260   | n }  ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   | n } ]`

`[ SCOMP   = { 0| 15  } ]`

`[ SEGMENT   = { NO   | YES } ]`

`[ SPACING   = { 36   | n } ]`

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SRUSIZE   = { 4056   |n } ]`

`[ SSERV   = { CFTPSITX | string } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = EXTERN`

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   | YES } ]`

`[ DISCTC   = { 90   | n } ]`

`[ DISCTD   =  { 120   | n } ]`

`[ DISCTR   = { 45   | n } ]`

`[ DISCTS   = { 165   | n } ]`

`[ DYNAM   = identifier  ]`

`[ EXITA   = identifier   ]`

`[ HIDE99   = { NO   |YES } ]`

`[ IDF   = string  ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ LOGON   = { YES   | NO } ]`

`[ MULTART   = { NO   | YES } ]`

`[ NACK = { YES | NO | ANY } ]`

`[ RCHKW   = { 2   | n } ]`

`[ RCOMP   =  { 0 | 10   |15 } ]`

`[ RESTART   = { 5   | n } ]`

`[ RESYNC   = { NO   | YES } ]`

`[ REVERSE   = { YES | NO } ]`

`[ RPACING   = { 36 | n } ]`

`[ RRUSIZE   = { 4056 | n } ]`

`[ RTO   = { 260   | n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   | n } ]`

`[ SCOMP   =  { 0 | 10   | 15} ]`

`[ SEGMENT   = { NO | YES } ]`

`[ SPACING   = { 36 | n } ]`

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SRUSIZE   = { 4056 | n } ]`

`[ SSERV   = { PESIT   | string } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = SIT`

`ID   = identifier `

`NET   = identifier  `

`[ CONCAT   = { NO   | YES } ]`

`[ DISCTC   = { 90   | n } ]`

`[ DISCTD   = { 240 | n } ]`

`[ DISCTR   = { 45 | n } ]`

`[ DISCTS   = { 285 | n } ]`

`[ DYNAM   = identifier ]`

`[ EXITA   = identifier ]`

`[ IDF   = string ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ MULTART   =  { NO   | YES } ]`

`[ RCHKW   = { 2   | n } ]`

`[ RCOMP   =  { 0 |   15 } ]`

`[ RESTART   = { 5   | n } ]`

`[ RESYNC   = { NO   | YES } ]`

`[ REVERSE   = { NO   | string } ]`

`[ RPACING   = { 36   | n } ]`

`[ RRUSIZE   = { 4050 | n } ]`

`[ RTO   = { 260 | n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2 | n } ]`

`[ SCOMP   = { 0 | 15 } ]`

`[ SEGMENT   = { NO | YES } ]`

`[ SPACING   = { 36   | n } ]`

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SRUSIZE   = { 4050 | n } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = DMZ  `

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   | YES } ]`

`[ CTO   = { 1   | n } ]`

`[ CYCLE   = { 10   | n } ]`

`[ DISCTC   = { 90   | n } ]`

`[ DISCTD   = { 120   | n } ]`

`[ DISCTR   = { 45   | n } ]`

`[ DISCTS   = { 65   | n  } ]`

`[ DYNAM   = identifier ]`

`[ EXITA   = identifier ]`

`[ IDF   = string ]`

`[ LOGON   = { YES   | NO } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ MULTART   = { NO   | YES } ]`

`[ PAD   = { NO   | YES } ] <Deprecated in Transfer CFT 3.5>`

`[ PART   = ( identifier, identifier, …) ]`

`[ RCHKW   = { 2   | n } ]`

`[ RCOMP   = { 0 | 10   | 15 } ]`

`[ RESTART   = { 5   | n  } ]`

`[ RESYNC   = { NO   | YES } ]`

`[ REVERSE   = { NO   | YES } ]`

`[ RPACING   = { 36   | n } ]`

`[ RRUSIZE   = n ]`

`[ RTO   = { 260   | n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   | n } ]`

`[ SCOMP   = { 0 | 10   | 15 } ]`

`[ SEGMENT   = { NO   | YES } ]`

`[ SPACING   = { 36   | n }]`

`[ SRIN   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SROUT   = { BOTH   | NONE | RECEIVER | SENDER } ]`

`[ SSERV   = { PESIT   | string } ]`

`[ SSL   = identifier ]`

`[ TURN   = { FILE   | MESSAGE } ]`

` `

[CFTPROT details](../../admin_intro/admin_config_commands/transfer_protocol_concepts)

[File
Transfer Protocol](../../admin_intro/admin_config_commands/transfer_protocol_concepts)

<span id="CFTRECV"></span>

#### CFTRECV ID= identifier: Description of model file receiving parameters

Syntax

`[ ACKEXEC = filename]`

`[ COMMENT   = string ]`

`[ CYCDATE   = { 0   | date } ]`

`[ CYCTIME   = { 0   | time } ]`

`[ DELETE   = { NO   | YES } ]`

`[ DIRNB   = { 0   | n } ]`

`[ DUPLICATE = { string 512 } ]`

`[ EXEC   = filename ]`

`[ EXECRALL = { all | parent| children} ]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘   ‘ | character } ]`

`[ FACTION   = { ‘   ‘ | DELETE | ERASE |  RENAME | VERIFY  } ]`

`[ FBLKSIZE   = { 0   | n } ]`

`[ FCHECK   = { NO   | YES } ]`

`[ FCODE   = { ‘   ‘ |BINARY | EBCDIC | ASCII } ]`

`[ FDB   = filename ]`

`[ FDELETE = " " |* | C |D | K | H | T | X]`

`[ FDISP   = { BOTH   | NEW | OLD } ]`

`[ FILENOTFOUND = { ABORT | IGNORE } ]`

`[ FKEYLEN   = { 0   | n } ]`

`[ FKEYPOS   = { 0   | n } ]`

`[ FLOWNAME = string ]`

`[ FLRECL   =  { 0   | n  } ]`

`[ FNAME   = filename ]`

`[ FORCE   = { NO   | YES }  ]`

`[ FORG   = { SEQ   | DIRECT | INDEXED | PART } ]`

`[ FPAD = { ' ' | character } ]`

`[ FRECFM   = { ‘   ‘ |F | V | U } ]`

`[ FSPACE   =  { 0   | n } ]`

`[ FTYPE   =  { ‘   ‘ | character } ]`

`[ GROUPID   = string ]`

`[ MACTION   =  { '   ' | REPLACE } ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { 10000101   | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NCOMP   = { 0 | 15   } ]`

`[ NETBAND = { 1...16} ]`

`[ NOTIFY   = string ]`

`[ NPAD = { ' ' | character } ]`

`[ NCODE   = { ‘   ‘ |ASCII | BINARY | EBCDIC } ] *available only when the protocol is SFTP`

`[ OPERMSG   = { 0   | 255 } ]`

`[ PRI   = { 128 | n } ]`

`[ RAPPL   = string ]`

`[ RKERROR   = { ' ' | DELETE | KEEP } ]`

`[ RPASSWD = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SERIAL = { ' ' | Y | Z | X } ]`

`[ SOURCEAPPL =  string ]`

`[ SPASSWD = string ]`

`[ STATE   = { DISP | HOLD | KEEP } ]`

`[ STORAGEACCOUNT = string ]`

`[ SUSER   = string ]`

`[ TARGETAPPL = string ]`

`[ TRK   =  { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ USERID   = { CFT server"userid" | string } ]`

`[ WFNAME   = filename ]`

`[ WORKINGDIR = string ]`

`[ XLATE   = identifier ]`

<span style="font-weight: bold;">**** ****</span>[CFTRECV details](../web_copilot_ui/flow_def_intro/cftrecv)

<span id="CFTSEND"></span>

#### CFTSEND ID= identifier: Description of model file sending parameters

Syntax

`[ ACKEXEC = filename]`

`[ ARCHIVEFNAME = string ]`

`[ COMMENT   = string ]`

`[ CYCDATE   = { 0   | date } ]`

`[ CYCLE   = { 0   | n } ]`

`[ CYCTIME   = { 0   | time } ]`

`[ DELETE   = { NO   | YES } ]`

`[ DUPLICATE = { string 512 } ]`

`[ EXEC   = filename ]`

`[ EXECSUB   = { LIST   | FILE | SUBF } ]`

`[ EXECSUBA = {LIST | FILE | SUBF } ]`

`[ EXECSUBPRE = { LIST   | FILE | SUBF } ]`

`[ EXIT   =  identifier   ]`

`[ FACC   = { ‘   ‘ | character } ]`

`[ FACTION   = { NONE   | DELETE | ERASE | ARCHIVE } ]`

`[ FBLKSIZE   = { 0   | n } ]`

`[ FCODE   = { ‘   ‘ |ASCII | BINARY | EBCDIC } ]`

`[ FDB   = filename ]`

`[ FDELETE = " " |* | C |D | K | H | T | X]`

`[ FDISP   = { SHR   | OLD | CHECK } ]`

`[ FILENOTFOUND =  { ABORT | IGNORE } ]`

`[ FILTER = string ]`

`[ FILTERTYPE = string ]`

`[ FKEYLEN   = { 0   | n } ]`

`[ FKEYPOS   = { 0   | n } ]`

`[ FLOWNAME = string ]`

`[ FLRECL   = { 0   | n } ]`

`[ FNAME    = { filename   | mask | dirname | #filename | #mask | #dirname } ]`

`[ FORCE   = { NO   | YES } ]`

`[ FORG   = { SEQ   | DIRECT | INDEXED | PART } ]`

`[ FPAD = { ' ' | character } ]`

`[ FRECFM   = { ‘   ‘ | F | U | V } ]`

`[ FSPACE   = { 0   | n } ]`

`[ FTYPE   = { ‘   ‘ | character } ]`

`[ GROUPID   = string ]`

`[ IDA = string ]`

`[ IMPL   = { NO   | YES } ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { 10000101|   date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NBLKSIZE   = { 0   | n } ]`

`[ NCODE   = { ‘   ‘ |ASCII | BINARY | EBCDIC } ]`

`[ NCOMP   =  { 0 |   15   } ]`

`[ NETBAND   = { 1...16} ]`

`[ NFNAME   =  { filename   | *filename } ]`

`[ NKEYLEN   = { 0   | n } ]`

`[ NKEYPOS   = { 0|   n } ]`

`[ NLRECL   =  { 0   | n } ]`

`[ NOTIFY   = string  ]`

`[ NPAD = { ' ' | character } ]`

`[ NRECFM   = { ‘   ‘ | F | U | V } ]`

`[ NSPACE   =  { 0   | n } ]`

`[ NTYPE   = character ]`

`[ OPERMSG   = { 0   | 255 } ]`

`[ PARM   = string  ]`

`[ PREEXEC = filename ]`

`[ PRI   = { 128   | n } ]`

`[ RAPPL   = string ]`

`[ RPASSWD = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SELFNAME   = filename ]`

`[ SERIAL = { ' ' | Y | Z | X } ]`

`[ SPART   = string ]`

`[ SPASSWD = string ]`

`[ SOURCEAPPL =  string ]`

`[ STATE   = { DISP   | HOLD | KEEP } ]`

`[ STORAGEACCOUNT = string ]`

`[ SUSER   = string ]`

`[ TARGETAPPL = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   =  { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ USERID   = { CFT   server "userid" | string } ]`

`[ WFNAME   = filename ]`

`[ WORKINGDIR = string ]`

`[ XLATE   = identifier ]`

[CFTSEND details](../web_copilot_ui/flow_def_intro/cftsend)

[Send
file template](../../concepts/cft_configuration_concepts_start_here/default_send_template_concepts)

<span id="CFTSSL"></span>

#### CFTSSL ID = identifier: Security files

Syntax

`CIPHLIST   = ( number, number, …) `

`DIRECT   = { CLIENT   | SERVER }`

`[ CACHE   = { NO   | YES } ]`

`[ CERFNAME   = string ]`

`[ COMMENT   = string ]`

`[ DEPTH   = { 10   | n } ]`

`[ DNISSUER   = ( string, string, …) ]`

`[ DNUSER   = { ( string, string, …) | ( string, OP (see Note), string ) } ] `

`[ INTERCID   = string ] `

`[ KEYEXT = { VERIFY | NONE } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ PARM   = string ]`

`[ PASSW = string ] `

`[ ROOTCID   = ( string, string, …) ]`

`[ TRACE   = { 0   | n } ]`

`[ USERCID   = string ]`

`[ VERIFY   = { NONE| REQUIRED   | OPTIONAL } ] *When DIRECT=SERVER`

`[ VERIFY   = { NONE| REQUIRED   | ENFORCED | OPTIONAL } ] *When DIRECT=CLIENT `

`[ VERSION   = { TLSV1  | SSLV3 | TLSV1 | SSLV3COMP | TLSV1COMP} ]`

<span style="font-weight: bold;">****Note****</span>: <span id="OP"></span>You can configure Transfer
CFT to accept or reject SSL connections based on logical operators used
within the DN of the certificate. For details refer to [dnuser](parameter_intro/dnuser)
parameter details.

[CFTSSL details](../../transport_security_start_here/configuring_transport_security_start_here/transport_security_cftssl)

[SSL/TLS security concepts](../../transport_security_start_here)

<span id="CFTTCP"></span>

#### CFTTCP: Network parameters of a TCP/IP partner

Syntax

`HOST   = string `

`ID   = identifier `

`[ CLASS   = { 1   | n } ]`

`[ CNXIN   = { 2   | n } ]`

`[ CNXINOUT   = { 4   | n } ]`

`[ CNXOUT   = { 2   | n } ]`

`[ IMAXTIME   = { 23595999   | time } ]`

`[ IMINTIME   = { 0   | time } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ OMAXTIME   = { 23595999   | time } ]`

`[ OMINTIME   = { 0   | time } ]`

`[ RETRYM   = { 12   | n } ]`

`[ RETRYN   = { 4   | n } ]`

`[ RETRYW   = { 1   | n } ]`

`[ VERIFY   = { 0   | n } ]`

[TCP
attributes for a partner](../../admin_intro/admin_config_commands/network_resource_concepts)

<span id="CFTUIPREF"></span>

#### CFTUIPREF MODE=mode

MODE= { CREATE | <u>REPLACE</u> | DELETE }

[ID](parameter_intro/id)
= identifier  

[TYPE](parameter_intro/type) = string

\[ [COMMENT](parameter_intro/comment)
= string \]

`[ CONTENT   = { ACTIVE   | FULL } ]`

\[ [ORIGIN](parameter_intro/origin) \] = string \]

<span id="CFTXLATE"></span>

#### CFTXLATE FNAME = filename: Define conversion tables

Syntax

`ID   = identifier  `

`[ COMMENT   = string ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FCODE   = { '   ' | ASCII | EBCDIC } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE } ]`

`[ NCODE   = { '   ' | ASCII | EBCDIC } ]`

`[ ORIGIN ] = string ]`

`[ TABLE ] = string ]`

[Conversion
tables](../../concepts/cft_configuration_concepts_start_here/translation_table_concepts)

<span id="CLEARCMD"></span>

#### CLEARCMD COMMAND = string: Delete a transfer request

Syntax

`USERID   = string`

`INDEX   = number`

[CLEARCMD details](../about_cftutil/managing_transfer_states/clearcmd_command)

<span id="CONFIG"></span>

#### CONFIG: Designate the communication medium and the files accessed by CFTUTIL

<span style="font-weight: normal;">Syntax</span>

CONFIG TYPE = { CAT | INPUT | OUTPUT | PARM | PART }

`FNAME = filename `

` `

CONFIG TYPE = COM

`MEDIACOM = FILE`

`FNAME = filename `

 

`CONFIG   TYPE = COM`

`MEDIACOM   = TCPIP`

`FNAME= string `

`[ HIGHPORT   =  { 65535   | n } ]`

`[ LOWPORT   = { 5000   | n } ]`

`[ PASSWORD = string } ]`

`[ PROXY   = string ]`

`[ ROOTCERT   = string ]`

`[ TIMEOUT   = 60   | n ]`

[Setting default CFTUTIL file names](../about_cftutil/redefining_cftutil_data_media)

<span id="COPYFILE"></span>

#### COPYFILE IFNAME = filename: Copy files with an off-line compression or decompression option

Syntax

`OFNAME   = filename `

`[ CREATE   = { ‘   ‘ | YES | NO } ]`

`[ IBLKSIZE   = { 0   | n } ]`

`[ ICHARSET = string ]`

`[ ICODE   = { ASCII | EBCDIC } ]`

`[ ICOMP   = { 0   | 15 } ]`

`[ ICT   =  { H   | C } ]`

`[ ILRECL   = { 0   | n } ]`

`[ IRECFM   = { F | V | U } ]`

`[ ITYPE   = { ‘ ‘ | character } ]`

`[ OBLKSIZE   = { 0 |n  }   ]`

`[ OCHARSET = string ] `

`[ OCODE   = { ASCII | EBCDIC } ]`

`[ OCOMP   = { 0   | 15 } ]`

`[ OCT   = { H | C } ]`

`[ OLRECL   = { 0   |n } ]`

`[ ORECFM   = { IRECFM   value | F | V| U } ]`

`[ OSPACE   = { 0   | n } ]`

`[ OTYPE   = { ‘   ‘ | character } ]`

`[ XLATE = string ]`

[Copying files off-line](../../admin_intro/admin_commands_intro/copyfile_command)

<span id="DELETE"></span>

#### DELETE PART = { identifier | mask }: Delete a catalog entry

Syntax

`[ BLKNUM   = { 0   | n } ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FORCE   =  { YES   | NO   } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   | transid } ]`

`[ IDTU   = string ]`

`[ STATE   = { *   | C | D | H | K | T | X } ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Deleting catalog entries](../../admin_intro/admin_commands_intro/delete_command)

<span id="DISPLAY"></span>

#### DISPLAY \[ CONTENT = { <span style="text-decoration: underline;">listcat</span> | identifier }\]: Display a model-formatted catalog

Syntax

`[ DATETIMEMAX = { 0 | 991231235959 } ]`

`[ DATETIMEMIN = { 0 | 991231235959 } ]`

`[ DIAGI = { * | 0 | ERROR } ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ EMPTY   = { ANY   | string } ]`

`[ FILE   = filename ]`

`[ FMODEL   =  string   ]`

`[ FOUT = string ]`

`[ HELP   = { NONE   | FIELDS | MODELS | COMMAND } ]`

`[ IDA   =  string   ]`

`[ IDF   = string ]`

`[ IDT   = string ]`

`[ IDTU   = string ]`

`[ MODE   = { ANY   | COLUMN | LINE } ]`

`[ NA   = { ANY   | string } ]`

`[ NIDT    = { string of 8 digits } ]`

`[ NPART   = string ]`

`[ PART   = string ]`

`[ RUSER   = string ]`

`[ SORTBY   = string ]`

`[ SUSER   = string ]`

`[ STATE   = { *   | character } ]`

`[ TYPE   = { *   | FILE | MESSAGE | REPLY | ALL } ]`

[Catalog output display model](../about_cftutil/monitoring_cftutil_intro/display_command)

<span id="END"></span>

#### END PART = { identifier | mask }: Change transfer to X state

Syntax

`[ APPCYCID    = string ]`

`[ APPOBJID    = string ]`

`[ APPSTATE    = string ]`

`[ SAPPL    = string ]`

`[ RUSER    = string ]`

`[ SUSER    = string ]`

`[ RPASSWD    = string ]`

`[ SPASSWD    = string ]`

`[ BLKNUM   = { 0   | n } ]`

`[ DIAGC = string ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FORCE   = { NO   | YES } ]`

`[ FNAME    = string ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   | transid } ] `

`[ IDTU   = string ]`

`[ ISTATE = { YES | NO } ]`

`[ ISTATE    = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ NFNAME    = string ]`

`[ PARM    = string ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ PRI Number { 0 - 256 } ]`

`[ RAPPL    = string ]`

`[ SCOPE = string ]`

`[ SIGFNAME    = string ]`

`[ STATE  = string ]`

[END details](../about_cftutil/managing_transfer_states/end_command)

<span id="HALT"></span>

#### HALT PART = { identifier | mask }: Suspend a transfer

Syntax

`[ BLKNUM   = { 0   | n } ]`

`[ DIAGC = string ]`

`[ DIAGP = string ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FORCE   = { YES | NO   } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier  ]`

`[ IDT   = { *   | transid } ]`

`[ IDTU   = string  ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Halting a transfer](../about_cftutil/managing_transfer_states/halt_command)

#### HELP

Syntax

`[ CMD = { string } ]`

`[ CONTENT = { BRIEF | DETAIL} ]`

`[ OFORMAT = { txt | xsd } ]`

<span id="INACT"></span>

#### INACT ID = identifier: Inactivate a partner

Syntax

`[ MODE   = { BOTH   | REQUESTER| SERVER } ]`

`[ FORCE   = { NO   | YES } ]`

`[ TYPE   = PART   | TRK | CRON | FOLDER ]`

[INACT details](../about_cftutil/reactivate_an_object_cl/inact_command)

<span id="KEEP"></span>

#### KEEP PART = { identifier | mask }: Abort a transfer

Syntax

`[ BLKNUM   = 0   | n ]`

`[ DIAGC = string ]`

`[ DIAGP = string ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FORCE   = { YES   | NO } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   | transid } ]`

`[ IDTU   = string  ]`

`[ STATE   = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

<span style="font-weight: bold;">**** ****</span>[Suspend transfers](../about_cftutil/managing_transfer_states/keep_command)

<span id="KSTATE"></span>

#### KSTATE IDF = identifier: Change a transfer state

Syntax

`IDTU   = local transfer counter identifier`

`PART   = partner identifier`

[KSTATE details](../about_cftutil/managing_transfer_states/kstate_command)

<span id="LISTCAT"></span>

#### LISTCAT TYPE = { <span style="text-decoration: underline;">ALL</span> | \* | FILE | MESSAGE | REPLY }: List catalog entries

Syntax

`[ CONTENT   = { BRIEF   | FULL | DEBUG | EXTEND | COMMUT | STAT | BLKNUM } ]`

`[ DATETIMEMAX = { 0 | 991231235959 } ]`

`[ DATETIMEMIN = { 0 | 991231235959 } ]`

`[ DIAGI = { * | 0 | ERROR } ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FILE   = filename ]`

`[ IDA   = { *   | identifier } ]`

`[ IDF   = { *   | identifier } ]`

`[ IDT   = { *   | transid } ]`

`[ IDTU   = string  ]`

`[ NIDT    = { string of 8 digits } ]`

`[ NPART   = { identifier | mask } ]`

`[ PART   = { *   | identifier | mask } ]`

`[ SORTBY   = string ]`

`[ STATE   = { *   | string } ]`

<span style="font-weight: bold;">**** ****</span>[LISTCAT details](../about_cftutil/monitoring_cftutil_intro/listcat_command)

<span id="LISTCOM"></span>

#### LISTCOM: Display contents of a communication media

Syntax

`[ CONTENT   = { ACTIVE   | FULL } ]`

`[ FILE   = filename ]`

`[ FIRST   = { 0   | n } ]`

`[ LAST   = { 0   | max. number of records } ]`

`[ VERIFY   = { NO   | YES } ]`

<span style="font-weight: bold;">**** ****</span>[LISTCOM details](../about_cftutil/monitoring_cftutil_intro/listcom_command)<span style="font-weight: bold;">**** ****</span>

<span id="LISTLOG"></span>

#### LISTLOG: Display and filter log content including merged nodes in cluster mode

Syntax

`[ LOGLEVEL = { F | E | W | I } ]`

`[ LINES = { -10000 | -20 | 10000 } ]`

`[ DATEMAX =  { 0 | 991231 } ]`

`[ DATEMIN           = { 0 | 991231 } ]`

`[ DATETIMEMAX = { 0 | 99123123595999 } ]`

`[ DATETIMEMIN { 0 | 99123123595999 } ]`

`[ TIMEMIN = { 0| 23595999 } ]`

`[ TIMEMAX           = { 0 | 23595999 } ]`

`[ PATTERN           =     string ]`

`[ DISPLAYNODEID     = { YES | NO } ]`

`[ NODE = string ]`

#### LISTNODE: Display all nodes

Syntax

No parameters

[Multi-node commands](../../about_multinode/multi_node_commands)

<span id="LISTPARM"></span>

#### LISTPARM: Display {{< TransferCFT/axwayvariablesComponentShortName  >}} partner details

Syntax

`[ ID   = { *   | identifier } ]`

`[ CONTENT   = { FULL   | BRIEF } ]`

`[ PART   = identifier  ]`

`[ TYPE   = { ACCNT | ALL   | APPL | AUTH | CAT | COM | CRON | EXIT | IDF | LOG | NET | PARM | PROT   | RECV | SEND | SSL | XLATE } ]`

[LISTPARM details](../about_cftutil/configuring_cft_start_here/listparm)

<span id="LISTPART"></span>

#### LISTPART: Display partners

Syntax

`[ ID   = { *   |identifier } ]`

`[ CONTENT   = { FULL   | BRIEF } ]`

`[ TYPE   = { ALL   | DEST |  PART |  TCP | } ]`

[LISTPART details](../about_cftutil/configuring_cft_start_here/listpart_command)

<span id="MQUERY"></span>

#### MQUERY : Query one or more {{< TransferCFT/axwayvariablesComponentShortName  >}} components

Syntax

OBJECT = CACHE

`[ OBJECT   = { CACHE   | SYSTEM | STATS | PROBE } ]`

`[ CONTENT   = { BRIEF   | FULL | STAT } ]`

`[ NAME   = { CAT   | COMMAND | CRON | DMZ | STAT } ]`

` `

OBJECT = SYSTEM

`[ OBJECT   = { CACHE   | SYSTEM | STATS | PROBE } ]`

`[ CONTENT   = { BRIEF   | FULL | STAT } ]`

`[ NAME   = { CFTMAIN | CFTTRK | CFTTFIL | CFTCOM | CFTTPRO | CFTEXIT | CFTPRX | CFTDSCAN } ]`

` `

OBJECT = STATS or PROBE

`[ OBJECT   = { CACHE   | SYSTEM | STATS | PROBE } ]`

`[ CONTENT   = { XMLBRIEF   | XMLFULL | RAW } ]`

`[ NAME   = { CAT   | COMMAND | CRON | DMZ | STAT } ]`

<span style="font-weight: bold;">**** ****</span>[MQUERY details](../../admin_intro/admin_commands_intro/querying_a_component_)

<span id="PURGE"></span>

#### PURGE: Delete records from the catalog 

Syntax

`[ TIMEP   = { 23595999   | time } ]`

[PURGE details](../../admin_intro/admin_commands_intro/purge_catalog)

<span id="RECONFIG"></span>

#### RECONFIG: Reload

Syntax

`[ TYPE   = { CRON | UCONF | CAT |  FOLDER | PARMCACHE | AM  } ] `

 [Manage configuration updates](../../admin_intro/admin_commands_intro/reconfig)

<span id="RECV"></span>

#### RECV IDF = { identifier | mask }: Request to receive transfer

Syntax

`PART   = identifier   `

`[ ACKEXEC = filename]`

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ COMMENT   = string  ]`

`[ CYCDATE   =  date    ]`

`[ CYCLE   =  { 0   | n } ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR | RESUME } ]`

`[ DIRNB   = n ]`

`[ EXEC   = filename ]`

`[ EXECRALL = { all | parent| children} ]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘   ‘ | character }  ]`

`[ FACTION   = { ‘   ‘ | DELETE | ERASE | RENAME | VERIFY } ]`

`[ FBLKSIZE   = n ]`

`[ FCODE   = { BINARY | EBCDIC | ASCII } ]`

`[ FCOMP   = { 0   | 15 } ]`

`[ FDATE   = { 0   | date } ]`

`[ FDB   = filename ]`

`[ FDISP   = { BOTH   | NEW | OLD }  ]`

`[ FILE   = { FIRST   | ALL }  ]`

`[ FKEYLEN   = { 0   | n }  ]`

`[ FKEYPOS   = { 0   | n }   ]`

`[ FLRECL   = n ]`

`[ FNAME   = filename ]`

`[ FORG   = { SEQ   | DIRECT | INDEXED | PART } ]`

`[ FPAD = { ' ' | character } ]`

`[ FRECFM   = { ‘   ‘ | F | V | U }  ]`

`[ FSPACE   = n ]`

`[ FTIME   = { 0 | time } ]`

`[ FTYPE   =  { ‘   ‘ | character } ]`

`[ IDA   = identifier ]`

`[ MACTION   = { '   ' | REPLACE }  ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   | time }  ]`

`[ MINDATE   = { current system date | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ MODE   = { REPLACE   | CREATE | DELETE }  ]`

`[ NCOMP   = { 0   | 15 } ]`

`[ NETBAND = { 1...16} ]`

`[ NFNAME   = filename ]`

`[ NFVER   =  { 0   | 255 } ]`

`[ NIDF   = string ]`

`[ NPAD = { ' ' | character } ]`

`[ PARM   = string ]`

`[ PRI   = { 128   | n } ]`

`[ PROT = identifier ] `

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SERIAL = { ' ' | Y | Z | X } ]`

`[ STATE   = { DISP   | HOLD | KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH }  ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ WFNAME   = filename ]`

`[ WORKINGDIR = string ]`

`[ WSTATES = { string } ]`

`[ WTIMEOUT = { integer } ]`

`[ XLATE   = identifier ]`

[Receiving files](../../concepts/c_recv)

<span id="RESUME"></span>

#### RESUME: Retrieves, in server mode, a blocked send request

Syntax

`PART   = string`

`[ DIRECT   = { SEND |RECV | BOTH   } ]`

`[ BLKNUM   = { 0   | n } ]`

`[ FORCE   = { NO   | YES } ]`

`[ IDA   = string ]`

`[ IDF   = string ]`

`[ IDT   = string ]`

`[ IDTU   = string ] `

`[ STATE  = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[RESUME details](../about_cftutil/managing_transfer_states/resume_command)

` `

<span id="SEND"></span>

#### SEND: Request to send transfer

<span class="MCDropDownHead dropDownHead">![Closed](/Images/TransferCFT/transparent.gif)Syntax</span>

[TYPE](parameter_intro/type)
= FILE

`IDF   = identifier  `

`PART   = identifier `

`[ ACKEXEC = filename]`

`[ ACKMINDATE = date ]`

`[ ACKMINTIME = time ]`

`[ ACKSTATE = { REQUIRE | IGNORE } ]`

`[ ACKTIMEOUT = { 0 | n }   ]`

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ ARCHIVEFNAME = string ]`

`[ COMMENT   = string ]`

`[ CYCDATE   = date ]`

`[ CYCLE   = { 0   | n }  ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR | RESUME } ]`

`[ EXEC   = filename ]`

`[ EXECSUB   = { LIST   | FILE | SUBF } ]`

`[ EXECSUBA = {LIST | FILE | SUBF }]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘ ‘ | character } ]`

`[ FACTION   = { NONE   | DELETE | ERASE |  ARCHIVE } ]`

`[ FBLKSIZE   = n ]`

`[ FCODE   = { ASCII | BINARY | EBCDIC } ]`

`[ FDATE   = date ]`

`[ FDB   = filename ]`

`[ FDISP   = { SHR   | OLD | CHECK } ]`

`[ FILTER = string ]`

`[ FILTERTYPE = string ]`

`[ FKEYLEN   = { 0   | n } ]`

`[ FKEYPOS   = { 0   | n } ]`

`[ FLRECL   = n ]`

`[ FNAME    = { filename   | mask | dirname | #filename | #mask | #dirname } ]`

`[ FNAMEABS   = { YES | NO }  ]`

`[ FORG   = { SEQ   | DIRECT | INDEXED } ]`

`[ FPAD = { ' ' | character } ]`

`[ FRECFM   = { ‘ ‘ | F | U | V }  ]`

`[ FSPACE   = n ]`

`[ FTIME   = time  ]`

`[ FTYPE   =  { ‘ ‘   | character } ]`

`[ IDA   = identifier ]`

`[ IPART   = identifier ]`

`[ MAXDATE   = { 99991231   | date } ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { current   system date | date } ]`

`[ MAXDURATION =  0...32767} ]`

`[ MINTIME   = { 0   | time } ]`

`[ NBLKSIZE   = n ]`

`[ NCODE   = { ASCII | BINARY | EBCDIC } ]`

`[ NCOMP   = { 0 | 15 } ]`

`[ NETBAND   = { 1...16} ]`

`[ NFNAME   = filename ]`

`[ NIDF   = string ]`

`[ NKEYLEN   = { 0   | n } ]`

`[ NKEYPOS   = { 0   | n }]`

`[ NLRECL   = n  ]`

`[ NPAD = { ' ' | character } ]`

`[ NRECFM   = { ‘   ‘ | F | U | V } ]`

`[ NSPACE   = { FSPACE   value | n } ]`

`[ NTYPE   = { ' ' | character } ]`

`[ PARM   = string ]`

`[ PRESTATE = { ' ' | character } ]`

`[ PREMINDATE = date ]`

`[ PREMINTIME = time ]`

`[ PRETIMEOUT = { 0 | n } ]`

`[ POSTMINDATE = date ]`

`[ POSTMINTIME = time ]`

`[ PRI   = { 128   | n } ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SELFNAME    = filename   ]`

`[ SERIAL = { ' ' | Y | Z | X } ]`

`[ SPART   = identifier ]`

`[ STATE   = { DISP   | HOLD | KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

`[ WORKINGDIR = string ]`

`[ WPHASES = { string } ]`

`[ WPHASESTEPS = { string } ]`

`[ WSTATES = { string } ]`

`[ WTIMEOUT = { integer } ]`

`[ XLATE   = identifier ]`

 

SEND TYPE = MESSAGE  

`IDM   = identifier  `

`MSG   = string   `

`PART   = identifier `

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ CYCDATE   = date ]`

`[ CYCLE   = { 0   | n } ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR | RESUME} ]`

`[ EXEC   = filename ]`

`[ IDA   = identifier ]`

`[ IPART   = identifier ]`

`[ MAXDATE   = { 99991231   | date  }   ]`

`[ MAXTIME   = { 23595999   | time }  ]`

`[ MINDATE   = { current   system date | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ PRI   = pri  ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SPART   = identifier ]`

`[ STATE   = { DISP   | HOLD | KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

` `

SEND TYPE = REPLY

`IDM   = identifier `

`IDT   = transid  `

`MSG   = string   `

`PART   = identifier `

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ CYCDATE   = date ]`

`[ CYCTIME   = time ]`

`[ EXEC = filename ]`

`[ IDA = identifier ]`

`[ IPART   = string ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { current   system date | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ PRI   = pri ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

` `

SEND TYPE = NACK

`IDM   = identifier `

`IDT   = transid  `

`MSG   = string   `

`PART   = identifier `

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ CYCDATE   = date ]`

`[ CYCTIME   = time ]`

`[ EXEC = filename ]`

`[ IDA = identifier ]`

`[ IPART   = string ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { current   system date | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ PRI   = pri ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`

[Sending files](../../concepts/using_the_send_command/send_command_basics)

<span id="SHUT"></span>

#### SHUT: Shut down {{< TransferCFT/axwayvariablesComponentShortName  >}} 

Syntax

`[ FAST   = { NO   | YES | KILL } ]`

`[ RESTART = { YES | NO } ] `

[Manage the {{< TransferCFT/axwayvariablesComponentLongName  >}} server: stop the server](../../admin_intro/start_stop_cft#Stop__server)

<span id="START"></span>

#### START: Restart transfer

Syntax

`PART   = { identifier | mask }   `

`[ BLKNUM   = { 0   | n } ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ FORCE   = YES | NO   ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   | transid } ]`

`[ IDTU   = string ]`

`[ MAXDURATION =  {0...32767} ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Restart transfers](../about_cftutil/managing_transfer_states/start_command)

<span id="SUBMIT"></span>

#### SUBMIT: Submit end-of-transfer procedure

Syntax

`PART   = { identifier | mask }  `

`[ APPSTATE =  string ]`

`[ BLKNUM   = { 0   | n } ]`

`[ DIRECT   = { BOTH   | RECV | SEND } ]`

`[ EXEC   = filename ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { * | transid } ]`

`[ IDTU   = string ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   | n } ]`

`[ KTIME   = { 0   | n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

[SUBMIT details](../about_cftutil/managing_transfer_states/submit_command)

<span id="SWAITCAT"></span>

#### SWAITCAT: Wait for a transfer state update

Syntax

`SELECT   = expression`

`[ PHASES  = string ]`

`[ PHASESTEPS = string ]`

`[ STATED = string ]`

`[ STATES   = string ]`

`[ TIMEOUT   = integer ]`

[SWAITCAT concepts](../about_cftutil/managing_transfer_states/swaitcat_concepts) 

[SWAITCAT examples](../about_cftutil/managing_transfer_states/sync_transfer_request_tasks)

<span id="SWITCH"></span>

#### SWITCH: Manually switch LOG or ACCNT files 

Syntax

`[ TYPE   = { LOG | ACCNT } ]`

[SWITCH details](../../admin_intro/admin_commands_intro/switching_files_manually)

[Toggle
log or accounting file]()

<span id="UCONFSET"></span>

#### UCONFSET: configuration utility

Syntax

`ID   = string  `

`[ VALUE   = {string} ]`

[UCONF details](../../admin_intro/uconf/uconf_commands)

<span id="UCONFGET"></span>

#### UCONFGET

Syntax

`ID   = string `

[UCONF details](../../admin_intro/uconf/uconf_commands)

<span id="LISTUCONF"></span>

#### LISTUCONF

Syntax

`ID   = string  `

`[ CONTENT = BRIEF | FULL ]`

`[ FOUT = string ]`

`[ SCOPE   = { INSTANCE | SET | * | ALL } ]  `

`[ VALUE = string ]`

[](../../admin_intro/uconf/uconf_commands)

UCONF details

<span id="WAIT"></span>

#### WAIT: Suspend CFTUTIL execution for the time indicated 

Syntax

`[ DURING   = { 0   | n } ]    *Time   in seconds`

`[ TIME   = { 0   | 23595999 } ] `

[Suspending CFTUTIL execution](../about_cftutil/wait_command)

<span id="WLOG"></span>

#### WLOG: Request to write a message in the LOG file

Syntax

`MSG   = string`

`SEVERITY =   string`

[WLOG details](../../admin_intro/admin_commands_intro/wlog)

<span id="Syntax_conventions"></span>

### Syntax conventions

The command syntax presented in this summary respects the following
general conventions.

1. Mandatory parameters are placed
    before optional parameters.
1. The default value of a parameter
    is placed before the other possible values in the list for this parameter.

### Typographic conventions

- \[   \]
    Optional parameters or values are displayed between square brackets
- {   }
    Parameter choices or values are displayed  between
    braces
- | The vertical
    bar separates an enumerated list of parameter values
- \_\_\_ Default values
    for a parameter are underlined
- , A comma separates
    a list of parameter values

For more information, see [TYPOGRAPHICAL CONVENTIONS.](../../gettingstarted_intro/my_first_transfer_flow_using_cg/typographical_conventions)

### Symbolic variables

The following symbolic variable syntaxes are valid in a {{< TransferCFT/axwayvariablesComponentShortName  >}}
environment:

- &VAR
- &+VAR
- &nVAR:
- &p.VAR
- &p.nVAR
- &(-string\_prefix)
    (+string\_suffix)
    (=string\_alternate)p.nVAR

For information on using symbolic variables in Transfer
CFT, refer to the topic [Symbolic
variables](symbolic_variables).

### Filename usage

This name is a complete physical filename. It can
be either:

- Created dynamically
    from symbolic variables, or
- Correspond to the
    name of a version file

#### Using the FNAME parameter

Filename conventions are as follows:

- filename

Transfers a file for which the name is specified by
a filename.

- mask

The term mask means that the filename includes at
least one wild card character (\* or :). This sends a file that lists all
files that match the mask.

- dirname

Sends a file that lists the contents of the directory.

- #filename

For each file name that is listed in the file, this
sends the corresponding file.

- #mask

For each file name that matches the mask, this sends
the corresponding file.

- #dirname

For each file that exists in the directory, this sends
the corresponding file.

Note: Use the @ symbol in a UNIX environment in place
of the # symbol.

For more information on filename conventions, refer
to [Filename conventions](filename_conventions).

<span id="About_UCONF"></span>

## Using UCONF

In order to merge functioning for all platforms, {{< TransferCFT/axwayvariablesComponentShortName  >}} features a configuration interface that provides product uniformity
regardless of platform differences.

The basic CFTUTIL services provided are:

- UCONFSET id=\*\*\*\*,value=\*\*\*\*
- UCONFGET id=\*\*\*\*
- LISTUCONF id=\*\*\*\*

This configuration replaces all prior configuring methods, such as manually
editing cft.ini, ENV\_CFT, \*.ini, trkapi.conf, and so on, which have been
replaced by a global profile file.

Additionally, UCONF can create all OS specific files that enable Transfer
CFT User Interface operations.

Accepted types include:

- int lower/higher
           
- bool true/false
              
- enum (id1 id2 id3
    id4)  
- identifier      
- string                        
- path (with abstract
    use of / \\) escaped by \\ :::: “/dsqfhdj”
- fname

The ‘$’ sign is a reserved character that is used to reference an environmental
variable. You have abstract use of environment variables using the $ sign.

For example: $(CFT\_INSTALL) -à getenv(“CFT\_INSTALL”)

Variables containing a period “.” refer to sections in the configuration
file. Do not modify this file.

For example: cft.working\_dir = $(cft.runtime\_dir)

The [UCONF](../../admin_intro/uconf) table lists the new UCONF
identifiers, the default values, and the former file values.
