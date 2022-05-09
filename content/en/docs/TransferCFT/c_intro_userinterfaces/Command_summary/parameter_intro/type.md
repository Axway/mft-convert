{
    "title": "type",
    "linkTitle": "type",
    "weight": "3660"
}<span id="type"></span>

### type

#### ABOUT

[ TYPE
= { ALL
&#124; HOST &#124; CFT } ]

Displays
the Transfer CFT product, host, and key information.

<span id="ACT__INACT"></span>

#### ACT

[TYPE = PART &#124; TRK &#124; CRON &#124; FOLDER 
]

Type of object to be deactivated.

- PART
    = Partner
- TRK
    = Select to restart Sentinel notification
- CRON
    = Enables the cronjob CRON=ID

#### INACT

[TYPE = PART &#124; TRK &#124; CRON &#124; FOLDER
]

Type of object to be deactivated.

- PART
    = Partner
- TRK
    = Select to stop dynamic Sentinel notification. All further
    messages are lost.
- CRON
    = Disables the cronjob CRON=ID

<span id="type_CFTCOM"></span>

#### CFTCOM

TYPE = { FILE
&#124; TCPIP}

Type of {{< TransferCFT/axwayvariablesComponentShortName  >}} communication medium.

This parameter can take the following values:

- FILE:
    communication is accomplished through a file
- TCPIP:
    communication is performed through the synchronous communication medium

<span id="type_CFTNET"></span>

#### CFTNET

[TYPE = { TCP } ] 

Defines the type of network resource. This parameter can take the following
values, according to the system:

- TCP: TCP/IP
    network access resource

#### CFTIDF

TYPE = { RECV &#124; SEND}

The transfer direction for which this correspondence is valid. Select
either:

- SEND:
    for send transfers
- RECV:
    for receive transfer

<span id="type_CFTEXIT"></span>

#### CFTEXIT

[TYPE = {FILE
&#124; ACCESS &#124; EXEC &#124; BOT }] 

The type of exit program, as follows:

- FILE (default value): Data is recorded
    in the monitor files
- ACCESS: To use a directory type EXIT
- EXEC: To use an end-of-transfer type EXIT
- BOT: To use a beginning-of-transfer type EXIT

<span id="type_CFTACCNT"></span>

#### CFTACCNT

[TYPE = {FILE
&#124; SYST }] 

This defines the accounting type. CFTACCNT TYPE parameters are:

- FILE (default value): Data is recorded
    in the {{< TransferCFT/axwayvariablesComponentLongName  >}} files defined in the fname
    and afname fields.
- SYST: Data is recorded in the files
    of the operating system accounting utility. Available
    only on z/OS (MVS).

<span id="type_CFTPROT"></span>

#### CFTPROT

[TYPE = {PeSIT &#124; ODETTE  }]

Type
of transfer protocol.

- PeSIT:
    PeSIT protocol
- ODETTE:
    OFTP (ODETTE) protocol
- SFTP: SFTP protocol

#### LISTPART

TYPE ={ALL &#124; DEST &#124; PART &#124; TCP}

<span id="Type_table1"></span>

#### Type table


| Value  | Meaning  |
| --- | --- |
| ALL  | Used to query the general and network characteristics of partners<br /> Parameters of the PARTNER file |
| DEST  | Used to query the parameters configured in the CFTDEST command: concern the broadcasting lists  |
| PART  | Used to query the parameters configured in the CFTPART command: description of the general data relative to partners  |
| TCP | Used to query the parameters configured in the CFTTCP command: TCP/IP network parameters associated with each partner supporting TCP/IP  |


<span id="type_CONFIG"></span>

#### CONFIG

TYPE = {CAT &#124; COM &#124; INPUT &#124; OUTPUT
&#124; PARM &#124; PART}

Defines the medium concerned.


| Value  | Medium concerned  |
| --- | --- |
| CAT  | Catalog file  |
| COM  | Communication medium  |
| INPUT  | Command input file  |
| OUTPUT  | Report output file  |
| PARM  | Parameter file  |
| PART  | Partner file  |


<span id="type_SWITCH"></span>

#### SWITCH

[TYPE = {LOG &#124; ACCNT}]

Defines the switch action for CFTLOG or CFTACCNT. File types are:

- LOG:
    The SWITCH command stops message writing on the current log file, switches
    to the alternate log file, and then executes the procedure specified in
    the EXEC parameter of the CFTLOG
    object.

<!-- -->

- ACCNT:
    The SWITCH command stops statistics from being written on the current
    statistical file, switches the writing to the alternate statistical file,
    and then executes the procedure specified in the EXEC
    parameter of the [CFTACCNT](../../../web_copilot_ui/conf_intro/cftaccnt) object.

#### CFTEXT

[TYPE = {[see Type
table below](#Type_table) }]

Defines the parameters to extract.

<span id="Type_table"></span>

#### Type table


| Value  | Meaning  | Command |
| --- | --- | --- |
| ALL  | All the parameter types of the CFTPARM and CFTPART files  |   |
| ACCNT  | Description of the statistical files  | CFTACCNT  |
| AUTH  | List of authorized files  | CFTAUTH  |
| CAT  | Catalog definition  | CFTCAT  |
| COM  | Description of {{< TransferCFT/axwayvariablesComponentShortName  >}} communication methods  | CFTCOM  |
| IDF  | File &quot;network&quot; identifier  | CFTIDF  |
| LOG  | Log file description  | CFTLOG  |
| NET  | Network description  | CFTNET  |
| PARM  | General parameters  | CFTPARM  |
| PART  | Partner definition  | CFTPART and {{< TransferCFT/axwayvariablesComponentShortName  >}} network  |
| PROT  | Protocol definition  | CFTPROT  |
| RECV  | Description of the files to be received  | CFTRECV  |
| SEND  | Description of the files to be sent  | CFTSEND  |
| XLATE  | Translation table definition  | CFTXLATE  |
| TCP  | TCP/IP partner definition  | CFTTCP  |
| UCONF  | Unified configuration  | CFTEXT  |


<span id="type_LISTPARM"></span>

#### LISTPARM

TYPE = {ACCNT &#124;
ALL &#124; AUTH &#124; CAT &#124; COM &#124; ETB &#124; IDF &#124; LOG &#124; NET &#124;PARM &#124; PROT &#124; RECV &#124; SEND
&#124; XLATE }

Defines the type of parameters to
list from the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter file.

TYPE can take the predefined values indicated in the Type table below.

#### Type table


| Value  | Definition  |
| --- | --- |
| ACCNT  | Used to query statistical file parameters<br /> These parameters are submitted when CFTACCNT commands are entered  |
| ALL  | Used to query all the parameters indicated in the PARAMETER file  |
| AUTH  | Used to query file authorization lists<br /> These lists are customized by the CFTAUTH commands  |
| CAT  | Used to query catalog parameters<br /> These parameters are submitted when CFTCAT commands are entered  |
| COM  | Used to query communication media parameters<br /> These parameters are submitted when CFTCOM commands are entered  |
| IDF  | Used to query file &quot;network&quot; identifiers<br /> Identifiers are customized by the CFTIDF commands  |
| LOG  | Used to query log file parameters<br /> These parameters are submitted when CFTLOG commands are entered  |
| NET  | Used to query network characteristic parameters<br /> These parameters are submitted when CFTNET commands are entered and differ according to the type of network configured  |
| PARM  | Used to query general parameters<br /> These parameters are submitted when CFTPARM commands are entered  |
| PROT  | Used to query protocol parameters<br /> These parameters are submitted when CFTPROT commands are entered and differ according to the protocol configured  |
| RECV  | Used to query the parameters of the files to be received<br /> These parameters are submitted when CFTRECV commands are entered  |
| SEND  | Used to query the parameters of the files to be sent<br /> These parameters are submitted when CFTSEND commands are entered  |
| XLATE  | Used to query translation tables<br /> Translation tables are customized by the CFTXLATE object |
| CFTFILE | Used to create, empty, or delete {{< TransferCFT/axwayvariablesComponentShortName  >}} files |
| LISTCAT | Used to query the information associated with the selected transfers, recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog |
| DISPLAY | Used to query the information as with the LISTCAT command. It uses an external XML file that lists and describes customized models. These models are used to format the output |
| ABOUT | Used to display the {{< TransferCFT/axwayvariablesComponentShortName  >}} computer characteristics |


#### RECONFIG

TYPE = { CRON &#124; UCONF &#124; CAT &#124; FOLDER &#124; PARMCACHE &#124; AM}

- CAT: Resize the catalog while the Transfer CFT is running (hot catalog resizing)
- CRON: Reload the CFTCRON objects
- FOLDER: Reload the CFTFOLDER objects
- UCONF: Reload the dynamic UCONF parameters
- PARMCACHE: Clear the parameter cache
- AM: Reload roles and privileges

#### CFTUIPREF

TYPE = string

Enter a name for the object.

 

[Return to Command index](../../)
