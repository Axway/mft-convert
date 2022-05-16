---

    title: phasesteps
    linkTitle: phasesteps
    weight: 2580

---
#### phasesteps

****SWAITCAT****

`[ PHASESTEPS = { D | H | C | K | X | T | E } ]`

Where the string <span class="bold_in_para">****sizemax=7****</span> and the default is X.

During processing, the phasesteps can be:

- \(D\) Disposable: When the processing of the Phase is ready to be executed, just a matter of time.
- \(H\) Hold: When the processing of the Phase is on hold and waiting for an action to be executed.
- \(C\) Processing/Current: When the processing of the Phase is executing.
- \(K\) Keep: When the processing of the Phase is stopped.
- <u>(X</u>) Done: This phase step only exists in the Done Phase, once all previous phases are complete.
- \(E\) Exit EOT: This phase step only exists in the Post-processing phase, to signal an [end-of-transfer exit](../../../../app_integration_intro/managing_exits/about_the_end_of_transfer_type_exit).

****Example****

`swaitcat select='(IDTU=="%_CAT_IDTU%")',timeout=30,phases=y,phasesteps=ch`

[Return to Command index](../../)
