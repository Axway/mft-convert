---

    title: opermsg
    linkTitle: opermsg
    weight: 2470

---
<span id="opermsg"></span>

### opermsg

<span id="opermsg_CFTRECV"></span><span id="opermsg_CFTLOG"></span>

#### CFTRECV, CFTSEND, CFTLOG

**\[OPERMSG = {<u>see table</u> | n}\]
 **{0..255}

Defines the categories of messages that are intended for the operator. All
messages are also written in the log file. This is a subset of the {{< TransferCFT/axwayvariablesComponentShortName  >}} log messages
defined by the algebraic sum of the values indicated in the following
table.


| Value  | Message category  | Type  |
| --- | --- | --- |
| 1  | Operating information messages  | I  |
| 2  | System information messages  | I  |
| 4  | Operating warning messages  | W  |
| 8  | System warning messages  | W  |
| 16  | Operating error messages  | E  |
| 32  | System error messages  | E  |
| 64  | Operating fatal error messages  | F  |
| 128  | System fatal error messages  | F  |


The I, W, E, F types correspond to the type of message indicated in
the log file. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} <a href="../../../../troubleshoot_intro/messages_and_error_codes_start_here" class="MCXref xref">Transfer CFT messages
and error codes</a> section.

The value "0" means that no message is sent to the operator.

The following table indicates, for each system, the default values of
the OPERMSG parameter.


| OS  | Default value for OPERMSG  |
| --- | --- |
| MVS (z/OS) | 0  |
| OS400  | 0  |
| UNIX  | 0 |
| Windows | 0 |


[Return to Command index](../../)
