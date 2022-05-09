{
    "title": "tlvcexec",
    "linkTitle": "tlvcexec",
    "weight": "3550"
}<span id="tlvcexec"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTCAT

****[ TLVCEXEC = { str1...64
} ]****

Batch to execute when the alert ends. The TLV parameters enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to
issue alerts when a critical CAT fill threshold is reached based on a percentage of the catalog being filled, where 0% indicates empty and 100% indicates full.

This
alert generates 2 actions:

- Sends a message
    to the LOG
- Executes
    a batch to react to the alert, the [TLVWEXEC](#)
    parameter

{{% TransferCFT/snippets/exec_cmd%}}

#### CFTCOM FILE

****[ TLVCEXEC = { str1...512
} ]****

Batch to execute when the alert ends. The TLV parameters enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to
issue alerts when a critical COM fill threshold is reached based on a percentage of the catalog being filled, where 0% indicates empty and 100% indicates full.

This
alert generates 2 actions:

- Sends a message
    to the LOG
- Executes
    a batch to react to the alert, the [TLVWEXEC](#)
    parameter

{{% TransferCFT/snippets/exec_cmd%}}

For CFTCOM, the symbolic variables that you can use in the batch are restricted to: &SYSDATE, &SYSTIME, &CFTEVENT, &SYSDAY, &CFTNAME, &RUNTIMEDIR.

[Return to Command index](../../)
