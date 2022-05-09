{
    "title": "segment",
    "linkTitle": "segment",
    "weight": "3140"
}<span id="segment"></span>

segment
-------

#### CFTPROT

**[SEGMENT     = { NO
&#124; <span class="underline">YES</span> }]** Only in sender mode          **Profile = ANY**

Option to segment file records in several FPDUs.

This option is not negotiated.

- **NO**: Segmentation
    "if necessary", which is automatically implemented when a record to be transferred is greater
    than the maximum size of an NSDU - 6 bytes. This relates to data FPDUs
    and not to the other protocol FPDUs.  
    Remember that the size of NSDUs being sent is negotiated using the SRUSIZE
    parameter of CFTPROT.

<!-- -->

- **YES** (default): "Systematic"
    segmentation, which is implemented by {{< TransferCFT/suitevariablesTransferCFTName  >}} to complete a "data unit" (NSDU).
    This option relates to all record sizes, and is only effective
    if used with the option CONCAT = YES.

This value may only be adopted (SEGMENT = YES) with
a Transfer CFT that supports concatenation of DTF (complete record) and DTFDA
(start of record), DTFMA (middle of record) or DTFFA (end of record) FPDUs
in a given data unit (NSDU).

In receiver mode, the {{< TransferCFT/axwayvariablesComponentShortName  >}} accepts
the FPDUs of segmented records regardless of the local value of the SEGMENT
parameter.

[Return to Command index](../../)
