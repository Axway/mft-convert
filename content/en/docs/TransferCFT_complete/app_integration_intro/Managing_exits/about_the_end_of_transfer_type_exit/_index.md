{
    "title": "About end-of-transfer exits",
    "linkTitle": "End-of-transfer exit",
    "weight": "330"
}The exit interface provides
the user program with a structure containing information relating to the
transfer. This book describes the end-of-transfer exit. It begins with this topic, which explains the
transfer state and how it relates to an end-of-transfer exit.

The end of transfer type EXIT task enables the user to take control
at the end of a send or receive file or message transfer, independently
of whether or not the transfer is completed.

<span id="Transfer_state"></span>

### Transfer state

The user program can also ask the {{< TransferCFT/axwayvariablesComponentShortName  >}} to modify the
transfer state (UPDATE) or to delete the catalog record (DELETE).

The following changes of state are authorized in the event of an UPDATE:

- H --&gt; K
- K --&gt; H
- T --&gt; X
- H or K --&gt; D
- T --&gt; T

If a transfer procedure is submitted, it is initiated when the exit
call ends.

When the EXIT task is called without any request for change to the related
catalog entry’s state, the end-of-transfer procedure is submitted according
to standard {{< TransferCFT/axwayvariablesComponentShortName  >}} rules.

When the EXIT task initiates a change of state, the procedure submission
rule depends on the final state of the catalog entry returned by the EXIT.

The following
table shows the transfer possible changes of state.


| Change of state  | End of transfer procedure submitted  |
| --- | --- |
| H to K  | Abnormal end of transfer procedure  |
| K to H  | Abnormal end of transfer procedure  |
| T to X  | No call  |
| H to D  | No call  |
| K to D  | No call  |
| T to T  | Normal end of transfer procedure  |

