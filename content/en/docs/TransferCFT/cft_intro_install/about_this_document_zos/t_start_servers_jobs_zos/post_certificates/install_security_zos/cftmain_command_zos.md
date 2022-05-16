---

    title: CFTMAIN controlled commands
    linkTitle: CFTMAIN controlled commands
    weight: 360

---
This section lists the Transfer CFT z/OS CFTUTIL commands, and additional object and class information for:

- Users with all rights except for the ALL\_CAT object

<!-- -->

- Users with all rights to the ALL\_CAT object

<!-- -->

- Users with all rights

## User with all right except for the ALL\_CAT object

FILE(ACCESS): PARM(READ), PART(READ), COM(UPD), CATLG(UPD), LOG(UPD).


| Command  | Object  | Class  | Variable  | UserID  | Action  |
| --- | --- | --- | --- | --- | --- |
| START | APPL | applcls | &amp;ID | Cmduser | Control |
| HALT | APPL | applcls | &amp;ID | Cmduser | Control |
| KEEP | APPL | applcls | &amp;ID | Cmduser | Control |
| END | APPL | applcls | &amp;ID | Cmduser | Control |
| SUBMIT | APPL | applcls | &amp;ID | Cmduser | Control |
| DELETE | APPL | applcls | &amp;ID | Cmduser | Delete |


## User with all rights to the ALL\_CAT object

FILE(ACCESS): PARM(READ), PART(READ), COM(UPD), CATLG(UPD), LOG(UPD).


| Command  | Object  | Class  | Variable  | UserID  | Action  |
| --- | --- | --- | --- | --- | --- |
| START | ALL_CAT | opercls | &amp;FNAME | Cmduser | Control |
| HALT | ALL_CAT | opercls | &amp;FNAME | Cmduser | Control |
| KEEP | ALL_CAT | opercls | &amp;FNAME | Cmduser | Control |
| END | ALL_CAT | opercls | &amp;FNAME | Cmduser | Control |
| SUBMIT | ALL_CAT | opercls | &amp;FNAME | Cmduser | Control |
| DELETE | ALL_CAT | opercls | &amp;FNAME | Cmduser | Delete |


## User with all rights

FILE(ACCESS):PARM(READ), PART(READ), COM(UPD), CATLG(UPD), LOG(UPD).

QQQ\_QQQ\_CHECK find header for second col


| Command  |   | Object  | Class  | Variables  | UserID  | Action  | Notes  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SHUT |   | SHUT | cmdecls |   | Cmduser | Create |   |
| MQUERY |   | MQUERY | cmdecls |   | Cmduser | Create |   |
| SWITCH LOG |   | SWT_LOG | cmdecls |   | Cmduser | Create |   |
| SWITCH ACCOUNT |   | SWT_ACNT | cmdecls |   | Cmduser | Create |   |
| RECV FILE | (Requester/ receiver)  | APPL | applcls | &amp;ID | Cmduser | Create |   |
|  - " -  |  - " -  | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART | Appluser (2) | Create | In receive mode, the &amp;FNAME variable is ignored. |
| File sending | (Server/sender)  | APPL | applcls | &amp;ID | Cmduser | Create |   |
|  - " -  |  - " -  | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART, &amp;FNAME | Appluser (2) | Create | Include PDS and GDG cases (1) |
|  - " -  |  - " -  | FILE | filecls | &amp;FNAME | Appluser (2) | Read | Not applicable |
| SEND FILE | (Requester/ sender)  | APPL | applcls | &amp;ID | Cmduser | Create |   |
|  - " -  |  - " -  | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART, &amp;FNAME | Appluser (2) | Create | Include PDS and GDG cases (1) |
|  - " -  |  - " -  | FILE | filecls | &amp;FNAME | Appluser (2) | Read | Not applicable |
| File reception | (Server/ receiver)  | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART | Appluser (2) | Create | In receive mode, the &amp;FNAME variable is ignored. |
| SEND MESSAGE | (Requester/ sender)  | APPL | applcls | &amp;ID | Cmduser | Create |   |
|  - " -  |  - " -  | MESSAGE | messcls | &amp;MODE, &amp;PART, &amp;IDM, &amp;SPART, &amp;RPART | Appluser (2) | Create |   |
| Message reception | (Server/ receiver)  | MESSAGE | messcls | &amp;MODE, &amp;PART, &amp;IDM, &amp;SPART, &amp;RPART | Appluser (2) | Create |   |
| SEND REPLY | (Requester/ sender)  | APPL | applcls | &amp;ID | Cmduser | Create |   |
| Reply reception | (Server/ receiver)  | APPL | applcls | &amp;ID | Trfuser (3) | Create |   |


(1)  If GDG &FNAME=CFTV2.GDGHAB(0)  
     Or         &FNAME=CFTV2.GDGHAB.G0002V00  
     If PDS   &FNAME=CFTV2.INSTALL(D40INIT)

\(2\) Appluser = System user (TSO) of the user defined by the CFTAPPL command.

\(3\) Trfuser = System user (TSO) of the original file transfer owner.
