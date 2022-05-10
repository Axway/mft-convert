---
    "title": "faction",
    "linkTitle": "faction",
    "weight": "1030"
---
<span id="faction"></span>

### faction

<span id="faction_CFTSEND"></span>

#### CFTSEND, SEND

**[FACTION = {NONE
&#124; DELETE &#124; ERASE &#124; ARCHIVE }]**

Action on the file after a send transfer:

- ****NONE****: No action on this file on completion
    of the transfer.
- ****DELETE****: Delete the file after transfer. Note the following conditions:
    -   No delete occurs if you are using SELFNAME and the FNAME is set to a directory mask (for example, \#dir is deleted, but \#dir/\* is ignored).
    -   If a file is added to the directory while a transfer is in progress, neither this new file nor is the directory is deleted.
- ****ERASE****: Erase the contents of the file
    after the transfer ("End Of File" mark at the beginning of the
    file)
- **ARCHIVE**: The source file is moved to the file name specified in the ARCHIVEFNAME parameter when the transfer is completed. If the transfer fails, the file is not moved. If the target file already exists, it is overwritten.

****FACTION limitations****

- ERASE is not supported when using a group of files in homogeneous mode, or when broadcasting (CFTDEST ).
- DELETE is not supported when broadcasting (CFTDEST ).
- ARCHIVE is not supported when using a group of files (homogeneous transfers), or when broadcasting (CFTDEST).
- OS-specific limitations:
    -   z/OS: VSAM, PDS, and GDG files are not supported.
    -   HP NonStop: ARCHIVE is not supported with Guardian files.

#### CFTRECV, RECV

**[FACTION = {VERIFY
&#124; DELETE &#124; ERASE &#124; RENAME &#124; RETRYRENAME }]**

> **Note**
>
> Note: The RENAME option is only available on Unix platforms.

Action on a file before a receive transfer except when using RENAME or RETRYRENAME, which are post transfer actions.

If a receiver file with the same name already exists, {{< TransferCFT/axwayvariablesComponentLongName  >}} performs
one of the following actions:

- ****VERIFY****: checks that the file is empty before the transfer occurs
- ****DELETE****:
    deletes the file before the transfer occurs
- ****ERASE****:
    erases the contents of the file before the transfer occurs
- **RENAME**: replaces the existing FNAME file after the transfer completes by renaming the WFNAME file (*Unix only*)
- **RETRYRENAME**: Renames the file on transfer completion in the post-processing phase, and includes a configurable retry mechanism. See also [Post-transfer file renaming](../../../../app_integration_intro/spoolout).

Requirements when using RENAME or RETRYRENAME:

- If the CFTRECV WFNAME is not defined, the transfer fails with DIAGI=138.
- The user (userid) who performs the transfer must have adequate rights to rename the file WFNAME to file FNAME. If not, the transfer fails with DIAGI=156.

{{% TransferCFT/snippets/fdisp_w_faction%}}


| OS  | Details  |
| --- | --- |
| z/OS | For VSAM files, only FACTION = ERASE is accepted. |


[Return to Command index](../../)[ ](#)
