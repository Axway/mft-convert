---
    title: "execsf"
    linkTitle: "execsf"
    weight: 890
---<span id="execsf"></span>

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

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

[Return to Command index](../../)
