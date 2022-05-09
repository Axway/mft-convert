{
    "title": "orecfm",
    "linkTitle": "orecfm",
    "weight": "2500"
}<span id="orecfm"></span>

### {{< TransferCFT/SystemTitle  >}}

#### COPYILE

****[ORECFM = {<span class="underline">IRECFM value</span> &#124; F &#124;
V &#124; U}]****

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
| UNIX | The value &quot;U&quot; is kept for compatibility with previous versions. It has the same meaning as ORECFM=V. |


 

[Return to Command index](../../)
