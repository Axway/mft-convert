{
    "title": "Transfer CFT object protection",
    "linkTitle": "Object protection",
    "weight": "330"
}This section describes protecting Transfer CFT objects and how to configure the following:

[Resource name generation](#Add)

[General resource profile definition](#Transfer)

- [Control the CFTxxx configuration commands](#Controll)
- [Control via ALL_xxx objects](#Controll2)
- [Control the SWITCH, MQUERY and SHUT commands](#Controll3)
- [Control via the APPL object](#Controll4)
- [Control FILE type transfers](#Controll5)
- [Control MESSAGE-type transfers](#Controll6)

<span id="Add"></span>

Add UCONF resource name generation control
------------------------------------------

Depending on the Transfer CFT command and parameter values , the Transfer CFT programs determine the ACTION type and OBJET name, whose properties are obtained from the ACTION (SECACT) and OBJECT (SECOBJ) dictionaries respectively. The VALUE field is set according to information in the file or command. The resource name passed to RACF has the following format:

```
Object_Name.Set_value
```

Depending on the object definition and the Transfer CFT configuration, RACF receives the following resource names:

```
CFTPARM.IDPARM0
      CFTSEND.DEFAULT     
      CFTRECV.DEFAULT     
      CFTAPPL.DEFAULT   
```

### Resource access control

Commands relating to specific information, CFTPARM for example, generate a single RACF control with the following format:

```
CFTPARM.IDPARM0    USER(usercmd)  ACCESS(create)
```

Where:

- CFTPARM.IDPARM0 is the resource name passed to RACF
- Usercmd is the system user (TSO) of the person submitting the command
- Create is the resource access mode

Commands relating to generic information, LISTPARM TYPE=ALL for example, generate a RACF control with the following format:

```
ALL_PARM.CFTV2.PARM   USER(usercmd)  ACCESS(read)
```

If the RACF return code is equal to zero, the Transfer CFT program lists all entries in the parameter file (PARM).

If the RACF return code is different from zero, the Transfer CFT program generates the following RACF controls:

```
CFTPARM.IDPARM0USER(usercmd)    ACCESS(read)
CFTSEND.DEFAULT      USER(usercmd)    ACCESS(read)
CFTRECV.DEFAULT     USER(usercmd)    ACCESS(read)
CFTAPPL.DEFAULT      USER(usercmd)    ACCESS(read)
```

If the RACF return code is equal to zero, the Transfer CFT program lists the entry.

[Transfer CFT z/OS files](../file_lists_zos) explains the commands controlled by the Transfer CFT programs.

<span id="Transfer"></span>

Transfer CFT general resource profile definition
------------------------------------------------

<span id="Controll"></span>

#### Control the CFTxxx configuration commands

If users in the grpaprm group are the only ones intended to modify CFTxxx commands, the samples provided are sufficient to grant them the necessary rights.

However, if specific authorizations must be granted to some users, declare the definitions in RACF as follows:

```
RDEFINE safcftcl    CFTRECV.DEFAULT UACC(NONE) OWNER( grpcft )
PERMIT   CFTRECV.DEFAULT   CLASS( safcftcl )    ACCESS(CONTROL) -
ID( grpcft   grpaprm   usera   userb   and so on  )
CONNECT   usera   GROUP( grpfprm ) OWNER( grpcft )
CONNECT   userb   GROUP( grpfprm ) OWNER( grpcft )
```
<span id="Controll2"></span>

#### Control via ALL_xxx objects

Only users in the grpdesk group are supposed to execute commands, the control of which is performed via ALL_xxx objects (ALL_CAT, ALL_COM, ALL_PART, ALL_PARM, ALL_EXT).  
The definitions supplied in the samples are sufficient to grant the necessary rights.

<span id="Controll3"></span>

#### Control the SWITCH, MQUERY and SHUT commands

Only users in the grpdesk group are supposed to execute these commands. The definitions supplied in the samples are sufficient to grant them the necessary rights.

<span id="Controll4"></span>

#### Control via the APPL object

The SEND, RECV, START, DELETE, HALT, KEEP, END and SUBMIT commands are controlled by the APPL object, and are dependent on the relevant CFTAPPL command ID.

If the following definitions are declared in the PARM file:

```
CFTPARM ID=IDPARM0,DEFAULT=DEFAULT, …
CFTSENDID=DEFAULT,IMPL=NO, …
CFTRECV   ID=DEFAULT,USERID=USRTSO, …
CFTAPPL   ID=DEFAULT,USERID=&USERID, …
CFTAPPL   ID=TEXT,USERID=USRTSO2, …
```

The parameter file CFTAPPL command is used to define the transfer owner. There are two possible scenarios for CFTAPPL, explicit and implicit definitions.

- When the user is explicitly indicated in the CFTAPPL ID=Text, USERID=Usrtso2, DIRECT=BOTH command, the owner of the transfer in send and/or receive mode is the one specified by the USERID parameter.
- When the user is implicitly indicated by the CFTAPPL ID=Default, USERID=&USERID, DIRECT=BOTH command, the owner, in requester mode, is deduced from the SEND or RECV command owner; in server mode, the owner is deduced from the user indicated in the CFTRECV or CFTSEND command.

If no user is indicated in the CFTRECV or CFTSEND command, the monitor user is considered to be the transfer owner.  Avoid this method for security reasons, as the end-of-transfer procedures would be performed under its authority.

The CFTAPPL command lets Transfer CFT control access to a parameter file identifier in one of several cases:

- Any file identifier defined in the parameter file by a CFTSEND ID= Default … or CFTRECV ID= Default … command must also be defined by a CFTAPPL ID= Default command; if not, the transfer is rejected.
- Any user (usrtso1) of a SEND IDF=Default,… or RECV IDF=Default,… command must correspond to an RACF profile:

RDEFINE safcftcl APPL.DefaultUACC(NONE) OWNER(GRPCFT)  
PERMIT APPL.DefaultCLASS(safcftcl) ID(usrtso1)ACCESS(CONTROL)

- Any file identifier not defined in the parameter file by a CFTSEND or CFTRECV command inherits the DEFAULT identifier definition information, if it already exists; if not, the transfer is rejected. In this case, the user must correspond to an RACF profile:

RDEFINE safcftcl APPL.DEFAULT  UACC(NONE) OWNER(GRPCFT)

PERMIT APPL.DEFAULT CLASS(safcftcl) ID(usrtso1) ACCESS(CONTROL)

- Any file identifier (Text for example) not defined in the parameter file by a CFTSEND or CFTRECV command can be defined by a CFTAPPL ID=Text, USERID=Usrtso2 command.  
    In this case, CFTAPPL command information is applied and completed with that of the DEFAULT identifier if it already exists. This possibility is used to force the transfer owner. The Usrtso1 user (belonging to the GRPTRF group) submitting the SEND or RECV command must correspond to an RACF profile:

RDEFINE safcftcl APPL.TEXT  UACC(NONE) OWNER(GRPCFT)

PERMIT APPL.TEXT CLASS(safcftcl) ID(Usrtso1) ACCESS(CONTROL)

> **Note**
>
> Note: As end-of-transfer commands are submitted from the transfer owner account (Usrtso2), these procedures cannot execute certain commands (DELETE, START, KEEP, and so on) unless the USRTSO2 user corresponds to a RACF profile type:PERMIT APPL.TEXT CLASS(safcftcl) ID(usrtso2) ACCESS(CONTROL)orPERMIT ALL_CAT.\*\* CLASS(safcftcl) ID(usrtso2) ACCESS(CONTROL)

<span id="Controll5"></span>

#### Control FILE type transfers

TRANSFER and COMMUT objects are used to check if the transfer owner has the right to send/receive a physical file to/from a partner site.

If the following definitions are declared in the PARM file:

```
CFTPARM ID=IDPARM0,DEFAULT=DEFAULT, …
CFTSEND   ID=DEFAULT,IMPL=NO, …
CFTRECV   ID=DEFAULT,USERID=USRTSO, …
CFTAPPL   ID=DEFAULT,USERID=&USERID, …
CFTAPPL   ID=TEXT,USERID=USRTSO2, …
CFTPART   ID=BANK, … 
```

If the Usrtso1 user, belonging to the GRPTRF group, submits the command:

```
SEND IDF=TEXT,PART=BANK,FNAME=CFTV2.TEST.CHECK
```

Then the transfer is rejected unless Usrtso2, the transfer owner, corresponds to an RACF profile of the type:

```
RDEFINE safcftcl TRANSFER.TEXT.S.BANK.CFTV2.TEST.\*  -
UACC(NONE) OWNER(GRPCFT)
PERMIT TRANSFER.TEXT.S.BANK.CFTV2.TEST.\* CLASS(safcftcl) –
ID(Usrtso2) ACCESS(CONTROL)
```

In server mode or if the Usrtso1 user submits the following command in requester mode:

```
RECV IDF=TEXT,PART=BANK,FNAME=CFTV2.TEST.CHECK
```

The transfer is rejected unless Usrtso2, the transfer owner, corresponds to an RACF profile of the type (in receive mode, the &FNAME variable is ignored if it is in the TRANSFER object definition):

```
RDEFINE safcftcl TRANSFER.TEXT.R.BANK UACC(NONE) OWNER(GRPCFT)
PERMIT TRANSFER.TEXT.R.BANK CLASS(safcftcl) –
ID(Usrtso2) ACCESS(CONTROL)
```
<span id="Controll6"></span>

#### Control MESSAGE type transfers

MESSAGE objects are used to check if the transfer owner has the right to send/receive a message to/from a partner site.

If the Usrtso1 user submits the command:

```
SEND IDM=TEXT,TYPE=MESSAGE,PART=BANK,MSG='SEND SUCCESSFUL'
```

Then the transfer is rejected unless Usrtso2, the transfer owner, corresponds to an RACF profile of the type:

```
RDEFINE safcftcl MESSAGE.TEXT.S.BANK UACC(NONE) OWNER(GRPCFT)
PERMIT MESSAGE.TEXT.S.BANK CLASS(safcftcl)-
ID(Usrtso2) ACCESS(CONTROL)
```

If the Usrtso1 user submits the command:

```
RECV IDM=TEXT, TYPE=REPLY, PART=BANK, IDT= C3110510, MSG='ACKNOWLEDGED'
```

Then the transfer is rejected unless Usrtso2, the transfer owner, corresponds to an RACF profile of the type:

```
RDEFINE safcftcl MESSAGE.TEXT.S.BANK UACC(NONE) OWNER(GRPCFT)
PERMIT MESSAGE.TEXT.S.BANK CLASS(safcftcl) –
ID(Usrtso2 GRPTRF) ACCESS(CONTROL)
```

The BANK partner Transfer CFT (if authorized) receives the response if the Usrtso3 user owning the transfer with IDT=C3110510 corresponds to an RACF profile type:

```
RDEFINEsafcftcl MESSAGE.TEXT.R.BANK UACC(NONE) OWNER(GRPCFT)
PERMIT MESSAGE.TEXT.R.BANK CLASS(safcftcl) –
ID(Usrtso3) ACCESS(CONTROL)
```

> **Note**
>
> Note: Objects whose VALUE parameter contains the &FNAME variable generate a resource name that takes into account the physical file name. RACF profiles must anticipate the differing cases, in particular PDS or GDG members.
