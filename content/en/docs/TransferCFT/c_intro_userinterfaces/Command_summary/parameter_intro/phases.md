---

    title: phases
    linkTitle: phases
    weight: 2570

---
### phases

****SWAITCAT****

**`[ PHASES =  { A | T | Y | Z | X }  ]`**

Where the string <span class="bold_in_para">****sizemax= 5****</span>.

Possible values:

- \(A\) preprocessing: All pre-transfer script execution occurs here
- \(T\) Transferring: All transfer execution occurs in this phase
- (Y) Post-processing: All the post-transfer script execution occurs here
- (Z) Acknowledgement: Acknowledgement reception/send steps and ack script execution occur here
- (<u>X</u>) Done: End condition when all of the previous phases are completed (default)

****Example****

```
swaitcat select='(IDTU=="%_CAT_IDTU%")',timeout=30,phases=y,phasesteps=ch
```
