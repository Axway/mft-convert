---
    "title": "Transferable files ",
    "linkTitle": "Transferable files",
    "weight": "320"
---
This
topic describes the Transfer
CFT parameters that are specific to UNIX concerning the characteristics of a transferable file.

- Characteristics of files automatically detected (or not) on transmission
- FTYPE and FCODE values implicitly
    associated during transmission
- FTYPE and FRECFM values on receipt

#### Characteristics of files automatically detected on transmission


| Parameter  | Automatically detected on transmission  |
| --- | --- |
| FSPACE  | YES  |
| FLRECL  | NO |
| FBLKSIZE  | NO  |
| FRECFM  | NO |
| FTYPE  | NO |


#### FTYPE values and FCODE values implicitly associated during transmission

{{% TransferCFT/snippets/ftype_unix%}}

FTYPE = J refers to stream text. The stream text type allows sending a text file that contains records that are larger than 32 KB. Unlike classical text types (T, O, X) the stream text type does not add an EOL sequence (LF or CRLF) at the end of the received file.

{{% TransferCFT/snippets/ftype_note%}}

#### FTYPE and FRECFM values on receipt


| FTYPE  | FRECFM  | Type of received file  |
| --- | --- | --- |
| B  | F  | Binary fixed-length sequential file  |
| B  | U /V | Binary sequential file  |
| V  | V  | Binary file emulating locally a variable file format  |
| T  | F  | Fixed-length sequential text file with LF as end-of-line separator  |
| T  | U /V  | Variable length sequential text file with LF as end-of-line separator  |
| O  | F  | Fixed-length sequential text file with CRLF as end-of-line separator  |
| O  | U/V  | Variable length sequential text file with CRLF as end-of-line separator  |
| X  | F  | Fixed-length sequential text file with LF as end-of-line separator  |
| X  | U/V  | Variable length sequential text file with LF as end-of-line separator  |
| J  | U/V  | Variable length sequential text file with LF as end-of-line separator  |


These values are either given explicitly in the CFTRECV command or deduced
from the protocol values received.

- On request Transfer CFT performs an access control on the files transferred.
    It determines, for example, if the initiator of the send request has read
    access rights on the file to be sent.
- On receipt, Transfer CFT creates the file if it does not exist.
- The organization, FORG, of the files sent or received by Transfer CFT
    is sequential.
