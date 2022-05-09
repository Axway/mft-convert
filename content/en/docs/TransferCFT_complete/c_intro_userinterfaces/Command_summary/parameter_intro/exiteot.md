{
    "title": "exiteot",
    "linkTitle": "exiteot",
    "weight": "1010"
}<span id="exiteot"></span>

### exiteot

#### CFTPARM

**[EXITEOT = *identifier*]**

EXIT identifier of 32 characters.

To activate an end of transfer EXIT task, the EXIT identifier that you
use must point to a CFTEXIT object.

You can use a symbolic variable as the identifier of an end of transfer
EXIT:

- EXITEOT = (&NPART,
    &PART, &IDF &GROUP or &IDA), where NPART is the network
    name of the remote partner (only the first eight characters are taken
    into account).
- PART is the local
    partner identifier
- IDF is the file
    or message identifier
- GROUP designates
    the partner group
- IDA is the local
    application identifier

[Return to Command index](../../)
