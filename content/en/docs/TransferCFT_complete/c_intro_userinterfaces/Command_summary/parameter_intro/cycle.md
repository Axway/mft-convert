{
    "title": "cycle",
    "linkTitle": "cycle",
    "weight": "580"
}<span id="cycle"></span>

### cycle

#### CFTSEND, SEND, CFTRECV, RECV

****CYCLE = {0 &#124; n}    {0..32767}****

Number of units (per [tcycle](../tcycle)) for the transfer cycle period.

Use this field to define the transfer activation cycle. Enter the period
of time to wait before activating the next transfer. This period can be
expressed in minutes, days, or months as defined in the [tcycle](../tcycle) field.

Enter one of the following values:

- 0
    (default value)
- any other value
    from 0 to 32767

#### CFTPROT

****[CYCLE = {10 &#124; n}]   {1..32767}****

The cycle parameter forms part of the {{< TransferCFT/axwayvariablesComponentShortName  >}} definition. This
parameter is only available for PeSIT DMZ. Use this parameter to set the
periodicity for creating a protocol session.

#### TURN

****[CYCLE = {10 &#124; n}]   {1..32767}****

[Return to Command index](../../)
