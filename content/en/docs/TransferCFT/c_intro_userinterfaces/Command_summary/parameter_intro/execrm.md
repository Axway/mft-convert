---
title: "execrm"
linkTitle: "execrm"
weight: 870
---<span id="execrm"></span>

### execrm

<span id="execrm_CFTPARM"></span>

#### CFTPARM

****[EXECRM = filename]
    {string 64}****

Generic name of the file describing
the procedures to be executed on completion of reception of a message.

This name may include the following symbolic variables:

* &IDM, &PARM
* &PART, &RPART,
    &SPART, &GROUP
* &RUSER, &SUSER,
    &USERID
* &RAPPL, &SAPPL

The character ‘&’ designates the char_symb character defined in
the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide* corresponding to your OS

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

[Return to Command index](../../)
