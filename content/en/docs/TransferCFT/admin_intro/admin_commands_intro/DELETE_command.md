---

    title: DELETE - Delete  catalog entries
    linkTitle: DELETE - Deleting catalog entries
    weight: 290

---
The DELETE command is used to <span id="delete_command"></span>delete one
or more catalog entries. A transfer in process is interrupted and its
transfer request disappears from the catalog.

This file is automatically deleted for an R, Receive, type request in
non terminated state, a state other than T and X, if the receive file
is a temporary file.

For any selected set of transfers, transfers are processed in batches
of 20 transfers every 5 seconds.

QQQ\_QQQ\_QQQ

## List of parameters of DELETE command


| DELETE <br /> Parameters  | Description  |
| --- | --- |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/blknum">BLKNUM</a>  | Block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/direct">DIRECT</a>  | Transfer direction for the requests in question.<br/> The possible values are:<br/> • BOTH: (default) takes both send transfers and receive transfers into account<br/> • RECV: limits the action to receive transfers<br/> • SEND: limits the action to send transfers |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/ida">IDA</a>  | Local identifier of the transfer assigned by the user or user application. The maximum length is 64 characters.<br/> Several catalog entries may be associated with a given IDA. There is no default value. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idf">IDF</a>  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value.<br/> If a set of transfers is selected, transfers are processed in batches of 20 every 5 seconds. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idu">IDT</a>  | Transfer identifier.<br/> This identifies a transfer for a given partner and direction. The value ‘*’ means that no selection is required using the IDT parameter (default value). |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idtu">IDTU</a>  | Transfer local counter identifier. |
| <a href="">KDATE</a>  | Command deposit date  |
| <a href="">KTIME</a>  | Command deposit time  |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/part">PART</a><br/> (Mandatory) | Partner identifier. |
| <a href="">PHASE</a>  | Phase of a catalog entry  |
| <a href="">PHASESTEP</a>  | Phase step of a catalog entry  |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/scope">SCOPE</a>  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN')  |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/state">STATE</a>  | Transfer state.<br/> The default value * means no selection is required to be made on the transfer state. (refer to the Transfer-related successive phases and actions). |


****Example 1****

```
DELETE        PART = SIE??
```

This command deletes all transfers, IDT = \* by default, in the send
and receive directions, DIRECT = BOTH by default, for the partners whose
identifier begins with "SIE" and contains 5 characters in all.

****Example 2****

```
DELETE
PART = HQ,
 
 
DIRECT = SEND,
 
 
IDF = ACCNT
 
```

This command deletes all transfers, IDT = \* by default, in the send
direction (DIRECT = SEND) of the model file ACCNT to the partner HQ.
