---
title: "fdisp"
linkTitle: "fdisp"
weight: 1130
--- <span id="fdisp"></span>

### fdisp

<span id="fdisp_CFTRECV"></span>

#### CFTSEND, SEND

****[FDISP = { SHR
&#124; CHECK }]****

File sharing option:

- SHR: Shared access, which means that
    a file can be transferred at the same time for two partners.
- CHECK:
    If the file is modified during a transfer, the transfer is aborted.

> **Note**
>
> The value "OLD" is deprecated and no longer available for SEND/CFTSEND.

> **Note**
>
> The CHECK feature is disabled on z/OS platforms (no action occurs when FDISP=CHECK).

> **Note**
>
> Caution  
> When FDISP is set to CHECK, Transfer CFT performs an FSTAT for each record, which has a significant negative impact on performance.

#### CFTRECV, RECV

****[FDISP = { NEW &#124; OLD &#124; <u>BOTH</u>
}]****

Presence check indicator of the receiver file used to determine the
action of the {{< TransferCFT/axwayvariablesComponentShortName  >}}:

- NEW: The file must be created by Transfer
    CFT. The transfer is refused if this file already exists
- OLD: The file must already exist.
- BOTH: If the file does not exist, it
    is created.

<span id="fdisp_CFTSEND"></span>

#### Combined parameter actions

The following table shows the combined effect of the FDISP and FACTION parameters when used in a RECV command.

> **Note**
>
> There no impact on FDISP when used in combination with RENAME or RETRYRENAME.

| CFTRECV, FDISP  | CFTRECV, FACTION  | Comments  |
| - - - | - - - | - - - |
| both  | delete  | If no file exists, the file is created. If file exists it is deleted and recreated (regardless of if it is empty or not).  |
| both  | erase  | If no file exists, the file is created. If file exists it is overwritten (no matter if it is empty or not).  |
| both  | verify  | If no file exists, the file is created. If file exists and it is not empty, the transfer is aborted. If file exists but it is empty, the file is overwritten.  |
| new  | verify  | If no file exists, the file is created. If file exists the transfer is aborted (regardless of if it is empty or not).  |
| old  | delete  | If no file exists, the transfer is aborted. If file exists the file is deleted and recreated (regardless of if it is empty or not).  |
| old  | erase  | If no file exists, the transfer is aborted. If file exists the file is overwritten (regardless of if it is empty or not).  |
| old  | verify  | If no file exists, the transfer is aborted. If file exists and it is not empty, the transfer is aborted. If file exists but it is empty, the file is overwritten.  |

[Return to Command index](../../)
