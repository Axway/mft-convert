{
    "title": "execse",
    "linkTitle": "execse",
    "weight": "880"
}<span id="execse"></span>

### execse

<span id="execse_CFTPARM"></span>

#### CFTPARM

****[EXECSE = filename]
   {string
64}****

Generic name of the file describing
the procedures to be executed, following an incident (Error) during a
send transfer, the transfer changing to the H or K state.

This name may include the following symbolic variables:

- &IDF, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL
- &DIAGI, &DIAGP
- &NIDF

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* that corresponds with your OS.

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
