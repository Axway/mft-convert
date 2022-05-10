---
    "title": "execre",
    "linkTitle": "execre",
    "weight": "850"
---
<span id="execre"></span>

### execre

<span id="execre_CFTPARM"></span>

#### CFTPARM

****[EXECRE = filename]
   {string
64}****

Specify the name of the file describing
the procedure to execute following a file reception error.

Generic name of the file describing the procedures to be executed, following
an incident (Error) occurring during a receive transfer, the transfer
changing to the H or K state.

This name may include the following symbolic variables:

- &IDF, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER
- &RAPPL, &SAPPL
- &DIAGI, &DIAGP
- &NIDF

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS. 

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
