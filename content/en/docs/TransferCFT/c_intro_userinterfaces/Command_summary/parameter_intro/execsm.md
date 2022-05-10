---
    "title": "execsm",
    "linkTitle": "execsm",
    "weight": "910"
---
<span id="execsm"></span>

### execsm

<span id="execsm_CFTPARM"></span>

#### CFTPARM

****[EXECSM =
filename]    {string 64}****

Generic name of the file describing
the procedures to be executed on completion of the sending of a message.

This name may include the following symbolic variables:

- &IDM, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS.

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
