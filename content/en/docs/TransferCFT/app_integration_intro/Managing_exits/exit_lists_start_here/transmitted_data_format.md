{
    "title": "Critera file and data format",
    "linkTitle": "Transmitted data format",
    "weight": "320"
}This section describes  the criteria file format, which corresponds to
the system version, and the physical characteristics
of the transmitted data.

<span id="Format_of_the_record_sent_in_V2"></span>

File selection criteria
-----------------------

****Example****

```
PARM LRECL = nnnn, FORMAT=X
SEND STATE = CDHKTX
RECV STATE = CDHKTX
```

Where:

- PARM: General
    parameters
- LRECL: The length
    of the records sent
- FORMAT: Indicates the file format of the communication area.

Format of sent records
----------------------

Indicates the file format of the communication area, where values are <span class="underline">1</span>, 2 C, T, J, or X (`1 `is the default).

****Format 1****

Displays using the same format as in {{< TransferCFT/headerfootervariableshflongproductname  >}} 3.5 and lower.

- Format V23: The record is truncated if LRECL&lt;503 or padded with spaces if LRECL&gt;503.
- Format V24: The record is truncated if LRECL&lt;1569 or padded with spaces if LRECL&gt;1569.


| Type  | V24 length  | V23 length  | Description  |
| --- | --- | --- | --- |
| CHAR  | 1  | 1  | 'L': start of record marker  |
| CHAR  | 1  | 1  | SEND/RECV flag  |
| CHAR  | 1  | 1  | STATE  |
| CHAR  | 32 | 8  | Send PARTner  |
| CHAR  | 32 | 8  | Receive PARTner  |
| CHAR  | 32 | 8  | IDF file identifier  |
| CHAR  | 8 | 8  | IDT transfer identifier  |
| CHAR  | 10 | 10  | Number of records  |
| CHAR  | 512 | 64  | Filename  |
| CHAR  | 3 | 3  | Transfer priority  |
| CHAR  | 8 | 8  | Date of submission to catalog |
| CHAR  | 8 | 8  | Time of submission to catalog or time of last modification  |
| CHAR  | 512 | 80  | User PARaMeter  |
| CHAR  | 28 | 15  | Send USER  |
| CHAR  | 28 | 15  | Receive USER  |
| CHAR  | 4 | 4  | Local parameter length  |
| CHAR  | 160 | 160  | Local user parameter  |
| CHAR  | 3 | 3  | Diagi  |
| CHAR  | 64 | 8  | Diagp  |
| Total | 1569 | 503 bytes  |   |


****Format 2****

The fields are as described in the following table, where the field size depends on the format version (V23 or V24).


| Type | V24 length | V23 length | Description |
| --- | --- | --- | --- |
| CHAR  | 1  | 1  | 'L': start of record marker  |
| CHAR  | 1  | 1  | SEND/RECV flag  |
| CHAR  | 1  | 1  | STATE  |
| CHAR | 1 | 1 | PHASE |
| CHAR | 1 | 1 | PHASESTEP |
| CHAR  | 64 | 8  | Send partner (PART) |
| CHAR  | 64 | 8  | Receive partner (PART) |
| CHAR  | 32 | 8  | IDF file identifier  |
| CHAR  | 8 | 8  | IDT transfer identifier  |
| CHAR  | 10 | 10  | Number of records  |
| CHAR  | 512 | 64  | Filename  |
| CHAR  | 3 | 3  | Transfer priority  |
| CHAR  | 8 | 8  | Date of submission to catalog |
| CHAR  | 8 | 8  | Time submitted to catalog, or time of last modification  |
| CHAR  | 512 | 80  | User parameter (PARM) |
| CHAR  | 32 | 15  | Send USER  |
| CHAR  | 32 | 15  | Receive USER  |
| CHAR  | 160 | 160  | Local user parameter  |
| CHAR  | 3 | 3  | Diagi  |
| CHAR  | 64 | 8  | Diagp  |


****Format C: CSV - Comma Separated Value****

Each field is separated by a comma.

For example: `S,X,X,X,MARTIN,PARIS,BIN,L1111482,2,,128,20191211,11481147,,,,,0,CP NONE`

****Format T: TSV - Tab Separated Value****

Each field is separated by a tab.

****Format J: JSON****

