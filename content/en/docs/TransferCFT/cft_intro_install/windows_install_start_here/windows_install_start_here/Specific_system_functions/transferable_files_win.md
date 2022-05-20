---
title: "Transferable files "
linkTitle: "Transferable files"
weight: 230
---This
topic describes the Transfer
CFT parameters that are specific to Windows concerning the characteristics of the transferable file.

- Characteristics
    of files automatically detected on transmission
    &lt;/li>
- FTYPE
    values and FCODE values implicitly associated during transmission
- FTYPE
    and FRECFM values on receipt

#### Characteristics of files automatically detected on transmission


| Parameter  | Automatically detected on transmission  |
| --- | --- |
| FSPACE  | YES  |
| FLRECL  | NO  |
| FBLKSIZE  | NO  |
| FRECFM  | NO  |
| FTYPE  | NO  |


#### FTYPE values and FCODE values implicitly associated during transmission


| FTYPE  | FCODE  | Type of sent file  |
| --- | --- | --- |
| ' '  | BINARY  | Binary  |
| B  | BINARY  | Binary  |
| V  | BINARY  | Binary file emulating locally a variable file format  |
| T  | ASCII  | Text file with LF or CRLF as end-of-line separator  |
| F  | ASCII  | Text file where the last character '1A' is transmitted (is not considered an EOF character)  |
| O  | ASCII  | Text file with CRLF as end-of-line separator  |
| X  | ASCII  | Text file with LF as end-of-line separator  |
| J  | ASCII  | Stream text<br/> Using stream text (J) allows a text type file to be sent that contains records that exceed 32 KB. As opposed to text type (FTYPE=T), stream text does not add an EOL sequence (LF or CRLF) to the received file.<br/> When using stream text (FTYPE=J), the sender and the receiver must both have the FTYPE set to J. Setting only the sender or receiver to FTYPE=J results in unexpected content for the transferred file. |


FTYPE = J refers to stream text.The stream text type allows sending a text file that contains records that are larger than 32 KB. Unlike classical text types (T, O, X) the stream text type does not add an EOL sequence (LF or CRLF) at the end of the received file.

> **Note**
>
> FTYPE J is available in Transfer CFT Transfer CFT 3.0.1 SP7 (UNIX and Windows) and higher.

#### FTYPE and FRECFM values on receipt


| FTYPE  | FRECFM  | Type of received file  |
| --- | --- | --- |
| B  | F  | Binary fixed-length sequential file  |
| B | U/V | Binary sequential file  |
| V  | V | Binary file emulating locally a variable file format  |
| T  | F  | Fixed-length sequential text file with CRLF as end-of-line separator  |
| T  | U/V | Variable length sequential text file with CRLF as end-of-line separator  |
| O  | F  | Fixed-length sequential text file with CRLF as end-of-line separator  |
| O  | U/V  | Variable length sequential text file with CRLF as end-of-line separator  |
| X | F  | Fixed-length sequential text file with LF as end-of-line separator  |
| X  | U/V  | Variable length sequential text file with LF as end-of-line separator  |
| J  | U/V  | Variable length sequential text file with CRLF as end-of-line separator  |

