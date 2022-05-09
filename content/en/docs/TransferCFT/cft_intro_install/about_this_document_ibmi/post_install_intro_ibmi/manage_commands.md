{
    "title": "Manage commands",
    "linkTitle": "Manage commands",
    "weight": "180"
}This section describes all of the command available to manage your Transfer CFT product.

Standard commands
-----------------


| Command | Comment |
| --- | --- |
| CFTSTART | Start Transfer CFT |
| CFTSTOP | Stop Transfer CFT |
| COPSTART | Start the UI server |
| COPSTOP | Stop the UI server |
| CFTMN | This is the procedure to manage Transfer CFT and to configure multi-node. Add the following action(s) to manage your product:<br/> • START<br/> • STOP<br/> • RESTART<br/> • ADD_NODE<br/> • REMOVE_NODE<br/> • ADD_HOST<br/> • REMOVE_HOST<br/> • REMOVE_NODE<br/> • ENABLE_NODE<br/> • DISABLE_NODE<br/> <blockquote> **Note**<br/> CFTMN is the equivalent of cft script for UNIX or Windows.<br/> </blockquote>  |


Deprecated commands
-------------------


| Replace this command...  | With the new command...  |
| --- | --- |
| SHUT | CFTSTOP + COPSTOP |
| CFTMGSBM | CFTSTART |
| STARTCOPB | COPSTART |
| COPSMNG | COPSTART |
| COPSTOPM | COPSTOP |
| STOPCOPL | COPSTOP |
| BACKGROUND_C | BACKGROUND |
| SNDCFTF  | CFTUTIL (See Example 1)  |
| SNDCFTSPLF  | CFTUTIL (See Example 2)  |
| MAJSECINI  | No replacement  |
| MAJSECENVG  | No replacement  |
| GENEDICT  | No replacement  |
| CPYFDBVAR  | No replacement  |
| FORMATCONF  | No replacement  |
| XFBOVRDBF  | No replacement  |
| CFTMG_ALC  | No replacement  |
| OVRDBF_ALL  | No replacement  |
| CFTTCOM_@  | No replacement  |
| FFT10412P, FFT1043P, UFT10412P and UFT1043P  | No replacement  |
| CFTINS301  | No replacement  |
| SI_PGM_BT2  | No replacement  |
| T_QXXCHGDT  | No replacement  |


**Example 1**

Send a member:

```
CFTUTIL send PART=<PART>, IDF=<IDF>, FNAME=&LIB/&FILE(&MBR)
```

**Example 2**

Send a spool file:

```
CFTUTIL send PART=<PART>, IDF=<IDF>, FNAME=&FILE/&SPLNBR/&WORK/&JOBNBR
```
