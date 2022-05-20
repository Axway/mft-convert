---
title: "execsfa"
linkTitle: "execsfa"
weight: 900
---<span id="execsfa"></span>

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

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

[Return to Command index](../../)
