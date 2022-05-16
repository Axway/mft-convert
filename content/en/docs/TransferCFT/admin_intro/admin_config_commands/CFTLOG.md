---

    title: Transfer log file
    linkTitle: CFTLOG - Transfer log file
    weight: 260

---
Use this command to defines the log file declarations. Transfer CFT
records any significant events that occur during a transfer.

Log parameters are defined in detail in the Parameter index. Click on
a specific parameter for more information.

Related
topics

- Command syntax
    [CFTLOG](../../../c_intro_userinterfaces/command_summary#CFTLOG)
- Object concepts
    [Log parameters]()


| Parameter  | Description  |
| --- | --- |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/afname">AFNAME</a>  | Name of the alternate log file. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/content">CONTENT</a> | The messages written in the active LOG file are filtered. The possible values are:<br/> • FULL: all the messages are printed out<br/> • BRIEF: the following messages no longer appear in the LOG |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/exec">EXEC</a> | Name of the procedure to be executed when switching to the other log file. By default this is rotate.cmd/bat. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/fname">FNAME</a> | Name of the log file. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/id">ID</a> | Identifier of the CFTLOG command. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/length">LENGTH</a> | Size of log file records. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/maxrec">MAXREC</a> | Number of records written in the log file, from which automatic switching will be performed. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/mode">MODE</a> | Action to do in the parameter or partner base. This parameter applies to all commands that affect Transfer CFT bases. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/notify">NOTIFY</a> | Defines the destination of the operator messages selected according to the value of the OPERMSG parameter |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/opermsg">OPERMSG</a> | Defines the transfer information message categories intended for the operator (all the messages also being written in the log file). |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/switch">SWITCH</a> | Time at which the Transfer CFT performs an automatic switch. When this parameter is not defined, log files are switched daily at midnight. |


<span class="bold_in_para">****Example**** </span>

This command defines the log file names:

```
CFTLOG    ID = IDLOG,
          FNAME = filename1,
          AFNAME = filename2,
          SWITCH = 2030,
          EXEC = procfname,
          OPERMSG = 240
```

Automatic switching is scheduled for 20:30. The procedure initiated
at the time of switching is located in the file named by the EXEC parameter.

The record size is the default value of 133 characters.

The value of the OPERMSG parameter allows only error messages to be
sent to the operator (240 = 128+64+32+16).
