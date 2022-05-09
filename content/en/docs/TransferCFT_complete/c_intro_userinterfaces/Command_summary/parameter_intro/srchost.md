{
    "title": "srchost",
    "linkTitle": "srchost",
    "weight": "3290"
}<span id="srchost"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTNET

****[ SRCHOST = { hostname1 &#124; 64 } ]****

This parameter is used only for outgoing calls on a resource. If
the value, either the local hostname or IP address, is assigned then that value is used
to select the interface on which the outgoing call will occur.

****Example****:

CFTPROT ID = PANY, SAP
= 1761....NET=ANY

CFTNET ID=ANY,
HOST = INADDR_ANY, SRCHOST= ****my.address.net****

Where ****my.address.net****
is used for the outgoing call.

 

[Return to Command index](../../)
