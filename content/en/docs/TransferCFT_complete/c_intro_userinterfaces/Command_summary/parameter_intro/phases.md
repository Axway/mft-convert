{
    "title": "phases",
    "linkTitle": "phases",
    "weight": "2590"
}### phases

#### SWAITCAT

****[ PHASES = { A &#124; T &#124; Y &#124; Z &#124; X } ]****

Where the string ****sizemax= 5****.

Possible values:

- (A) preprocessing: All pre-transfer script execution occurs here
- (T) Transferring: All transfer execution occurs in this phase
- (Y) Post-processing: All the post-transfer script execution occurs here
- (Z) Acknowledgement: Acknowledgement reception/send steps and ack script execution occur here
- (X) Done: End condition when all of the previous phases are completed

****Example****

```
swaitcat select='(IDTU=="%_CAT_IDTU%")',timeout=30,phases=y,phasesteps=ch
```

[Return to Command index](../../)
