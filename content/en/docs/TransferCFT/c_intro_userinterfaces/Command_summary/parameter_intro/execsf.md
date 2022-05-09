{
    "title": "execsf",
    "linkTitle": "execsf",
    "weight": "890"
}<span id="execsf"></span>

### execsf

<span id="execsf_CFTPARM"></span>

#### CFTPARM

****[EXECSF = filename]
   {string
64}****

Generic name of the file describing the procedures to be executed on
completion of sending a file.

This name may include the following symbolic variables:

- &IDF, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL
- &NIDF

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* that corresponds with your OS.

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
