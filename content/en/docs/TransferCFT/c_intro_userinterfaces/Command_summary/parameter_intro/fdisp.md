---
    "title": "fdisp",
    "linkTitle": "fdisp",
    "weight": "1130"
---
<span id="fdisp"></span>

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
> Note: The value "OLD" is deprecated and no longer available for SEND/CFTSEND.

> **Note**
>
> Note: The CHECK feature is disabled on z/OS platforms (no action occurs when FDISP=CHECK).

> **Note**
>
> Caution  
> When FDISP is set to CHECK, Transfer CFT performs an FSTAT for each record, which has a significant negative impact on performance.

#### CFTRECV, RECV

****[FDISP = { NEW &#124; OLD &#124; <span class="underline">BOTH</span>
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

{{% TransferCFT/snippets/fdisp_w_faction%}}

[Return to Command index](../../)
