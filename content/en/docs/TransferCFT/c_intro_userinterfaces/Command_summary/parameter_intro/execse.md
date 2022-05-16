---

    title: execse
    linkTitle: execse
    weight: 890

---
<span id="execse"></span>

### execse

<span id="execse_CFTPARM"></span>

#### CFTPARM

****\[EXECSE = filename\]
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

The character ‘&’ designates the char\_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* that corresponds with your OS.

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

[Return to Command index](../../)