{
    "title": "execrf",
    "linkTitle": "execrf",
    "weight": "860"
}<span id="execrf"></span>

### execrf

<span id="execrf_CFTPARM"></span>

#### CFTPARM

****[EXECRF = filename]     {string 64}****

Generic name of the file describing the procedures to be executed on
completion of the reception of a file.

This name may include the following symbolic variables:

- &IDF, &PARM
- &PART, &RPART,
    &SPART, &GROUP
- &RUSER, &SUSER,
    &USERID
- &RAPPL, &SAPPL
- &NIDF

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS.

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
