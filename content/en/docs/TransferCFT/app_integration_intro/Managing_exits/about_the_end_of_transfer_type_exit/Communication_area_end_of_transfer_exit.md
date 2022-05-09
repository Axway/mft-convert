{
    "title": "About  the communication area",
    "linkTitle": "About the communication area",
    "weight": "350"
}This topic describes the end-of-transfer exit communication area. The
communication area contains the fields which are described in the tables
below:

- [General
    information](#General_information_fields)
- [Local
    file information](#Local_file_information_fields)
- [Network
    file information](#Network_file_information)
- [Transfer
    information](#Transfer_information)
- [Information
    returned by the user (in update mode)](#Information_returned_by_the_user)
- [Information
    input/returned by the user](#Information_input_returned_by_the_user)
- [Additional
    information](#Additional_information)

The following tables list all the fields of the communication structure.

<span id="General_information_fields"></span>

### General information fields


| Field | Explanation |
| --- | --- |
| idt | Transfer identifier |
| idf | File identifier (if TYPE = FILE) |
| idm | Message identifier (if TYPE= MESSAGE) |
| ida | Application identifier |
| part | Partner name |
| direct | Transfer direction (S/R) |
| diagi | Internal diagnostics code |
| diagp | Protocol diagnostics code |


<span id="Local_file_information_fields"></span>

### Local file information fields


| Field | Explanation |
| --- | --- |
| fblksize | File block size |
| fcode | Data code (A, E or B) |
| fkeylen | Key length (indexed file) |
| fkeypos | Key position (indexed file) |
| flrecl | Record length |
| fname | Name of transferred file |
| forg | File organization:<br/> • C: contiguous<br/> • I: indexed<br/> • D: direct |
| ftype | File type |
| frecfm | Record format:<br/> • F: fixed<br/> • U: undefined<br/> • V: variable |
| fspace | Space allocated (in kilobytes) |
| ftime | File creation time (HHMMSSCC) |
| fdate | File creation date (YYYYMMDD) |
| fday | File creation day |
| fmonth | File creation month |
| fyear | File creation year |
| unit | File medium name |
| unitc | File medium type |
| nbchar | Number of characters transferred |
| nbr | Number of records transferred |


<span id="Network_file_information"></span>

### Network file information


| Field | Explanation |
| --- | --- |
| nidf | Network identifier of the file |
| npart | Remote partner |
| nfname | Network name of the file |
| nkeylen | Network key length (indexed) |
| nkeypos | Network key position (indexed) |


<span id="Transfer_information"></span>

### Transfer information

Field

Explanation

diagc

Additional diagnostics code

diagx

rfu ( complementary to diagp)

msg

Message text sent

parm

user-defined network parameter

profil

PeSIT only

Profile:

- D: PESIT
    E (DMZ)
- C: PESIT
    D CFT
- S: PESIT
    D SIT
- A: PESIT
    E

prottyp

Protocol type:

- P: PeSIT
- O: ODETTE

rappl

Receiving application

rpart

Receiving partner

ruser

Receiving user

sappl

Sending application

spart

Sending partner

suser

Sending user

ipart

Intermediate partner

ctime

Catalog deposit time (HHMMSSCC)

cdate

Catalog deposit date (YYYYMMDD)

cday

Catalog deposit day

cmonth

Catalog deposit month

cyear

Catalog deposit year

btime

Transfer start time (HHMMSSCC)

bdate

Transfer start date (YYYYMMDD)

bday

Transfer start day

bmonth

Transfer start month

byear

Transfer star year

etime

Transfer end time (HHMMSSCC)

edate

Transfer end date (YYYYMMDD)

eday

Transfer end day

emonth

Transfer end month

eyear

Transfer end year

xlate

Translation table identifier

group

Group

userid

User
identifier

cMode

SSL mode Client/Server

cAuthPolicy

SSL auth Anonymous/Simple/Double

bCipher

SSL cipher suite

sParm

SSL command free parameters

sRemoteUserDn

Remote User certificate Dn

sRemoteIssuerDn

Remote Issuer Dn

sRemoteCaId

Remote CA Alias

sUserCId

User Certificate Alias

sCertFname

File including Remote certificate

sProf

SSL profile Id.

sRemoteSerial

Serial Number

ExitFree

Free Area between all EXITs

XferCycleId

CycleId for tracking occurrences

XferObjectcId

Name of the transfer tracking class

Please refer to the CFTPARM page for additional parameters and details
&lt;/tbody&gt;

<span id="Information_input_returned_by_the_user"></span>

### Information input/returned by the user


| Field | Explanation |
| --- | --- |
| version | Exit version<br/> The value of this parameter is used to identify the version of the EXIT task supplied. |
| comment | Local comment<br/> This field can be modified by the user program. Any modifications are taken into account when the catalog is updated (if usraction = UPDATE with or without change of state). |
| state | State requested on UPDATE (D, H, K, X). See [Transfer states.](../#Transfer_state) |


<span id="Information_returned_by_the_user"></span>

### Information returned by the user


| Field | Explanation |
| --- | --- |
| usraction | Action requested by the user:<br/> • U: UPDATE<br/> • D: DELETE<br/> • N: NONE<br/> Note: If the value of the parameter is N (NONE), any changes to parameters that have been requested, for example, modifications made to the comment field, are ignored. |
| usrmsg | User message, 80 characters, recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} LOG file (CFTS18I)<br/> This message can contain the code returned by the user function and other information. |


<span id="Additional_information"></span>

### Additional information


| Field | Explanation |
| --- | --- |
| recblkn | Catalog block number |
| typ | Transfer type:<br/> • 1: FILE<br/> • 2: MSG<br/> • 3: REPLY |
| stated | Transfer acknowledgement |
| nspart | Transfer origin |
| nrpart | Transfer destination |
| idtp | Protocol identifier |
| idexit | EXIT identifier |
| parmexit | User EXIT parameter (PARM parameter of the CFTEXIT command) |
| language | Language:<br/> • C: C language<br/> • O: COBOL |

