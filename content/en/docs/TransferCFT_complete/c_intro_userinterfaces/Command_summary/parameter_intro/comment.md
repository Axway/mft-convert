{
    "title": "comment",
    "linkTitle": "comment",
    "weight": "450"
}<span id="Comment"></span>

### comment

#### ABOUT, SEND, RECV, CFTPART

**[COMMENT =
*string80 characters*]**

Free comment.

This comment is displayed and can be used to indicate a specific item
of information (e.g. customer name, etc.).

**ODETTE**

Set up for EERP.  
The ODETTE protocol has two distinct processing methods for the EERP: mode
86       
(GALIA) and mode 91 (ODETTE work group).  
The value of the COMMENT field allows the identification of the mode:  

\- mode 86 : COMMENT = ‘EERP=86’  
- mode 91 : COMMENT = ‘EERP=91’  
If this parameter is not defined, the explicit value is set in the protocol definition (CFTPROT).

#### CFTSEND, CFTRECV, SEND, RECV

**[COMMENT = *string160*]**

Enter a local free-text description for the receive transfer. You can
enter up to 160 alphanumeric characters. The content of this field:

- is
    not analyzed or used during transfers
- can
    be queried locally using the command LISTCAT CONTENT=FULL
- is
    restored using the symbolic variable &COMMENT

#### CFTACCNT, CFTEXIT, CFTIDF, CFTLOG, CFTAUTH, CDTCAT, CFTCOM, CFTDEST, CFTETB

**[COMMENT =
*string80 characters*]**

#### CFTXLATE, CFTPARM, CFTPART, CFTAPPL, CFTPROT, CFTTCP

**[COMMENT =
*string80 characters*]**

[Return to Command index](../../)
