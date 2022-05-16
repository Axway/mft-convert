---

    title: orecfm
    linkTitle: orecfm
    weight: 2480

---
<span id="orecfm"></span>

### {{< TransferCFT/SystemTitle  >}}

#### COPYILE

****\[ORECFM = {<u>IRECFM value</u> | F |
V | U}\]****

Output record format:

- F:
    fixed
- V:
    variable
- U:
    undefined

The possible values for each system are indicated in the corresponding
specific Operations Guide. If the output file is compressed (OCOMP
not 0), the value of the ORECFM parameter is forced
to V.


| OS  | Details  |
| --- | --- |
| UNIX | The value "U" is kept for compatibility with previous versions. It has the same meaning as ORECFM=V. |


 

[Return to Command index](../../)
