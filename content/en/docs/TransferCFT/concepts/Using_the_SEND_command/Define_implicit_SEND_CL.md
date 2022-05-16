---

    title: Defining an implicit SEND 
    linkTitle: Defining an implicit SEND
    weight: 210

---
Files can be made available for a transfer request either implicitly
or explicitly. The explicit mode provides more security because the server
must have authorization to make a file available. In the implicit mode,
IMPL parameter is set to yes, which enables a file to be available for
transfer without a specific request for that file.

QQQ\_QQQ\_QQQ

SEND command parameter:


| Command description | Use this command to make a file available for transfer without it being explicitly requested.  |
| --- | --- |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/impl">IMPL</a> parameter | The implicit send makes a file available. Select from:<br/> • YES<br/> • NO<br/> This parameter must be set to NO for the default model file description. |
| Other parameters | For other SEND parameters refer to <a href="../send_command_basics">Sending files</a>.  |


For SEND command details refer to the following syntax:

<span class="MCDropDownHead dropDownHead">![Closed](/Images/TransferCFT/transparent.gif)Syntax</span>

[TYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/type)
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
