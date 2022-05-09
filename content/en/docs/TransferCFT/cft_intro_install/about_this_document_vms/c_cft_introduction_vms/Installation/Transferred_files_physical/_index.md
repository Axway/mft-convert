{
    "title": "Transferred files physical properties",
    "linkTitle": "Work with files",
    "weight": "220"
}Transferred files can be designated by physical or logical names. As the files are accessed by {{< TransferCFT/axwayvariablesComponentShortName  >}} sub-processes, the logical names used must be defined in a table of logical names at least at JOB level.

The physical file name must include the name and name extension. If there is no extension, add the '.' character to the end of the file name. Otherwise, the transfer fails with an ERRLREC error.

Information on physical file properties is exchanged between partners during every transfer.

****Sender****

These values are given explicitly in the CFTSEND command or by sending a request to an RMS service.

****Receiver****

These values are given explicitly in the CFTRECV command or received at protocol level.

The following table describes the physical properties for transferred files.


| Parameter  | Definition  |
| --- | --- |
| FSPACE | Disk space occupied by the file in KB. |
| FLRECL | Maximum size of a record in the file. |
| FBLKSIZE | The first multiple of 512 greater than the length of the record in the file. |
| FRECFM | Record format (Fixed / Variable / Undefined). |
| FORG | File organization (Seq / Direct / Indexed). |
| FTYPE | File type. A character that specifies the physical type of file (see table below). |
| FCODE | Code type (ASCII/EBCDIC/BINARY). Default value assigned according to the file FTYPE (see table below). |
| FKEYLEN | For an indexed file: key length. |
| FKEYPOS | For an indexed file: key offset in the record. |


The following table describes the FTYPE, FRECFM and FCODE assigned by {{< TransferCFT/axwayvariablesComponentShortName  >}} according to the file type.


| FAB Record Format Field Value (FAB$B_RFM)  | FRECFM  | FTYPE  | FCODE  |
| --- | --- | --- | --- |
| FAB$C_FIX | 'F' | ' ' | ASCII |
| FAB$C_UDF | 'U' | ' ' | ASCII |
| FAB$C_VAR | 'V' | 'P' | ASCII |
| FAB$C_VFC | 'V' | 'C' | ASCII |
| FAB$C_STM | 'V' | 'F' | ASCII |
| FAB$C_STMLF | 'V' | 'L' | ASCII |
| FAB$C_STMCR | 'V' | 'R' | ASCII |


The following table describes the file type according to the FRECFM and FTYPE parameters.


| FRECFM  | FTYPE  | FAB Record Format Field Value (FAB$B_RFM)  |
| --- | --- | --- |
| 'F' |   | FAB$C_FIX |
| 'U' |   | FAB$C_UDF |
| 'V' | 'C' | FAB$C_VFC |
| 'V' | 'F' | FAB$C_STM |
| 'V' | 'L' | FAB$C_STMLF |
| 'V' | 'R' | FAB$C_STMCR |
| 'V' | 'P' | FAB$C_VAR |


Received file versions
----------------------

During receive operations, {{< TransferCFT/axwayvariablesComponentShortName  >}} can generate successive file versions according to the file name syntax associated with the CFTRECV command.

- &lt;filename&gt;.&lt;ext&gt; or &lt;filename&gt;.&lt;ext&gt;;  
    The transfer might be refused, depending on the CFTRECV command FACTION and FDISP parameters. The file envelope is reused if available or deleted and then recreated.

<!-- -->

- &lt;filename&gt;.&lt;ext&gt;;n   
    The file is created with the requested version number. If it already exists in a later version, the transfer is refused. If it exists in the same version, the transfer might be refused, depending on the CFTRECV command FACTION and FDISP parameters. The file envelope is reused or deleted and then recreated.

<!-- -->

- &lt;filename&gt;.&lt;ext&gt;;+1   
    If the file does not exist, it is created with version ;1. Otherwise, it is created with a version that is greater than the most recent file version available. The transfer may be refused, depending on the CFTRECV command FACTION and FDISP parameters. If the transfer is successful, a new version is systematically created.

> **Note**
>
> Note: If you use the &lt;filename&gt;;+1 syntax, you must use the temporary receive file (CFTRECV command WFNAME parameter). If you do not use this syntax, Transfer CFT cannot restart an aborted transfer.
