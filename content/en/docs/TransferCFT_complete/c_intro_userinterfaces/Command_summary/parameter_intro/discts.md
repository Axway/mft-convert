{
    "title": "discts",
    "linkTitle": "discts",
    "weight": "740"
}<span id="discts"></span>

### discts

#### CFTPROT

****[DISCTS =
{ <span class="underline">see the table</span> &#124; n} ] {0..3600}  ****

The synchronous connection time as managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}. If {{< TransferCFT/axwayvariablesComponentShortName  >}} does not receive requests to communicate in discts (the number of seconds), {{< TransferCFT/axwayvariablesComponentShortName  >}} closes the session.

The session established for a transfer remains active for DISCTS seconds
after the completion of this transfer. If at the end of this timeout,
no new transfer has been received, the connection is freed by the ABORT
FPDU.

If the value is 0, the wait time-out is infinite.

The default value (in seconds) depends on the protocol and is indicated
in the following table.

Default values are:


| Protocol  | Default value  |
| --- | --- |
| PeSIT ANY profile | 60 |
| ODETTE  | 65  |


[Return to Command index](../../)
