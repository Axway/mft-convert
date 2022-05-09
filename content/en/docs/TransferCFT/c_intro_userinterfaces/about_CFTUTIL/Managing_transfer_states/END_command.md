{
    "title": "END - Confirm the end of processing",
    "linkTitle": "END - Change transfer state",
    "weight": "300"
}You can use the END command to declare
that the processing script
(pre, post, or acknowledgement) was executed correctly. More specifically, if there is an associated processing procedure, the PREEXEC, EXEC, ACKEXEC parameters of the CFTPARM,CFTSEND/SEND and CFTRECV/RECV commands are executed.

Typically, you submit the END command to Transfer CFT via a processing procedure(s). You can, however, submit it manually. After the END command is executed, once transfers are in the C phasestep the associated
catalog entry changes to the next phase. See [About phase and phasestep](../../../../concepts/phase_and_phasestep) for details on transfer work flows.

END parameters
--------------

There are two categories of parameters that you can use with the END command, those that you can use to select transfers, and those that you use to modify the catalog entry. Parameters that have an affect on the transfer entry in the catalog are listed in the **Modify in catalog** column. All others are parameters are related to the transfers as described in **Select for transfers** only.


| Parameters  | Select for transfers  | Modify in catalog  |
| --- | --- | --- |
| [APPCYCID](../../../command_summary/parameter_intro/appcycid)  | Modify the processing cycle identifier  |   |
| [APPOBJID](../../../command_summary/parameter_intro/appobjid)  | Modify the tracked object name  |   |
| [APPSTATE]()  |   | State of the end phase for the processing script to restart<br/> Specify an application state for the processing script that will help the script to restart at the right step if the script is relaunched. |
| [BLKNUM ](../../../command_summary/parameter_intro/blknum) | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |   |
| [DIAGC]()  |   | Modify the complimentary diagnostic information.  |
| [DIRECT ](../../../command_summary/parameter_intro/direct) | Transfer direction for the requests in question.<br/> The possible values are:<br/> • BOTH: (default) takes both send transfers and receive transfers into account,<br/> • RECV: limits the action to receive transfers,<br/> • SEND: limits the action to send transfers. |   |
| [FNAME](../../../command_summary/parameter_intro/fname)  |   | Modify the FNAME, name of the local file, directory, indirection file, selection mask or selection directory.  |
| [IDA ](../../../command_summary/parameter_intro/ida) | Local identifier of the transfer assigned by the user or user application. |   |
| [IDF ](../../../command_summary/parameter_intro/idf) | Model file identifier. |   |
| [IDT]() | Transfer identifier. |   |
| [IDTU](../../../command_summary/parameter_intro/idtu) | Catalog identifier. It is a unique, local reference to a transfer. |   |
| [ISTATE]()  |   | Intermediate state indicating if the phase has finished:<br/> • YES: The END command is only a checkpoint, the phase is not finished.<br/> • NO (default): This is the final end command indicating that the processing is over. Once the END completes, the transfer enters the next phase. |
| [KDATE]()  | Command deposit date  |   |
| [KTIME]()  | Command deposit time  |   |
| [NFNAME](../../../command_summary/parameter_intro/nfname)  |   | Modify the NFNAME, the name of the physical file at the receiver partner site.  |
| [PARM](../../../command_summary/parameter_intro/parm)  | User parameter  |   |
| [PART](../../../command_summary/parameter_intro/part) | Partner identifier.<br/> The value of this parameter may be:<br/> • an *identifier*: the command only concerns the transfers with this partner<br/> • a *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this masks |   |
| [PRI](../../../command_summary/parameter_intro/pri)  | Priority of scheduled transfers  |   |
| [PHASE]()  | The transfer phase of a catalog entry at which the command is applied.  |   |
| [PHASESTEP]()  | The phase step of a catalog entry at which the command is applied.  |   |
| [RAPPL](../../../command_summary/parameter_intro/rappl)  |   | Modify the RAPPL, the identifier of the file receiver application.  |
| [RPASSWD](../../../command_summary/parameter_intro/rpassw)  |   | Modify the RPASSWD, the password for the user who is receiving the file.  |
| [RUSER](../../../command_summary/parameter_intro/ruser)  |   | Modify the RUSER, the identifier for the user who is receiving the file.  |
| [STATE](../../../command_summary/parameter_intro/state)  | Transfer request state  |   |
| [SCOPE](../../../command_summary/parameter_intro/scope)  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN')  |   |
| [SIGFNAME](../../../command_summary/parameter_intro/sigfname)  |   | Modify the SIGFNAME, which contains signatures of the different signatories and the subscriber as defined by SUSER.  |
| [SAPPL](../../../command_summary/parameter_intro/sappl)  |   | Modify the SAPPL, the identifier of the file sender application.  |
| [SUSER](../../../command_summary/parameter_intro/suser)  |   | Modify the SUSER, the identifier for the user who is sending the file.  |
| [SPASSWD](../../../command_summary/parameter_intro/spasswd)  |   | Modify the SPASSWD, the password for the user who is sending the file.  |


Using the END command
---------------------

**Using ISTATE=YES**

Upon file reception the END command selects the transfer for the defined partner, and modifies the `DIAGC `in the catalog (`step1_completed` in the catalog) at which point the transfer does not go to the next phase because you have set `ISTATE=YES`.

```
END
 PART = HQ,
 ISTATE=YES,
 DIAGC=step1_completed,
 DIRECT=RECV,
  IDF = TEST,
 IDA = X32451
```

Log output:

```
CFTR12I END Treated for USER <user> : DIAGC value was "" and is now "step1_completed"
```

**Using ISTATE=NO**

Upon file reception the END command selects the transfer for the defined partner, and modifies the `DIAGC `in the catalog (end_completed in the catalog), at which point the transfer goes to the next phase because you have set `ISTATE=NO`.

```
END
 PART = HQ,
 ISTATE=NO,
 DIAGC=end_completed,
   DIRECT=RECV,
 IDF = TEST,
 IDA = X32451
```

Log output:

```
CFTR12I END Treated for USER <user> : DIAGC value was "step1_completed" and is now "end_completed"
```
