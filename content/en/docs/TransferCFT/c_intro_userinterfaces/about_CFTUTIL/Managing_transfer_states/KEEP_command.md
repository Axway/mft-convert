{
    "title": "KEEP - Suspend  transfers",
    "linkTitle": "KEEP - Suspending transfers",
    "weight": "320"
}This topic describes the KEEP command, which is used to <span id="About_the_KEEP_Command"></span>suspend
one or all of the send, or one or all of the receive, transfers with selected
partners.

The difference between suspending and interrupting, a HALT, is that
the transfer can only be restarted by a manual operator command, the START
command.

The suspended transfers are set to the ****K****
phasestep. The monitor ensures the integrity of the data in case of suspension
and, depending on the protocol used, authorizes the restarting of the
transfer from the last synchronization point set before the interruption,
or simply from the beginning of the file.


| Parameters  | Description  |
| --- | --- |
| [BLKNUM](../../../command_summary/parameter_intro/blknum)  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| [DIAGC]()  | Protocol diagnostic code  |
| [DIAGP](../../../command_summary/parameter_intro/diagp)  | Complimentary diagnostic information  |
| [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction for the requests in question. |
| [IDA](../../../command_summary/parameter_intro/ida)  | Local identifier of the transfer assigned by the user or user application.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| [IDF](../../../command_summary/parameter_intro/idf)  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| [IDT](../../../command_summary/parameter_intro/idu)  | Transfer identifier. |
| [IDTU](../../../command_summary/parameter_intro/idtu)  | Transfer local counter identifier. |
| [KDATE]()  | Command deposit date.  |
| [KTIME]()  | Command deposit time.  |
| [PART](../../../command_summary/parameter_intro/part) | Partner identifier.<br/> The associated value of this parameter can be either a:<br/> • *Identifier*: the command only concerns the transfers with this partner<br/> • *Mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask |
| [PHASE]()  | Phase of a catalog entry.  |
| [PHASESTEP]()  | Phase step of a catalog entry.  |
| [SCOPE](../../../command_summary/parameter_intro/scope)  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN').  |
| [STATE](../../../command_summary/parameter_intro/state)  | Transfer request state.  |


#### Example 1

```
KEEP PART = PARIS2
```

This command suspends all transfers (IDT = \* by default) in the send
and receive directions (DIRECT = BOTH by default) with the partner PARIS2.

#### Example 2

```
KEEP
PART = PARIS\*,
IDF = PAY,
DIRECT = RECV
```

This command suspends the receiving of the file PAY from a partner,
whose identifier begins with PARIS.

Modify transfer entries parameters
----------------------------------

The following tables describes the parameters used to modify a transfer entry in the catalog.


| Command  | Parameter  | Value  | Description  |
| --- | --- | --- | --- |
| KEEP  | DIAGC  | string  | Specify a comment.  |
| KEEP  | DIAGP  | string  | Specify a comment.  |

