{
    "title": "About  the communication area",
    "linkTitle": "About the communication area",
    "weight": "320"
}This section describes the file exit communication area structure and describes the fields of the communication structure.


| Field | Explanation |
| --- | --- |
| ****mtype**** | Transfer stage<br /> The possible values are:<br/> • 0 (ALLOC_TYP) before the file is allocated<br/> • 1 (OPEN_TYP) before the file is opened<br/> • 2 (TRANS_TYP) before the start of the transfer<br/> • 3 (DATA_TYP) before a record is sent or after it is received<br/> • 4 (CHECK_TYP) after a synchronization point<br/> • 5 (RESTART_TYP) before repositioning<br/> • 6 (DTEND_TYP) before the end of the file<br/> • 7 (CLOSE_TYP) before the file is closed<br/> • 8 (ENDTR_TYP) before the end of the transfer<br/> • 9 (ABORT_TYP) after a transfer interruption  |
| ****masc****  | Mask for selecting stages<br /> This field comprises 16 bytes; each byte can take the value 0 or 1 and is associated with a stage:<br/> • Byte 0 =&gt; ALLOC_TYP<br/> • Byte 1 =&gt; OPEN_TYP<br/> • Byte 2 =&gt; TRANS_TYP<br/> • Byte 3 =&gt; DATA_TYP<br/> • Byte 4 =&gt; CHECK_TYP<br/> • Byte 5 =&gt; RESTART_TYP<br/> • Byte 6 =&gt; DTEND_TYP<br/> • Byte 7 =&gt; CLOSE_TYP<br/> • Byte 8 =&gt; ENDTR_TYP<br/> • Byte 9 =&gt; ABORT_TYP<br/> • Bytes 10 to 15 =&gt; Reserved<br/> Byte 0 always equals 1.<br/> If you set a byte to 1, it means that you want to take control during the associated stage<br /> The value of the field mtype indicates the rank of the byte associated with the stage |
| ****access **** | File access managements under the control of:<br/> • Transfer CFT if set to 0<br /> {{< TransferCFT/axwayvariablesComponentShortName  >}} takes charge of all operations; you can, however, change certain features of the file (name, size, format, etc.) at the allocation and opening stages<br/> • The user if set to 1<br /> You are responsible for all operations performed on the file: allocation, opening, read/write, closing, de-allocation |
| ****retsync**** | The return is:<br/> • 0 : synchronous<br /> The user function processes the stage and returns control to the interface.<br/> • 1: asynchronous<br /> The user function returns control to the interface before processing the stage (deferred processing). The transfer is suspended and the interface waits for an end of processing message from the user.<br/> As the user does not yet have the information or tools required to use the possibility of an asynchronous return, the return will always be synchronous  |
| ****ret1**** | Return code:<br/> • 0 = processing ok<br/> • 9 = refusal and end of transfer<br/> Other values are defined depending on the transfer stage. For more information, refer to the [Using the Communication Structure](../using_file_exit_comm_area). |
| ****ret2**** | Error message.<br /> This message appears in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog in the DIAGP field (protocol diagnosis). |
| ****us_sem**** | Reserved |
| ****us_ctx**** | Reserved |
| ****idexit **** | EXIT identifier |
| ****exname**** | User name that must correspond to the value of the exaref parameter of the **exfini** function. |
| ****parmexit**** | User exit parameter |
| ****version**** | Exit version either V24 or higher |
| ****language**** | Language used by the user program:<br/> • C: C language<br/> • O: COBOL |
| ****reserv**** | Size of the user working area that must correspond to the value of the RESERV parameter of the CFTEXIT command |
| ****etrace**** | Reserved |
| ****waittask**** | EXIT task authorized maximum inactivity time (in minutes)<br /> It must correspond to the value of the WAITTASK parameter of the CFTEXIT command.<br /> If set to 1441, the EXIT task is permanent. |
| ****part**** | Partner local identifier  |
| ****idf **** | File logical identifier |
| ****nidf **** | File network identifier |
| ****idt**** | Transfer identifier |
| ****direct**** | Transfer direction:<br/> • S: Send<br/> • R: Receive |
| ****mode **** | Mode<br/> • R: Requester<br/> • S: Server |
| ****relance**** | The possible values are:<br/> • 1: Restart<br/> • 2: Do not restart |
| ****prot**** | Communication protocol |
| ****prof**** | ****PeSIT only****<br/> Profile |
| ****spart**** | Partner sending the file  |
| ****rpart**** | Partner receiving the file |
| ****suser**** | User sending the file |
| ****ruser**** | User receiving the file |
| ****fpassw**** | Password associated with the file |
| ****sappl**** | Application sending the file |
| ****rappl**** | Application receiving the file  |
| ****userid**** | User identifier |
| ****groupid**** | Identifier of the group to which the user belongs  |
| ****exec**** | Name of the end-of-transfer procedure  |
| ****fdate**** | Date associated with the file  |
| ****ftime**** | Time associated with the file |
| ****fdisp**** | File availability  |
| ****faction**** | Action on the file  |
| ****state**** | Transfer state |
| ****parm**** | Private parameter  |
| ****comment**** | Comment |
| ****fname**** | File name  |
| ****fksize**** | Key size |
| ****fkloc**** | Key position  |
| ****flrecl**** | Record size |
| ****fblksize**** | Block size  |
| ****frecfm**** | Record format  |
| ****frecfmx**** | ****z/OS (MVS) only****<br/> Record extended format |
| ****fspace**** | File allocation size |
| ****ftype**** | File type |
| ****fcode**** | Data code |
| ****forg**** | File organization |
| ****facc**** | Access method |
| ****fsyst**** | Operating system  |
| ****nfname**** | File network name |
| ****nfver**** | Version  |
| ****nlrecl**** | Record size  |
| ****nblksize**** | Block size |
| ****nrecfm**** | Record format |
| ****nrecfmx**** | Extended record format (z/OS / MVS)  |
| ****nspace**** | File allocation size |
| ****ntype**** | File type  |
| ****ncode**** | Data code  |
| ****norg**** | File organization  |
| ****nsyst**** | Operating system  |
| ****ncomp**** | Data compression |
| ****fcars**** | Number of bytes written  |
| ****frecs**** | Number of records written  |
| ****ecars**** | Number of bytes before compression and after decompression  |
| ****nrecs**** | Number of records sent  |
| ****rpos**** | Value of last synchronization point  |
| ****notify**** | Notification  |
| ****msg**** | User message |
| ****ldata**** | Length of data sent  |
| ****idtu**** | Local transfer counter identifier |
| ****cMode**** | SSL mode Client/Server |
| ****cAuthPolicy**** | SSL auth Anonymous/Simple/Double |
| ****bCipher**** | SSL cipher suite |
| ****sParm**** | SSL command free parameters |
| ****sRemoteUserDn**** | Remote User certificate Dn |
| ****sRemoteIssuerDn**** | Remote Issuer Dn |
| ****sRemoteCaId**** | Remote CA Alias |
| ****sUserCId**** | User Certificate Alias |
| ****sCertFname**** | File including Remote certificate |
| ****sProf**** | SSL profil Id. |
| ****sRemoteSerial**** | Serial Number |
| ****ExitFree**** | Free Area between all EXITs |
| ****nspart**** | Network first sender partner name |
| ****nrpart**** | Network last receiver partner name |
| ****XferCycleId**** | CycleId of trace occurences |
| ****XferObjectcId**** | Name of the XFB transfer trace class |


