---

    title: START  - Restart transfers
    linkTitle: START - Restarting transfers
    weight: 360

---
This topic describes the START command and its parameters.

Only the Transfer CFT requesting the transfer can initiate
a START command.

Transfers in the H or K phasestep in the catalog change to
the D phasestep, after this command is executed. These transfers are restarted
after scanning the catalog if the required resources are available.


| ODETTE | The START command has no effect on the interruption of a RECV command. Transfer CFT in this case operates in server mode and no restart is possible. A new RECV command has to be activated to restart transfers. |
| --- | --- |



| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/blknum">BLKNUM</a>  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| <a href="../../../command_summary/parameter_intro/direct">DIRECT</a>  | Transfer direction for the requests in question.<br/> The possible values are:<br/> • <span >****BOTH****</span>: (default) takes both send transfers and receive transfers into account<br/> • <span >****RECV****</span>: limits the action to receive transfers<br/> • <span >****SEND****</span>: limits the action to send transfers |
| <a href="../../../command_summary/parameter_intro/force">FORCE</a>  | Indicates whether a request, that was not executed during its time slot should be forced to immediately restart. |
| <a href="../../../command_summary/parameter_intro/ida">IDA</a> | Local identifier of the transfer assigned by the user or user application.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a>  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| <a href="../../../command_summary/parameter_intro/idu">IDT</a>  | Transfer identifier. |
| <a href="../../../command_summary/parameter_intro/idtu">IDTU</a>  | Transfer local counter identifier. |
| <a href="">KDATE</a>  | Command deposit date  |
| <a href="">KTIME</a>  | Command deposit time  |
| <a href="">PHASE</a>  | Phase of a catalog entry  |
| <a href="">PHASESTEP</a>  | Phase step of a catalog entry  |
| <a href="../../../command_summary/parameter_intro/scope">SCOPE</a>  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN')  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>  | Transfer request state  |
| <a href="../../../command_summary/parameter_intro/part">PART</a>  | Partner identifier.<br/> The value of this parameter may be:<br/> • an *identifier*: the command only concerns the transfers with this partner<br/> • a *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask |


#### Example 1

```
START
 PART = PARIS5
```

Restarts all the transfers, IDT = \* by default, in the send and receive
directions, DIRECT = BOTH by default, with the partner PARIS5.

#### Example 2

```
START
 PART = PARIS5,
 IDF = PAY,
 DIRECT = RECV
```

Restarts the receive transfers for the PAY IDF from the partner PARIS5.
