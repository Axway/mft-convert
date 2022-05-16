---

    title: Delivered templates for IBM i (iSeries)
    linkTitle: Delivered templates for IBM i (iSeries)
    weight: 350

---
This topic describes the {{< TransferCFT/axwayvariablesCompanyName  >}} {{< TransferCFT/axwayvariablesComponentShortName  >}} API templates and CL programs  for the IBM i (iseries) platform. You may decide to use the delivered samples as a basis for integrating APIs, or as a model to create your own.

The Transfer CFT IBM i templates are located in the production library (CFTPROD by default), in the CFTSRC folder.

<span id="COBOL"></span>

## COBOL language (CBLLE file extension)

In the **CL program** column, click the program link for information on how to compile and execute.


| CL program  | Associated COBOL  | Description  |
| --- | --- | --- |
| I_TCFTU_CBL  | TCFTU_CBL1  | Synchronous API.  |
| I_INTCAT  | INTCAT  | Same results as CFTUTIL LISTCAT, with some filtering allowed.  |
| I_ACCNTPGM  | ACCNTPGM  | Statistics based on the V23 ACCNT file.  |
| I_ACCNT24  | ACCNTPGM24  | Statistics based on the V24 ACCNT file.  |


<span id="RPG"></span>

## RPG language (.RPGLE file extension)

In the **Template** column, click the template link to view the sample template as a text file.


| Template  | Function  | Services | Description  |
| --- | --- | --- | --- |
| <a href="">TCFTI_RPG</a>  | CFTI  | OPEN - CLOSE - NEXT - SELECT- MODIF  | Using CFTI function (CFTAPI V23 structure )  |
| <a href="">TCFTI2_RPG</a>  | cftaix  | OPEN - CLOSE - SELECT240 - NEXT240 - SORT - DO -  | Using CFTIX function (CFTAPI V24 structure )  |
| <a href="">TCFTU_RP1</a>  | CFTU  | SEND - DELETE  | Tests the CFTU, CFTC functions (asynchronous API)  |
| <a href="">TCFTU_RP1N</a>  | CFTU  | SEND  | Using PLIST for receiving the SEND command  |
| <a href="">TCFTU_RP2</a>  | CFTU  | COM, SEND, GETXINFO, CLOSEAPI  | Tests the CFTU function (synchronous API)  |


## How to compile and execute a COBOL sample

<span id="TCFTU"></span>

### I\_TCFTU\_CBL program

```
CRTCBLMOD MODULE(<<span class="bold_in_para">****CFTPGM****</span>>/TCFTU_CBL1) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC)
SRCMBR(TCFTU_CBL1) DBGVIEW(\*ALL) REPLACE(\*YES)
 
CRTPGM PGM(<<span class="bold_in_para">****CFTPGM****</span>>/TCFTU_CBL1) MODULE(<******CFTPGM******>/TCFTU_CBL1)
BNDSRVPGM(<CFTPGM>/LIBAPISRV1) REPLACE(\*YES)
 
CRTBNDCL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_TCFTU_CB) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC)
SRCMBR(I_TCFTU_CB) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_TCFTU_CB)
```
<span id="INTCAT"></span>

### I\_INTCAT program

```
CRTCBLMOD MODULE(<<span class="bold_in_para">****CFTPGM****</span>>/INTCAT) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC)
SRCMBR(INTCAT) DBGVIEW(\*ALL) REPLACE(\*YES)
 
CRTPGM PGM(<<span class="bold_in_para">****CFTPGM****</span>>/INTCAT) MODULE(<<span class="bold_in_para">****CFTPGM****</span>>/INTCAT)
BNDSRVPGM(<<span class="bold_in_para">****CFTPGM****</span>>/LIBAPISRV1) REPLACE(\*YES)
 
CRTBNDCL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_INTCAT) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC)
SRCMBR(I_INTCAT) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_INTCAT)
```
<span id="ACCNTPGM"></span>

### I\_ACCNTPGM program for ACCNT V23

```
CRTBNDCBL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/ACCNTPGM) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC) SRCMBR(ACCNTPGM) OPTION(\*EVENTF) DBGVIEW(\*SOURCE) REPLACE(\*YES)
 
CRTBNDCL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_ACCNTPGM) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC) SRCMBR(I_ACCNTPGM) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(<CFTPGM>/I_ACCNTPGM)
```
<span id="ACCNT24"></span>

### I\_ACCNT24 program for ACCNT V24

```
CRTBNDCBL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/ACCNTPGM24) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC) SRCMBR(ACCNTPGM24) OPTION(\*EVENTF) DBGVIEW(\*SOURCE) REPLACE(\*YES)
 
CRTBNDCL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_ACCNT24) SRCFILE(<<span class="bold_in_para">****CFTPGM****</span>>/CFTSRC) SRCMBR(I_ACCNTPGM) OPTION(\*EVENTF) REPLACE(\*YES) DBGVIEW(\*SOURCE)
 
CALL PGM(<<span class="bold_in_para">****CFTPGM****</span>>/I_ACCNT24)
```
