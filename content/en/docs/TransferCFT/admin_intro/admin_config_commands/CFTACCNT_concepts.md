---
title: "Recording  mode for statistical data"
linkTitle: "CFTACCNT - Recording mode for statistical data "
weight: 220
---The CFTACCNT object defines the recording mode for statistical data
of correctly terminated transfers. See also the parameter list
[CFTACCNT.](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftaccnt)

<span id="About_the_CFTACCNT_object"></span>

## About the CFTACCNT object

Two recording modes are available:

* Recording data
    in {{< TransferCFT/axwayvariablesComponentShortName >}} files. When the primary file is full, {{< TransferCFT/axwayvariablesComponentShortName >}}
    switches to an alternate file. This mode is available on all operating
    systems.
* Recording data
    in the files of the operating system accounting utility.

The CFTACCNT object can be permanently deleted from the configuration.
Once the CFTACCNT is deactivated, it appears gray in the left pane object
manager. For more information refer to Deleting
the CFTACCNT (UI) object.

In addition to defining the CFTACCNT object, for the statistical mode
to operate, you must define the CFT ACCNT parameter in the CFTPARM object.

CFTACCNT is used to define the recording mode of the statistical data
for correctly terminated transfers, that is transfers that are in the
T or X state. This command is taken into account when the ACCNT parameter
of the CFTPARM command is defined.

{{< TransferCFT/axwayvariablesComponentShortName  >}} authorizes only one identifier of the CFTACCNT type.

Two recording modes can be used, depending on the system:

* Recording of data
    in {{< TransferCFT/axwayvariablesComponentShortName >}} files. In this case, the CFTACCNT command defines the names of the files receiving
    the data and their management (parameter setting TYPE = FILE). This mode
    is available on ALL SYSTEMS.

<!-- -->

* Recording of data
    in the files of the accounting utility of the operating system in question
    parameter setting TYPE = SYST. This
    mode is only available on z/OS (MVS) systems.

