{
    "title": "XFBTransfer system attributes",
    "linkTitle": "XFBTransfer system attributes",
    "weight": "220"
}This section provides information on the following attributes:

- [Monitoring errors](#Monitori)
- [Monitoring tracked-event messages](#Monitori2)
- [Monitoring processing cycles](#Monitori3)
- [List of Sentinel states](#List)

<span id="Monitori"></span>

Monitoring errors
-----------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| IsException<br/>  | Integer<br/>  |   | Values:<br/> • 0: The relevant Tracked-Event Message does not describe an exception.<br/> • 1: The relevant Tracked-Event Message describes one or more exceptions. | Not used |
| IsAlert<br/>  | Integer<br/>  |   | Values:<br/> • 0: No alert is associated with the relevant Tracked-Event Message.<br/> • 1: The relevant Tracked-Event Message is associated with a processing exception that generated an alert. | IsAlert |
| ReturnCode | String | 20 | Processing details that the relevant tracked application generated. When the value of IsAlert is 1, this attribute often contains an error code. | DIAGI + DIAGP |
| ReturnMessage | String | 250 | Processing details that the relevant tracked application generated | DIAGC |


<span id="Monitori2"></span>

Monitoring tracked-event messages
---------------------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| GMTDiff | Int. |   | Positive or negative difference between the GMT (Greenwich Mean Time) and the local time, expressed in minutes. | uconf:sentinel.trkgmtdiff |
| ProductName | String | 50 | Name of the product that generated the relevant Tracked-Event. For customer applications this name is defined via the Universal Agent. | uconf: sentinel.trkproductname |
| ProductIPAddr | String | 20 | Domain Name Server (DNS) of the product/application that generated the relevant Tracked Event. | uconf: sentinel.trkproductipaddr |
| ProductOS | String. | 20 | Operating system of the application that generated the relevant Tracked Event. | {{< TransferCFT/axwayvariablesComponentShortName  >}} target |
| State | String | 29 | Status of the relevant Tracked Event. The possible values of this attribute depend on the tracked application/product and file transfer protocol used. See [List of Sentinel states](#List). | PHASE/PHASESTEP combination |


<span id="Monitori3"></span>

Monitoring processing cycles
----------------------------


| Sentinel<br/> attribute | Data Type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| CycleId | String | 250 | CycleId of the relevant Tracked Event. | TRKR |
| IsEnd | Int. |   | Values:<br/> • 0: The relevant Processing Cycle is not complete.<br/> • 1 : The relevant Processing Cycle is complete. | IsEnd set to 1 when PHASE=X |


<span id="List"></span>

List of Sentinel states
-----------------------

### Roles of transfer partners

Although each transfer occurs between only two transfer partners, each transfer partner plays two roles:

- Sender or Receiver  (the file is sent or received)
- Requester or Server (the transfer request is sent or received)

For a given transfer, only the following combinations of partner roles are possible:

- Sender/Requester and Receiver/Server (the partner that sent the file requested the transfer)
- Receiver/Requester and Sender/Server the partner that received the file requested the transfer)

In the XFBTransfer Tracked Object, the Attributes ‘Direction” and “IsServer” reflect these roles (see below).

****Receiver/Server transfer states****


| ****State**** | ****Compatible Sentinel State**** | ****IsEnd**** | ****IsAlert**** | ****IsException**** | ****Description**** |
| --- | --- | --- | --- | --- | --- |
| RECEIVING | RECEIVING | 0 | 0 | 0 | File data transmission in progress |
| CANCELED | CANCELED | 0 | 1 | 1 | File data transmission locally canceled (KEEP command) |
| SUSPENDED | SUSPENDED | 0 | 0 | 1 | File data transmission locally suspended (HALT command) |
| INTERRUPTED | INTERRUPTED | 0 | 1 | 1 | File data transmission remotely suspended |
| RECEIVED | RECEIVED | 0 | 0 | 0 | File data transmission ended |
| TO_ROUTE | TO_ROUTE | 0 | 0 | 0 | Received file to be routed (only on the relay site in store and forward mode) |
| POST_PROC | RECEIVED | 0 | 0 | 0 | Post processing in progress |
| POST_PROC_ABORT | RECEIVED | 0 | 1 | 1 | Post processing aborted by the application (KEEP command) |
| ACK_EXPECTED | RECEIVED | 0 | 0 | 0 | Waiting for a local acknowledgement |
| ACKED | ACKED | 1 | 0 | 0 | Transfer locally acknowledged by the application |
| POST_PROC_ACK_ABORT | RECEIVED/ACKED/NACKED | 0 | 1 | 1 | Post processing of the Acknowledgement phase aborted by the application (KEEP command) |
| COMPLETED | CONSUMED | 1 | 0 | 0 | Transfer completed (this state is replaced by ROUTED on the relay site in store and forward mode) |
| ROUTED | ROUTED | 1 | 0 | 0 | File successfully routed (only on the relay site in store and forward mode) |


****Sender/Server transfer states****


| ****State**** | ****Compatible Sentinel State**** | ****IsEnd**** | ****IsAlert**** | ****IsException**** | ****Description**** |
| --- | --- | --- | --- | --- | --- |
| AVAILABLE | AVAILABLE | 0 | 0 | 0 | Transfer available |
| SENDING | SENDING | 0 | 0 | 0 | File data transmission in progress |
| CANCELED | CANCELED | 0 | 1 | 1 | File data transmission locally canceled (KEEP command) |
| SUSPENDED | SUSPENDED | 0 | 0 | 1 | File data transmission locally suspended (HALT command) |
| INTERRUPTED | INTERRUPTED | 0 | 1 | 1 | File data transmission remotely suspended |
| SENT | SENT | 0 | 0 | 0 | File data transmission ended |
| POST_PROC | SENT | 0 | 0 | 0 | Post processing in progress |
| POST_PROC_ABORT | SENT | 1 | 1 | 1 | Post processing aborted by the application (KEEP command) |
| ACK_EXPECTED | SENT | 0 | 0 | 0 | Waiting for a remote acknowledgement |
| ENDED_TO_ACK | ENDED_TO_ACK | 0 (ack expected)/1 | 0 | 0 | Transfer remotely acknowledged |
| POST_PROC_ACK_ABORT | SENT/ENDED_TO_ACK/ENDED_TO_NACK | 0 | 1 | 1 | Post processing of the Acknowledgement phase aborted by the application (KEEP command) |
| COMPLETED | CONSUMED | 1 | 0 | 0 | Transfer completed |


****Receiver/Requester transfer states****


| ****State**** | ****Compatible Sentinel State**** | ****IsEnd**** | ****IsAlert**** | ****IsException**** | ****Description**** |
| --- | --- | --- | --- | --- | --- |
| TO_EXECUTE | TO_EXECUTE | 0 | 0 | 0 | Transfer to execute (delayed or to restart) |
| RECEIVING | RECEIVING | 0 | 0 | 0 | File data transmission in progress |
| CANCELED | CANCELED | 0 | 1 | 1 | File data transmission locally canceled (KEEP command) |
| SUSPENDED | SUSPENDED | 0 | 0 | 1 | File data transmission locally suspended (HALT command) |
| INTERRUPTED | INTERRUPTED | 0 | 1 | 1 | File data transmission remotely suspended |
| RECEIVED | RECEIVED | 0 | 0 | 0 | File data transmission ended |
| POST_PROC | RECEIVED | 0 | 0 | 0 | Post processing in progress |
| POST_PROC_ABORT | RECEIVED | 0 | 1 | 1 | Post processing aborted by the application (KEEP command) |
| ACK_EXPECTED | RECEIVED | 0 | 0 | 0 | Waiting for a local acknowledgement |
| ACKED | ACKED | 1 | 0 | 0 | Transfer locally acknowledged by the application |
| POST_PROC_ACK_ABORT | RECEIVED/ACKED/NACKED | 0 | 1 | 1 | Post processing of the Acknowledgement phase aborted by the application (KEEP command) |
| COMPLETED | CONSUMED | 1 | 0 | 0 | Transfer completed |


****Sender/Requester transfer states****


| ****State**** | ****Compatible Sentinel State**** | ****IsEnd**** | ****IsAlert**** | ****IsException**** | ****Description**** |
| --- | --- | --- | --- | --- | --- |
| PRE_PROC | AVAILABLE/TO_EXECUTE | 0 | 0 | 0 | Pre-processing in progress |
| PRE_PROC_ABORT | CANCELED | 0 | 1 | 1 | Pre-processing aborted by the application (KEEP command) |
| TO_EXECUTE | TO_EXECUTE | 0 | 0 | 0 | Transfer to execute (delayed or to restart) |
| SENDING | SENDING | 0 | 0 | 0 | File data transmission in progress |
| CANCELED | CANCELED | 0 | 1 | 1 | File data transmission locally canceled (KEEP command) |
| SUSPENDED | SUSPENDED | 0 | 0 | 1 | File data transmission locally suspended (HALT command) |
| INTERRUPTED | INTERRUPTED | 0 | 1 | 1 | File data transmission remotely suspended |
| SENT | SENT | 0 | 0 | 0 | File data transmission ended |
| POST_PROC | SENT | 0 | 0 | 0 | Post-processing in progress |
| POST_PROC_ABORT | SENT | 0 | 1 | 1 | Post-processing aborted by the application (KEEP command) |
| ACK_EXPECTED | SENT | 0 | 0 | 0 | Waiting for a remote acknowledgement |
| ENDED_TO_ACK | ENDED_TO_ACK | 0 (ack expected)/1 | 0 | 0 | Transfer remotely acknowledged |
| POST_PROC_ACK_ABORT | SENT/ENDED_TO_ACK/ENDED_TO_NACK | 0 | 1 | 1 | Post-processing of the Acknowledgement phase aborted by the application (KEEP command) |
| COMPLETED | CONSUMED | 1 | 0 | 0 | Transfer completed |

