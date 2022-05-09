{
    "title": "Configure Transfer CFT",
    "linkTitle": "Sentinel Universal Agent",
    "weight": "230"
}To restrict tracking to important transfers only, use the XFB.Transfer Tracked Object activation parameters in the transfer commands (SEND and RECV), in the partner definitions (CFTPART) and in the {{< TransferCFT/axwayvariablesComponentShortName  >}} files (CFTSEND and CFTRECV).

An activation parameter can have the following values:

- NO: no tracking

<!-- -->

- ALL: complete tracking is tracking for each change in transfer status

<!-- -->

- SUMMARY: summary tracking is tracking for creation and end of transfer processes

<!-- -->

- UNDEFINED: undefined value

Pre- and post-processing tracking
---------------------------------

A file transfer only represents one of the actions involved in the processing chain. In terms of tracking, {{< TransferCFT/axwayvariablesComponentShortName  >}} provides the means of linking this action to the actions that take place both before and after processing.

### Pre-processing tracking

{{< TransferCFT/axwayvariablesComponentShortName  >}} provides two parameters in a transfer deposit command. An application that is tracking its activity can use these parameters to supply:

- Tracked Instance identifier (application Tracked Instances CycleId) with a maximum length of 250 characters

<!-- -->

- Tracked Object name, with a maximum length of 50 characters

An application can use the Sentinel TRKUTIL or API UA components to notify its own tracked events.

APPCYCID and APPOBJID parameters of the SEND/RECV commands

- APPCYCID: CycleId of the application Tracked Instances

<!-- -->

- APPOBJID: name of the application Tracked Object

### Post-transfer processing

The {{< TransferCFT/axwayvariablesComponentShortName  >}} end-of-transfer procedures offer two symbolic variables &XFRCYCID, &XFROBJID for all EXECxx procedures:

- &XFRCYCID: CycleId of transfer Tracked Instances

<!-- -->

- &XFROBJID: name of the XFB.Transfer Tracked Object

Fields XferCycleId[32+1] and XferObjetcId[32+1] in the exit structures:

- End-of-transfer exit

<!-- -->

- File exit (phase before allocation)

<!-- -->

- Directory exit (requester mode only)

### Troubleshooting the Sentinel server or Event router

In the event of a communication problem with the Sentinel Server or Event Router, the CFTS31W message is displayed in the {{< TransferCFT/axwayvariablesComponentShortName  >}} LOG, and the trace message is stocked in the buffer file of the Sentinel tracking task.

When you restart {{< TransferCFT/axwayvariablesComponentShortName  >}}, the application tries to re-send all records in the buffer file. This operation can be time consuming because it depends on the number of messages in the file. As a precaution, you should use the TRKUTIL utility to empty the buffer file before restarting {{< TransferCFT/axwayvariablesComponentShortName  >}}.

**Example TRKUTIL utility use**

```
trkutil Sendfile <Name of buffer file>,config=<Name of configuration file>
```
