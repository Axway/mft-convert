---

    title: SUBMIT  - Submit a processing procedure
    linkTitle: SUBMIT - Submitting an end-of-transfer
    weight: 370

---
This topic describes the SUBMIT command, which is used to submit a processing procedure for a selected
transfer.

The procedure inherits the transfer context symbolic variables.

You can start, or restart, this procedure when the file or message transfer
is in the T phasestep.

This command has no effect on the transfers belonging to a broadcasting
list even if these transfers are in the T phasestep. Alternatively, it can
be applied to a generic transfer, whose PART parameter equals the broadcasting
list identifier, when it is in the T phasestep.


| Parameters  | Description  |
| --- | --- |
| <a href="">APPSTATE</a>  | State of the [end] phase for the processing script to restart  |
| <a href="../../../command_summary/parameter_intro/blknum">BLKNUM</a>  | Catalog block number. If the values '*' or ' ' are used then all transfers are selected regardless of the block that they belong to. |
| <a href="../../../command_summary/parameter_intro/direct">DIRECT</a> | Transfer direction for the requests in question.<br/> Possible values are:<br/> • BOTH: (default) takes both send transfers and receive transfers into account<br/> • RECV: limits the action to receive transfers<br/> • SEND: limits the action to send transfers |
| <a href="../../../command_summary/parameter_intro/exec">EXEC</a>  | Name of the file containing the procedure to be executed.<br/> By default, this name is the one defined by the parameters:<br/> • EXEC of the SEND/RECV command (according to the transfer direction),<br/> • or (if this parameter is not defined) EXECSF or EXECRF of CFTPARM (according to the transfer direction). |
| <a href="../../../command_summary/parameter_intro/ida">IDA</a>  | Local identifier of the transfer assigned by the user or user application. |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a>  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. There is no default value. |
| <a href="../../../command_summary/parameter_intro/idu">IDT</a>  | Transfer identifier.<br/> This identifies a transfer for a given partner. The value ‘*****’ means that no selection is required using the IDT parameter (default value). |
| <a href="../../../command_summary/parameter_intro/idtu">IDTU</a>  | Transfer local counter identifier. |
| <a href="../../../command_summary/parameter_intro/part">PART</a> <br/> (Mandatory) | Partner identifier.<br/> The value of this parameter can be:<br/> • *Identifier*: the command only concerns the transfers with this partner<br/> • *Mask*: the command concerns the transfers with the partners, whose identifiers correspond to this mask |
| <a href="">PHASE</a>  | Phase of a catalog entry.  |
| <a href="">PHASTESTEP</a>  | Phase step of a catalog entry.  |
| <a href="../../../command_summary/parameter_intro/scope">SCOPE</a>  | Scope &lt;PARENT&gt; ('PARENT','ALL','CHILDREN').  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>  | Transfer request state.  |


#### Example 1 - Single Transfer

```
SUBMIT
 IDT = A1020301,
 IDF = file1,
 IDA = appli1,
 DIRECT = SEND,
 EXEC = myprog
```

The procedure defined in the file "myprog" is
submitted.

This procedure inherits the values of the parameters of
the transfer selected by the SUBMIT command. This provides the possibility
of substituting the symbolic variables which may have been used in the
procedure submitted.

#### Example 2 - Broadcast list

```
CFTDEST
 IDT = list,
 PART = (part1, part2, part3)
```

```
CFTPART
 ID = part1, ....
```

```
CFTPART
 ID = part2, ....
```

 

```
CFTPART
 ID = part3, ....
```

 

```
CFTSEND
 PART = list,
 IDF = myfile,
 EXEC = myprog
```

The procedure myprog is executed ONCE when all the transfers
are completed.

Before the post-processing procedure is executed, the catalog
looks like this.


| STATE  | PART  | IDF  |
| --- | --- | --- |
| SFT  | LIST  | MYFILE (generic transfer)  |
| SFT  | PART1  | MYFILE  |
| SFT  | PART2  | MYFILE  |
| SFT  | PART3  | MYFILE  |


The post-processing procedure can be submitted again as
follows:

```
SUBMIT
PART = list,
IDF = myfile,
```

This relates to the generic transfer. A SUBMIT command
applied to a transfer in the list (SUBMIT PART = PART1 for example) has no effect.
