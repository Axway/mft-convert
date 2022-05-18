---
title: "ruser"
linkTitle: "ruser"
weight: 3040
--- <span id="ruser"></span>

### ruser

<span id="ruser_CFTRECV"></span><span id="ruser_CFTSEND"></span>

#### CFTRECV, CFTSEND, RECV, SEND, DISPLAY, LISTCAT

**[RUSER = *string* 32 ]** 

*PeSIT E only*

Identifier of the user who is receiving the file.

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

The server/sender
partner sends and controls this parameter, where:

| PeSIT E standard | In standard PeSIT E, the RUSER parameter value is transported in the PI 04, but its length is limited to 8- characters. Therefore, the PI 04 contains the concatenated value along with the value of the RAPPL parameter. |
| --- | --- |
| **<br /> **PeSIT E CFT/CFT | In PeSIT E between 2 {{< TransferCFT/axwayvariablesComponentShortName  >}}s, the RUSER parameter value is transported in the PI 99 if this value exceeds 8 characters. |

> **Note**
>
> You can use the RUSER and SUSER fields when performing a catalog search. Additionally these fields are integrated into the catalog index. See SUSER.

See PeSIT extension PI codes for information on PI codes.

[Return to Command index](../../)
