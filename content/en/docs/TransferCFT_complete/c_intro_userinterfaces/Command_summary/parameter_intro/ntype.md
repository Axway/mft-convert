{
    "title": "ntype",
    "linkTitle": "ntype",
    "weight": "2380"
}<span id="ntype"></span>

### ntype

#### CFTSEND, SEND

**[NTYPE = {<span class="underline">see comment</span> &#124; c}]
       ODETTE,
PeSIT D CFT profile, PeSIT E CFT/CFT,  OS**

File type, in protocol terms.

This parameter is used to designate the partner receiver file type.


| **PeSIT D CFT profile<br /> PeSIT E CFT/CFT** | *Default value*<br/> If this parameter is not defined, Transfer CFT assigns a default value, as a function of the:<br/> • local file type (FTYPE parameter),<br/> • operating system of the transfer recipient (SYST parameter of CFTPART).<br/> The default values are indicated in *NTYPE sent by default*. |
| --- | --- |
| **ODETTE** | The specific value NTYPE = T may be used to request the sending of a file in ODETTE text format. Refer to Managing Protocols. |


 

[Return to Command index](../../)