```
{"TRANSFERS":[{"DIRECT":"S","STATE":"X","PHASE":"X","PHASESTEP":"X","SPART":"MARTIN","RPART":"PARIS","IDF":"ID_EXITL","IDT":"L1111482","NBR":"2","FNAME":"","PRI":"128","DATEK":"20191211","TIMEK":"11481147","PARM":"","SUSER":"","RUSER":"","COMMENT":"","DIAGI":"0","DIAGP":"CP NONE"}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
,{"DIRECT":"R","STATE":"X","PHASE":"X","PHASESTEP":"X","SPART":"PARIS","RPART":"MARTIN","IDF":"BIN","IDT":"L1111475","NBR":"2","FNAME":"pub\\\\L1111475_A000002Q.RCV","PRI":"128","DATEK":"20191211","TIMEK":"11475343","PARM":"My \\"param\\" with quote","SUSER":"","RUSER":"","COMMENT":"","DIAGI":"0","DIAGP":"CP NONE"}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
], "TOTAL":"2"}            
```

****Format X: XML****

```
<?xml version="1.0" encoding="UTF-8"?><CAT><TRANSFER DIRECT="S" STATE="X" PHASE="X" PHASESTEP="X" SPART="MARTIN" RPART="PARIS" IDF="ID_EXITL" IDT="L1111423" NBR="2" FNAME="" PRI="128" DATEK="20191211" TIMEK="11422580" PARM="" SUSER="" RUSER="" COMMENT="" DIAGI="0" DIAGP="CP NONE"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  <TRANSFER DIRECT="R" STATE="X" PHASE="X" PHASESTEP="X" SPART="PARIS" RPART="MARTIN" IDF="BIN" IDT="L1111415" NBR="2" FNAME="pub\\L1111415_A000002K.RCV" PRI="128" DATEK="20191211" TIMEK="11415981" PARM="My  &quot;param&quot; with quote" SUSER="" RUSER="" COMMENT="" DIAGI="0" DIAGP="CP NONE"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
<TOTAL>2</TOTAL></CAT>      
```

Determine the LRECL 
--------------------

If **LRECL&gt;4096** or **LRECL&lt;109**, the record length is
forced to 503.

- The unused *Cata* field is forced blank or to
    0, in the case of a length.  
    The record is truncated if LRECL&lt;503 or padded with spaces if LRECL&gt;503
    (LRECL is a PARM parameter).
- The **User parameter** field corresponds to the PARM parameter of
    the CFTSEND or SEND command.
- The **Send user** field corresponds to the SUSER parameter of the
    CFTSEND or SEND command.
- The **Receive user** field corresponds to the RUSER parameter of
    the CFTSEND or SEND command.
- The **Local user parameter** field corresponds to the COMMENT parameter
    of the CFTSEND or SEND command for a send transaction or the COMMENT parameter
    of the CFTRECV or RECV command for a receive transaction.

The **Send partner** (SPART) field contains:

- For a send, the
    local partner, the PART parameter of the CFTPARM object
- For a receive,
    the partner identifier, the ID parameter of the CFTPART object

The **Receive partner** (RPART) field contains:

- For a receive,
    the local partner, the PART parameter of the CFTPARM object
- For a send, the
    partner identifier, the ID parameter of the CFTPART object

The commands have the same syntax as CFTUTIL:

**COMMAND** PARAMETER = *value,*
PARAMETER = *value,* etc./\*
Comment \*/

The commands are processed as follows:

- If the criteria
    file is not found, the exit-list generates a file opening error
- The criteria file
    must contain at least one SEND command or one RECV command
- All the commands
    present must include at least one parameter
- The absence of
    a SEND or RECV command implicitly means **no selection**. In the absence
    of a SEND command, no send transfer (SFD, SFH records, etc.) is selected.
    In the absence of a RECV command, no receive transfer (RFD, RFH records,
    etc.) is selected.

### About format version lengths

This section describes the minimum length of a list record including
significant items such as the filename, which depends on the selected version.

****Format version V23****

109 is the minimum length. If n&lt;109 or n&gt;4096, the default value 503 is used.

****Format version V24****

629 is the minimum length. If n&lt;629 or n&gt;4096, the default value 1569 is used.

- SEND = selection
    criteria for send transfers
- RECV = selection
    criteria for received transfers:
    -   STATE: transfer
        status
