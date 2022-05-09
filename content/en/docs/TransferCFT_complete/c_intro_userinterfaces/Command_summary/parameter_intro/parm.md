{
    "title": "parm",
    "linkTitle": "parm",
    "weight": "2550"
}<span id="parm"></span>

### parm

#### CFTEXIT

**[PARM = {string64}]**

Use this field to pass information from the monitor to the user EXIT
programs. You can enter up to 64 alphanumeric characters of free text.

#### CFTSEND, SEND, RECV

**[PARM = {string512}]** **    PeSIT**

User message sent to the partner
with the file transfer.

The value of this parameter can be accessed through the symbolic variable
**&PARM**.

Important: In PESIT D CFT, a maximum of only 80 characters can be transmitted to the receiver.

See the [xlate](../xlate) parameter for transcoding details.

#### CFTNET, TYPE = TCP

**[PARM = { string 512}]**

#### CFTSSL

**[PARM = {string64}]**

Free-form parameter associated with the security profile.

This local data item is not used by the SSL protocol. It
can be reused as a symbolic variable in an end of transfer procedure (&SSLPARM).
It is also passed to the end of transfer, directory or file exits.

[Return to Command index](../../)
