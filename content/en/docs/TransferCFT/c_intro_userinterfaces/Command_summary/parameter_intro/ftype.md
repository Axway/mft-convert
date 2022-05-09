{
    "title": "ftype",
    "linkTitle": "ftype",
    "weight": "1380"
}<span id="ftype"></span>

### ftype

<span id="ftype_CFTRECV"></span>

#### CFTRECV, CFTSEND

****[ FTYPE = { c } ]  OS-specific****

The file type. Some FTYPE parameter values are OS specific. Refer to the Transfer CFT OS-specific documentation for more information.

> **Note**
>
> Note: When using the SFTP protocol and FTYPE = T, O, X, J or F, the file is considered a text file and there is no specific treatment according to the value. This means that the newline character (EOL character) can be the CRLF (x0Dx0A) or LF (x0A) on Windows, or LF (x0A) on UNIX systems.

****UNIX<span id="UNIX_ftype"></span>****

{{% TransferCFT/snippets/ftype_unix%}}

See also, [UNIX &gt; Transferable files](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/transferable_files_unix).

****Windows****

{{% TransferCFT/snippets/ftype_windows%}}

\*Supports ASCII file handling (X'1A') (SEND/RECV FTYPE=F).

See also, [Windows &gt; Transferable files](../../../../cft_intro_install/windows_install_start_here/windows_install_start_here/specific_system_functions/transferable_files_win).

****z/OS****

Implicit indicates that the FTYPE is automatically detected by the OS.

{{% TransferCFT/snippets/ftype_zOS%}}

> **Note**
>
> Note: FTYPE values are OS specific. Refer to the Transfer CFT z/OS documentation for more information.

****IBM i (OS400)****

****Native files****

The following table lists the different types of files that can be used according to the type of data to transfer.

> **Note**
>
> Note: Bold values indicate a recommended combination. For example, FTYPE=D and FRECFM=V, are the recommended settings for PF-DTA files with variable data.


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

The behavior of the values ‘’ and ‘ ’, for FTYPE and FRECFM respectively, are not detailed in the following table. These values correspond to `undefined`, meaning that the transfer in emission takes the value of both the file type and the member content..


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
| ‘J’  | ‘V’, ‘F’, ‘ ’  | Stream text is an alternative way to transfer a text file. Every line of a file must end with an LF or CR/LF. However, during a transfer the CR/LF are changed to LFs. This enables a quicker reading, and a faster transfer.<br/> When using stream text (FTYPE=J), the sender and the receiver must both have the FTYPE set to J. Setting only the sender or receiver to FTYPE=J results in unexpected content for the transferred file.<br/> <blockquote> **Note**<br/> Note: This transfer mode is not available for native side transfers.<br/> </blockquote>  |


> **Note**
>
> Note: FTYPE values are OS specific. Refer to the Transfer CFT IBM i Installation and Operations Guide for more information.

**HP NonStop**

For Unix files, use the values in the Unix [table](#UNIX_ftype) above. For native HP NonStop files, the values are as follows.

{{% TransferCFT/snippets/ftype_hp_ns%}}

****OpenVMS (VMS)****

The FTYPE is automatically detected when sending a file.

{{% TransferCFT/snippets/ftype_vms%}}

> **Note**
>
> Note: FTYPE values are OS specific. Refer to the Transfer CFT OpenVMS Installation and Operation Guide for more information.

[Return to Command index](../../)
