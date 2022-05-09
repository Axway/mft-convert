{
    "title": "START  - Restart transfers",
    "linkTitle": "START - Restarting transfers",
    "weight": "350"
}This topic describes the START command and its parameters.

Only the Transfer CFT requesting the transfer can initiate
a START command.

Transfers in the H or K phasestep in the catalog change to
the D phasestep, after this command is executed. These transfers are restarted
after scanning the catalog if the required resources are available.


| ODETTE | The START command has no effect on the interruption of a RECV command. Transfer CFT in this case operates in server mode and no restart is possible. A new RECV command has to be activated to restart transfers. |
| --- | --- |



| Parameter  | Description  |
| --- | --- |
| [BLKNUM](../../../command_summary/parameter_intro/blknum)  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction for the requests in question.<br/> The possible values are:<br/> • ****BOTH****: (default) takes both send transfers and receive transfers into account<br/> • ****RECV****: limits the action to receive transfers<br/> • ****SEND****: limits the action to send transfers |
| [FORCE](../../../command_summary/parameter_intro/force)  | Indicates whether a request, that was not executed during its time slot should be forced to immediately restart. |
| [IDA](../../../command_summary/parameter_intro/ida) | Local identifier of the transfer assigned by the user or user application.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| [IDF](../../../command_summary/parameter_intro/idf)  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| [IDT](../../../command_summary/parameter_intro/idu)  | Transfer identifier. |
| [IDTU](../../../command_summary/parameter_intro/idtu)  | Transfer local counter identifier. |
| [KDATE]()  | Command deposit date  |
| [KTIME]()  | Command deposit time  |
| [PHASE]()  | Phase of a catalog entry  |
| [PHASESTEP]()  | Phase step of a catalog entry  |
| [SCOPE](../../../command_summary/parameter_intro/scope)  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN')  |
| [STATE](../../../command_summary/parameter_intro/state)  | Transfer request state  |
| [PART](../../../command_summary/parameter_intro/part)  | Partner identifier.<br/> The value of this parameter may be:<br/> • an *identifier*: the command only concerns the transfers with this partner<br/> • a *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask |


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
