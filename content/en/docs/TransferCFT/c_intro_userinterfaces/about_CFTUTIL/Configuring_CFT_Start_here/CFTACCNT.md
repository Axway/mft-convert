{
    "title": "Accounting records - CFTACCNT  ",
    "linkTitle": "CFTACCNT - Accounting records transfer",
    "weight": "270"
}<span id="About_CFTACCNT"></span>This page describes the <span id="kanchor46"></span>CFTACCNT command.

Use this command to define the recording mode of the statistical data
concerning correctly terminated transfers (T or X state).

Two recording modes are available:

- Recording data
    in {{< TransferCFT/axwayvariablesComponentShortName  >}} files. When the primary file is full, {{< TransferCFT/axwayvariablesComponentShortName  >}}
    switches to an alternate file. This mode is available on all operating
    systems.
- Recording data
    in the files of the operating system accounting utility.

To activate
the statistical mode, you must also configure the [CFTPARM](../../../web_copilot_ui/conf_intro/cftparm)
object.

****Related
topics****

- Object concepts
    [How to define transfer
    accounting records](../../../../admin_intro/admin_config_commands/cftaccnt_concepts)

Parameter descriptions
----------------------


| Parameter  | Description  |
| --- | --- |
| ID | Identifier of the CFTACCNT command. |
| MODE  | Select to perform one of the following:<br/> • CREATE<br/> • REPLACE<br/> • DELETE |
| [TYPE](../../../command_summary/parameter_intro/type#type_CFTACCNT)  | Defines the accounting type.<br/> • FILE: statistical data is recorded in the Transfer CFT files described by the FNAME and AFNAME parameters.<br/> • SYST: statistical data is recorded in a &quot;system&quot; file, through an interface with the system accounting utility. If TYPE=SYST then you must define ACCID. |
| [ACCID](../../../command_summary/parameter_intro/accid#accid_CFTACCNT)  | Only used if TYPE=SYST Accounting system file identifier records. |
| COMMENT  |   |
| LANGUAGE | Programming language of the application using the statistical data recorded.<br/> • COBOL<br/> • C |
| [FNAME](../../../command_summary/parameter_intro/fname#fname_CFTACCNT) | Name of the statistical file. |
| [AFNAME](../../../command_summary/parameter_intro/afname#afname_CFTACCNT) | Name of the alternate statistical file. |
| [MAXREC](../../../command_summary/parameter_intro/maxrec)  | Statistical file maximum number of records. |
| [EXEC](../../../command_summary/parameter_intro/exec#exec_CFTACCNT)  | Name of the procedure to be executed when Transfer CFT switches to the other statistical file<br/> (SWITCH). |
| [SWITCH](../../../command_summary/parameter_intro/switch#switch)  | Time at which Transfer CFT automatically switches to the alternate statistical file.<br/> When this parameter is not defined, Transfer CFT switches statistical files daily at midnight. |


Examples
--------

****TYPE = FILE****

```
CFTACCNT
ID = ACCNT,
LANGUAGE = C,
FNAME = <filename1>,
AFNAME = <filename2>,
MAXREC = 1000,
EXEC = <filename3>
```

This command defines the recording of statistical data concerning transfers
in files specific to Transfer CFT. It designates the statistical and alternate
statistical files (FNAME and AFNAME parameters). Data is recorded in these
files for use by an application program written in C language.

File switching takes place manually (SWITCH command) or when the number
of statistical records equals 1000. The procedure initiated at the time
switching occurs is located in the file designated by the EXEC parameter.

****TYPE = SYST****

```
CFTACCNT
ID = ACCNT,
TYPE = SYST,
ACCID = n       z/OS
```

This command defines the recording of statistical data
concerning transfers in operating system accounting utility files, using
an identifier indicated by the ACCID parameter, for the systems designated.
