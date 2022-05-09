{
    "title": "HALT- Interrupt transfers ",
    "linkTitle": "HALT - Halting a transfer",
    "weight": "310"
}This command is used to suspend one or all the send and/or receive transfers,
with the partners selected.

The transfers halted are set to the H phasestep in the catalog. They can
be reactivated:

- By an operator
    START command
- On receiving a
    transfer reactivation request from the partner

See also [LISTCAT.](../../monitoring_cftutil_intro/listcat_command)

{{< TransferCFT/axwayvariablesComponentLongName  >}} ensures the integrity of the data in case of interruption
and, depending on the protocol used, authorizes the restarting of the
transfer from the last synchronization point set before the interruption,
or simply from the beginning of the file.

You can use the HALT command to interrupt a transfer on Transfer CFT.
This interruption changes the entry to the H phasestep. Alternatively, the
KEEP command changes the entry to the K phasestep.

An accidental halt due to a network incident, a protocol error or a
software or hardware interrupt, makes the entry change to the H, K, or
D phasestep, with the diagnostic codes specifying the origin. For more information
refer to the Transfer CFT *[Codes,
Diagnostics and Messages](../../../../troubleshoot_intro/messages_and_error_codes_start_here)* topics.

*In requester mode*, a transfer in
the D state (phase T and phasestep D) is automatically restarted. A transfer in the K or H phasestep
must be manually restarted by a START command.


| Parameters  | Description  |
| --- | --- |
| [BLKNUM](../../../command_summary/parameter_intro/blknum) | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction for the requests in question.<br/> The possible values are:<br/> • BOTH: (default) takes both send transfers and receive transfers into account,<br/> • RECV: limits the action to receive transfers,<br/> • SEND: limits the action to send transfers. |
| [IDA](../../../command_summary/parameter_intro/ida)  | Local identifier of the transfer assigned by the user or user application. The maximum length is 64 characters.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| [IDF](../../../command_summary/parameter_intro/idf)  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| [IDT](../../../command_summary/parameter_intro/idu)  | Transfer identifier. |
| [IDTU](../../../command_summary/parameter_intro/idtu)  | Transfer local counter identifier. |
| [STATE](../../../command_summary/parameter_intro/state)  | Transfer request state  |
| [KDATE]()  | Command deposit date  |
| [KTIME]()  | Command deposit time  |
| [PHASE]()  | Phase of a catalog entry  |
| [PHASESTEP]()  | Phase step of a catalog entry  |
| [SCOPE](../../../command_summary/parameter_intro/scope)  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN')  |
| [DIAGC]()  | Complimentary diagnostic information  |
| [DIAGP](../../../command_summary/parameter_intro/diagp)  | Protocol diagnostic code  |
| [PART](../../../command_summary/parameter_intro/part) <br/> (Mandatory) | Partner identifier.<br/> The associated value of this parameter can be:<br/> • *identifier*: the command only concerns the transfers with this partner<br/> • *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask |


Halt the send transfer of the ACCNT model file for the
partner HQ.

#### Example 1

```
HALT PART = \*
```

This command halts all transfers (IDT = \* by default) in the send and
receive directions (DIRECT = BOTH by default), for all the partners.

#### Example 2

```
HALT
 PART = HQ,
 DIRECT = SEND,
 IDF = ACCNT
```

This command Halts the send transfer of the ACCNT model file for the
partner HQ.

Modify transfer entry parameters
--------------------------------

The following tables describes the parameters used to modify a transfer entry in the catalog.


| Command  | Parameter  | Value  | Description  |
| --- | --- | --- | --- |
| HALT  | DIAGC  | string  | Specify a comment.  |
| HALT  | DIAGP  | string  | Specify a comment.  |

