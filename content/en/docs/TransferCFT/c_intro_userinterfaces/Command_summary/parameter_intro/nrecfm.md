{
    "title": "nrecfm",
    "linkTitle": "nrecfm",
    "weight": "2330"
}<span id="nrecfm"></span>

### nrecfm

#### CFTSEND, SEND

**[NRECFM = {<span class="underline">FRECFM value</span> &#124; F &#124; U
&#124; V}]     ODETTE,
PeSIT, OS**

File record format defined in protocol terms:

- ****F****: fixed
- ****V****: variable
- ****U****: undefined


| PeSIT D EXTERN profile<br /> PeSIT ANY profile<br /> PeSIT E | In PeSIT protocol with the EXTERN profile, the value NFRECFM = U is not known in protocol terms and is changed by the {{< TransferCFT/axwayvariablesComponentShortName  >}} to NFRECFM = V. This value (U) is sent with no modification in PeSIT D CFT profile or in PeSIT E from {{< TransferCFT/axwayvariablesComponentShortName  >}} to {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| --- | --- |
| **ODETTE** | For the ODETTE protocol, refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Protocol section* for the use of this parameter. |
| **PeSIT SIT profile** | The NRECFM = U value is only recognized between two Transfer CFTs. |


[Return to Command index](../../)
