{
    "title": "CFTPART",
    "linkTitle": "nrpart",
    "weight": "2310"
}<span id="nrpart"></span>

### CFTPART

**[NRPART = {<span class="underline">value of the NPART
parameter of CFTPARM</span> &#124; *string*}]**

- string24     PeSIT  
- string25     ODETTE
- string64     SFTP  

Network identifier by which the
remote partner identifies itself to the local {{< TransferCFT/axwayvariablesComponentLongName  >}} (for incoming calls).

The local partner must retrieve the CFTPART description such that the
associated NSPART parameter corresponds to this value.

The partner must use this name to identify itself to the {{< TransferCFT/suitevariablesTransferCFTName  >}} during the connection phase. On the partner side, this value corresponds to the NSPART parameter of the CFTPART command that describes the local Transfer CFT.

**ODETTE: **In server mode, there
may be several CFTPART objects with the same NRPART. For this purpose,
IMINTIME = IMAXTIME should be specified.

 

 

[Return to Command index](../../)
