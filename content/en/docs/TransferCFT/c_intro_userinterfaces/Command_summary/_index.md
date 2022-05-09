{
    "title": "Transfer CFT command guide and syntax ",
    "linkTitle": "Command guide and parameters",
    "weight": "120"
}This topic provides a useful list of {{< TransferCFT/axwayvariablesComponentShortName  >}} commands,
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

`[ TYPE   = { ALL   &#124; HOST &#124; CFT } ]`

`[ KEY = { FIRST &#124; ALL } ]`

[ABOUT details](../about_cftutil/about_command)

<span id="ACT"></span>

#### ACT: Reactivates a partner

Syntax

`ID = identifier `

`[ TYPE   = { PART   &#124; TRK &#124; CRON &#124; FOLDER } ]`

`[ MODE   = { BOTH   &#124; REQUESTER&#124;   SERVER } ]`

[ACT details](../about_cftutil/reactivate_an_object_cl)

<span id="CFTACCNT"></span>

#### CFTACCNT: Defines transfer accounting records

Syntax

{{% TransferCFT/snippets/CFTACCNT%}}

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

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

CFTAPPL MODE = DELETE

`ID   = identifier `

`USERID   = string`

`DIRECT   = { BOTH   &#124;  SEND   &#124; RECV }`

`[ COMMENT   = string ]`

