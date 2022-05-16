---

    title: Configuring  the environment
    linkTitle: Configuring the environment
    weight: 370

---
This topic describes how to configure the environment for an end-of-transfer
type exit. Before you submit this EXIT, you must customize the following
{{< TransferCFT/axwayvariablesComponentShortName  >}} objects:

- [Defining
    the CFTEXIT object](#Defining_the_CFTEXIT_object): to describe the EXIT environment and how this
    EXIT is activated

<!-- -->

- [Defining
    the CFTPARM object](#Defining_the_CFTPARM_object): to indicate the CFTEXIT object identifier

Each CFTEXIT object corresponds to an EXIT task. The number of EXIT
tasks of all types simultaneously active is limited to a number depending
on the operating system.

An end-of -tansfer EXIT task is activated in memory before the EXECxx
submission call is deactivated, following a time-out or {{< TransferCFT/axwayvariablesComponentLongName  >}} shutdown.

<span id="Defining_the_CFTEXIT_object"></span>

## Defining the CFTEXIT object

<span class="bold_in_para">****Syntax****</span>

`CFTEXITID = identifier,TYPE = EXEC,[FORMAT = { V23   | V24 }][LANGUAGE = {COBOL | C},][MODE = {REPLACE | CREATE | DELETE},][PARM = string,][PROG = {CFTEXIT | string},][RESERV = string ][WAITTASK = {1441 | n}]`


| Parameter | Definition |
| --- | --- |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/id">ID</a> <br/> (Mandatory) | Command identifier (32 +1). The value of this identifier corresponds to the identifier defined in the EXITEOT parameter of the related CFTPARM object. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/format">FORMAT</a> | Indicates the format for the communication area.<br/> • V23 (Default value): The communication area between {{< TransferCFT/axwayvariablesComponentShortName  >}} and user’s exits remains the same.<br/> • V24: The communication area takes into account the length of the new identifier. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/language">LANGUAGE</a> | Language in which the user program is written.<br/> The possible values are COBOL and C language.<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} uses this attribute to exchange data with the program using the EXIT via the structure best suited to the language in which it is implemented. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/parm">PARM</a>  | Free user field (64 +1). |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/prog">PROG</a>  | Name of the executable module associated with the EXIT task (512 +1). This module is built from the interface provided with Transfer CFT linked to the program written by the user. In order to facilitate identification of the associated module, it is advised to name it CFTEXIE. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/reserv">RESERV</a>  | Size of the working area reserved for the user.<br/> This area is not used by the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface. You can use it to save data required for the processing of the program that you have written. This area is de-allocated when the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface de-selects the file. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/type">TYPE</a> <br/> (Mandatory) | EXEC |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/waittask">WAITTASK</a>  | Time during which a file access task is inactive (in minutes) before being shut down |


<span id="Defining_the_CFTPARM_object"></span>

## Defining the CFTPARM object

****Syntax****

`CFTPARMID = identifier,[EXITEOT = identifier,]....`


| Parameter | Definition |
| --- | --- |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/id">ID</a><br/> (Mandatory) | CFTPARM object identifier. |
| <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/exiteot">EXITEOT</a>  | EXIT identifier. To activate an end-of-transfer EXIT, you must specify an identifier that points to a CFTEXIT object. |


 