<span id="C_Language_structure"></span>

### C Language structure

If you want to keep an exit that was created in a version of Transfer
CFT ****prior**** to V2.4, you can continue
to use the format displayed below. The ****exitdT****
communication structure between the interface and the user program is
defined as follows:

```
typedef struct {
union {
struct {
/\* Structure for the C language \*/
/\* ... \*/
} exC;
struct {
/\* Structure for the COBOL language \*/
/\* ... \*/
} exnC;
} exU;
} exitdT, \*exitdTp;
```

If you want to create an exit using the V2.4 format ****exitdnT****
communication structure between the interface and the user program is
defined below:

```
typedef struct {
union {
struct {
/\* Structure for the C language \*/
/\* ... \*/
} exC;
struct {
/\* Structure for the COBOL language \*/
/\* ... \*/
} exnC;
} exU;
} exitdnT, \*exitdnTp;
```

The structures for the C and COBOL languages are described in ****exfus.h****
that is delivered with {{< TransferCFT/axwayvariablesComponentShortName  >}} .

### COBOL language structure

If the user program is written in COBOL, you must comply with C-COBOL
interfacing rules.

The communication structure between the interface and the user program
is defined in the ****exfus.cop****
file that is delivered with the {{< TransferCFT/axwayvariablesComponentShortName  >}} product.
