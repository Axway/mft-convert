{
    "title": "maxtrans",
    "linkTitle": "maxtrans",
    "weight": "1960"
}<span id="maxtrans"></span>

### maxtrans

#### CFTPARM

****[MAXTRANS = { <span class="underline">256</span> &#124; n} ]****

The maximum authorized number of transfers in parallel. When using multi node, this is the number of transfers per node.

- 1 to **1000** for all operating systems (256 default)

This value sets the physical limit for the product independently of
the limit set by the software key.

The UCONF` cft.server.maxtrans` parameter overrides MAXTRANS as long as it is not set to 0. If `cft.server.maxtrans `is set to 0, the MAXTRANS value is used.

[Return to Command index](../../)
