{
    "title": "Map Transfer CFT and Sentinel states",
    "linkTitle": "Mapping Transfer CFT and Sentinel states",
    "weight": "250"
}You can use the ****Mapping states**** table to find equivalent states and phases as described below.

- Phase: The phase indicates where you are in your transfer.
- Phasestep: Within each phase there is a phase step, which is either a process or a step.
- Diag: Diagnostic codes provide a explanation of the source of the error detected or the refusal.
- {{< TransferCFT/axwayvariablesComponentShortName  >}} state: The {{< TransferCFT/axwayvariablesComponentShortName  >}} status as displayed in the catalog.
- Compatibility state: The {{< TransferCFT/axwayvariablesComponentShortName  >}} status as displayed in the catalog when using the backward compatibility mode.
- Sentinel state: The state attribute identifies the step of the transfer process.

For details on Sentinel states, see [XFBTransfer Tracked Objects]().

Mapping states

In the following table, the state that is sent to Sentinel when the COMPAT parameter is set to NO, is displayed in the Sentinel state column. When COMPAT is set to YES, the state listed in the Compatible Sentinel state column is sent to Sentinel. For more information on the COMPAT settings, see [{{< TransferCFT/axwayvariablesComponentShortName  >}} backward compatibility](../../../concepts/phase_and_phasestep/processing_compatability).


|   | Phase | Phasestep | Transfer CFT State | Transfer CFT Compatible State (uconf:<br/> cft.state_compat=Yes) | Diagi | Acked | Sentinel<br/> State | Compatible Sentinel State (uconf:<br/> cft.state_compat=Yes) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Pre-processing | A | H | A | A | 0 |   | PRE_PROC | AVAILABLE |
|   | A | D | A | A |   |   | PRE_PROC | TO_EXECUTE |
|   | A | C | A | A |   |   | PRE_PROC | TO_EXECUTE |
|   | A | K | A | A | 121 |   | PRE_PROC_ABORT | CANCELED |
|   |   |   |   |   |   |   |   |   |
| Transfer | T | H | H | H | 0 |   | AVAILABLE | AVAILABLE |
|   | T | D | D | D |   |   | TO_EXECUTE | TO_EXECUTE |
|   | T  | D  | D  | D  | NOT = 0  |   | INTERRUPTED  | INTERRUPTED  |
|   | T | C | C | C |   |   | SENDING/RECEIVING | SENDING/RECEIVING |
|   | T | K | K | K |   |   | CANCELED | CANCELED |
|   | T | H | H | H | 121 |   | SUSPENDED | SUSPENDED |
|   | T | H | H | H | 621 |   | INTERRUPTED | INTERRUPTED |
|   |   |   |   |   |   |   |   |   |
| Post-processing | Y | E | Y | T/X |   |   | POST_PROC | SENT/RECEIVED |
|   | Y | H | Y | T/X |   |   | POST_PROC | SENT/RECEIVED |
|   | Y | D | Y | T/X |   |   | POST_PROC | SENT/RECEIVED |
|   | Y | C | Y | T/X |   |   | POST_PROC | SENT/RECEIVED |
|   | Y | C | Y | T/X |   | A | POST_PROC | ENDED-TO-ACK/ACKED |
|   | Y | C | Y | T/X |   | N | POST_PROC | ENDED-TO-NACK/NACKED |
|   | Y | K | Y | T/X |   |   | POST_PROC_ABORT | SENT/RECEIVED |
|   | Y | K | Y | T/X |   | A | POST_PROC_ABORT | ENDED-TO-ACK/ACKED |
|   | Y | K | Y | T/X |   | N | POST_PROC_ABORT | ENDED-TO-NACK/NACKED |
|   |   |   |   |   |   |   |   |   |
| Ack-processing | Z | H | Z | T/X |   |   | ACK_EXPECTED | SENT/RECEIVED |
|   | Z | D | Z | T/X |   |   | POST_PROC_ACK | SENT/RECEIVED |
|   | Z | D | Z | T/X |   | A | ENDED-TO-ACK/ACKED | ENDED-TO-ACK/ACKED |
|   | Z | D | Z | T/X |   | N | ENDED-TO-NACK/NACKED | ENDED-TO-NACK/NACKED |
|   | Z | C | Z | T/X |   |   | POST_PROC_ACK | SENT/RECEIVED |
|   | Z | C | Z | T/X |   | A | POST_PROC_ACK | ENDED-TO-ACK/ACKED |
|   | Z | C | Z | T/X |   | N | POST_PROC_ACK | ENDED-TO-NACK/NACKED |
|   | Z | K | Z | T/X |   |   | POST_PROC_ACK_ABORT | SENT/RECEIVED |
|   | Z | K | Z | T/X |   | A | POST_PROC_ACK_ABORT | ENDED-TO-ACK/ACKED |
|   | Z | K | Z | T/X |   | N | POST_PROC_ACK_ABORT | ENDED-TO-NACK/NACKED |
|   |   |   |   |   |   |   |   |   |
| Done | X | X | X | T/X |   |   | COMPLETED | CONSUMED |
|   |   |   |   |   |   |   |   |   |

