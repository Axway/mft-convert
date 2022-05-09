{
    "title": "Preprocessing phase",
    "linkTitle": "Preprocessing phase",
    "weight": "160"
}The preprocessing phase consists of running a script, which is specified using the parameter PREEXEC (of SEND and CFTSEND commands) prior to a file transfer. Once the preprocessing is complete, the transfer request moves on to the next, Transfer, phase.

To notify the end of the processing to Transfer CFT, the preprocessing script must perform an END command with istate=no.

> **Note**
>
> Caution  
> The transfer request remains in phase=A and phasestep=C until Transfer CFT receives the END command related to this transfer request.

As the preprocessing phase did not exist in versions prior to {{< TransferCFT/axwayvariablesComponentShortName  >}} 3.0, during this phase the former state has the adapted state and is always set to A.

During this phase, the phase step can be:

- (H) Hold: waiting for an incoming receive to start the preprocessing or a start command
- (D) Disposable: scheduled to start
- (C) Current: processing running, waiting for a CFTUTIL end with istate=no
- (K) Keep: processing failed or stopped

State and prestate dependent actions
------------------------------------

The following preprocessing actions occur according to the status of the prestate and state. For each of the following cases, set ****PREEXEC**** to an existing script.

- Immediate preprocessing then send
    -   PRESTATE =  DISP 
    -   STATE = DISP 
- Hold... waits for a start or receive, then preprocesses, and sends
    -   PRESTATE= HOLD
    -   STATE = DISP
- Immediate preprocessing then Hold...waits for a receive or start, and then sends
    -   PRESTATE= DISP
    -   STATE = HOLD
- Hold ... waits for a receive command, preprocesses, and then sends or
- Hold... waits for start command, preprocesses, repeats Hold ... waits for receive or start then sends
    -   PRESTATE= HOLD
    -   STATE = HOLD
- Preprocessing then send on receive
    -   IMPL=YES

> **Note**
>
> Note: See Processing commands: general usage for a description of the processing command parameters and values.
