{
    "title": "Exit routines - CFTEXIT",
    "linkTitle": "Exit routines - CFTEXIT",
    "weight": "270"
}<span id="Activating_an_exit_command_line"></span>This topic describes the
CFTEXIT command, used to start an EXIT task.

********Related
topics********

- Command syntax
    [CFTEXIT](../../../command_summary#CFTEXIT)
- Object concepts
    [Managing exits](../../../../app_integration_intro/managing_exits)


| Parameters  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id) | Command identifier |
| [LANGUAGE](../../../command_summary/parameter_intro/language) | Language used to write the user program.<br/> The possible values are COBOL and C language.<br/> This attribute enables Transfer CFT to exchange data with the program using the EXIT task via the structure best suited to the language in which it is implemented. |
| [PARM](../../../command_summary/parameter_intro/parm)  | Free field available to the user. |
| [PROG](../../../command_summary/parameter_intro/prog) | Name of the executable module corresponding to the EXIT task to be activated.<br/> This module consists of the interface provided with the Transfer CFT product and linked with the program written by the user.<br/> To facilitate identification of the associated modules, it is advised to name these modules as follows:<br/> • CFTEXA: Directory type EXIT<br/> • CFTEXF: File type EXIT<br/> • CFTEXIB: Beginning-of-transfer EXIT<br/><br/> • CFTEXE: End-of-transfer type EXIT<br/> You can use the last two characters to assign a sequential number to the name, for example CFTEXA01. |
| [RESERV](../../../command_summary/parameter_intro/reserv) | Size of the work area reserved for the user. |
| [TYPE](../../../command_summary/parameter_intro/type) | Select the exit type, FILE, ACCESS, BOT, or EXEC. |
| [FORMAT](../../../command_summary/parameter_intro/format) | Indicates the format for the communication area. |
| [WAITTASK](../../../command_summary/parameter_intro/waittask) | Inactivity time (in minutes) of the EXIT task. Beyond this value, this task is deactivated, i.e. unloaded from the memory.<br/> This parameter only applies to file EXIT tasks. |


****Example 1 - Parameter setting for a file
type EXIT****

```
CFTSEND
ID = PAY,
EXIT = IDEXIT
```

 

```
CFTEXIT
ID = IDEXIT,
LANGUAGE = C,
RESERV = 4000,
PROG = FILEXEC
```

****Example 2 - Parameter setting for an directory
type EXIT****

```
CFTPROT
ID = PSIDTH,
TYPE = PESIT,
EXIT = EXAM
 
```

 

```
CFTEXIT
ID = EXA,
PARM = SAMPLE,
LANGUAGE = C,
PROG = MYEXA,
TYPE = ACCESS
```
