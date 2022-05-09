{
    "title": "ctrlpart",
    "linkTitle": "ctrlpart",
    "weight": "520"
}<span id="ctrlpart"></span>

### ctrlpart

#### CFTPART

****[ CTRLPART = { IGNORE
&#124; ALL &#124; RPART &#124; SPART } ]****

Optional parameter that is relevant only in server mode. When a transfer is initiated by a remote partner, {{< TransferCFT/axwayvariablesComponentShortName  >}} always controls its identity. If the remote sender is not the initial sender, store and forward mode, the {{< TransferCFT/axwayvariablesComponentShortName  >}} can choose to control the initial sender identity according to CTRLPART parameter.

- IGNORE (default): No control of the remote sender identity.
- ALL: Control of the remote sender identity. If the partner is unknown, the transfer is rejected with message
- RPART
- SPART

CFTT15E NPART=&part _ Not found

 

[Return to Command index](../../)
