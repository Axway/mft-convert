{
    "title": "RESUME  - Retrieve a blocked transfer",
    "linkTitle": "RESUME - Retrieving a blocked request",
    "weight": "340"
}This topic describes the <span id="About_the_RESUME_Command"></span>RESUME
command. This command retrieves, in server mode, a blocked send request
that has a *hold* phasestep, and has a diagnostic codes other than zero.

The **requester** may either:

- Restart the receive
    operation, or
- Start a new receive
    operation

Description:

Use this command to retrieve, in server mode, a blocked
send request with the hold phasestep, the diagnostic codes of which
are not null.

This command resets the diagnostic code to 0 and the diagnostic
code to HOLD.


| Parameter  | Description  |
| --- | --- |
| [BLKNUM](../../../command_summary/parameter_intro/blknum)  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction for the requests in question. |
| [IDA](../../../command_summary/parameter_intro/ida)  | Local identifier of the transfer assigned by the user or user application.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| [IDF](../../../command_summary/parameter_intro/idf)  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| [IDT](../../../command_summary/parameter_intro/idu)  | Transfer identifier.<br/> This identifies a transfer for a given partner. The value ‘*****’ means that no selection is required using the IDT parameter (default value). |
| [IDTU](../../../command_summary/parameter_intro/idtu)  | Transfer local counter identifier. |
| [KDATE]()  | Command deposit date.  |
| [KTIME]()  | Command deposit time.  |
| [PART](../../../command_summary/parameter_intro/part) <br/> (Mandatory) | Partner identifier.<br/> The associated value of this parameter may be:<br/> • *identifier*: the command only concerns the transfers with this partner,<br/> • *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask. |
| [PHASE]()  | Phase of a catalog entry.  |
| [PHASESTEP]()  | Phase step of a catalog entry.  |
| [SCOPE](../../../command_summary/parameter_intro/scope)  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN').  |
| [STATE](../../../command_summary/parameter_intro/state)  | Transfer request state.  |


#### Example 1

```
RESUME
PART = NEW_YORK5
```

Retrieval of all the transfers in server mode with partner NEW_YORK5.

#### Example 2

```
RESUME
PART = NEW_YORK5,
IDF = PAY
```

Retrieval of the transfers in server mode with partner NEW_YORK5 for
the PAYs IDF.