`[ GROUPID   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

[CFTAPPL details](../web_copilot_ui/flow_def_intro/cftappl)

[Assign a transfer owner]()

#### CFTAUTH: Defines an Authorized Flow Definition list

Syntax

`FNAME = filename `

`ID = identifier`

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

` `

Or

 

`IDF = (identifier &#124; mask, identifier &#124; mask, …)`

`ID = identifier`

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

**** ****

<span id="CFTCAT"></span>

#### CFTCAT: Defines Catalog attributes

Syntax

`FNAME   = filename `

`ID   = identifier  `

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ RH   = { 10   &#124; n }  ]`

`[ RKERROR   = { KEEP   &#124; DELETE } ]`

`[ RT   = { 10   &#124; n } ]`

`[ RX   = { 10   &#124; n } ]`

`[ SH   = { 10   &#124; n } ]`

`[ ST   = { 10   &#124; n } ]`

`[ SX   = { 10   &#124; n } ]`

`[ TIMEP   = { 23595999   &#124; HHMMSSCC } ]`

`[ TLVCEXEC   = { n } ]`

`[ TLVCLEAR   = { TLVWARN-10 &#124; n } ]`

`[ TLVWEXEC   = { n } ]`

`[ TLVWRATE   = { 60 &#124; n } ]`

`[ TLVWARN   = { 80 &#124; n } ]`

`[ UPDAT   = { 256   &#124; n } ]`

`[ WSCAN   = { 5   &#124; n } ]`

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

`[ MODE   = { REPLACE &#124; CREATE &#124; DELETE } ]`

`[ TLVCEXEC   = { n } ]`

`[ TLVCLEAR   = { TLVWARN-20 &#124; n } ]`

`[ TLVWEXEC   = { n } ]`

`[ TLVWRATE   = { 60 &#124; n } ]`

`[ TLVWARN   = { 70 &#124; n } ]`

`[ WSCAN   = { 60   &#124; n } ]`

` `

#### CFTCOM TYPE = TCPIP

`ID   = identifier `

`HOST   = string `

`PORT   = number`

`PROTOCOL   = { XHTTP }`

`[ ADDRLIST   = ( string, string, ...) ]`

`[ COMMENT   =  string   ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

**** ****[CFTCOM](../web_copilot_ui/conf_intro/cftcom)

[Communication
media](../../admin_intro/admin_config_commands/communication_media_concepts)

<span id="CFTCRON"></span>

#### CFTCRON: Define {{< TransferCFT/axwayvariablesComponentShortName  >}} cron jobs

Syntax

`ID = identifier`

`CRONTAB   = string`

`EXEC   = filename`

`EXECPOLICY        = [ INSTANCE &#124;ALLNODES ]`

`TIME   = { string &#124; @shutdown &#124; @startup } [FOR   DETAILS: CFTCRON   time syntax]`

`[ PARM   = string ]`

`[ COMMENT   = string ]`

`[ STATE = { ACTIVE &#124; NOACTIVE } ]`

`[ TYPE   = { EXEC   &#124; CFTUTIL } ]`

`[ USERID   = { CFT   server "userid"   &#124; string } ]`

[Define script execution](../web_copilot_ui/flow_def_intro/cftcron)

<span id="CFTDEST"></span>

#### CFTDEST: Definition of a list of partners 

Syntax

CFTDEST FNAME

`ID = identifier `

`FNAME   = filename `

`[ EXEC   = { DEST   &#124; PART &#124; CHILDREN} ]`

`[ EXECA = { DEST   &#124; PART &#124; CHILDREN} ]`

`[ EXECPRE = { DEST   &#124; PART &#124; CHILDREN} ]`

`[ COMMENT   = string ]`

`[ FOR   = { BOTH   &#124; COMMUT &#124;   LOCAL } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NOPART   = { ABORT   &#124; CONTINUE &#124; IGNORE } ]`

CFTDEST PART

`ID = identifier `

`PART   = (identifier, identifier, ...) `

`[ EXEC   = { DEST   &#124; PART } ]`

`[ COMMENT   = string ]`

`[ FOR   = { BOTH   &#124; COMMUT &#124;   LOCAL } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NOPART   = { ABORT   &#124; CONTINUE &#124; IGNORE } ]`

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

`[ FORMAT   = { V23   &#124; 23 &#124; V24 &#124; 24 } ]`

`[ LANGUAGE   = { COBOL   &#124; C } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ PARM   = string ]`

`[ PROG   = { CFTEXIT   &#124; string } ]`

`[ RESERV   = { 16384   &#124; n } ]`

`[ WAITTASK   = { 1441   &#124; n } ]`

 

#### CFTEXIT TYPE = { FILE &#124; ACCESS &#124; EXEC &#124; BOT}

`ID   = identifier `

`TYPE   = { FILE &#124; ACCESS &#124;  EXEC &#124; BOT } `

`[ COMMENT   = string ]`

`[ FORMAT   = { V23   &#124; 23 &#124; V24 &#124; 24 } ]`

`[ LANGUAGE   = { COBOL   &#124; C } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ PARM   = string ]`

`[ PROG   = { CFTEXIT   &#124; string } ]`

`[ RESERV   = { 1024   &#124; n } ]`

`[ WAITTASK   = { 1441   &#124; n } ]`

**** ****CFTEXIT details

[Exit
tasks](../../app_integration_intro/managing_exits)

<span id="CFTEXT"></span>

#### CFTEXT: Extract data from the parameter and partner files 

Syntax

`[ TYPE   = { ALL   &#124; ACCNT &#124; APPL &#124; AUTH &#124; CAT &#124; COM &#124; CRON &#124; DEST &#124;  EXIT &#124; IDF &#124; LOG   &#124;  NET &#124; PARM &#124; PART &#124; PROT &#124; RECV &#124; SEND &#124; SSL &#124;  TCP &#124;  XLATE } ]`

`[ CONTENT = { FULL &#124; BRIEF } ]`

`[ FOUT   = filename ]`

`[ FPARM   = filename ]`

`[ FPART   = filename ]`

`[ ID   = { *   &#124; identifier &#124; mask } ]`

[Export
configuration](../about_cftutil/configuring_cft_start_here/cftext_command)

<span id="CFTFILE"></span>

#### CFTFILE: Create or delete {{< TransferCFT/axwayvariablesComponentShortName  >}} files

Syntax

`TYPE   = { PARM &#124; PART }`

`FNAME   = filename  `

`[ HABFNAME   = filename ] `

`[ FBLKSIZE   = { 0   &#124;n } ]`

`[ FSPACE   = n ]`

`[ FSPACEX   = n ]`

`[ MODE   = { CREATE   &#124; REPLACE &#124; DELETE } ]`

` `

#### CFTFILE { CAT &#124; COM }

`TYPE   = {  CAT   &#124; COM }`

`FNAME   = filename `

`[ RECNB   = n ]`

`[ FBLKSIZE   = { 0   &#124;n } ]`

`[ FSPACE   = { 0   &#124; n } ]`

`[ FSPACEX   =  { 0   &#124; n } ]`

`[ HABFNAME   = filename ]`

`[ MODE   = { CREATE   &#124; REPLACE &#124; DELETE } ]`

`[ NODE = { 0   &#124; n } ] available for TYPE=CAT`

` `

#### CFTFILE { ACCNT &#124; LOG }

`TYPE   = { ACCNT &#124; LOG }`

`FNAME   = filename `

`[ FBLKSIZE   = 0   &#124; n ]`

`[ FSPACE   = 0   &#124;n ]`

`[ FSPACEX   = 0   &#124;n ]`

`[ MODE   = { CREATE   &#124; REPLACE &#124; DELETE } ]`

[Manually create internal datafile files](../../admin_intro/admin_commands_intro/cftfile)

#### CFTFOLDER

See [CFTFOLDER](../web_copilot_ui/flow_def_intro/cftfolder) for additional parameter details.

`IDF = string         </code></p>`

`PART = string         </code></p>`

`SCANDIR         = string</code></p>`

`WORKDIR = string         </code></p>`

`[ ARCHIVEDIR = string ]`

`[ ENABLESUBDIR = { YES &#124; NO } ]`

`[ FILEIDLEDELAY = n ]`

`[ METHOD = { FILE &#124; MOVE }] `

`[ STATE = { ACTIVE &#124; } ]`

`[ INTERVAL = n ]`

`[ FILECOUNT = n ]`

`[ FILESIZEMIN = n ]`

`[ FILESIZEMAX = n ]`

`[ INCLUDEFILTER = string ]`

`[ EXCLUDEFILTER = string ]`

`[ RESUBMITCHANGED { YES &#124; NO }]`

`[ FILTERTYPE ]`

`[ GROUPID = string ]`

`[ RENAMEMETHOD ]`

`[ RENAMESEPARATOR = string ] `

`[ USEFSEVENTS = { YES &#124; NO } ]`

`[ USERID = string ]`

<span id="CFTIDF"></span>

#### CFTIDF ID = identifier: Correspondence between the network identifier and the local identifier of a transferred model file 

Syntax

`NIDF   = string `

`PART   = identifier `

`TYPE   = { RECV   &#124; SEND } `

`[ COMMENT   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

**** ****[CFTIDF details](../web_copilot_ui/flow_def_intro/cftidf)

[File
template/virtual file association](../../concepts/cft_configuration_concepts_start_here/network_file_identifier_concepts)

<span id="CFTLOG"></span>

#### CFTLOG FNAME = filename: Log file management parameters 

Syntax

`ID   = identifier `

`[ AFNAME   = filename ]`

`[ COMMENT   = string ]`

`[ CONTENT   = { FULL   &#124; BRIEF } ]`

`[ EXEC   = filename ]`

`[ FORMAT   = { V23   &#124; 23 &#124; V24 &#124; 24 } ]`

`[ LENGTH   = { 160   &#124; n } ]`

`[ MAXREC   = { 0   &#124; n } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NOTIFY   = identifier ]`

`[ OPERMSG   = n ]`

`[ SWITCH   = { 00000000   &#124; time } ]`

[CFTLOG details]()

[Transfer
Log file]()

<span id="CFTNET"></span>

#### CFTNET: Local network resources

Syntax

#### CFTNET TYPE = TCP

`HOST   = { string &#124; INADDR_ANY } `

`ID   = { identifier &#124; *identifier } `

`[ CALL   = { INOUT   &#124; IN &#124; OUT } ]`

`[ CLASS   = { 1 &#124; n } ]`

`[ MAXCNX   = { 384   &#124; n } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ SRCHOST=   { hostname1 &#124; n} ]`

`[ SRCPORTS= { string } ]`

` `

#### CFTNET TYPE = SR

`HOST   = { string &#124; INADDR_ANY } `

`ID   = { identifier &#124; *identifier } `

`[ RECALLHOST = { string } ]`

`[ PORT = {0 ...65535 } ]`

`[ SRCHOST    = { string } ] `

`[ SSLTERM           { YES &#124; NO } ]`

#### PROTOCOL = GENERIC

`HOST   = string `

`ID   = identifier `

`INET   = identifier`

`PORT   =  n `

`[ CALL   = { INOUT   &#124; IN &#124; OUT } ]`

`[ CLASS   = { 1 &#124; n } ]`

`[ MAXCNX   = { 384   &#124; n } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ PARM   = string ]`

`[ SRCHOST=   { hostname1 &#124; n} ]`

` `

#### CFTNET TYPE = TCP

#### PROTOCOL = SOCKS4, SOCKS5

`HOST   = string  `

`ID   = identifier `

`INET   = identifier`

`PORT   =  n `

`[ CALL   = { INOUT   &#124; IN &#124; OUT } ]`

`[ CLASS   = { 1 &#124; n } ]`

`[ MAXCNX   = { 32   &#124; n } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ SRCHOST=   { hostname1 &#124; n} ]`

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

`KEY   = {string &#124; #filename } `

`NET   = ( identifier ,  identifier   ,...) `

`PART   = identifier  `

`PARTFNAM   = filename  `

`PROT   = ( identifier ,  identifier   , ...) `

`[ ACCNT   = identifier  ]`

`[ BUFSIZE   =  { 4096   &#124; n } ]`

`[ COMMENT   = string ]`

`[ CRONTABS   = (crontab, crontab, …) ]`

`[ DEFAULT   = { DEFAUT   &#124; identifier } ]`

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

`[ FBUFSIZE   = { 0   &#124;65535 } ]`

`[ LENAPPL   = { 32   &#124; 1 } ]`

`[ LOG   = identifier  ]`

`[ MAXTASK   = { 8   &#124; n }  ]`

`[ MAXTRANS   =  { 256   &#124; 1 } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE }  ]`

`[ NPART   = string ]`

`[ PKIPASSW   = { PKIPASSW &#124; string } ]`

`[ RCVALLER   = { STOP   &#124; CONTINUE } ]`

`[ SECFNAME   = filename ]`

`[ SSLMTASK   = { 8   &#124; n } ]`

`[ SSLTTASK   =  {3   &#124; n } ]`

`[ SSLWTASK   = { 10   &#124;n } ]`

`[ SSLWRESP   = { 60   &#124; n } ]`

`[ TRACE   = string ]`

`[ TRANTASK   = { 3   &#124; n } ]`

`[ TRKPART   =  { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

`[ TRKRECV   =  { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

`[ TRKSEND   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

`[ USERCTRL   = { NO   &#124; YES } ]`

`[ WAITRESP   = { 60   &#124; n } ]`

`[ WAITTASK   = { 10   &#124; n } ] `

[CFTPARM details](../web_copilot_ui/conf_intro/cftparm)

[Transfer CFT general
parameters](../../admin_intro/admin_config_commands/cftparm_general_parameters)

<span id="CFTPART"></span>

#### CFTPART ID = identifier: Partner definition parameters  

Syntax

`PROT   = { (identifier &#124; mask , identifier &#124; mask , .... ) } `

`[ COMMENT   = string  ]`

`[ COMMUT   = { YES   &#124; NO &#124; SERVER }   ]`

`[ CTRLPART   = { IGNORE   &#124; ALL &#124; RPART &#124; SPART } ]`

`[ FPREFIX   = string ]`

`[ GROUP   = identifier ]`

`[ IDF   = identifier  ]`

`[ IMAXTIME   = { 23595999   &#124; time } ]`

`[ IMINTIME   = { 0   &#124; time } ]`

`[ IPART   = identifier ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NACK = { YES &#124; NO &#124; ANY } ]`

`[ NRPART   = string ]`

`[ NRPASSW = string  ]`

`[ NSPART   = string  ]`

`[ NSPASSW   = string  ]`

`[ OMAXTIME   = { 23595999   &#124; time } ]`

`[ OMINTIME   = { 0   &#124; time } ]`

`[ RAUTH   = { *   &#124; identifier } ]`

`[ SAP   = (string, string, …) ]`

`[ SAUTH   = { *   &#124; identifier } ]`

`[ SSL   = identifier ]`

`[ STATE   = {ACTIVEBOTH   &#124; ACTIVEREQ &#124; ACTIVESERV &#124; NOACTIVE } ]`

`[ SYST   = { ‘   ‘ &#124; GCOS7 &#124; GCOS8 &#124; GUARD &#124;  MVS &#124;  OS400 &#124;   UNIX &#124; VM &#124; VMS &#124;  VSE &#124; WINNT &#124; BS2000 } ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

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

`[ DISCTD   = { 20   &#124; n } ]`

`[ DISCTS   = { 65   &#124; n } ]`

`[ DYNAM   = identifier  ]`

`[ EERP   = { 91   &#124; 86 } ]`

`[ EXITA   = identifier ]`

`[ IDF   = string  ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ PAD   = { NO   &#124; YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCOMP   =  { 0   &#124; 15 } ]`

`[ RCREDIT   = { 4   &#124; n } ]`

`[ RESTART   = { 5   &#124; n } ]`

`[ RESYNC   = { NO   &#124; YES } ]`

`[ REVERSE   = { YES   &#124; NO } ]`

`[ RRUSIZE   = { 2048   &#124; n } ]`

`[ RTO   = { 260   &#124; n } ]`

`[ SAP   = string ]`

`[ SCOMP   = { 0 &#124; 1 &#124;15 } ]`

`[ SCREDIT   = { 4   &#124; n } ]`

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SRUSIZE   = { 2048 &#124; n } ]`

`[ SSL   = identifier ]`

`[ TCP   = { CFT   &#124; OFTP} ]`

` `

CFTPROT TYPE = PESIT

`PROF   = ANY `

`ID   = identifier  `

`NET   = identifier `

`[ CONCAT   = { NO   &#124; YES } ] `

`[ DISCTC   = { 60   &#124; n } ] `

`[ DISCTD   = { 10   &#124; n } ] `

`[ DISCTR   = { 45   &#124; n } ] `

`[ DISCTS   = { 60   &#124;n } ] `

`[ DYNAM   = identifier ] `

`[ EXITA   = identifier ] `

`[ HIDE99   = { NO   &#124; YES } ] `

`[ IDF   = string ] `

`[ LOGON   = { YES   &#124; NO } ] `

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ] `

`[ MULTART   = { NO   &#124; YES } ] `

`[ NACK = { YES &#124; NO &#124; ANY} ]`

`[ PAD   = { NO   &#124; YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCHKW   = { 3   &#124; n } ] `

`[ RCOMP   = { 0    &#124; 15 } ] `

`[ RESTART   = { 5   &#124; n } ] `

`[ RESYNC   = { NO   &#124; YES } ] `

`[ REVERSE   = { NO   &#124; YES } ] `

`[ RPACING   = { 32767   &#124; n } ] `

`[ RRUSIZE   = { 32750   &#124;n } ] `

`[ RTO   = { 260   &#124; n } ] `

`[ SAP   = string ] `

`[ SCHKW   = { 3   &#124; n } ] `

`[ SCOMP   = { 0 &#124; 15} ] `

`[ SEGMENT   = { NO   &#124; YES } ] `

`[ SPACING   = { 32767   &#124; n } ] `

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ] `

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ] `

`[ SRUSIZE   = { 32750   &#124; n } ] `

`[ SSERV   = { GSIT   &#124; string } ] `

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT  

`PROF   = CFT`

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   &#124; YES } ]`

`[ DISCTC   = { 90   &#124; n }  ]`

`[ DISCTD   =  { 20   &#124; n } ]`

`[ DISCTR   = { 45   &#124; n } ]`

`[ DISCTS   = { 65   &#124; n } ]`

`[ DYNAM   = identifier  ]`

`[ EXITA   = identifier  ]`

`[ HIDE99   = { NO   &#124;YES } ]`

`[ IDF   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ MULTART   = { NO   &#124; YES } ]`

`[ PAD   = { NO   &#124; YES } ] <Deprecated in Transfer CFT 3.5>`

`[ RCHKW   = { 2   &#124; n }  ]`

`[ RCOMP   = { 0 &#124;15} ]`

`[ RESTART   = { 5   &#124; n } ]`

`[ RESYNC   = { NO   &#124; YES } ]`

`[ REVERSE   = { NO   &#124; YES } ]`

`[ RPACING   = { 36   &#124; n } ]`

`[ RRUSIZE   = { 4056   &#124; n } ]`

`[ RSERV   = string ]`

`[ RTO   = { 260   &#124; n }  ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   &#124; n } ]`

`[ SCOMP   = { 0&#124; 15  } ]`

`[ SEGMENT   = { NO   &#124; YES } ]`

`[ SPACING   = { 36   &#124; n } ]`

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SRUSIZE   = { 4056   &#124;n } ]`

`[ SSERV   = { CFTPSITX &#124; string } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = EXTERN`

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   &#124; YES } ]`

`[ DISCTC   = { 90   &#124; n } ]`

`[ DISCTD   =  { 120   &#124; n } ]`

`[ DISCTR   = { 45   &#124; n } ]`

`[ DISCTS   = { 165   &#124; n } ]`

`[ DYNAM   = identifier  ]`

`[ EXITA   = identifier   ]`

`[ HIDE99   = { NO   &#124;YES } ]`

`[ IDF   = string  ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ LOGON   = { YES   &#124; NO } ]`

`[ MULTART   = { NO   &#124; YES } ]`

`[ NACK = { YES &#124; NO &#124; ANY } ]`

`[ RCHKW   = { 2   &#124; n } ]`

`[ RCOMP   =  { 0 &#124; 10   &#124;15 } ]`

`[ RESTART   = { 5   &#124; n } ]`

`[ RESYNC   = { NO   &#124; YES } ]`

`[ REVERSE   = { YES &#124; NO } ]`

`[ RPACING   = { 36 &#124; n } ]`

`[ RRUSIZE   = { 4056 &#124; n } ]`

`[ RTO   = { 260   &#124; n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   &#124; n } ]`

`[ SCOMP   =  { 0 &#124; 10   &#124; 15} ]`

`[ SEGMENT   = { NO &#124; YES } ]`

`[ SPACING   = { 36 &#124; n } ]`

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SRUSIZE   = { 4056 &#124; n } ]`

`[ SSERV   = { PESIT   &#124; string } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = SIT`

`ID   = identifier `

`NET   = identifier  `

`[ CONCAT   = { NO   &#124; YES } ]`

`[ DISCTC   = { 90   &#124; n } ]`

`[ DISCTD   = { 240 &#124; n } ]`

`[ DISCTR   = { 45 &#124; n } ]`

`[ DISCTS   = { 285 &#124; n } ]`

`[ DYNAM   = identifier ]`

`[ EXITA   = identifier ]`

`[ IDF   = string ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ MULTART   =  { NO   &#124; YES } ]`

`[ RCHKW   = { 2   &#124; n } ]`

`[ RCOMP   =  { 0 &#124;   15 } ]`

`[ RESTART   = { 5   &#124; n } ]`

`[ RESYNC   = { NO   &#124; YES } ]`

`[ REVERSE   = { NO   &#124; string } ]`

`[ RPACING   = { 36   &#124; n } ]`

`[ RRUSIZE   = { 4050 &#124; n } ]`

`[ RTO   = { 260 &#124; n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2 &#124; n } ]`

`[ SCOMP   = { 0 &#124; 15 } ]`

`[ SEGMENT   = { NO &#124; YES } ]`

`[ SPACING   = { 36   &#124; n } ]`

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SRUSIZE   = { 4050 &#124; n } ]`

`[ SSL   = identifier ]`

 

CFTPROT TYPE = PESIT

`PROF   = DMZ  `

`ID   = identifier `

`NET   = identifier `

`[ CONCAT   = { NO   &#124; YES } ]`

`[ CTO   = { 1   &#124; n } ]`

`[ CYCLE   = { 10   &#124; n } ]`

`[ DISCTC   = { 90   &#124; n } ]`

`[ DISCTD   = { 120   &#124; n } ]`

`[ DISCTR   = { 45   &#124; n } ]`

`[ DISCTS   = { 65   &#124; n  } ]`

`[ DYNAM   = identifier ]`

`[ EXITA   = identifier ]`

`[ IDF   = string ]`

`[ LOGON   = { YES   &#124; NO } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ MULTART   = { NO   &#124; YES } ]`

`[ PAD   = { NO   &#124; YES } ] <Deprecated in Transfer CFT 3.5>`

`[ PART   = ( identifier, identifier, …) ]`

`[ RCHKW   = { 2   &#124; n } ]`

`[ RCOMP   = { 0 &#124; 10   &#124; 15 } ]`

`[ RESTART   = { 5   &#124; n  } ]`

`[ RESYNC   = { NO   &#124; YES } ]`

`[ REVERSE   = { NO   &#124; YES } ]`

`[ RPACING   = { 36   &#124; n } ]`

`[ RRUSIZE   = n ]`

`[ RTO   = { 260   &#124; n } ]`

`[ SAP   = string ]`

`[ SCHKW   = { 2   &#124; n } ]`

`[ SCOMP   = { 0 &#124; 10   &#124; 15 } ]`

`[ SEGMENT   = { NO   &#124; YES } ]`

`[ SPACING   = { 36   &#124; n }]`

`[ SRIN   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SROUT   = { BOTH   &#124; NONE &#124; RECEIVER &#124; SENDER } ]`

`[ SSERV   = { PESIT   &#124; string } ]`

`[ SSL   = identifier ]`

`[ TURN   = { FILE   &#124; MESSAGE } ]`

` `

[CFTPROT details](../../admin_intro/admin_config_commands/transfer_protocol_concepts)

[File
Transfer Protocol](../../admin_intro/admin_config_commands/transfer_protocol_concepts)

<span id="CFTRECV"></span>

#### CFTRECV ID= identifier: Description of model file receiving parameters

Syntax

`[ ACKEXEC = filename]`

`[ COMMENT   = string ]`

`[ CYCDATE   = { 0   &#124; date } ]`

`[ CYCTIME   = { 0   &#124; time } ]`

`[ DELETE   = { NO   &#124; YES } ]`

`[ DIRNB   = { 0   &#124; n } ]`

`[ DUPLICATE = { string 512 } ]`

`[ EXEC   = filename ]`

`[ EXECRALL = { all &#124; parent&#124; children} ]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘   ‘ &#124; character } ]`

`[ FACTION   = { ‘   ‘ &#124; DELETE &#124; ERASE &#124;  RENAME &#124; VERIFY  } ]`

`[ FBLKSIZE   = { 0   &#124; n } ]`

`[ FCHECK   = { NO   &#124; YES } ]`

`[ FCODE   = { ‘   ‘ &#124;BINARY &#124; EBCDIC &#124; ASCII } ]`

`[ FDB   = filename ]`

`[ FDELETE = " " &#124;* &#124; C &#124;D &#124; K &#124; H &#124; T &#124; X]`

`[ FDISP   = { BOTH   &#124; NEW &#124; OLD } ]`

`[ FILENOTFOUND = { ABORT &#124; IGNORE } ]`

`[ FKEYLEN   = { 0   &#124; n } ]`

`[ FKEYPOS   = { 0   &#124; n } ]`

`[ FLOWNAME = string ]`

`[ FLRECL   =  { 0   &#124; n  } ]`

`[ FNAME   = filename ]`

`[ FORCE   = { NO   &#124; YES }  ]`

`[ FORG   = { SEQ   &#124; DIRECT &#124; INDEXED &#124; PART } ]`

`[ FPAD = { ' ' &#124; character } ]`

`[ FRECFM   = { ‘   ‘ &#124;F &#124; V &#124; U } ]`

`[ FSPACE   =  { 0   &#124; n } ]`

`[ FTYPE   =  { ‘   ‘ &#124; character } ]`

`[ GROUPID   = string ]`

`[ MACTION   =  { '   ' &#124; REPLACE } ]`

`[ MAXDATE   =  { 99991231   &#124; date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   &#124; time } ]`

`[ MINDATE   = { 10000101   &#124; date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NACTION = NONE &#124; DELETE &#124; ARCHIVE } ]`

`[ NARCHIVENAME = string ]`

`[ NCOMP   = { 0 &#124; 15   } ]`

`[ NETBAND = { 1...16} ]`

`[ NOTIFY   = string ]`

`[ NPAD = { ' ' &#124; character } ]`

`[ NCODE   = { ‘   ‘ &#124;ASCII &#124; BINARY &#124; EBCDIC } ] *available only when the protocol is SFTP`

`[ OPERMSG   = { 0   &#124; 255 } ]`

`[ PRI   = { 128 &#124; n } ]`

`[ RAPPL   = string ]`

`[ RKERROR   = { ' ' &#124; DELETE &#124; KEEP } ]`

`[ RPASSWD = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SERIAL = { ' ' &#124; Y &#124; Z &#124; X } ]`

`[ SOURCEAPPL =  string ]`

`[ SPASSWD = string ]`

`[ STATE   = { DISP &#124; HOLD &#124; KEEP } ]`

`[ STORAGEACCOUNT = string ]`

`[ SUSER   = string ]`

`[ TARGETAPPL = string ]`

`[ TRK   =  { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

`[ USERID   = { CFT server"userid" &#124; string } ]`

`[ WFNAME   = filename ]`

`[ WORKINGDIR = string ]`

`[ XLATE   = identifier ]`

**** ****[CFTRECV details](../web_copilot_ui/flow_def_intro/cftrecv)

<span id="CFTSEND"></span>

#### CFTSEND ID= identifier: Description of model file sending parameters

Syntax

`[ ACKEXEC = filename]`

`[ ARCHIVEFNAME = string ]`

`[ COMMENT   = string ]`

`[ CYCDATE   = { 0   &#124; date } ]`

`[ CYCLE   = { 0   &#124; n } ]`

`[ CYCTIME   = { 0   &#124; time } ]`

`[ DELETE   = { NO   &#124; YES } ]`

`[ DUPLICATE = { string 512 } ]`

`[ EXEC   = filename ]`

`[ EXECSUB   = { LIST   &#124; FILE &#124; SUBF } ]`

`[ EXECSUBA = {LIST &#124; FILE &#124; SUBF } ]`

`[ EXECSUBPRE = { LIST   &#124; FILE &#124; SUBF } ]`

`[ EXIT   =  identifier   ]`

`[ FACC   = { ‘   ‘ &#124; character } ]`

`[ FACTION   = { NONE   &#124; DELETE &#124; ERASE &#124; ARCHIVE } ]`

`[ FBLKSIZE   = { 0   &#124; n } ]`

`[ FCODE   = { ‘   ‘ &#124;ASCII &#124; BINARY &#124; EBCDIC } ]`

`[ FDB   = filename ]`

`[ FDELETE = " " &#124;* &#124; C &#124;D &#124; K &#124; H &#124; T &#124; X]`

`[ FDISP   = { SHR   &#124; OLD &#124; CHECK } ]`

`[ FILENOTFOUND =  { ABORT &#124; IGNORE } ]`

`[ FILTER = string ]`

`[ FILTERTYPE = string ]`

`[ FKEYLEN   = { 0   &#124; n } ]`

`[ FKEYPOS   = { 0   &#124; n } ]`

`[ FLOWNAME = string ]`

`[ FLRECL   = { 0   &#124; n } ]`

`[ FNAME    = { filename   &#124; mask &#124; dirname &#124; #filename &#124; #mask &#124; #dirname } ]`

`[ FORCE   = { NO   &#124; YES } ]`

`[ FORG   = { SEQ   &#124; DIRECT &#124; INDEXED &#124; PART } ]`

`[ FPAD = { ' ' &#124; character } ]`

`[ FRECFM   = { ‘   ‘ &#124; F &#124; U &#124; V } ]`

`[ FSPACE   = { 0   &#124; n } ]`

`[ FTYPE   = { ‘   ‘ &#124; character } ]`

`[ GROUPID   = string ]`

`[ IDA = string ]`

`[ IMPL   = { NO   &#124; YES } ]`

`[ MAXDATE   =  { 99991231   &#124; date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   &#124; time } ]`

`[ MINDATE   = { 10000101&#124;   date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NBLKSIZE   = { 0   &#124; n } ]`

`[ NCODE   = { ‘   ‘ &#124;ASCII &#124; BINARY &#124; EBCDIC } ]`

`[ NCOMP   =  { 0 &#124;   15   } ]`

`[ NETBAND   = { 1...16} ]`

`[ NFNAME   =  { filename   &#124; *filename } ]`

`[ NKEYLEN   = { 0   &#124; n } ]`

`[ NKEYPOS   = { 0&#124;   n } ]`

`[ NLRECL   =  { 0   &#124; n } ]`

`[ NOTIFY   = string  ]`

`[ NPAD = { ' ' &#124; character } ]`

`[ NRECFM   = { ‘   ‘ &#124; F &#124; U &#124; V } ]`

`[ NSPACE   =  { 0   &#124; n } ]`

`[ NTYPE   = character ]`

`[ OPERMSG   = { 0   &#124; 255 } ]`

`[ PARM   = string  ]`

`[ PREEXEC = filename ]`

`[ PRI   = { 128   &#124; n } ]`

`[ RAPPL   = string ]`

`[ RPASSWD = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SELFNAME   = filename ]`

`[ SERIAL = { ' ' &#124; Y &#124; Z &#124; X } ]`

`[ SPART   = string ]`

`[ SPASSWD = string ]`

`[ SOURCEAPPL =  string ]`

`[ STATE   = { DISP   &#124; HOLD &#124; KEEP } ]`

`[ STORAGEACCOUNT = string ]`

`[ SUSER   = string ]`

`[ TARGETAPPL = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH } ]`

`[ TRK   =  { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

`[ USERID   = { CFT   server "userid" &#124; string } ]`

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

`DIRECT   = { CLIENT   &#124; SERVER }`

`[ CACHE   = { NO   &#124; YES } ]`

`[ CERFNAME   = string ]`

`[ COMMENT   = string ]`

`[ DEPTH   = { 10   &#124; n } ]`

`[ DNISSUER   = ( string, string, …) ]`

`[ DNUSER   = { ( string, string, …) &#124; ( string, OP (see Note), string ) } ] `

`[ INTERCID   = string ] `

`[ KEYEXT = { VERIFY &#124; NONE } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ PARM   = string ]`

`[ PASSW = string ] `

`[ ROOTCID   = ( string, string, …) ]`

`[ TRACE   = { 0   &#124; n } ]`

`[ USERCID   = string ]`

`[ VERIFY   = { NONE&#124; REQUIRED   &#124; OPTIONAL } ] *When DIRECT=SERVER`

`[ VERIFY   = { NONE&#124; REQUIRED   &#124; ENFORCED &#124; OPTIONAL } ] *When DIRECT=CLIENT `

`[ VERSION   = { TLSV1  &#124; SSLV3 &#124; TLSV1 &#124; SSLV3COMP &#124; TLSV1COMP} ]`

****Note****: <span id="OP"></span>You can configure Transfer
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

`[ CLASS   = { 1   &#124; n } ]`

`[ CNXIN   = { 2   &#124; n } ]`

`[ CNXINOUT   = { 4   &#124; n } ]`

`[ CNXOUT   = { 2   &#124; n } ]`

`[ IMAXTIME   = { 23595999   &#124; time } ]`

`[ IMINTIME   = { 0   &#124; time } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ OMAXTIME   = { 23595999   &#124; time } ]`

`[ OMINTIME   = { 0   &#124; time } ]`

`[ RETRYM   = { 12   &#124; n } ]`

`[ RETRYN   = { 4   &#124; n } ]`

`[ RETRYW   = { 1   &#124; n } ]`

`[ VERIFY   = { 0   &#124; n } ]`

[TCP
attributes for a partner](../../admin_intro/admin_config_commands/network_resource_concepts)

<span id="CFTUIPREF"></span>

#### CFTUIPREF MODE=mode

MODE= { CREATE &#124; <span class="underline">REPLACE</span> &#124; DELETE }

[ID](parameter_intro/id)
= identifier  

[TYPE](parameter_intro/type) = string

[ [COMMENT](parameter_intro/comment)
= string ]

`[ CONTENT   = { ACTIVE   &#124; FULL } ]`

[ [ORIGIN](parameter_intro/origin) ] = string ]

<span id="CFTXLATE"></span>

#### CFTXLATE FNAME = filename: Define conversion tables

Syntax

`ID   = identifier  `

`[ COMMENT   = string ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FCODE   = { '   ' &#124; ASCII &#124; EBCDIC } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NCODE   = { '   ' &#124; ASCII &#124; EBCDIC } ]`

`[ ORIGIN ] = string ]`

`[ TABLE ] = string ]`

[Conversion
tables](../../concepts/transfer_command_overview/using_transcoding/translation_table_concepts)

<span id="CLEARCMD"></span>

#### CLEARCMD COMMAND = string: Delete a transfer request

Syntax

`USERID   = string`

`INDEX   = number`

[CLEARCMD details](../about_cftutil/managing_transfer_states/clearcmd_command)

<span id="CONFIG"></span>

#### CONFIG: Designate the communication medium and the files accessed by CFTUTIL

Syntax

CONFIG TYPE = { CAT &#124; INPUT &#124; OUTPUT &#124; PARM &#124; PART }

`FNAME = filename `

` `

CONFIG TYPE = COM

`MEDIACOM = FILE`

`FNAME = filename `

 

`CONFIG   TYPE = COM`

`MEDIACOM   = TCPIP`

`FNAME= string `

`[ HIGHPORT   =  { 65535   &#124; n } ]`

`[ LOWPORT   = { 5000   &#124; n } ]`

`[ PASSWORD = string } ]`

`[ PROXY   = string ]`

`[ ROOTCERT   = string ]`

`[ TIMEOUT   = 60   &#124; n ]`

[Setting default CFTUTIL file names](../about_cftutil/redefining_cftutil_data_media)

<span id="COPYFILE"></span>

#### COPYFILE IFNAME = filename: Copy files with an off-line compression or decompression option

Syntax

`OFNAME   = filename `

`[ CREATE   = { ‘   ‘ &#124; YES &#124; NO } ]`

`[ IBLKSIZE   = { 0   &#124; n } ]`

`[ ICHARSET = string ]`

`[ ICODE   = { ASCII &#124; EBCDIC } ]`

`[ ICOMP   = { 0   &#124; 15 } ]`

`[ ICT   =  { H   &#124; C } ]`

`[ ILRECL   = { 0   &#124; n } ]`

`[ IRECFM   = { F &#124; V &#124; U } ]`

`[ ITYPE   = { ‘ ‘ &#124; character } ]`

`[ OBLKSIZE   = { 0 &#124;n  }   ]`

`[ OCHARSET = string ] `

`[ OCODE   = { ASCII &#124; EBCDIC } ]`

`[ OCOMP   = { 0   &#124; 15 } ]`

`[ OCT   = { H &#124; C } ]`

`[ OLRECL   = { 0   &#124;n } ]`

`[ ORECFM   = { IRECFM   value &#124; F &#124; V&#124; U } ]`

`[ OSPACE   = { 0   &#124; n } ]`

`[ OTYPE   = { ‘   ‘ &#124; character } ]`

`[ XLATE = string ]`

[Copying files off-line](../../admin_intro/admin_commands_intro/copyfile_command)

<span id="DELETE"></span>

#### DELETE PART = { identifier &#124; mask }: Delete a catalog entry

Syntax

`[ BLKNUM   = { 0   &#124; n } ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FORCE   =  { YES   &#124; NO   } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   &#124; transid } ]`

`[ IDTU   = string ]`

`[ STATE   = { *   &#124; C &#124; D &#124; H &#124; K &#124; T &#124; X } ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Deleting catalog entries](../../admin_intro/admin_commands_intro/delete_command)

<span id="DISPLAY"></span>

#### DISPLAY [ CONTENT = { listcat &#124; identifier }]: Display a model-formatted catalog

Syntax

`[ DATETIMEMAX = { 0 &#124; 991231235959 } ]`

`[ DATETIMEMIN = { 0 &#124; 991231235959 } ]`

`[ DIAGI = { * &#124; 0 &#124; ERROR } ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ EMPTY   = { ANY   &#124; string } ]`

`[ FILE   = filename ]`

`[ FMODEL   =  string   ]`

`[ FOUT = string ]`

`[ HELP   = { NONE   &#124; FIELDS &#124; MODELS &#124; COMMAND } ]`

`[ IDA   =  string   ]`

`[ IDF   = string ]`

`[ IDT   = string ]`

`[ IDTU   = string ]`

`[ MODE   = { ANY   &#124; COLUMN &#124; LINE } ]`

`[ NA   = { ANY   &#124; string } ]`

`[ NIDT    = { string of 8 digits } ]`

`[ NPART   = string ]`

`[ PART   = string ]`

`[ RUSER   = string ]`

`[ SORTBY   = string ]`

`[ SUSER   = string ]`

`[ STATE   = { *   &#124; character } ]`

`[ TYPE   = { *   &#124; FILE &#124; MESSAGE &#124; REPLY &#124; ALL } ]`

[Catalog output display model](../about_cftutil/monitoring_cftutil_intro/display_command)

<span id="END"></span>

#### END PART = { identifier &#124; mask }: Change transfer to X state

Syntax

`[ APPCYCID    = string ]`

`[ APPOBJID    = string ]`

`[ APPSTATE    = string ]`

`[ SAPPL    = string ]`

`[ RUSER    = string ]`

`[ SUSER    = string ]`

`[ RPASSWD    = string ]`

`[ SPASSWD    = string ]`

`[ BLKNUM   = { 0   &#124; n } ]`

`[ DIAGC = string ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FORCE   = { NO   &#124; YES } ]`

`[ FNAME    = string ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   &#124; transid } ] `

`[ IDTU   = string ]`

`[ ISTATE = { YES &#124; NO } ]`

`[ ISTATE    = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

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

#### HALT PART = { identifier &#124; mask }: Suspend a transfer

Syntax

`[ BLKNUM   = { 0   &#124; n } ]`

`[ DIAGC = string ]`

`[ DIAGP = string ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FORCE   = { YES &#124; NO   } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier  ]`

`[ IDT   = { *   &#124; transid } ]`

`[ IDTU   = string  ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Halting a transfer](../about_cftutil/managing_transfer_states/halt_command)

#### HELP

Syntax

`[ CMD = { string } ]`

`[ CONTENT = { BRIEF &#124; DETAIL} ]`

`[ OFORMAT = { txt &#124; xsd } ]`

<span id="INACT"></span>

#### INACT ID = identifier: Inactivate a partner

Syntax

`[ MODE   = { BOTH   &#124; REQUESTER&#124; SERVER } ]`

`[ FORCE   = { NO   &#124; YES } ]`

`[ TYPE   = PART   &#124; TRK &#124; CRON &#124; FOLDER ]`

[INACT details](../about_cftutil/reactivate_an_object_cl/inact_command)

<span id="KEEP"></span>

#### KEEP PART = { identifier &#124; mask }: Abort a transfer

Syntax

`[ BLKNUM   = 0   &#124; n ]`

`[ DIAGC = string ]`

`[ DIAGP = string ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FORCE   = { YES   &#124; NO } ]`

`[ IDA   = identifier  ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   &#124; transid } ]`

`[ IDTU   = string  ]`

`[ STATE   = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

**** ****[Suspend transfers](../about_cftutil/managing_transfer_states/keep_command)

<span id="KSTATE"></span>

#### KSTATE IDF = identifier: Change a transfer state

Syntax

`IDTU   = local transfer counter identifier`

`PART   = partner identifier`

[KSTATE details](../about_cftutil/managing_transfer_states/kstate_command)

<span id="LISTCAT"></span>

#### LISTCAT TYPE = { ALL &#124; \* &#124; FILE &#124; MESSAGE &#124; REPLY }: List catalog entries

Syntax

`[ CONTENT   = { BRIEF   &#124; FULL &#124; DEBUG &#124; EXTEND &#124; COMMUT &#124; STAT &#124; BLKNUM } ]`

`[ DATETIMEMAX = { 0 &#124; 991231235959 } ]`

`[ DATETIMEMIN = { 0 &#124; 991231235959 } ]`

`[ DIAGI = { * &#124; 0 &#124; ERROR } ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FILE   = filename ]`

`[ IDA   = { *   &#124; identifier } ]`

`[ IDF   = { *   &#124; identifier } ]`

`[ IDT   = { *   &#124; transid } ]`

`[ IDTU   = string  ]`

`[ NIDT    = { string of 8 digits } ]`

`[ NPART   = { identifier &#124; mask } ]`

`[ PART   = { *   &#124; identifier &#124; mask } ]`

`[ SORTBY   = string ]`

`[ STATE   = { *   &#124; string } ]`

**** ****[LISTCAT details](../about_cftutil/monitoring_cftutil_intro/listcat_command)

<span id="LISTCOM"></span>

#### LISTCOM: Display contents of communication media

Syntax

`[ CONTENT   = { ACTIVE   &#124; FULL &#124; STAT } ]`

`[ FILE   = filename ]`

`[ FIRST   = { 0   &#124; n } ]`

`[ LAST   = { 0   &#124; max. number of records } ]`

`[ VERIFY   = { NO   &#124; YES } ]`

**** ****[LISTCOM details](../about_cftutil/monitoring_cftutil_intro/listcom_command)**** ****

<span id="LISTLOG"></span>

#### LISTLOG: Display and filter log content including merged nodes in cluster mode

Syntax

`[ LOGLEVEL = { F &#124; E &#124; W &#124; I } ]`

`[ LINES = { -10000 &#124; -20 &#124; 10000 } ]`

`[ DATEMAX =  { 0 &#124; 991231 } ]`

`[ DATEMIN           = { 0 &#124; 991231 } ]`

`[ DATETIMEMAX = { 0 &#124; 99123123595999 } ]`

`[ DATETIMEMIN { 0 &#124; 99123123595999 } ]`

`[ TIMEMIN = { 0&#124; 23595999 } ]`

`[ TIMEMAX           = { 0 &#124; 23595999 } ]`

`[ PATTERN           =     string ]`

`[ DISPLAYNODEID     = { YES &#124; NO } ]`

`[ NODE = string ]`

#### LISTNODE: Display all nodes

Syntax

No parameters

[Multi-node commands](../../about_multinode/multi_node_commands)

<span id="LISTPARM"></span>

#### LISTPARM: Display {{< TransferCFT/axwayvariablesComponentShortName  >}} partner details

Syntax

`[ ID   = { *   &#124; identifier } ]`

`[ CONTENT   = { FULL   &#124; BRIEF } ]`

`[ PART   = identifier  ]`

`[ TYPE   = { ACCNT &#124; ALL   &#124; APPL &#124; AUTH &#124; CAT &#124; COM &#124; CRON &#124; EXIT &#124; IDF &#124; LOG &#124; NET &#124; PARM &#124; PROT   &#124; RECV &#124; SEND &#124; SSL &#124; XLATE } ]`

[LISTPARM details](../about_cftutil/configuring_cft_start_here/listparm)

<span id="LISTPART"></span>

#### LISTPART: Display partners

Syntax

`[ ID   = { *   &#124;identifier } ]`

`[ CONTENT   = { FULL   &#124; BRIEF } ]`

`[ TYPE   = { ALL   &#124; DEST &#124;  PART &#124;  TCP &#124; } ]`

[LISTPART details](../about_cftutil/configuring_cft_start_here/listpart_command)

<span id="MQUERY"></span>

#### MQUERY : Query one or more {{< TransferCFT/axwayvariablesComponentShortName  >}} components

Syntax

OBJECT = CACHE

`[ OBJECT   = { CACHE   &#124; SYSTEM &#124; STATS &#124; PROBE } ]`

`[ CONTENT   = { BRIEF   &#124; FULL &#124; STAT } ]`

`[ NAME   = { CAT   &#124; COMMAND &#124; CRON &#124; DMZ &#124; STAT } ]`

` `

OBJECT = SYSTEM

`[ OBJECT   = { CACHE   &#124; SYSTEM &#124; STATS &#124; PROBE } ]`

`[ CONTENT   = { BRIEF   &#124; FULL &#124; STAT } ]`

`[ NAME   = { CFTMAIN &#124; CFTTRK &#124; CFTTFIL &#124; CFTCOM &#124; CFTTPRO &#124; CFTEXIT &#124; CFTPRX &#124; CFTDSCAN } ]`

` `

OBJECT = STATS or PROBE

`[ OBJECT   = { CACHE   &#124; SYSTEM &#124; STATS &#124; PROBE } ]`

`[ CONTENT   = { XMLBRIEF   &#124; XMLFULL &#124; RAW } ]`

`[ NAME   = { CAT   &#124; COMMAND &#124; CRON &#124; DMZ &#124; STAT } ]`

**** ****[MQUERY details](../../admin_intro/admin_commands_intro/querying_a_component_)

<span id="PURGE"></span>

#### PURGE: Delete records from the catalog 

Syntax

`[ TIMEP   = { 23595999   &#124; time } ]`

[PURGE details](../../admin_intro/admin_commands_intro/purge_catalog)

<span id="RECONFIG"></span>

#### RECONFIG: Reload

Syntax

`[ TYPE   = { CRON &#124; UCONF &#124; CAT &#124;  FOLDER &#124; PARMCACHE &#124; AM  } ] `

 [Manage configuration updates](../../admin_intro/admin_commands_intro/reconfig)

<span id="RECV"></span>

#### RECV IDF = { identifier &#124; mask }: Request to receive transfer

Syntax

`PART   = identifier   `

`[ ACKEXEC = filename]`

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ COMMENT   = string  ]`

`[ CYCDATE   =  date    ]`

`[ CYCLE   =  { 0   &#124; n } ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR &#124; RESUME } ]`

`[ DIRNB   = n ]`

`[ EXEC   = filename ]`

`[ EXECRALL = { all &#124; parent&#124; children} ]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘   ‘ &#124; character }  ]`

`[ FACTION   = { ‘   ‘ &#124; DELETE &#124; ERASE &#124; RENAME &#124; VERIFY } ]`

`[ FBLKSIZE   = n ]`

`[ FCODE   = { BINARY &#124; EBCDIC &#124; ASCII } ]`

`[ FCOMP   = { 0   &#124; 15 } ]`

`[ FDATE   = { 0   &#124; date } ]`

`[ FDB   = filename ]`

`[ FDISP   = { BOTH   &#124; NEW &#124; OLD }  ]`

`[ FILE   = { FIRST   &#124; ALL }  ]`

`[ FKEYLEN   = { 0   &#124; n }  ]`

`[ FKEYPOS   = { 0   &#124; n }   ]`

`[ FLRECL   = n ]`

`[ FNAME   = filename ]`

`[ FORG   = { SEQ   &#124; DIRECT &#124; INDEXED &#124; PART } ]`

`[ FPAD = { ' ' &#124; character } ]`

`[ FRECFM   = { ‘   ‘ &#124; F &#124; V &#124; U }  ]`

`[ FSPACE   = n ]`

`[ FTIME   = { 0 &#124; time } ]`

`[ FTYPE   =  { ‘   ‘ &#124; character } ]`

`[ IDA   = identifier ]`

`[ MACTION   = { '   ' &#124; REPLACE }  ]`

`[ MAXDATE   =  { 99991231   &#124; date } ]`

`[ MAXDURATION =  {0...32767} ]`

`[ MAXTIME   = { 23595999   &#124; time }  ]`

`[ MINDATE   = { current system date &#124; date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ NACTION = NONE &#124; DELETE &#124; ARCHIVE } ]`

`[ NARCHIVENAME = string ]`

`[ NCOMP   = { 0   &#124; 15 } ]`

`[ NETBAND = { 1...16} ]`

`[ NFNAME   = filename ]`

`[ NFVER   =  { 0   &#124; 255 } ]`

`[ NIDF   = string ]`

`[ NPAD = { ' ' &#124; character } ]`

`[ PARM   = string ]`

`[ PRI   = { 128   &#124; n } ]`

`[ PROT = identifier ] `

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SERIAL = { ' ' &#124; Y &#124; Z &#124; X } ]`

`[ STATE   = { DISP   &#124; HOLD &#124; KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH }  ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

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

`[ DIRECT   = { SEND &#124;RECV &#124; BOTH   } ]`

`[ BLKNUM   = { 0   &#124; n } ]`

`[ FORCE   = { NO   &#124; YES } ]`

`[ IDA   = string ]`

`[ IDF   = string ]`

`[ IDT   = string ]`

`[ IDTU   = string ] `

`[ STATE  = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[RESUME details](../about_cftutil/managing_transfer_states/resume_command)

` `

<span id="SEND"></span>

#### SEND: Request to send transfer

{{% TransferCFT/snippets/SEND_command%}}

[Sending files](../../concepts/send_command/send_command_basics)

<span id="SHUT"></span>

#### SHUT: Shut down {{< TransferCFT/axwayvariablesComponentShortName  >}} 

Syntax

`[ FAST   = { NO   &#124; YES &#124; KILL } ]`

`[ RESTART = { YES &#124; NO } ] `

[Manage the {{< TransferCFT/axwayvariablesComponentLongName  >}} server: stop the server](../../admin_intro/start_stop_cft#Stop__server)

<span id="START"></span>

#### START: Restart transfer

Syntax

`PART   = { identifier &#124; mask }   `

`[ BLKNUM   = { 0   &#124; n } ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ FORCE   = YES &#124; NO   ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { *   &#124; transid } ]`

`[ IDTU   = string ]`

`[ MAXDURATION =  {0...32767} ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

`[ PHASE = string ]`

`[ PHASESTEP = string ]`

`[ SCOPE = string ]`

[Restart transfers](../about_cftutil/managing_transfer_states/start_command)

<span id="SUBMIT"></span>

#### SUBMIT: Submit end-of-transfer procedure

Syntax

`PART   = { identifier &#124; mask }  `

`[ APPSTATE =  string ]`

`[ BLKNUM   = { 0   &#124; n } ]`

`[ DIRECT   = { BOTH   &#124; RECV &#124; SEND } ]`

`[ EXEC   = filename ]`

`[ IDA   = identifier ]`

`[ IDF   = identifier ]`

`[ IDT   = { * &#124; transid } ]`

`[ IDTU   = string ]`

`[ STATE  = string ]`

`[ KDATE   = { 0   &#124; n } ]`

`[ KTIME   = { 0   &#124; n } ]`

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

[SWAITCAT examples](../../app_integration_intro/synch_comm_tcpip_intro/sync_transfer_request_tasks)

<span id="SWITCH"></span>

#### SWITCH: Manually switch LOG or ACCNT files 

Syntax

`[ TYPE   = { LOG &#124; ACCNT } ]`

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

`[ CONTENT = BRIEF &#124; FULL ]`

`[ FOUT = string ]`

`[ SCOPE   = { INSTANCE &#124; SET &#124; * &#124; ALL } ]  `

`[ VALUE = string ]`

[](../../admin_intro/uconf/uconf_commands)

UCONF details

<span id="WAIT"></span>

#### WAIT: Suspend CFTUTIL execution for the time indicated 

Syntax

`[ DURING   = { 0   &#124; n } ]    *Time   in seconds`

`[ TIME   = { 0   &#124; 23595999 } ] `

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

- [   ]
    Optional parameters or values are displayed between square brackets
- {   }
    Parameter choices or values are displayed  between
    braces
- &#124; The vertical
    bar separates an enumerated list of parameter values
- ___ Default values
    for a parameter are underlined
- , A comma separates
    a list of parameter values

For more information, see [TYPOGRAPHICAL CONVENTIONS.](typographical_conventions)

### Symbolic variables

The following symbolic variable syntaxes are valid in a {{< TransferCFT/axwayvariablesComponentShortName  >}}
environment:

- &VAR
- &+VAR
- &nVAR:
- &p.VAR
- &p.nVAR
- &(-string_prefix)
    (+string_suffix)
    (=string_alternate)p.nVAR

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

- \#filename

For each file name that is listed in the file, this
sends the corresponding file.

- \#mask

For each file name that matches the mask, this sends
the corresponding file.

- \#dirname

For each file that exists in the directory, this sends
the corresponding file.

Note: Use the @ symbol in a UNIX environment in place
of the \# symbol.

For more information on filename conventions, refer
to [Filename conventions](filename_conventions).

<span id="About_UCONF"></span>

Using UCONF
-----------

In order to merge functioning for all platforms, {{< TransferCFT/axwayvariablesComponentShortName  >}} features a configuration interface that provides product uniformity
regardless of platform differences.

The basic CFTUTIL services provided are:

- UCONFSET id=\*\*\*\*,value=\*\*\*\*
- UCONFGET id=\*\*\*\*
- LISTUCONF id=\*\*\*\*

This configuration replaces all prior configuring methods, such as manually
editing cft.ini, ENV_CFT, \*.ini, trkapi.conf, and so on, which have been
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

For example: $(CFT_INSTALL) -à getenv(“CFT_INSTALL”)

Variables containing a period “.” refer to sections in the configuration
file. Do not modify this file.

For example: cft.working_dir = $(cft.runtime_dir)

The[UCONF](../../admin_intro/uconf) table lists the new UCONF
identifiers, the default values, and the former file values.
