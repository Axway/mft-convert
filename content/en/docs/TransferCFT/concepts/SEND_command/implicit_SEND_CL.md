---
title: "Defining an implicit SEND "
linkTitle: "Defining an implicit SEND"
weight: 200
---Files can be made available for a transfer request either implicitly
or explicitly. The explicit mode provides more security because the server
must have authorization to make a file available. In the implicit mode,
IMPL parameter is set to yes, which enables a file to be available for
transfer without a specific request for that file.

Description:

Use this command to make a file available for transfer
without it being explicitly requested.


| Parameter  | Description  |
| --- | --- |
| [IMPL](../../../c_intro_userinterfaces/command_summary/parameter_intro/impl)  | The implicit send makes a file available. Select from:<br/> • YES<br/> • NO<br/> This parameter must be set to NO for the default model file description. |
| Other parameters | For other SEND parameters refer to [Sending files](../send_command_basics).  |


For SEND command details refer to the following syntax:

![Closed](/Images/TransferCFT/transparent.gif)Syntax

[TYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/type)
= FILE

`IDF   = identifier  `

`PART   = identifier `

`[ ACKEXEC = filename]`

`[ ACKMINDATE = date ]`

`[ ACKMINTIME = time ]`

`[ ACKSTATE = { REQUIRE &#124; IGNORE } ]`

`[ ACKTIMEOUT = { 0 &#124; n }   ]`

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ ARCHIVEFNAME = string ]`

`[ COMMENT   = string ]`

`[ CYCDATE   = date ]`

`[ CYCLE   = { 0   &#124; n }  ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR &#124; RESUME } ]`

`[ EXEC   = filename ]`

`[ EXECSUB   = { LIST   &#124; FILE &#124; SUBF } ]`

`[ EXECSUBA = {LIST &#124; FILE &#124; SUBF }]`

`[ EXIT   = identifier  ]`

`[ FACC   = { ‘ ‘ &#124; character } ]`

`[ FACTION   = { NONE   &#124; DELETE &#124; ERASE &#124;  ARCHIVE } ]`

`[ FBLKSIZE   = n ]`

`[ FCODE   = { ASCII &#124; BINARY &#124; EBCDIC } ]`

`[ FDATE   = date ]`

`[ FDB   = filename ]`

`[ FDISP   = { SHR   &#124; OLD &#124; CHECK } ]`

`[ FILTER = string ]`

`[ FILTERTYPE = string ]`

`[ FKEYLEN   = { 0   &#124; n } ]`

`[ FKEYPOS   = { 0   &#124; n } ]`

`[ FLRECL   = n ]`

`[ FNAME    = { filename   &#124; mask &#124; dirname &#124; #filename &#124; #mask &#124; #dirname } ]`

`[ FNAMEABS   = { YES &#124; NO }  ]`

`[ FORG   = { SEQ   &#124; DIRECT &#124; INDEXED } ]`

`[ FPAD = { ' ' &#124; character } ]`

`[ FRECFM   = { ‘ ‘ &#124; F &#124; U &#124; V }  ]`

`[ FSPACE   = n ]`

`[ FTIME   = time  ]`

`[ FTYPE   =  { ‘ ‘   &#124; character } ]`

`[ IDA   = identifier ]`

`[ IPART   = identifier ]`

`[ MAXDATE   = { 99991231   &#124; date } ]`

`[ MAXTIME   = { 23595999   &#124; time } ]`

`[ MINDATE   = { current   system date &#124; date } ]`

`[ MAXDURATION =  0...32767} ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ NBLKSIZE   = n ]`

`[ NCODE   = { ASCII &#124; BINARY &#124; EBCDIC } ]`

`[ NCOMP   = { 0 &#124; 15 } ]`

`[ NETBAND   = { 1...16} ]`

`[ NFNAME   = filename ]`

`[ NIDF   = string ]`

`[ NKEYLEN   = { 0   &#124; n } ]`

`[ NKEYPOS   = { 0   &#124; n }]`

`[ NLRECL   = n  ]`

`[ NPAD = { ' ' &#124; character } ]`

`[ NRECFM   = { ‘   ‘ &#124; F &#124; U &#124; V } ]`

`[ NSPACE   = { FSPACE   value &#124; n } ]`

`[ NTYPE   = { ' ' &#124; character } ]`

`[ PARM   = string ]`

`[ PRESTATE = { ' ' &#124; character } ]`

`[ PREMINDATE = date ]`

`[ PREMINTIME = time ]`

`[ PRETIMEOUT = { 0 &#124; n } ]`

`[ POSTMINDATE = date ]`

`[ POSTMINTIME = time ]`

`[ PRI   = { 128   &#124; n } ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SELFNAME    = filename   ]`

`[ SERIAL = { ' ' &#124; Y &#124; Z &#124; X } ]`

`[ SPART   = identifier ]`

`[ STATE   = { DISP   &#124; HOLD &#124; KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH } ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

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

`[ CYCLE   = { 0   &#124; n } ]`

`[ CYCTIME   = time ]`

`[ DACTION = { ERROR &#124; RESUME} ]`

`[ EXEC   = filename ]`

`[ IDA   = identifier ]`

`[ IPART   = identifier ]`

`[ MAXDATE   = { 99991231   &#124; date  }   ]`

`[ MAXTIME   = { 23595999   &#124; time }  ]`

`[ MINDATE   = { current   system date &#124; date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ PRI   = pri  ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SPART   = identifier ]`

`[ STATE   = { DISP   &#124; HOLD &#124; KEEP } ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH } ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

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

`[ MAXDATE   =  { 99991231   &#124; date } ]`

`[ MAXTIME   = { 23595999   &#124; time } ]`

`[ MINDATE   = { current   system date &#124; date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ PRI   = pri ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH } ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`

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

`[ MAXDATE   =  { 99991231   &#124; date } ]`

`[ MAXTIME   = { 23595999   &#124; time } ]`

`[ MINDATE   = { current   system date &#124; date } ]`

`[ MINTIME   = { 0   &#124; time } ]`

`[ PRI   = pri ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   &#124; MIN &#124; MONTH } ]`

`[ TRK   = { UNDEFINED   &#124; ALL &#124; SUMMARY &#124; NO } ]`
