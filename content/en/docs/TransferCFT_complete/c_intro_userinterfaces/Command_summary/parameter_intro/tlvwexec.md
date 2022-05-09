{
    "title": "tlvwexec",
    "linkTitle": "tlvwexec",
    "weight": "3570"
}<span id="tlvwexec"></span>

### {{< TransferCFT/SystemTitle  >}}

The symbolic variables that you can use in a script are restricted to: &SYSDATE, &SYSTIME, &CFTEVENT, &SYSDAY, &CFTNAME, &RUNTIMEDIR (and &FCAT for CFTCAT).

#### CFTCAT

****[ TLVWEXEC = { str1...64}
]****

Batch to execute when the TLVWARN is reached. The TLV parameters enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to
issue alerts when a critical CAT threshold is reached based on a percentage of the catalog being filled, where 0% indicates empty and 100% indicates full.

This
alert generates 2 actions:

- Sends a message
    to the LOG
- Executes a batch
    to react to the alert (when CFTCAT/[TLVWARN](../tlvwarn) is reached)

{{% TransferCFT/snippets/exec_cmd%}}

#### CFTCOM FILE

****[ TLVWEXEC = { str1...512 }
]****

Batch to execute when the TLVWARN is reached. The TLV parameters enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to
issue alerts when a critical COM threshold is reached based on a percentage of the communication media being filled, where 0% indicates empty and 100% indicates full.

This
alert generates 2 actions:

- Sends a message
    to the LOG
- Executes a batch
    to react to the alert (when CFTCOM/[TLVWARN](../tlvwarn) is reached)

{{% TransferCFT/snippets/exec_cmd%}}

[Return to Command index](../../)
