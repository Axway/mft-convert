{
    "title": "opermsg",
    "linkTitle": "opermsg",
    "weight": "2490"
}<span id="opermsg"></span>

### opermsg

<span id="opermsg_CFTRECV"></span><span id="opermsg_CFTLOG"></span>

#### CFTRECV, CFTSEND, CFTLOG

**[OPERMSG = { <span class="underline">0</span> &#124; n}]
 **{0..255}

Defines the categories of messages that are intended for the operator. All
messages are also written in the log file. This is a subset of the {{< TransferCFT/axwayvariablesComponentShortName  >}} log messages
defined by the algebraic sum of the values indicated in the following
table, where:

- Operating: Includes all messages related to transfers and CRON jobs.
- System: Includes all internal messages, such as Transfer CFT start, stop, catalog, tasks, multi-node, and so on.


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


The I, W, E, F types correspond to the type of message in
the log file. See {{< TransferCFT/axwayvariablesComponentShortName  >}} [Transfer CFT messages and error codes](../../../../troubleshoot_intro/messages_and_error_codes_start_here) for more information.

The value "0", which is the default, means that no message is sent to the operator.

**Example**

```
CFTLOG    ID = IDLOG,
          FNAME = $cftlog,
         NOTIFY=<user_name>,
          OPERMSG = 240
```

The messages indicated by the OPERMSG parameter are sent to the &lt;user_name&gt; defined in NOTIFY. This OPERMSG value is an algebraic sum of the various types of selected messages. In this example, you calculate System fatal  (128), Operating fatal  (64), System  (32), plus Operating error messages (16). The sum in this example is 240 (128+64+32+16).

[Return to Command index](../../)
