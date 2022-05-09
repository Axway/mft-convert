{
    "title": "Before the end of the file",
    "linkTitle": "Stage Before the end of the file",
    "weight": "390"
}During this stage, you can add a record. To do so, you must modify the
zdata parameter and the ldata field.

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 2: add<br/> • 9: refusal and end of transfer |
| ret2 | Error message  |
| msg | Message sent to the standard output |
| ldata |  Record length (in bytes) |


### Field values


| Field | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 6 | 6 | 6 | 6 |
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
| ExitFree | = | X | = | x |
| nspart | = | = | = | = |
| nrpart | = | = | = | = |
| XferCycleId | = | = | = | = |
| XferObjectId | = | = | = | = |

