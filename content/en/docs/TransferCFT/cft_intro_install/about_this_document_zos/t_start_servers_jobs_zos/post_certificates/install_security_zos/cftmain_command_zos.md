---
title: "CFTMAIN controlled commands"
linkTitle: "CFTMAIN controlled commands"
weight: 360
--- This section lists the Transfer CFT z/OS CFTUTIL commands, and additional object and class information for:

- Users with all rights except for the ALL_CAT object

<!- - - - >

- Users with all rights to the ALL_CAT object

<!- - - - >

- Users with all rights

## User with all right except for the ALL_CAT object

FILE(ACCESS): PARM(READ), PART(READ), COM(UPD), CATLG(UPD), LOG(UPD).

| Command  | Object  | Class  | Variable  | UserID  | Action  |
| --- | --- | --- | --- | --- | --- |
| START | APPL | applcls | &amp;ID | Cmduser | Control |
| HALT | APPL | applcls | &amp;ID | Cmduser | Control |
| KEEP | APPL | applcls | &amp;ID | Cmduser | Control |
| END | APPL | applcls | &amp;ID | Cmduser | Control |
| SUBMIT | APPL | applcls | &amp;ID | Cmduser | Control |
| DELETE | APPL | applcls | &amp;ID | Cmduser | Delete |

## User with all rights to the ALL_CAT object

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

QQQ_QQQ_TEST START

#### SHUT

**Object**: SHUT

**Class**: cmdecls

**UserID**: Cmduser

**Action**: Create

#### MQUERY

**Object**: MQUERY

**Class**: cmdecls

**UserID**: Cmduser

**Action**: Create

#### SWITCH LOG

**Object**: SWT_LOG

**Class**: cmdecls

**UserID**: Cmduser

**Action**: Create

#### SWITCH  ACCOUNT

**Object**: SWT_ACNT

**Class**: cmdecls

**UserID**: Cmduser

**Action**: Create

#### RECV FILE

(Requester/ receiver)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Cmduser

**Action**: Create

**Object**: TRANSFER

**Class**: xfercls

**Variable**: &MODE, &PART, &IDF, &SPART,  &RPART,  &IPART

**UserID**: Appluser

**Actions**:  
\* Create

**Note**: In receive mode, the &FNAME variable is ignored.

#### File sending

(Server/sender)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Cmduser

**Action**: Create

**Object**: TRANSFER

**Class**: xfercls

**Variable**: &MODE, &PART, &IDF, &SPART, &RPART, &IPART, &FNAME

**UserID**: Appluser

**Action**: Create

**Note**: Include PDS and GDG cases (\*)

**Object**: FILE

**Class**: filecls

**Variable**: &FNAME

**UserID**: Appluser

**Action**: Read

**Note**: Not applicable

#### SEND FILE

(Requester/ sender)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Cmduser

**Action**: Create

**Object**: TRANSFER

**Class**: xfercls

**Variable**: &MODE, &PART, &IDF, &SPART, &RPART, &IPART, &FNAME

**UserID**: Appluser

**Action**: Create

**Note**: Include PDS and GDG cases (\*)

**Object**: FILE

**Class**: filecls

**Variable**: &FNAME

**UserID**: Appluser

**Action**: Read

**Note**: Not applicable

#### File reception

(Server/ receiver)

**Object**: TRANSFER

**Class**: xfercls

**Variable**: &MODE, &PART, &IDF, &SPART, &RPART, &IPART

**UserID**: Appluser

**Action**: Create

**Note**: In receive mode, the &FNAME variable is ignored.

#### SEND MESSAGE

(Requester/ sender)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Cmduser

**Action**: Create

**Object**: MESSAGE

**Class**: messcls

**Variable**: &MODE, &PART, &IDM, &SPART, &RPART

**UserID**: Appluser

**Action**: Create

#### Message reception

(Server/ receiver)

**Object**: MESSAGE

**Class**: messcls

**Variable**: &MODE, &PART, &IDM, &SPART, &RPART

**UserID**: Appluser

**Action**: Create

#### SEND REPLY

(Requester/ sender)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Cmduser

**Action**: Create

#### Reply reception

(Server/ receiver)

**Object**: APPL

**Class**: applcls

**Variable**: &ID

**UserID**: Trfuser

**Action**: Create

(\*)  If GDG &FNAME=CFTV2.GDGHAB(0)

     Or         &FNAME=CFTV2.GDGHAB.G0002V00

     If PDS   &FNAME=CFTV2.INSTALL(D40INIT)

Appluser = System user (TSO) of the user defined by the CFTAPPL command.

Trfuser = System user (TSO) of the original file transfer owner.

QQQ_QQQ_TEST END

|   | Object  | Class  | Variable  | UserID  | Action  | Notes  |
| --- | --- | --- | --- | --- | --- | --- |
| SHUT | SHUT | cmdecls |   | Cmduser | Create |   |
| MQUERY | MQUERY | cmdecls |   | Cmduser | Create |   |
| SWITCH LOG | SWT_LOG | cmdecls |   | Cmduser | Create |   |
| SWITCH ACCOUNT | SWT_ACNT | cmdecls |   | Cmduser | Create |   |
| RECV FILE<br/> (Requester/ receiver) | APPL | applcls | &amp;ID | Cmduser | Create |   |
| - " - | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART | Appluser | Create | In receive mode, the &amp;FNAME variable is ignored. |
| File sending<br/> (Server/sender) | APPL | applcls | &amp;ID | Cmduser | Create |   |
| - " - | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART, &amp;FNAME | Appluser | Create | Include PDS and GDG cases (*) |
| - " - | FILE | filecls | &amp;FNAME | Appluser | Read<br/>  | Not applicable |
| SEND FILE<br/> (Requester/ sender) | APPL | applcls | &amp;ID | Cmduser | Create |   |
| - " - | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART, &amp;FNAME | Appluser<br/>  | Create<br/>  | Include PDS and GDG cases (*) |
| - " - | FILE | filecls | &amp;FNAME | Appluser | Read | Not applicable |
| File reception<br/> (Server/ receiver) | TRANSFER | xfercls | &amp;MODE, &amp;PART, &amp;IDF, &amp;SPART, &amp;RPART, &amp;IPART | Appluser | Create | In receive mode, the &amp;FNAME variable is ignored. |
| SEND MESSAGE<br/> (Requester/ sender) | APPL | applcls | &amp;ID | Cmduser | Create |   |
| - " - | MESSAGE | messcls | &amp;MODE, &amp;PART, &amp;IDM, &amp;SPART, &amp;RPART | Appluser | Create |   |
| Message reception<br/> (Server/ receiver) | MESSAGE | messcls | &amp;MODE, &amp;PART, &amp;IDM, &amp;SPART, &amp;RPART | Appluser<br/>  | Create<br/>  |   |
| SEND REPLY<br/> (Requester/ sender) | APPL | applcls | &amp;ID | Cmduser<br/>  | Create<br/>  |   |
| Reply reception<br/> (Server/ receiver) | APPL | applcls | &amp;ID | Trfuser | Create |   |

(\*)  If GDG &FNAME=CFTV2.GDGHAB(0)

     Or         &FNAME=CFTV2.GDGHAB.G0002V00

     If PDS   &FNAME=CFTV2.INSTALL(D40INIT)

Appluser = System user (TSO) of the user defined by the CFTAPPL command.

Trfuser = System user (TSO) of the original file transfer owner.
