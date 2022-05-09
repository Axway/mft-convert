{
    "title": "trk",
    "linkTitle": "trk",
    "weight": "3610"
}<span id="trk"></span>

### trk

<span id="trk_CFTRECV"></span><span id="trk_CFTSEND"></span>

#### CFTRECV, CFTSEND, RECV, SEND

****[TRK = {<span class="underline">"value of TRKSEND/TRKRECV from CFTPARM</span>"
&#124; ALL &#124; NO &#124; SUMMARY &#124; UNDEFINED}]****

Specifies how much detail {{< TransferCFT/axwayvariablesComponentShortName  >}} provides
Sentinel about transfers. {{< TransferCFT/axwayvariablesComponentShortName  >}} sends detail about the
transfers in the form of tracked instances.

The possible values of this parameter include:

- NO: The
    monitor never sends tracked instances to Sentinel
- ALL:
    For each step of each transfer process, the monitor sends a tracked instance
    to Sentinel
- SUMMARY:
    For both the initial step and the final step of each transfer process,
    the monitor sends a tracked instance to Sentinel
- UNDEFINED: The
    tracking options are defined in the TRK parameter of the CFTPART command

<span id="trk_CFTPART"></span>

#### CFTPART

**[TRK = { ALL
&#124; NO &#124; SUMMARY &#124; <span class="underline">UNDEFINED</span>}]**

Specification of how much detail {{< TransferCFT/axwayvariablesComponentShortName  >}} provides Sentinel about transfers
with a partner. {{< TransferCFT/axwayvariablesComponentShortName  >}} sends detail about the transfers in the form of tracked
instances. The possible values of this parameter include:

- NO: The monitor
    never sends tracked instances to Sentinel
- ALL: For each
    step of each transfer process, the monitor sends a tracked instance to
    Sentinel
- SUMMARY: For both
    the initial step and the final step of each transfer process, the monitor
    sends a tracked instance to Sentinel
- UNDEFINED: The
    tracking options are defined in the TRK parameter of the CFTPART command

[Return to Command index](../../)
