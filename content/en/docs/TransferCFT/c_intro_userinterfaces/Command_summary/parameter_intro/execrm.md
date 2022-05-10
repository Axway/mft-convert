---
    "title": "execrm",
    "linkTitle": "execrm",
    "weight": "870"
---
<span id="execrm"></span>

### execrm

<span id="execrm_CFTPARM"></span>

#### CFTPARM

****[EXECRM = filename]
    {string 64}****

Generic name of the file describing
the procedures to be executed on completion of reception of a message.

This name may include the following symbolic variables:

- &IDM, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
