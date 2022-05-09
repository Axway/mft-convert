{
    "title": "tlvwarn",
    "linkTitle": "tlvwarn",
    "weight": "3560"
}<span id="tlvwarn"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTCAT, CFTCOM=FILE

****[ TLVWARN = { 0...100 }
]****

Command file usage limit before issuing an alert. Overall, the TLV parameters enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to
issue alerts when a critical CAT or COM threshold is reached based on a percentage of the catalog or communication media being filled, where 0% indicates empty and 100% indicates full.

This
alert generates 2 actions:·

- Sends a message
    to the LOG
- Executes
    a batch to react to the alert, the [TLVWEXEC](../tlvcexec)
    parameter

Once an alert has been issued, steps are
repeated when an action on catalog or communication media is done, but
do not occur more than every [TLVWRATE](../tlvwrate)
seconds. The alert ends when the fill level drops below the [TLVCLEAR](../tlvclear) level.

The catalog or communication media usage limit before issuing an alert is expressed as a percentage, for example the value 80 indicates 80%.
When this limit is reached, the [TLVWEXEC](../tlvwexec)
is executed.

- CFTCAT: Default 80
- CFTCOM FILE: Default 70
- 0 means that no
    alert is issued

<!-- -->
