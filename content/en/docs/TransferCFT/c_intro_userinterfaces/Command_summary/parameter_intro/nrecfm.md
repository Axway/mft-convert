---

    title: nrecfm
    linkTitle: nrecfm
    weight: 2310

---
<span id="nrecfm"></span>

### nrecfm

#### CFTSEND, SEND

**\[NRECFM = {<u>FRECFM value</u> | F | U
| V}\]     ODETTE,
PeSIT, OS**

File record format defined in protocol terms:

- <span style="font-weight: bold;">****F****</span>: fixed
- <span style="font-weight: bold;">****V****</span>: variable
- <span style="font-weight: bold;">****U****</span>: undefined


| PeSIT D EXTERN profile<br /> PeSIT ANY profile<br /> PeSIT E | In PeSIT protocol with the EXTERN profile, the value NFRECFM = U is not known in protocol terms and is changed by the {{< TransferCFT/axwayvariablesComponentShortName  >}} to NFRECFM = V. This value (U) is sent with no modification in PeSIT D CFT profile or in PeSIT E from {{< TransferCFT/axwayvariablesComponentShortName  >}} to {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| --- | --- |
| **ODETTE** | For the ODETTE protocol, refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Protocol section* for the use of this parameter. |
| **PeSIT SIT profile** | The NRECFM = U value is only recognized between two Transfer CFTs. |


[Return to Command index](../../)
