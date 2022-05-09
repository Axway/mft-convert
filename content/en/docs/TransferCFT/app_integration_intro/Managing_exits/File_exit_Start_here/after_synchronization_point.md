{
    "title": "After a synchronization point",
    "linkTitle": "Stage After a synchronization point",
    "weight": "430"
}The value of the synchronization point designates:

- For the sender, the position of
    the last record read in the file
- For the receiver, the position of the next record to be written

For the receiver, a file type
EXIT (DIRECT = R) with file accessing managed by the user, the
user function has to return to {{< TransferCFT/axwayvariablesComponentShortName  >}} the value of the synchronization
point, the number of records and the number of bytes written in the file.
This information is used by the monitor to complete the catalog entry
and is supplied to the user function during the RESTART_TYP stage.

At the sender end for a file type
EXIT (DIRECT = S) or if file accessing is managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}, the
{{< TransferCFT/axwayvariablesComponentShortName  >}} has all the information required to set a synchronization
point.

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer<br/> If file accessing is managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}:<br/> • 1: record modified<br/> • 2: one or more records inserted<br/> • 3: record deleted  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |
| rpos | Value of the last synchronization point  |
| frecs | Number of records written  |
| fcars | Number of bytes written  |


### Field values


| Field | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 4 | 4 | 4 | 4 |
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
| fcars | fcars | x | fcars | x |
| frecs | frecs | x | frecs | x |
| ecars | / | / | / | / |
| nrecs | / | / | / | / |
| rpos | / | x | / | x |
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

