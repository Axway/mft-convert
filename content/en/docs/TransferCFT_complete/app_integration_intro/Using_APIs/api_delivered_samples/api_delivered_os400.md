{
    "title": "Delivered templates for IBM i (iSeries)",
    "linkTitle": "Delivered templates for IBM i (iSeries)",
    "weight": "350"
}This topic describes the {{< TransferCFT/axwayvariablesCompanyName  >}} {{< TransferCFT/axwayvariablesComponentShortName  >}} API templates and CL programs  for the IBM i (iseries) platform. You may decide to use the delivered samples as a basis for integrating APIs, or as a model to create your own.

The Transfer CFT IBM i templates are located in the production library (CFTPROD by default), in the CFTSRC folder.

<span id="COBOL"></span>

COBOL language (CBLLE file extension)
-------------------------------------

In the **CL program** column, click the program link for information on how to compile and execute.


| CL program  | Associated COBOL  | Description  |
| --- | --- | --- |
| I_TCFTU_CBL  | TCFTU_CBL1  | Synchronous API.  |
| I_INTCAT  | INTCAT  | Same results as CFTUTIL LISTCAT, with some filtering allowed.  |
| I_ACCNTPGM  | ACCNTPGM  | Statistics based on the V23 ACCNT file.  |
| I_ACCNT24  | ACCNTPGM24  | Statistics based on the V24 ACCNT file.  |


<span id="RPG"></span>

RPG language (.RPGLE file extension)
------------------------------------

In the **Template** column, click the template link to view the sample template as a text file.


| Template  | Function  | Services | Description  |
| --- | --- | --- | --- |
| [TCFTI_RPG]()  | CFTI  | OPEN - CLOSE - NEXT - SELECT- MODIF  | Using CFTI function (CFTAPI V23 structure )  |
| [TCFTI2_RPG]()  | cftaix  | OPEN - CLOSE - SELECT240 - NEXT240 - SORT - DO -  | Using CFTIX function (CFTAPI V24 structure )  |
| [TCFTU_RP1]()  | CFTU  | SEND - DELETE  | Tests the CFTU, CFTC functions (asynchronous API)  |
| [TCFTU_RP1N]()  | CFTU  | SEND  | Using PLIST for receiving the SEND command  |
| [TCFTU_RP2]()  | CFTU  | COM, SEND, GETXINFO, CLOSEAPI  | Tests the CFTU function (synchronous API)  |


How to compile and execute a COBOL sample
-----------------------------------------

<span id="TCFTU"></span>

### I_TCFTU_CBL program

```
CRTCBLMOD MODULE(< CFTPGM
>/TCFTU_CBL1) SRCFILE(< CFTPGM
>/CFTSRC)
SRCMBR(TCFTU_CBL1) DBGVIEW(\*ALL) REPLACE(\*YES)
 
CRTPGM PGM(< CFTPGM
>/TCFTU_CBL1) MODULE(<****CFTPGM
>/TCFTU_CBL1)
BNDSRVPGM(<CFTPGM>/LIBAPISRV1) REPLACE(\*YES)
 
CRTBNDCL PGM(< CFTPGM
>/I_TCFTU_CB) SRCFILE(< CFTPGM
>/CFTSRC)
SRCMBR(I_TCFTU_CB) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(< CFTPGM
>/I_TCFTU_CB)
```
<span id="INTCAT"></span>****

### I_INTCAT program

```
CRTCBLMOD MODULE(< CFTPGM
>/INTCAT) SRCFILE(< CFTPGM
>/CFTSRC)
SRCMBR(INTCAT) DBGVIEW(\*ALL) REPLACE(\*YES)
 
CRTPGM PGM(< CFTPGM
>/INTCAT) MODULE(< CFTPGM
>/INTCAT)
BNDSRVPGM(< CFTPGM
>/LIBAPISRV1) REPLACE(\*YES)
 
CRTBNDCL PGM(< CFTPGM
>/I_INTCAT) SRCFILE(< CFTPGM
>/CFTSRC)
SRCMBR(I_INTCAT) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(< CFTPGM
>/I_INTCAT)
```
<span id="ACCNTPGM"></span>

### I_ACCNTPGM program for ACCNT V23

```
CRTBNDCBL PGM(< CFTPGM
>/ACCNTPGM) SRCFILE(< CFTPGM
>/CFTSRC) SRCMBR(ACCNTPGM) OPTION(\*EVENTF) DBGVIEW(\*SOURCE) REPLACE(\*YES)
 
CRTBNDCL PGM(< CFTPGM
>/I_ACCNTPGM) SRCFILE(< CFTPGM
>/CFTSRC) SRCMBR(I_ACCNTPGM) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(<CFTPGM>/I_ACCNTPGM)
```
<span id="ACCNT24"></span>

### I_ACCNT24 program for ACCNT V24

```
CRTBNDCBL PGM(< CFTPGM
>/ACCNTPGM24) SRCFILE(< CFTPGM
>/CFTSRC) SRCMBR(ACCNTPGM24) OPTION(\*EVENTF) DBGVIEW(\*SOURCE) REPLACE(\*YES)
 
CRTBNDCL PGM(< CFTPGM
>/I_ACCNT24) SRCFILE(< CFTPGM
>/CFTSRC) SRCMBR(I_ACCNTPGM) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(< CFTPGM
>/I_ACCNT24)
```
