{
    "title": "execsfa",
    "linkTitle": "execsfa",
    "weight": "900"
}<span id="execsfa"></span>

### execsfa

<span id="execsfa_CFTPARM"></span>

#### CFTPARM

****[EXECSFA =
filename]    {string 64}****

Generic name of the file describing
the procedures to be executed on receiving an acknowledgement (REPLY type
message), following the sending of a file.

This name may include the following symbolic variables:

- &IDF, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL
- &TRTYPE

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS.

The procedure to execute on receiving an
acknowledgement following a file send operation.

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
