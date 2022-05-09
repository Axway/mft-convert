{
    "title": "Before the end of the transfer",
    "linkTitle": "Stage Before the end of the transfer",
    "weight": "420"
}If the user function manages file accessing, it must de-allocate the
file.

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |
| exec | Name of the end of transfer procedure  |
| faction | Action to be taken on the file after sending |


### Field values


| Field | Sender mode<br /> Before  | Sender mode<br /> After  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 8 | 8 | 8 | 8 |
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
| rpart | = | = | = | x |
| suser | = | = | = | = |
| ruser | = | = | = | = |
| fpassw | = | = | = | = |
| sappl | = | = | = | = |
| rappl | = | = | = | = |
| userid | = | = | = | = |
| groupid | = | = | = | = |
| exec | = | x | = | x |
| fdate | = | = | = | = |
| ftime | = | = | = | = |
| fdisp | = | = | = | = |
| faction | = | x | = | = |
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
| XferObjectcId | = | = | = | = |

