---
title: "Acknowledgement phase"
linkTitle: "Acknowledgement phase"
weight: 190
---Acknowledgment phase consists of running a script, which is specified using the parameter ACKEXEC (of CFTSEND, CFTRECV, SEND and RECV commands) or parameters EXECSFA or EXECSMA (of CFTPARM command), when the transfer request has been acknowledged locally or by the remote partner. Once the Acknowledgment is done, the transfer request goes to the Done phase.

To notify the end of the processing to Transfer CFT, the acknowledgment processing script must perform a END command with istate=no.

> **Note**
>
> Caution  
> The transfer request remains in phase=Z and phasestep=C until Transfer CFT receives the END command related to this transfer request

 

During the acknowledgment phase, the phase step can be:

* \(H\) Hold: waiting for the send/receive of the reply
* \(D\) Disposable: scheduled to start
* \(C\) Current: processing running, waiting for a CFTUTIL end with istate=no
* \(K\) Keep: processing failed or stopped

The behavior for this phase is fully dependent on the ackstate.

There are two possibilities for ackstate:

* Require:
    *   If an Acknowledgement is received during the post processing, it waits until the end of the post-processing to move to the Acknowledgement phase (Z) and launch the ackexec/execsfa/execsma with phasestep (C) if needed. When this completes, it goes to the Done phase.
    *   If no Acknowledgement is received at the end of the post-processing, or if there is no post processing, it remains in the Acknowledgement (Z) phase with a phasestep of (H). As soon as the acknowledgement is received/sent, its launches the ackexec/execsfa/execsma with phasestep (C) if needed. When they finish, it moves to the Done phase.

<!-- -->

* Ignore:

    > **Note**
    >
    > To configure backward compatibility ack behavior, set the unified configuration to: uconf:cft.state_compat=Yes

* Not backward compatible:  
    If an Acknowledgement is received during the post-processing, it waits until the end of the post-processing to move to the Acknowledgement phase (Z) and launch the ackexec/execsfa/execsma with phasestep (C) if applicable. When that finishes, it moves to the Done phase.  
    If no Acknowledgement was received at the end of the Post-processing or if there is no Post-processing, then it moves to the Done phase. As soon as the Acknowledgement is received/sent in the Done phase, ackexec/execsfa/execsma is launched, if applicable.

* Backward compatible:  
    We launch the ackexec/execsfa/execsma, if applicable, as soon as the Acknowledgement is received regardless of the current Phase. Consequently,this Acknowledgement phase is not seen. After the end of the previous step (post-processing if applicable, or transfer), it moves directly to the next Done phase.

> **Note**
>
> See Processing commands: general usage for a description of the processing command parameters and values.
