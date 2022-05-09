{
    "title": "notify",
    "linkTitle": "notify",
    "weight": "2260"
}<span id="notify"></span>

### notify

#### CFTLOG, CFTSEND, **<span id="notify_CFTRECV"></span>**CFTRECV, SEND, RECV

****NOTIFY = {string see
below}****

Defines the destination of the operator
messages selected according to the value of the OPERMSG parameter as follows:

- All
    log file messages if defined in the CFTLOG object
- For
    all transfer messages with the IDF if defined in CFTSEND or CFTRECV
- For
    all messages from a transfer that was requested with the command if used
    in SEND or RECV

The value of this parameter is a left aligned 8-character string.

The destination of these messages may be, according to the system:

- the
    {{< TransferCFT/axwayvariablesComponentShortName  >}} "submitter" corresponding to the standard
    output associated with the {{< TransferCFT/axwayvariablesComponentShortName  >}} (the submittal screen,
    for example)  
    The value of the NOTIFY parameter must be then be set to ‘ ’ (8 blank
    characters)
- an
    operator console  
    The NOTIFY parameter value must begin by the 2 characters OP

<!-- -->

- a computer
    user  
    The value of the NOTIFY parameter indicates the user system identifier
    according to the format "xxxxxxxx"

The following table indicates the possible recipients for each system
involved:

- YES indicates that the corresponding
    recipient type exists
- NO indicates the corresponding recipient
    type does not exist


| OS  | Monitor submitter  | Operator console  | Any user  |
| --- | --- | --- | --- |
| MVS (z/OS) | NO  | YES  | YES  |
| OS400 (IBM i) | YES  | YES  | YES  |
| UNIX  | YES  | YES  | YES  |
| VMS  | NO  | YES  | YES  |
| Windows | YES  | NO  | NO  |


The following table indicates, for each system, the default values of
the NOTIFY parameter supported. The value ‘ ’ corresponds to 7 blank characters.


| OS  | Default values for NOTIFY  |
| --- | --- |
| MVS (z/OS) | OP  |
| OS400  | ‘ ’  |
| UNIX  | ‘ ’  |
| VMS  | OP  |
| Windows | ‘ ’  |


In the operator console, the possible choices are indicated in
the following table. If ‘OP’ is indicated for the interpreted characters,
only these two characters (OP) are interpreted; the following characters
are not significant.


| Operator console OS | Characters interpreted  | Messages sent to... |
| --- | --- | --- |
| MVS (z/OS) | OP  | Operator console(s)  |
| OS400  | OP  | QSYSOPR &quot;message-queue&quot;  |
| UNIX  | OP  | Operator console  |
| VMS  | Opxxxxxx  | System console and output peripheral system LOG file identified by the &quot;xxxxxx&quot; link present in the current monitor execution directory  |
| Windows | Not applicable  |   |


For the user:


| User OS  | Messages are...  |
| --- | --- |
| MVS (z/OS) | Sent by SEND to the specified TSO USERID; in this case, the {{< TransferCFT/axwayvariablesComponentShortName  >}} program must be authorized (APF). |
| VMS | Sent to the &quot;VMS User &quot; designated by its VMS name. In this case, the {{< TransferCFT/axwayvariablesComponentShortName  >}} task must have the OPER privilege. |


[Return to Command index](../../)
