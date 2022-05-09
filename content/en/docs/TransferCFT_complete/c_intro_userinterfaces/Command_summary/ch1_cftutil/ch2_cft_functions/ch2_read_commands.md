{
    "title": "Read commands",
    "linkTitle": "Read commands",
    "weight": "300"
}The QUERY, TEST, and WAITCAT commands can locate the first transfer that meets a certain set of criteria. But, if there are other transfers that meet this same criteria, they are overlooked. To overcome this limitation and find these additional transfers, you can use the read commands.

Three read commands are available and described in this section:

- BEGSELCA: Establishes the selection criteria.
- GETCAT: Reads a transfer if it meets the BEGSELCA selection criteria.
- ENDSELCA: Ends the command series.

### BEGSELCA command

#### Syntax

The BEGSELCA command defines the selection criteria for transfers. The FTEMOIN field sets the name of a file to be used as a control to compare the transferred file with the control file. The compared file is opened with the same file attributes as those found in the catalog.

```
BEGSELCA IDA = STR,
DIRECT = STR,
IDF = STR,
PART = STR,
STATE = STR,
IDF = STR,
NIDF = STR,
MSG = STR,
SUSER = STR,
RUSER = STR,
SAPPL = STR,
SPART = STR,
RAPPL = STR,
RPART
PARM = STR,
STATED = STR,
FRECFM = STR,
NRECFM = STR,
FNAME = STR,
NFNAME = STR,
DIAGI = STR,
DIAGP = STR,
FLRECL = STR,
NLRECL = STR,
FBLKSIZE = STR,
NBLKSIZE = STR,
USERID = STR,
JOBNAME = STR,
PROT = STR
PRI = STR,
NCOMP = STR,
FSPACE = STR,
NSPACE = STR,
NCODE = STR,
FCODE = STR,
FREC = STR,
NREC = STR,
FCAR = STR,
NCAR = STR,
ECAR = STR,
FTIME = STR,
FDATE = STR,
FRECFM = STR
NRECFM = STR
FTEMOIN = STR
```

#### Parameters

- IDA: Character string specifying the selection criteria IDA field.
- DIRECT: Character string specifying the selection criteria DIRECT field.
- IDF: Character string specifying the selection criteria IDF field.
- PART: Character string specifying the selection criteria SHARE field.
- STATE: Character string specifying the selection criteria STATE field.
- IDF: Character string specifying the selection criteria IDF field.
- NIDF: Character string specifying the selection criteria NIDF field.
- MSG: Character string specifying the selection criteria MSG field.
- SUSER: Character string specifying the selection criteria SUSER field.
- RUSER: Character string specifying the selection criteria RUSER field.
- SAPPL: Character string specifying the selection criteria Sappl field.
- SPART: Character string specifying the selection criteria SPART field.
- RAPPL: Character string specifying the selection criteria RAPPL field.
- RPART: Character string specifying the selection criteria RPART field.
- PARM: Character string specifying the criteria for selecting the PARM field.
- STATED: Character string specifying the selection criteria field STATED field
- FRECFM: Character string specifying the selection criteria field FRECFM field
- NRECFM: Character string specifying the selection criteria field NRECFM field
- FNAME: Character string specifying the selection criteria for the FNAME field
- NFNAME: Character string specifying the selection criteria field NFNAME field
- DIAGI: Character string specifying the selection criteria for the DIAGI field
- DIAGP: Character string specifying the selection criteria for the DIAGP
- FLRECL: Character string specifying the selection criteria for the FLRECL
- NLRECL: Character string specifying the selection criteria for the NLRECL
- FBLKSIZE: Character string specifying the selection criteria for the FBLKSIZE
- NBLKSIZE: Character string specifying the selection criteria for the NBLKSIZE
- USERID: Character string specifying the selection criteria for the USERID
- JOBNAME: Character string specifying the selection criteria for the JOBNAME
- PROT: Character string specifying the selection criteria for the PROT
- PRI: Character string specifying the selection criteria for the PRI
- NCOMP: Character string specifying the selection criteria for the NCOMP
- FSPACE: Character string specifying the selection criteria for the FSPACE
- NSPACE: Character string specifying the selection criteria for the NSPACE
- NCODE: Character string specifying the selection criteria for the NCODE
- FCODE: Character string specifying the selection criteria for the FCODE
- FREC: Character string specifying the selection criteria for the FREC
- NREC: Character string specifying the selection criteria for the NREC
- FCAR: Character string specifying the selection criteria for the FCAR
- NCAR: Character string specifying the selection criteria for the NCAR
- ECAR: Character string specifying the selection criteria for the ECAR
- FTIME Character string specifying the selection criteria for the FDATE
- FDATE Character string specifying the selection criteria for the FTIME
- FRECFM character identifying the value of the parameter F/V/U
- NRECFM character identifying the value of the parameter F/V/U
- FTEMOIN: String specifying the name of the comparison file.

