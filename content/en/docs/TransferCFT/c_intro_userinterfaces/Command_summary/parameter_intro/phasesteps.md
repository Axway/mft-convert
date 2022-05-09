{
    "title": "phasesteps",
    "linkTitle": "phasesteps",
    "weight": "2600"
}#### phasesteps

****SWAITCAT****

****[ PHASESTEPS = { D &#124; H &#124; C &#124; K &#124; X &#124; E } ]****

Where the string ****sizemax=8****.

During processing, the phasesteps can be:

- (D) Disposable: When the processing of the Phase is ready to be executed.
- (H) Hold: When the processing of the Phase is on hold and waiting for an action to be executed.
- (C) Processing/Current: When the processing of the Phase is executing.
- (K) Keep: When the processing of the Phase is stopped.
- (X) Done: This phase step only exists in the Done Phase, once all previous phases are complete.
- (E) Exit EOT: This phase step only exists in the Post-processing phase, to signal an [end-of-transfer exit](../../../../app_integration_intro/managing_exits/about_the_end_of_transfer_type_exit).

****Example****

```
swaitcat select='(IDTU=="%_CAT_IDTU%")',timeout=30,phases=y,phasesteps=ch
```

[Return to Command index](../../)
