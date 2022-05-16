---

    title: multart
    linkTitle: multart
    weight: 2050

---
<span id="multart"></span>

### multart

#### CFTPROT

****\[MULTART = { <span style="text-decoration: none;">NO</span>
| <u>YES</u> } \]****

An option to group several records of the files sent in one FPDU.

- <span style="font-weight: bold;">****YES:****</span>In sender mode yes is recommended
    if the partner supports multi-record FPDUs (default = yes). In receiver mode the Transfer
    CFT accepts multi-record FPDUs regardless of the value of this
    parameter.
- <span style="font-weight: bold;">****NO:****</span> One FPDU contains
    only one record. Only applicable in sender mode.

[Return to Command index](../../)
