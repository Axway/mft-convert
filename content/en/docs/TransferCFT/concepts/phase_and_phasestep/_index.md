{
    "title": "About phase and phasestep",
    "linkTitle": "Transfer workflow",
    "weight": "140"
}{{< TransferCFT/axwayvariablesComponentShortName  >}} provides a complete processing workflow that includes file preprocessing, transfer processing, post processing script execution and acknowledgement. As  of {{< TransferCFT/axwayvariablesComponentShortName  >}} 3.0 processing features have evolved, replacing the former ****state**** with the Phase/Phase step pair, to improve the visibility of a flow. The processing phase shows where you are in your transfer. Within each phase there is a phase step, which can be either a process or a step.

This topic presents processing ****concepts****, step overviews in the following sections:

- [Types of processing phases](#Types)
- [Phasesteps in each phase](#Types2)
- [About backward compatibility](#About)
- [Example usage](#Example)

<span id="Types"></span>

Types of processing phases
--------------------------

The term ****phase**** refers to the highest level in the transfer flow cycle:

- (A) Pre-processing: All pre-transfer script execution occurs here
- (T) Transferring: All transfer execution occurs in this phase
- (Y) Post-processing: All post-transfer script execution occurs here
- (Z) Acknowledgement: Acknowledgement reception/send steps and ack script execution occur here
- (X) Done: End condition when all of the previous phases are completed

![](/Images/TransferCFT/phase_simple.png)

<span id="Types2"></span>

Phasesteps in each phase
------------------------

The ****phasestep**** refers to the details that can occur during any given phase:

- (D) At disposal: The processing of the Phase is ready to be executed; it is ready to go.
- (H) Hold: The processing of the Phase is on hold and waiting for an action to be executed.
- (C) Processing/Current: The Phase processing is being executed.
- (K) Keep: The Phase processing is stopped.
- (X) Done: This phase step only exists for the Done phase, once all previous phases are complete.
- (E) Exit EOT: This phase step only exists for the Post-processing phase, to signal an [end-of-transfer exit](../../app_integration_intro/managing_exits/about_the_end_of_transfer_type_exit).

![](/Images/TransferCFT/temp_phase_steps.png)

> **Note**
>
> Note: The CFTSTATE in the Transfer CFT catalog corresponds to the transfer state in diagrams and tables.


| Phase  | Phasestep  | <span id="State"></span>State  | Description  |
| --- | --- | --- | --- |
| Pre-processing (A)  | Hold (H)  | Pre-processing (A)  | Pre-processing available  |
| Pre-processing (A)  | Available (D)  | Pre-processing (A)  | Pre-processing is waiting resource to start  |
| Pre-processing (A)  | Processing (C)  | Pre-processing (A)  | Pre-processing in progress  |
| Pre-processing (A)  | Killed (K)  | Pre-processing (A)  | Pre-processing is canceled  |
| Transfer (T)  | Hold (H)  | Hold transfer (H)  | Transfer available (diagi=0) or interrupted (diagi &lt;&gt; 0)  |
| Transfer (T)  | Available (D)  | Available (D)  | Transfer is waiting on a resource to start  |
| Transfer (T)  | Processing (C)  | Processing (C)  | Transfer is in progress  |
| Transfer (T)  | Killed (K)  | Killed (K)  | Transfer is canceled  |
| Post-processing (Y)  | Hold (H)  | Post-processing (Y)  | Post-processing is interrupted  |
| Post-processing (Y)  | Exit EOT (E)  | Post-processing (Y)  | Exit in progress  |
| Post-processing (Y)  | Available (D)  | Post-processing (Y)  | Post-processing is waiting for a resource to start  |
| Post-processing (Y)  | Processing (C)  | Post-processing (Y)  | Post-processing in progress  |
| Post-processing (Y)  | Killed (K)  | Post-processing (Y)  | Post-processing is canceled  |
| Ack (Z)  | Hold (H)  | Ack (Z)  | Transfer request waits for an application acknowledgement before continuing the flow  |
| Ack (Z)  | Available (D)  | Ack (Z)  | Ack processing is not started  |
| Ack (Z)  | Processing (C)  | Ack (Z)  | Ack processing in progress  |
| Ack (Z)  | Killed (K)  | Ack (Z)  | Ack processing is canceled  |
| Done (X)  | Done (X)  | Done (X)  | Flow finished  |


<span id="About"></span>

About backward compatibility
----------------------------

For continued compatibility and to help in transitioning, clients may still use the former states, which were not modified. Alternately, they can use the adapted states that reflect the phase and phase step. See [Compatibility](processing_compatability).

<span id="Example"></span>

Example usage
-------------

This guide provides the following usage example for implementation: [Adding a zip script.](../about_transfer_processing/preprocess_use_case2)

> **Note**
>
> Note: See Processing commands: general usage for a description of the processing command parameters and values.
