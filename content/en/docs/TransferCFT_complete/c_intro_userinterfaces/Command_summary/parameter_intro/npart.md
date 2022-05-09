{
    "title": "npart",
    "linkTitle": "npart",
    "weight": "2300"
}<span id="npart"></span>

### npart

#### LISTCAT

****[NPART = { identifier &#124; mask }]****

Network name of the transfer partner. The value of this parameter may
be:

- An
    identifier: the selection relates to the transfers performed with this
    network name
- A mask:
    the selection is generic and relates to the transfers performed with the
    network identifier whose name corresponds to this mask
- Omitted:
    the selection is made according to the criteria indicated in the PART
    parameter

#### CFTPARM

**[NPART = { <span class="underline">PART</span> <span class="underline">value</span>&#124; string }]**

The default network identifier for the local site.

- Default value for the CFTPART command's NSPART parameter.
- Check for OS specificity for the size and format as this name is sent by some file transfer protocols.

#### DISPLAY

**[NPART = {string }]**

<span id="npart_CFTPART"></span>

#### CFTPART

**[NPART = {<span class="underline">ID value</span> &#124; string 64}] **

Network identifier for the partner(s) for the selected transfers.

This value can be:

- An identifier: Relates to the transfers performed with this network name.
- A mask: Generically relates to the transfers performed with the network identifier whose name corresponds to this mask.
- Omitted: The selection is made according to the criteria defined in the PART parameter.

The remote partner must this name to identify itself to the local Transfer
CFT, during the connection phase. At the remote site, this value
corresponds to the NSPART parameter of the CFTPART object used in the
transfer.

[Return to Command index](../../)
