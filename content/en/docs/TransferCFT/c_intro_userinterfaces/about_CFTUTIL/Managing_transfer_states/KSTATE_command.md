---

    title: KSTATE - Suspend a transfer when offline
    linkTitle: KSTATE - Suspending a catalog transfer
    weight: 340

---
The KSTATE
command is used to suspend a transfer in the catalog when the Transfer CFT is offline. {{< TransferCFT/axwayvariablesComponentLongName  >}} must
be shut down before the command is run and then restarted. The transfer
must exist in the catalog and be in one of the following phasesteps: in process
<span style="font-weight: bold;">****C****</span>, available <span style="font-weight: bold;">****D****</span>,
or hold <span style="font-weight: bold;">****H****</span>. After execution of
the command, the phasestep is set to <span style="font-weight: bold;">****K****</span>.

The command generates a WLOG command which reports the event in the
LOG file.


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a> | Model file identifier. |
| <a href="../../../command_summary/parameter_intro/idtu">IDTU</a> | Local transfer counter identifier. |
| <a href="../../../command_summary/parameter_intro/part">PART</a><br/> **(Mandatory)** | Identifier of the partner. |

