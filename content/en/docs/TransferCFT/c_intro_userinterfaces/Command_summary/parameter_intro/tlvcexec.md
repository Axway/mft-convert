---
title: "tlvcexec"
linkTitle: "tlvcexec"
weight: 3550
--- <span id="tlvcexec"></span>

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

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

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

> **Note**
>
> To use direct script execution instead of the template script processing, preface the &lt;EXEC>value with 'cmd:'. For example, &lt;EXEC>='cmd:myscript.sh &PART &IDT &IDTU'. See Directly processing a program or script for details, examples, restrictions, and support.

For CFTCOM, the symbolic variables that you can use in the batch are restricted to: &SYSDATE, &SYSTIME, &CFTEVENT, &SYSDAY, &CFTNAME, &RUNTIMEDIR.

[Return to Command index](../../)
