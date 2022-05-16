---

    title: ftype
    linkTitle: ftype
    weight: 1390

---
<span id="ftype"></span>

### ftype

<span id="ftype_CFTRECV"></span>

#### CFTRECV, CFTSEND

****\[ FTYPE = { c } \]  OS-specific****

The file type. Some FTYPE parameter values are OS specific. Refer to the Transfer CFT OS-specific documentation for more information.

> **Note**
>
> When using the SFTP protocol and FTYPE = T, O, X, J or F, the file is considered a text file and there is no specific treatment according to the value. This means that the newline character (EOL character) can be the CRLF (x0Dx0A) or LF (x0A) on Windows, or LF (x0A) on UNIX systems.

****UNIX<span id="UNIX_ftype"></span>****


| FTYPE  | FCODE  | Type of sent file  |
| --- | --- | --- |
| ‘ ‘  | BINARY  | Binary  |
| B  | BINARY  | Binary  |
| V  | BINARY  | Binary file emulating locally a variable file format  |
| T  | ASCII  | Text file with LF or CRLF as end-of-line separator  |
| O  | ASCII  | Text file with CRLF as end-of-line separator  |
| X  | ASCII  | Text file with LF as end-of-line separator  |
| J  | ASCII  | Stream text<br/> Using stream text (J) allows a text type file to be sent that contains records that exceed 32 KB. As opposed to text type (FTYPE=T), stream text does not add an EOL sequence (LF or CRLF) to the received file.<br/> When using stream text (FTYPE=J), the sender and the receiver must both have the FTYPE set to J. Setting only the sender or receiver to FTYPE=J results in unexpected content for the transferred file. |


See also, [UNIX &gt; Transferable files](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/aix_with_ibm_hacmp_intro/specific_configurations_intro/transferable_files_unix).

****Windows****


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


\*Supports ASCII file handling (X'1A') (SEND/RECV FTYPE=F).

See also, [Windows &gt; Transferable files](../../../../cft_intro_install/windows_install_start_here/windows_install_start_here/specific_system_functions/transferable_files_win).

****z/OS****

Implicit indicates that the FTYPE is automatically detected by the OS.


| FTYPE  | FCODE  | Type of sent file  |
| --- | --- | --- |
| Implicit  | BINARY  | Disk sequential files |
| Implicit  | BINARY  | Members of PDS files (1 transfer per member) |
| Implicit  | BINARY  | Designated version of a file in GDG |
| Implicit  | BINARY  | Multi-volume disk file |
| Implicit  | BINARY  | VSAM KSDS or ESDS file |
| A | BINARY  | Print file with ASA jump codes (z/OS to z/OS) |
| M | BINARY  | Print file with machine jump codes (z/OS to z/OS) |
| S | BINARY  | Spanned variable format file (z/OS to z/OS) |


<span class="autonumber"></span>HFS file characteristics


| FTYPE  | FCODE  | Type of sent file  |
| --- | --- | --- |
| B | BINARY  | Binary file (default) |
| T | EBCDIC  | Text file |
| J  | EBCDIC  | Stream text<br/> Using stream text (J) allows a text type file to be sent that contains records that exceed 32 KB. As opposed to text type (FTYPE=T), stream text does not add an EOL sequence (LF or CRLF) to the received file.<br/> When using stream text (FTYPE=J), the sender and the receiver must both have the FTYPE set to J. Setting only the sender or receiver to FTYPE=J results in unexpected content for the transferred file. |


> **Note**
>
> FTYPE values are OS specific. Refer to the Transfer CFT z/OS documentation for more information.

****IBM i (OS400)****

****Native files****

The following table lists the different types of files that can be used according to the type of data to transfer.

> **Note**
>
> Bold values indicate a recommended combination. For example, FTYPE=D and FRECFM=V, are the recommended settings for PF-DTA files with variable data.

QQQ\_QQQ\_QQQ