For more information on the TYPE parameter in the statistical recording
mode, see [Recording mode TYPE](#Recordin). For each terminated transfer, {{< TransferCFT/axwayvariablesComponentShortName  >}} records the information
contained in the following table.

****CFTACCNT list of headings****


| Heading  | Offset<br/> V24 | Offset<br/> V23 |
| --- | --- | --- |
| Transfer mode (server or requester)  | 0  | 0  |
| Transfer direction  | 1  | 1  |
| Transfer type (File, Message or Reply message)  | 2  | 2  |
| Immediate partner identifier (PART)  | 3  | 3  |
| Sender identifier (SPART)  | 68  | 12  |
| Receiver identifier (RPART)  | 133  | 21  |
| Identifier of the sending user application (SUSER)  | 198  | 30  |
| Identifier of the receiving user application (RUSER)  | 231  | 46  |
| Model file identifier (IDF)  | 264  | 62  |
| Local application identifier (IDA)  | 297  | 71  |
| Transfer identifier (IDT)  | 330  | 80  |
| Number of records sent  | 339  | 89  |
| Number of characters in the file (FBYTE) | 350  | 100  |
| Number of characters sent over the line (NBYTE) | 361  | 111  |
| Date and time of command entry in the catalog  | 372  | 122  |
| Date and time of start of transfer  | 390  | 140  |
| Date and time of end of transfer  | 408  | 158  |
| Transfer duration (in seconds)  | 426  | 176  |
| Broadcasting list indicator  | 433  | 183  |
| Application protocol identifier  | 434  | 184  |
| Transfer owner identifier (USERID)  | 467  | 193  |
| Group identifier (GROUPID)  | 500  | 209  |
| On line data compression rate  | 533  | 225  |
| File record maximum size (FLRECL)  | 536  | 228  |
| File (FRECFM)  | 542  | 234  |
| Protocol compression code (NCOMP)  | 543  | 235  |
| Name of the file sent (FNAME)  | 546  | 238  |
| {{< TransferCFT/axwayvariablesComponentShortName  >}} private parameter (PARM)  | 1059  | 303  |
| Sender application identifier (SAPPL)  | 1572  | 384  |
| Receiver application identifier (RAPPL)  | 1621  | 433  |
| Partner group (GROUP)  | 1670  | 482 (*)  |
| Number of characters in the file (FBYTE_EXTENDED)  | 1703  |   |
| Number of characters sent over the line (NBYTE_EXTENDED)  | 1719  |   |
| Logic file network identifier (NIDF)  | 1735  |   |
| Unused  | 1768  |   |
| Total | 2048  | 491<br/> z/OS: 482 |


(\*) z/OS: For format V23 the partner group is not included in account structure. The total length is 482.

To use the account file in an application, you must refer to the header file that is delivered in the Transfer CFT runtime directory as either a cftcnt.h (C language for all platforms) or cftcnt.cop (COBOL on z/OS and IBM i) file.

<span id="Recordin"></span>

## Recording mode TYPE

This section describes the parameters to define the type of recording
mode. You can select from either the saving the recorded information in
a file, or in the system. Declaring where to save transfer information
is defined in the TYPE parameter.

<span id="Setting_TYPE_to_FILE"></span>

### Setting TYPE to FILE

When you
define the Type parameter to File
in the CFTACCNT object, you must define the following parameters as well.

* afname
* exec
* fname
* maxrec
* switch

The {{< TransferCFT/axwayvariablesComponentShortName  >}} statistical
file is full when the maximum number of records, the MAXREC parameter,
is reached. When this happens, the {{< TransferCFT/axwayvariablesComponentShortName  >}} switches to an alternate
file and executes the procedure defined by the EXEC parameter.

The SWITCH operating command also allows the operator to manually switch
the statistical file on request.

{{< TransferCFT/axwayvariablesComponentShortName  >}} requires at least 1 empty statistical file at the time
it is activated. If the first file designated by FNAME is not empty, Transfer
CFT switches to the second file labeled AFNAME. The procedure associated
with the switching can regenerate an empty file.

The statistical files must be created before the {{< TransferCFT/axwayvariablesComponentShortName  >}} is activated.
This operation is performed in the command CFTFILE TYPE = ACCNT.

When you shut down {{< TransferCFT/axwayvariablesComponentShortName  >}}, using the SHUT command, {{< TransferCFT/axwayvariablesComponentShortName  >}}
executes the switching procedure to empty the last statistical file in
process.

<span id="Setting_TYPE_to_SYST"></span>

### Setting TYPE to SYST

When you
define the Type parameter as SYST
in the CFTACCNT object, you must define the ACCID
parameter as well.

The CFTACCNT command references
the {{< TransferCFT/axwayvariablesComponentShortName  >}} application via the utility.


| Protocol  | Details  |
| --- | --- |
| z/OS (MVS) | The ACCID parameter identifying the {{< TransferCFT/axwayvariablesComponentShortName  >}} application must be defined. |


Syntax

CFTACCNT TYPE = FILE

`TYPE   = FILE`

`FNAME   = filename `

`ID   = identifier `

`[ AFNAME   = filename ]`

`[ COMMENT   = string ]`

`[ EXEC   = filename ]`

`[ LANGUAGE   = { COBOL   &#124; C } ]`

`[ MAXREC   = { 0   &#124; n } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`

`[ SWITCH   = { 00000000   &#124; time } ]`

`[ FORMAT   = { V23   &#124; 23 &#124; V24 &#124; 24} ]`

 

CFTACCNT TYPE = SYST

`TYPE   = SYST`

`ACCID   = n `

`ID   = identifier `

`[ COMMENT   = string ]`

`[ FORMAT   = { V23   &#124; 23 &#124; V24 &#124; 24} ]`

`[ LANGUAGE   = { COBOL   &#124; C } ]`

`[ MODE   = { REPLACE   &#124; CREATE &#124; DELETE } ]`
