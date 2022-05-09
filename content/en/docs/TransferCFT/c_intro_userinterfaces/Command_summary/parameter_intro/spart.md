{
    "title": "spart",
    "linkTitle": "spart",
    "weight": "3270"
}<span id="spart"></span>

### spart

#### CFTSEND, SEND

**[SPART = *string64*}]**

Network designation by which the local {{< TransferCFT/axwayvariablesComponentShortName  >}} identifies
itself to its partner.

If the NSPART parameter of the CFTPART OBJECT associated with SEND is
not defined, the SPART value is taken into account.

****In requester mode SPART = string64.****

The value of this parameter is usually equal to the identifier (ID parameter
of the CFTPART object). In this case the NSPART parameter that is sent
is the one defined in the CFTPART object. As such, the NSPART that is
sent may exceed 8 characters.

If the CFTPART object does not exist, the NSPART default parameter sent
equals the value of the SPART parameter and therefore is not more than
8 characters.

This mechanism is used in particular on the intermediate site providing
VAN store and forward: when the SEND command is activated for a transfer
to the destination site, this SPART parameter is in fact used to set the
NSPART parameter being sent, to the value of the INITIAL sender of the
file.

[Return to Command index](../../)