| FTYPE | FRECFM | Supported files and data organizations (if applicable). |
| --- | --- | --- |
| ‘D’  | ‘F’  | **PF-DTA with fixed data**, PF-DTA with variable data, PF-SRC, SAVF  |
| ‘D’  | ‘V’  | PF-DTA with fixed data, **PF-DTA with variable data**, PF-SRC, SAVF  |
| ‘S’  | ‘F’  | PF-DTA with fixed data, PF-DTA with variable data, **PF-SRC** |
| ‘S’  | ‘V’  | PF-DTA with fixed data, PF-DTA with variable data, PF-SRC  |
| ‘E’  | ‘F’  | PF-DTA with fixed data, PF-DTA with variable data, **PF-SRC**  |
| ‘E’  | ‘V’  | PF-DTA with fixed data, PF-DTA with variable data, PF-SRC  |
| ‘Z’  | ‘F’, ‘V’  | **SAVF**  |


****Default FTYPE or FRECFM value****

The behavior of the values ‘’ and ‘ ’, for FTYPE and FRECFM respectively, are not detailed in the following table. These values correspond to <span class="code">`undefined`</span>, meaning that the transfer in emission takes the value of both the file type and the member content..


|   | Default FTYPE | Default FRECFM |
| --- | --- | --- |
| PF-DTA<br /> Member containing fixed data | ‘D’  | ‘F’  |
| PF-DTA<br /> Member containing variable data | ‘D’  | ‘V’  |
| PF-SRC  | ‘D’  | ‘F’  |
| SAVF  | ‘Z’  | ‘F’  |


****IFS files****

The following table lists the different types of files that can be used according to the type of data to transfer when using IFS.


| FTYPE  | FRECFM  | Type of file  |
| --- | --- | --- |
| ‘S’  | ‘V’, ‘F’, ‘ ’  | Text  |
| ‘D’ , ‘ ’  | ‘V’, ‘F’, ‘ ’  | Text  |
| ‘E’  | ‘V’, ‘F’, ‘ ’  | Text  |
| ‘Z’  | ‘V’, ‘F’, ‘ ’  | Binary  |
| ‘J’  | ‘V’, ‘F’, ‘ ’  | Stream text is an alternative way to transfer a text file. Every line of a file must end with an LF or CR/LF. However, during a transfer the CR/LF are changed to LFs. This enables a quicker reading, and a faster transfer.<br/> When using stream text (FTYPE=J), the sender and the receiver must both have the FTYPE set to J. Setting only the sender or receiver to FTYPE=J results in unexpected content for the transferred file.<br/> <blockquote> **Note**<br/> This transfer mode is not available for native side transfers.<br/> </blockquote>  |


> **Note**
>
> FTYPE values are OS specific. Refer to the Transfer CFT IBM i Installation and Operations Guide for more information.

**HP NonStop**

For Unix files, use the values in the Unix [table](#UNIX_ftype) above. For native HP NonStop files, the values are as follows.


| FTYPE  | FCODE  | Type of sent file  |
| --- | --- | --- |
| ‘ ‘  | BINARY  | Binary  |
| B  | BINARY  | Binary  |
| V  | BINARY  | Binary file emulating locally a variable file format  |
| T  | ASCII  | Text file with LF or CRLF as end-of-line separator  |
| O  | ASCII  | Text file with CRLF as end-of-line separator  |
| X  | ASCII  | Text file with LF as end-of-line separator  |
| J  | ASCII  | Text file with LF or CRLF as end-of-line separator<br/> FTYPE = J refers to stream text. The stream text type allows sending a text file that contains records that are larger than 32 KB. Unlike classical text types (T, O, X) the stream text type does not add an EOL sequence (LF or CRLF) at the end of the received file. |
| E  | ASCII  | Edit native files.  |
| N  | BINARY  | Non-edit native file (force the detection of native files rather than OSS ones).  |


[Return to Command index](../../)
