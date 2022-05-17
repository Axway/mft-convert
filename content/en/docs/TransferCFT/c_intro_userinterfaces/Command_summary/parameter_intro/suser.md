---
    title: "suser"
    linkTitle: "suser"
    weight: 3440
---<span id="suser"></span>

### suser

<span id="suser_CFTSEND"></span>

#### CFTRECV, CFTSEND, SEND, RECV, DISPLAY, LISTCAT

**[SUSER = *string* 32 ]**

*PeSIT E only*

Identifier for the user who is sending the file.

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

The server/sender
partner sends and controls this parameter, where:


| PeSIT E standard | In standard PeSIT E, the SUSER parameter is transported in the PI 03, and its length is limited to 8-characters. Therefore, the PI 03 contains the concatenated value along with the value of the SAPPL parameter. |
| --- | --- |
| **<br /> **PeSIT E CFT/CFT | In PeSIT E between 2 {{< TransferCFT/axwayvariablesComponentShortName  >}}s, the SUSER parameter value is transported in the PI 99 if this value is longer than 8 characters. |


> **Note**
>
> You can use the RUSER and SUSER fields when performing a catalog search. Additionally these fields are integrated into the catalog index. See RUSER.

See PeSIT extension PI codes for information on PI codes.

[Return to Command index](../../)
