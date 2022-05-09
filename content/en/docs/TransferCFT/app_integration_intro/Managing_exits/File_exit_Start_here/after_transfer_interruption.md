{
    "title": "After a transfer interruption",
    "linkTitle": "Stage After a transfer interruption",
    "weight": "380"
}This event occurs if there is a:

- Mains supply cut
    or protocol fault
- Request by one
    of the partners (HALT or KEEP command)
- Request by the
    user function during one of the previous stages (ret1 = 9)

If the user function manages file accessing, it must de-allocate the
file and save the zwork working area for subsequent restarting purposes,
before handing back control to {{< TransferCFT/axwayvariablesComponentShortName  >}}.

### Fields to define


| Field  | Description  |
| --- | --- |
| ret1 | Return code:<br/> • 0: processing ok<br/> • 9: refusal and end of transfer  |
| ret2 | Error message  |
| msg | Message sent to the standard output  |
| exec | Name of the end of transfer procedure  |
| state | Transfer state ('H' or 'K') |


### Field values


| Field | Sender mode<br /> Before<br />  | Sender mode<br /> After<br />  | Receiver mode<br /> Before  | Receiver mode<br /> After  |
| --- | --- | --- | --- | --- |
| mtype | 9 | 9 | 9 | 9 |
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
| state | = | = | = | x |
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