### GETCAT

#### Syntax

No parameters are supplied for this command. The GETCAT function searches the catalog record corresponding to the selection criteria indicated by the BEGSELCA command parameters. If a record matches the criteria, predefined variables are initialized and can be exploited.

The predefined variable _CMDRET is always 0 for OK, and cannot be tested to see if the read was successful or not. In cases where there are additional transfers that meets the criteria, the variables _CAT_PART, and _CAT_IDT _CAT_IDTU have the keyword value of NIL.

This enables you to compare the value of the partner string NIL to see if the read action is complete. See the ENDSELCA example in the following section.

### ENDSELCA

#### Syntax

The ENDSELCA function finishes the selection reading. This command is imperative to close the catalog.

#### Example

```
LONG NAME = VAR, INIT = 0
LONG NAME = CMP, INIT = 0
CHAR NAME = STRNIL , SIZE = 10, INIT = NIL
CHAR NAME = PART , SIZE = 10
/\* set the variable _ECHO to 0 to exclude messages related to WHILE and IF \*/
/\* to customize the display. \*/
_MOV NAME=_ECHO,VALUE=0
BEGSELCA DIRECT=SEND,DIAGI=406
PRINT MSG='Partner DTSA File Transfer Diags Appli. '
PRINT MSG=' Id. Id. CFT Protocol Id. '
PRINT MSG='-------- ---- -------- -------- --- -------- --------'
WHILE NAME = VAR, VALUE = 0, TYPE = EQU
GETCAT
Deleted: 1.3.
Deleted: 1.3.1.
Deleted: 1.3.
Deleted: 1.3.1.
Deleted:
13
_STRCPY NAME=PART, STR=%_CAT_PART%
_STRCMP NAME=PART, STR=STRNIL ,RC=CMP
IF NAME=CMP, VALUE=0, TYPE=EQU
BRKWHILE
ENDIF
PRINT MSG='%[-8.8]_CAT_PART% %_CAT_DIRECT%%_CAT_TYP%%_CAT_STATE% %[-
8.8]_CAT_IDF% %_CAT_IDT% %_CAT_DIAGI% %_CAT_DIAGP% %_CAT_IDA%'
ENDWHILE
ENDSELCA
EXIT
```

### MQUERY

#### Syntax

The MQUERY command displays the current log content, the catalog and cache for the Transfer CFT.

```
MQUERY OBJECT = STR,
      CONTENT = STR,
      NAME = STR,
```

#### Parameters

- OBJECT: Possible values ​​are:
    -   CACHE: Select to list the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog or command cache.
    -   SYSTEM: Lists system information.
- CONTENT: Possible values ​​are FUL / BRIEF.
- NAME: Possible values ​​are:
    -   CAT: Lists  the catalog cache.
    -   COMMAND: Lists  the command cache.

#### Example

```
MQUERY NAME=CAT,CONTENT=FULL
CFTI24I =========================== TRANSFERS ==================================
CFTI24I pri minTime minDate reqTime reqDate cat_blk part
CFTI24I ======================================================================
CFTI24I Transfers_Non_Ready : 3
CFTI24I 128 11:52:27 TODAY 11:50:27 TODAY 180 LOOP
CFTI24I 128 12:15:50 TODAY 11:50:50 TODAY 182 LOOP
CFTI24I 128 12:40:41 TODAY 11:50:41 TODAY 181 LOOP
CFTI24I Transfers_Ready : 0 ( 0 Partners )
CFTI24I Transfers_Time__Locked : 0 ( 0 Partners )
CFTI24I Transfers_State_Locked : 0 ( 0 Partners )
CFTI24I ======================== PARTNERS =====================================
CFTI24I name count state locked diag diagp minTime minDate
CFTI24I ======================================================================
CFTI24I Partners : 1
CFTI24I LOOP 3 NRDY 0 0
CFTI24I Partners_Ready : 0
CFTI24I Partners_Time__Locked : 0
CFTI24I Partners_State_Locked : 0
MQUERY Treated for USER userid
```
