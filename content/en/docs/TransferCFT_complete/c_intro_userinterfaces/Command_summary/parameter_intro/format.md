{
    "title": "format",
    "linkTitle": "format",
    "weight": "1260"
}<span id="format"></span>

### format

<span id="format_CFTLOG"></span>

#### CFTLOG

****[FORMAT = V23
&#124; V24 ]****

Optional parameter. Indicates the format for log messages.

- ****V23**** (Default value): The Identifier’s
    length is truncated to 8 characters.
- ****V24****: The complete Identifier is displayed.
    The length of the Identifier can be up to 32 characters. Some messages
    related to a transfer includes the IDTU (Local transfer counter identifier)
    value.

<span id="format_CFTACCNT"></span>

#### CFTACCNT

****[FORMAT = V23
&#124; V24 ]****

Optional parameter. The FORMAT parameter indicates whether the former
record structure should be used (V23 values for compatibility reasons)
or if the new structure is to be applied (V24 values).

- ****V23**** (Default value)
- V24

<span id="format_CFTEXIT"></span>

#### CFTEXIT

****[ FORMAT = { string } ]****

Indicates the file format of the communication area for an exit list. The possible values are 1, 2 C, T, J or X, where `1 `is the default.

****Example****

```
PARM LRECL = nnnn, FORMAT=X
SEND STATE = CDHKTX
RECV STATE = CDHKTX
```

##### Format 1

Displays using the same format as in {{< TransferCFT/headerfootervariableshflongproductname  >}} 3.5 and lower, though phase and phasestep no longer display.

##### Format 2


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


##### Format C: CSV - Comma Separated Value

Each field is separated by a comma.

For example: `S,X,X,X,MARTIN,PARIS,BIN,L1111482,2,,128,20191211,11481147,,,,,0,CP NONE`

##### Format T: TSV - Tab Separated Value

Each field is separated by a tab.

##### Format J: JSON

```
{"TRANSFERS":[{"DIRECT":"S","STATE":"X","PHASE":"X","PHASESTEP":"X","SPART":"MARTIN","RPART":"PARIS","IDF":"ID_EXITL","IDT":"L1111482","NBR":"2","FNAME":"","PRI":"128","DATEK":"20191211","TIMEK":"11481147","PARM":"","SUSER":"","RUSER":"","COMMENT":"","DIAGI":"0","DIAGP":"CP NONE"}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
,{"DIRECT":"R","STATE":"X","PHASE":"X","PHASESTEP":"X","SPART":"PARIS","RPART":"MARTIN","IDF":"BIN","IDT":"L1111475","NBR":"2","FNAME":"pub\\\\L1111475_A000002Q.RCV","PRI":"128","DATEK":"20191211","TIMEK":"11475343","PARM":"My \\"param\\" with quote","SUSER":"","RUSER":"","COMMENT":"","DIAGI":"0","DIAGP":"CP NONE"}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
], "TOTAL":"2"}            
```

##### Format X: XML

```
<?xml version="1.0" encoding="UTF-8"?><CAT><TRANSFER DIRECT="S" STATE="X" PHASE="X" PHASESTEP="X" SPART="MARTIN" RPART="PARIS" IDF="ID_EXITL" IDT="L1111423" NBR="2" FNAME="" PRI="128" DATEK="20191211" TIMEK="11422580" PARM="" SUSER="" RUSER="" COMMENT="" DIAGI="0" DIAGP="CP NONE"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  <TRANSFER DIRECT="R" STATE="X" PHASE="X" PHASESTEP="X" SPART="PARIS" RPART="MARTIN" IDF="BIN" IDT="L1111415" NBR="2" FNAME="pub\\L1111415_A000002K.RCV" PRI="128" DATEK="20191211" TIMEK="11415981" PARM="My  &quot;param&quot; with quote" SUSER="" RUSER="" COMMENT="" DIAGI="0" DIAGP="CP NONE"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
<TOTAL>2</TOTAL></CAT>      
```

`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     `

[Return to Command index](../../)
