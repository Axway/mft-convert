{
    "title": "CFTUTIL predefined variables",
    "linkTitle": "CFTUTIL predefined variables",
    "weight": "290"
}A number of variables are predefined in CFTUTIL for the catalog fields. These variables can be used in all of the batch programming commands.

Each variable begins with the underscore character _. For example, to display the IDTU value of a transfer, specify:

```
PRINT MSG='Transfer IDTU value: %_CAT_IDTU%'
```


| Variable  | Description  |
| --- | --- |
| _CAT_CFTV  | Catalog version.  |
| _CAT_COMMENT  | Comment associated with the IDF.  |
| _CAT_DATEB  | Transfer start date.  |
| _CAT_DATEC  | IDF cycledate value.  |
| _CAT_DATED  | Transfer filing date.  |
| _CAT_DATED  | End of transfer date.  |
| _CAT_DATEK  | Date of last writing.  |
| _CAT_DATEM  | Value of the IDF MINDATE parameter.  |
| _CAT_DATEMAX  | Value of the IDF MAXDATE parameter.  |
| _CAT_DEST  | CFTDEST command identifier.  |
| _CAT_DIAGC  | Additional diagnostic value.  |
| _CAT_DIAGI  | Internal diagnostic value.  |
| _CAT_DIAGP  | Diagnostic protocol value. |
| _CAT_DIFTYP  | Broadcast type:<br/> • L for broadcasting to multiple partners.<br/> • N for sending to a single partner. |
| _CAT_DIRECT  | Transfer direction.  |
| _CAT_EXEC  | IDF procedure name.  |
| _CAT_FACTION  | FACTION parameter value.  |
| _CAT_FCOMP  | FCOMP parameter value.  |
| _CAT_FDISP  | FDISP parameter value.  |
| _CAT_FILTYP  | File type:<br/> • L for file list.<br/> • N for normal file. |
| _CAT_FLAG  | Indicates if the transfer mode is REQUESTER or SERVER.  |
| _CAT_FNAME  | Name of file to be transferred.  |
| _CAT_FTNAME  | Temporary file name.  |
| _CAT_GROUP  | Name of the group initiating the request.  |
| _CAT_GROUPID  | Name of the group that the transfer owner belongs to.  |
| _CAT_IDA  | Application identifier.  |
| _CAT_IDEXIT  | CFTEXIT command file type identifier.  |
| _CAT_IDEXITA  | CFTEXIT command directory type identifier.  |
| _CAT_IDEXITE  | CFTEXIT command ETEBAC3 type identifier.  |
| _CAT_IDF  | Identifier associated with the IDF.  |
| _CAT_IDR  | IDR value.  |
| _CAT_IDT  | PROTC value.  |
| _CAT_IDTD  | IDT value.  |
| _CAT_IDTU  | IDTU value.  |
| _CAT_IPART  | Channel partner value.  |
| _CAT_JOBNAME  | Name of the task for which the transfer was created.  |
| _CAT_NCOMP  | Network compression value.  |
| _CAT_NFNAME  | Network filename.  |
| _CAT_NIDF  | Network IDF value.  |
| _CAT_NPART  | Connection partner value.  |
| _CAT_ORIGIN  | Origin of owner of the transfer.  |
| _CAT_PARM  | PARM parameter value.  |
| _CAT_PART  | PART value.  |
| _CAT_PIDF  | The previous value of the file ID.  |
| _CAT_PRI  | Transfer priority.  |
| _CAT_PROT  | Associated CFTPROT identifier.  |
| _CAT_PROTC  | PROTC value.  |
| _CAT_RAPPL  | RAPPL parameter value.  |
| _CAT_RELANCE  | Indicates if there was a restart or not.  |
| _CAT_REQGROUP  | Value of the initiator group for the command.  |
| _CAT_REQUSER  | Value of the user initiating the command.  |
| _CAT_RPART  | File receiving partner.  |
| _CAT_RUSER  | RUSER parameter value.  |
| _CAT_SAPPL  | SAPPL parameter value.  |
| _CAT_SGD  | Date  |
| _CAT_SGT  | Time  |
| _CAT_SPART  | SPART parameter value.  |
| _CAT_STATE  | State of the local transfer.  |
| _CAT_STATED  | State of the remote transfer.  |
| _CAT_SUSER  | SUSER parameter value.  |
| _CAT_TCYCLE  | TCYCLE parameter value.  |
| _CAT_TIMEB  | Transfer start time.  |
| _CAT_TIMEC  | TCYCLE parameter value.  |
| _CAT_TIMED  | Creation time of the transfer.  |
| _CAT_TIMEE  | Transfer end time.  |
| _CAT_TIMEK  | Time of last writing.  |
| _CAT_TIMEM  | MINTIME parameter value.  |
| _CAT_TIMEMAX  | MAXTIME parameter value.  |
| _CAT_TIMMAXC  | Maximum time for a call.  |
| _CAT_TIMMC  | Minimum time for a call.  |
| _CAT_TYP  | Transfer type.  |
| _CAT_USERID  | Transfer owner.  |
| _CAT_XLATE  | Associated CFTXLATE identifier card.  |
| _CAT_FDBNAME  | VFM name used for transfer.  |

