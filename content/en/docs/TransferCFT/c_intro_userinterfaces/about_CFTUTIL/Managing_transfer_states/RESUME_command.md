---

    title: RESUME  - Retrieve a blocked transfer
    linkTitle: RESUME - Retrieving a blocked request
    weight: 350

---
This topic describes the <span id="About_the_RESUME_Command"></span>RESUME
command. This command retrieves, in server mode, a blocked send request
that has a *hold* phasestep, and has a diagnostic codes other than zero.

The <span style="font-style: italic;">**requester**</span> may either:

- Restart the receive
    operation, or
- Start a new receive
    operation

QQQ\_QQQ\_QQQ

Use this command to retrieve, in server mode, a blocked
send request with the hold phasestep, the diagnostic codes of which
are not null. This command resets the diagnostic code to 0 and the diagnostic
code to HOLD.


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/blknum">BLKNUM</a>  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| <a href="../../../command_summary/parameter_intro/direct">DIRECT</a>  | Transfer direction for the requests in question. |
| <a href="../../../command_summary/parameter_intro/ida">IDA</a>  | Local identifier of the transfer assigned by the user or user application.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a>  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| <a href="../../../command_summary/parameter_intro/idu">IDT</a>  | Transfer identifier.<br/> This identifies a transfer for a given partner. The value ‘*****’ means that no selection is required using the IDT parameter (default value). |
| <a href="../../../command_summary/parameter_intro/idtu">IDTU</a>  | Transfer local counter identifier. |
| <a href="">KDATE</a>  | Command deposit date.  |
| <a href="">KTIME</a>  | Command deposit time.  |
| <a href="../../../command_summary/parameter_intro/part">PART</a> <br/> (Mandatory) | Partner identifier.<br/> The associated value of this parameter may be:<br/> • *identifier*: the command only concerns the transfers with this partner,<br/> • *mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask. |
| <a href="">PHASE</a>  | Phase of a catalog entry.  |
| <a href="">PHASESTEP</a>  | Phase step of a catalog entry.  |
| <a href="../../../command_summary/parameter_intro/scope">SCOPE</a>  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN').  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>  | Transfer request state.  |


#### Example 1

```
RESUME
PART = NEW_YORK5
```

Retrieval of all the transfers in server mode with partner NEW\_YORK5.

#### Example 2

```
RESUME
PART = NEW_YORK5,
IDF = PAY
```

Retrieval of the transfers in server mode with partner NEW\_YORK5 for
the PAYs IDF.
