{
    "title": "tlvwrate",
    "linkTitle": "tlvwrate",
    "weight": "3580"
}<span id="tlvwrate"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTCAT, CFTCOM FILE

****[ TLVWRATE = { 0...65535
} ]****

The minimum amount of time, in seconds, to wait before resending an alert. When an alert is issued, the wait time is TLVWRATE, in seconds, before resending the alert.

This
alert generates 2 actions:

- Sends a message
    to the LOG
- Executes
    a batch to react to the alert, the [TLVWEXEC](../tlvcexec)
    parameter

This value is in seconds, where:

- Default is 60
- 0 means that only
    the first alert is issued

[Return to Command index](../../)
