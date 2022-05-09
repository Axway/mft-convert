{
    "title": "Transfer log file",
    "linkTitle": "Log file - CFTLOG",
    "weight": "190"
}Use this command to defines the log file declarations. Transfer CFT
records any significant events that occur during a transfer.

Log parameters are defined in detail in the Parameter index. Click on
a specific parameter for more information.

Related
topics

- Command syntax
    [CFTLOG](../../../command_summary#CFTLOG)
- Object concepts
    [Log parameters]()


| Parameter  | Description  |
| --- | --- |
| [AFNAME](../../../command_summary/parameter_intro/afname)  | Name of the alternate log file. |
| [CONTENT](../../../command_summary/parameter_intro/content) | The messages written in the active LOG file are filtered. The possible values are:<br/> • FULL: all the messages are printed out<br/> • BRIEF: the following messages no longer appear in the LOG |
| [EXEC](../../../command_summary/parameter_intro/exec) | Name of the procedure to be executed when switching to the other log file. By default this is rotate.cmd/bat. |
| [FNAME](../../../command_summary/parameter_intro/fname) | Name of the log file. |
| [ID](../../../command_summary/parameter_intro/id) | Identifier of the CFTLOG command. |
| [LENGTH](../../../command_summary/parameter_intro/length) | Size of log file records. |
| [MAXREC](../../../command_summary/parameter_intro/maxrec) | Number of records written in the log file, from which automatic switching will be performed. |
| [MODE](../../../command_summary/parameter_intro/mode) | Action to do in the parameter or partner base. This parameter applies to all commands that affect Transfer CFT bases. |
| [NOTIFY](../../../command_summary/parameter_intro/notify) | Defines the destination of the operator messages selected according to the value of the OPERMSG parameter |
| [OPERMSG](../../../command_summary/parameter_intro/opermsg) | Defines the transfer information message categories intended for the operator (all the messages also being written in the log file). |
| [SWITCH](../../../command_summary/parameter_intro/switch) | Time at which the Transfer CFT performs an automatic switch. When this parameter is not defined, log files are switched daily at midnight. |


****Example****

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
