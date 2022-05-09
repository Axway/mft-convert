{
    "title": "Post-processing phase",
    "linkTitle": "Post-processing phase",
    "weight": "180"
}The post-processing phase consists of running a script, which is specified using the parameter EXEC (of CFTSEND, CFTRECV, SEND and RECV commands) or parameters EXECSF, EXECRF, EXECSM or EXECRM (of CFTPARM command), after the transfer phase. Once the post processing is done, the transfer request goes to the Acknowledgment phase or Done phase depending to the parameter ACKSTATE.

During the post processing phase if the parameter EXITEOT is set, the End-Of-Transfer Exit is executed before the post processing script.

To notify the end of the processing to Transfer CFT, the post processing script must perform an END command with istate=no.

> **Note**
>
> Caution  
> The transfer request remains in phase=Y and phasestep=C until Transfer CFT receives the END command related to this transfer request.

During this phase, the phase step can be:

- ExitEOT (E): trying to launch the ExitEOT or waiting that it give the hand back.
- Hold (H): waiting for a start command.
- Disposable (D): scheduled to start.
- Current (C): processing running, waiting for a CFTUTIL end with istate=no
- Keep (K): processing failed or stopped.

If neither an EOT exit or other exec was specified, the transfer passes to the acknowledgment phase or the done phase.

> **Note**
>
> Note: See Processing commands: general usage for a description of the processing command parameters and values.
