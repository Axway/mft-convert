{
    "title": "rkerror",
    "linkTitle": "rkerror",
    "weight": "2950"
}<span id="rkerror"></span>

### rkerror

#### CFTCAT, CFTRECV

**[RKERROR = {<span class="underline">KEEP</span>** &#124; **DELETE}]**

**A**ction to be taken if a transfer
aborts due to the receiving file creation error (server mode):

- ****KEEP****:
    the transfer remains in the catalog
- ****DELETE****:
    the transfer is removed from the catalog

If the RKERROR parameter is also set in the CFTRECV command, the CFTRECV
command takes precedence.

 

[Return to Command index](../../)
