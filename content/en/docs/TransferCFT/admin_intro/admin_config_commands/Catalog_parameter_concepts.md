{
    "title": "Catalog attributes ",
    "linkTitle": "CFTCAT - Catalog attributes ",
    "weight": "230"
}****Related
topics****

- Command syntax
    [CFTCAT](../../../c_intro_userinterfaces/command_summary#CFTCAT)
- Parameter list
    [CFTCAT](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftcat)

<span id="About_the_CFTCAT_object"></span>

### About the CFTCAT object

The {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog displays all {{< TransferCFT/axwayvariablesComponentShortName  >}} transfers in list
form. The catalog file contains control data that is associated with transfers.
A record, that corresponds with a status indicator, exists for each transfer.

Each day the catalog fills with records. The catalog parameters can
be defined to delete records that exceed a certain limit, or to purge
the catalog at a preset time. The catalog purge can be initiated at midnight (default value) or at
a specific time. The catalog is purged in batches of ten entries. Between
each batch, Transfer CFT handles all pending transfer events. As a result,
if the transfer activity is intense, the catalog purge may be extended
but does not disrupt transfers.

Catalog functions include a method to recover information about the
CFT that is using the catalog.

For each ****PARM****, there is one
associated CFTCAT.

### Catalog levels

Catalog levels enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to issue alerts
when a critical fill threshold, the [TLVWARN](../../../c_intro_userinterfaces/command_summary/parameter_intro/tlvwarn)
parameter Threshold Limit Value Warning, for the catalog file is reached.
This alert triggers:

- Sending
    a message to the LOG and to Sentinel
- Executing
    a batch to react to the alert (TLVWEXEC parameter)

Once an alert is issued, these two actions are repeated
each time a Catalog event occurs, but not more than once every TLVWRATE
seconds. This alert ends when the fill level drops below the TLVCLEAR
level.
