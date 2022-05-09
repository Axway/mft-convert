{
    "title": "KSTATE - Suspend a transfer when offline",
    "linkTitle": "KSTATE - Suspending a catalog transfer",
    "weight": "330"
}The KSTATE
command is used to suspend a transfer in the catalog when the Transfer CFT is offline. {{< TransferCFT/axwayvariablesComponentLongName  >}} must
be shut down before the command is run and then restarted. The transfer
must exist in the catalog and be in one of the following phasesteps: in process
****C****, available ****D****,
or hold ****H****. After execution of
the command, the phasestep is set to ****K****.

The command generates a WLOG command which reports the event in the
LOG file.


| Parameter  | Description  |
| --- | --- |
| [IDF](../../../command_summary/parameter_intro/idf) | Model file identifier. |
| [IDTU](../../../command_summary/parameter_intro/idtu) | Local transfer counter identifier. |
| [PART](../../../command_summary/parameter_intro/part)<br/> **(Mandatory)** | Identifier of the partner. |

