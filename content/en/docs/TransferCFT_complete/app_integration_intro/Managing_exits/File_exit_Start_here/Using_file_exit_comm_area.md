{
    "title": "Use the communication structure",
    "linkTitle": "Using the communication structure",
    "weight": "370"
}The following topics describe the stages in a file transfer. These are:

- [Before
    the file is allocated](#Before_the_File_is_Allocated)
- [Before
    the file is opened](#Before_the_File_is_Opened)
- [Before
    the start of the transfer](#Before_the_Start_of_the_Transfer)
- [Before
    a record is sent or after it is received](#Before_a_Record_is_Sent_or_After_it_is_Received)
- Before
    repositioning
- After
    a synchronization point
- Before
    the end of the file
- Before
    the file is closed
- Before
    the end of the transfer
- After
    a transfer interruption

Each stage is explained in a table as follows:

- Fields to define - the first table indicates the fields that you must update
- Field values - the second table indicates the
    values for each field of the communication area as seen from the sender
    or the receiver side before and after the call.  

The symbols used are indicated in the following table.


| Symbol  | Description  |
| --- | --- |
| / | This field does not apply to the current field  |
| = | This field keeps the value taken in the previous stage  |
| x | This field can be modified by the {{< TransferCFT/axwayvariablesComponentShortName  >}} (before the call) or by the user function (after the call)  |
| * | This field can be defined by the user function  |


The return code (ret1) must always be defined.

<span id="Before_the_File_is_Allocated"></span>

Before the file is allocated
----------------------------

This is the first stage in a file transfer. During this stage, you indicate
the stages at which you want to take control by defining the masc parameter.

<span id="Fields_to_define"></span>

### Fields to define


| Field  | Description  |
| --- | --- |
| masc | Mask used to select stages  |
| access | File access:<br/> • 0: controlled by {{< TransferCFT/axwayvariablesComponentShortName  >}}<br/> • 1: controlled by the user |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |


<span id="Field_values"></span>

### Field values


| Field | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After<br />  |
| --- | --- | --- | --- | --- |
| mtype | 0 | 0 | 0 | 0 |
| masc | 10000000...000 | * | 10000000...000 | * |
| access | 0 | * | 0 | * |
| retsync | 0 |   | 0 |   |
| ret1 | 0 | * | 0 | * |
| ret2 | blank | * | blank | * |
| us-sem |   |   |   |   |
| us-ctx |   |   |   |   |
| idexit | IDEXIT | = | IDEXIT | = |
| exname |   | = |   | = |
| parmexit | PARM | = | PARM | = |
| version | FORMAT | = | FORMAT | = |
| language | LANGUAGE | = | LANGUAGE | = |
| reserv | RESERV | = | RESERV | = |
| waittask | WAITTASK | = | WAITTASK | = |
| part | PART | = | PART | = |
| idf | IDF | = | IDF | = |
| nidf | NIDF | = | NIDF | = |
| idt | IDT | = | IDT | = |
| direct | DIRECT | = | DIRECT | = |
| mode | MODE | = | MODE | = |
| relance | RELANCE | = | RELANCE | = |
| prot | PROT | = | PROT | = |
| prof | PROF | = | PROF | = |
| spart | SPART | = | SPART | = |
| rpart | RPART | = | RPART | = |
| suser | SUSER | = | SUSER | = |
| ruser | RUSER | = | RUSER | = |
| fpassw | FPASSW | x | FPASSW | = |
| sappl | SAPPL | = | SAPPL | = |
| rappl | RAPPL | = | RAPPL | = |
| userid | USERID | x | USERID | x |
| groupid | GROUPID | x | GROUPID | x |
| exec | EXEC | = | EXEC | = |
| fdate | FDATE | x | FDATE | x |
| ftime | FTIME | x | FTIME | x |
| fdisp | FDISP | = | FDISP | x |
| faction | FACTION | = | FACTION | x |
| state | STATE | x | STATE | x |
| parm | PARM | x | PARM | x |
| comment | COMMENT | x | COMMENT | x |
| fname | FNAME | x | FNAME | x |
| fksize | FKEYSIZE | x | FKEYSIZE | x |
| fkloc | FKEYPOS | x | FKEYPOS | x |
| flrecl | FLRECL | x | FLRECL | x |
| fblksize | FBLKSIZE | x | FBLKSIZE | x |
| frecfm | FRECFM | x | FRECFM | x |
| frecfmx | FRECFMX | x | FRECFMX | x |
| fspace | FSPACE | x | FSPACE | x |
| ftype | FTYPE | x | FTYPE | x |
| fcode | FCODE | x | FCODE | x |
| forg | FORG | x | FORG | x |
| facc | FACC | x | FACC | x |
| fsyst | FSYST | x | FSYST | x |
| nfname | NFNAME | x | NFNAME | = |
| nfver | NFVER | x | NFVER | = |
| nlrecl | NLRECL | x | NLRECL | = |
| nblksize | NBLKSIZE | x | NBLKSIZE | = |
| nrecfm | NRECFM | x | NRECFM | = |
| nrecfmx | NRECFMX | x | NRECFMX | = |
| nspace | NSPACE | x | NSPACE | = |
| ntype | NTYPE | x | NTYPE | = |
| ncode | NCODE | x | NCODE | = |
| norg | NORG | x | NORG | = |
| nsyst | NSYST | x | NSYST | = |
| ncomp | NCOMP | x | NCOMP | x |
| fcars | / | / | / | / |
| frecs | / | / | / | / |
| ecars | / | / | / | / |
| nrecs | / | / | / | / |
| rpos | / | / | / | / |
| notify | NOTIFY | x | NOTIFY | x |
| msg | blank | * | blank | * |
| ldata | / | / | / | / |
| idtu | IDTU | = | IDTU | = |
| cMode | CMODE | = | CMODE | = |
| cAuthPolicy | CAUTHPOLICY | = | CAUTHPOLICY | = |
| bCipher | BCIPHER | = | BCIPHER | = |
| sParm | SPARM | = | SPARM | = |
| sRemoteUserDn | SREMOTEUSERDN | = | SREMOTEUSERDN | = |
| sRemoteIssuerDn | SREMOTEISSUERDN | = | SREMOTEISSUERDN | = |
| sRemoteCaId | SREMOTECAID | = | SREMOTECAID | = |
| sUserCId | SUSERCID | = | SUSERCID | = |
| sCertFname | SCERTFNAME | = | SCERTFNAME | = |
| sProf | SPROF | = | SPROF | = |
| sRemoteSerial | SREMOTESERIAL | = | SREMOTESERIAL | = |
| ExitFree | EXITFREE | x | EXITFREE | x |
| nspart | NSPART | = | NSPART | = |
| nrpart | NRPART | = | NRPART | = |
| XferCycleId | TRKR | = | TRKR | = |
| XferObjectcId | &quot;XFBTransfer&quot; | = | &quot;XFBTransfer&quot; | = |


<span id="Before_the_File_is_Opened"></span>

Before the file is opened
-------------------------

If the user function manages file accessing, it must open the file at
this stage.

### Fields to define


| Field  | Meaning  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |


### Field values


|   | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 1 | 1 | 1 | 1 |
| masc | = | * | = | * |
| access | = | = | = | = |
| retsync | 0 |   | 0 |   |
| ret1 | 0 | * | 0 | * |
| ret2 | blank | * | blank | * |
| us-sem |   |   |   |   |
| us-ctx |   |   |   |   |
| idexit | = | = | = | = |
| exname | = | = | = | = |
| parmexit | = | = | = | = |
| version | = | = | = | = |
| language | = | = | = | = |
| reserv | = | = | = | = |
| waittask | = | = | = | = |
| part | = | = | = | = |
| idf | = | = | = | = |
| nidf | = | = | = | = |
| idt | = | = | = | = |
| direct | = | = | = | = |
| mode | = | = | = | = |
| relance | = | = | = | = |
| prot | = | = | = | = |
| prof | = | = | = | = |
| spart | = | = | = | = |
| rpart | = | = | = | = |
| suser | = | = | = | = |
| ruser | = | = | = | = |
| fpassw | = | = | = | = |
| sappl | = | = | = | = |
| rappl | = | = | = | = |
| userid | = | = | = | = |
| groupid | = | = | = | = |
| exec | = | = | = | = |
| fdate | = | = | = | = |
| ftime | = | = | = | = |
| fdisp | = | = | = | = |
| faction | = | = | = | = |
| state | = | = | = | = |
| parm | x | x | x | x |
| comment | = | = | = | = |
| fname | = | = | = | = |
| fdb | = | = | = | = |
| fksize | x | x | = | = |
| fkloc | x | x | = | = |
| flrecl | x | x | x | = |
| fblksize | x | x | x | = |
| frecfm | x | x | x | = |
| frecfmx | x | x | x | = |
| fspace | x | x | x | = |
| ftype | x | x | x | = |
| fcode | x | x | x | = |
| forg | x | x | x | = |
| facc | x | x | = | = |
| fsyst | = | = | = | = |
| nfname | = | = | = | = |
| nfver | = | = | = | = |
| nlrecl | x | x | = | = |
| nblksize | x | x | = | = |
| nrecfm | x | x | = | = |
| nrecfmx | x | x | = | = |
| nspace | x | x | = | = |
| ntype | x | x | = | = |
| ncode | x | x | = | = |
| norg | x | x | = | = |
| nsyst | = | = | = | = |
| ncomp | = | = | = | = |
| fcars | / | / | / | / |
| frecs | / | / | / | / |
| ecars | / | / | / | / |
| nrecs | / | / | / | / |
| rpos | / | / | / | / |
| notify | = | = | = | = |
| msg | blank | * | blank | * |
| ldata | / | / | / | / |
| idtu | = | = | = | = |
| cMode | = | = | = | = |
| cAuthPolicy | = | = | = | = |
| bCipher | = | = | = | = |
| sParm | = | = | = | = |
| sRemoteUserDn | = | = | = | = |
| sRemoteIssuerDn | = | = | = | = |
| sRemoteCaId | = | = | = | = |
| sUserCId | = | = | = | = |
| sCertFname | = | = | = | = |
| sProf | = | = | = | = |
| sRemoteSerial | = | = | = | = |
| ExitFree | = | x | = | x |
| nspart | = | = | = | = |
| nrpart | = | = | = | = |
| XferCycleId | = | = | = | = |
| XferObjectId | = | = | = | = |


<span id="Before_the_Start_of_the_Transfer"></span>

Before the start of the transfer
--------------------------------

Only a File type EXIT at the sender end, DIRECT = S’ can process this
stage.

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |


### Field values


|   | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 2 | 2 |   |   |
| masc | = | * |   |   |
| access | = | = |   |   |
| retsync | 0 |   |   |   |
| ret1 | 0 | * |   |   |
| ret2 | blank | * |   |   |
| us-sem |   |   |   |   |
| us-ctx |   |   |   |   |
| idexit | = | = |   |   |
| exname | = | = |   |   |
| parmexit | = | = |   |   |
| version | = | = |   |   |
| language | = | = |   |   |
| reserv | = | = |   |   |
| waittask | = | = |   |   |
| part | = | = |   |   |
| idf | = | = |   |   |
| nidf | = | = |   |   |
| idt | = | = |   |   |
| direct | = | = |   |   |
| mode | = | = |   |   |
| relance | = | = |   |   |
| prot | = | = |   |   |
| prof | = | = |   |   |
| spart | = | = |   |   |
| rpart | = | = |   |   |
| suser | = | = |   |   |
| ruser | = | = |   |   |
| fpassw | = | = |   |   |
| sappl | = | = |   |   |
| rappl | = | = |   |   |
| userid | = | = |   |   |
| groupid | = | = |   |   |
| exec | = | = |   |   |
| fdate | = | = |   |   |
| ftime | = | = |   |   |
| fdisp | = | = |   |   |
| faction | = | = |   |   |
| state | = | = |   |   |
| parm | x | x |   |   |
| comment | = | = |   |   |
| fname | = | = |   |   |
| fdb | = | = |   |   |
| fksize | = | = |   |   |
| fkloc | = | = |   |   |
| flrecl | x | = |   |   |
| fblksize | x | = |   |   |
| frecfm | x | = |   |   |
| frecfmx | x | = |   |   |
| fspace | x | = |   |   |
| ftype | x | = |   |   |
| fcode | x | = |   |   |
| forg | x | = |   |   |
| facc | = | = |   |   |
| fsyst | = | = |   |   |
| nfname | = | = |   |   |
| nfver | = | = |   |   |
| nlrecl | x | x |   |   |
| nblksize | x | x |   |   |
| nrecfm | x | x |   |   |
| nrecfmx | x | x |   |   |
| nspace | x | x |   |   |
| ntype | x | x |   |   |
| ncode | x | x |   |   |
| norg | x | x |   |   |
| nsyst | = | = |   |   |
| ncomp | = | = |   |   |
| fcars | / | / |   |   |
| frecs | / | / |   |   |
| ecars | / | / |   |   |
| nrecs | / | / |   |   |
| rpos | / | / |   |   |
| notify | = | = |   |   |
| msg | blank | * |   |   |
| ldata | / | / |   |   |
| idtu | = | = |   |   |
| cMode | = | = |   |   |
| cAuthPolicy | = | = |   |   |
| bCipher | = | = |   |   |
| sParm | = | = |   |   |
| sRemoteUserDn | = | = |   |   |
| sRemoteIssuerDn | = | = |   |   |
| sRemoteCaId | = | = |   |   |
| sUserCId | = | = |   |   |
| sCertFname | = | = |   |   |
| sProf | = | = |   |   |
| ExitFree | = | x |   |   |
| nspart | = | = |   |   |
| nrpart | = | = |   |   |
| XferCycleId | = | = |   |   |
| XferObjectId | = | = |   |   |


<span id="Before_a_Record_is_Sent_or_After_it_is_Received"></span>

Before a record is sent or after it is received
-----------------------------------------------

At the sender end, DIRECT =
S, the user function is called before the record is sent to the
remote site, and after the record is read if {{< TransferCFT/axwayvariablesComponentShortName  >}} manages file
accessing. If file accessing is managed by the user function, the latter
has to read the record and define the ldata field as well as the zdata
parameter before handing back control to {{< TransferCFT/axwayvariablesComponentShortName  >}}.

At the receiver end, DIRECT = R,
the user function is called after the record is received, and before the
record is written if {{< TransferCFT/axwayvariablesComponentShortName  >}} manages file accessing. If file accessing
is managed by the user function, the latter has to write the record before
handing back control to {{< TransferCFT/axwayvariablesComponentShortName  >}}.

At this stage (DATA_TYP) and before the record is sent or after it is
received, the user function can perform the following operations:

- Modify the record
    sent or to be sent
- Insert one or more
    records
- Delete the record
- Set the "END
    of FILE" event

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0 = processing ok.<br /> Record not modified<br/> • 4 = end of file<br /> The previous record becomes the last one<br/> • 9 = refusal and end of transfer<br/> If file accessing is managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}:<br/> • 1 = record modified<br /> The user function must modify the ldata field and the zdata parameter. The record length must be consistent with the value of the flrecl field in order not to cause a read or write error.<br/> • 2 = one or more records inserted<br /> At the time the first record is inserted, you can save the current record in the zwork working area before handing back control to Transfer CFT. In the insertion mode (as long as ret1 = 2), the zdata is not defined by {{< TransferCFT/axwayvariablesComponentShortName  >}} in the following DATA_TYP stages and the user can continue to insert as many records as required.<br/> • 3 = record deleted <br /> On returning from the user function, {{< TransferCFT/axwayvariablesComponentShortName  >}} ignores the current record. The record is not sent when in send mode, and not written into the file when in receive mode. |
| ret2 | Error message  |
| msg | Message sent to the standard output  |
| ldata | Record length (in bytes)  |


#### Compression

Records are compressed as a result of a negotiation between the sender
partner and the receiver partner. At the
sender end, records are compressed before being sent by the {{< TransferCFT/axwayvariablesComponentShortName  >}}. At the receiver end, the
records are decompressed by the {{< TransferCFT/axwayvariablesComponentShortName  >}} immediately after
they are received. The records {{< TransferCFT/axwayvariablesComponentShortName  >}} supplies to the user function
are never in compressed form.

The ncomp field designates the compression algorithm used. The default
value of this field is the value specified in the NCOMP parameter of the
CFTSEND or CFTRECV command associated with the File type EXIT.

The user function can modify the ncomp field at the first stage in the
transfer (ALLOC_TYP). A zero value inhibits compression.

### Field values


| Field | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 3 | 3 | 3 | 3 |
| masc | = | * | = | * |
| access | = | = | = | = |
| retsync | 0 |   | 0 |   |
| ret1 | 0 | * | 0 | * |
| ret2 | blank | * | blank | * |
| us-sem |   |   |   |   |
| us-ctx |   |   |   |   |
| idexit | = | = | = | = |
| exname | = | = | = | = |
| parmexit | = | = | = | = |
| version | = | = | = | = |
| language | = | = | = | = |
| reserv | = | = | = | = |
| waittask | = | = | = | = |
| part | = | = | = | = |
| idf | = | = | = | = |
| nidf | = | = | = | = |
| idt | = | = | = | = |
| direct | = | = | = | = |
| mode | = | = | = | = |
| relance | = | = | = | = |
| prot | = | = | = | = |
| prof | = | = | = | = |
| spart | = | = | = | = |
| rpart | = | = | = | = |
| suser | = | = | = | = |
| ruser | = | = | = | = |
| fpassw | = | = | = | = |
| sappl | = | = | = | = |
| rappl | = | = | = | = |
| userid | = | = | = | = |
| groupid | = | = | = | = |
| exec | = | = | = | = |
| fdate | = | = | = | = |
| ftime | = | = | = | = |
| fdisp | = | = | = | = |
| faction | = | = | = | = |
| state | = | = | = | = |
| parm | = | = | = | = |
| comment | = | = | = | = |
| fname | = | = | = | = |
| fdb | = | = | = | = |
| fksize | = | = | = | = |
| fkloc | = | = | = | = |
| flrecl | = | = | = | = |
| fblksize | = | = | = | = |
| frecfm | = | = | = | = |
| frecfmx | = | = | = | = |
| fspace | = | = | = | = |
| ftype | = | = | = | = |
| fcode | = | = | = | = |
| forg | = | = | = | = |
| facc | = | = | = | = |
| fsyst | = | = | = | = |
| nfname | = | = | = | = |
| nfver | = | = | = | = |
| nlrecl | = | = | = | = |
| nblksize | = | = | = | = |
| nrecfm | = | = | = | = |
| nrecfmx | = | = | = | = |
| nspace | = | = | = | = |
| ntype | = | = | = | = |
| ncode | = | = | = | = |
| norg | = | = | = | = |
| nsyst | = | = | = | = |
| ncomp | = | = | = | = |
| fcars | / | x | / | x |
| frecs | / | x | / | x |
| ecars | / | / | / | / |
| nrecs | / | / | / | / |
| rpos | / | / | / | / |
| notify | = | = | = | = |
| msg | blank | * | blank | * |
| ldata | / | * | ldata | * |
| idtu | = | = | = | = |
| cMode | = | = | = | = |
| cAuthPolicy | = | = | = | = |
| bCipher | = | = | = | = |
| sParm | = | = | = | = |
| sRemoteUserDn | = | = | = | = |
| sRemoteIssuerDn | = | = | = | = |
| sRemoteCaId | = | = | = | = |
| sUserCId | = | = | = | = |
| sCertFname | = | = | = | = |
| sProf | = | = | = | = |
| sRemoteSerial | = | = | = | = |
| ExitFree | = | x | = | x |
| nspart | = | = | = | = |
| nrpart | = | = | = | = |
| XferCycleId | = | = | = | = |
| XferObjectId | = | = | = | = |

