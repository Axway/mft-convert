---
    title: "maxcnx"
    linkTitle: "maxcnx"
    weight: 1890
---<span id="maxcnx"></span>

### maxcnx

#### CFTNET

****[MAXCNX     = { <u>384</u>
&#124; n} ]      {0...MAXTRANS value up to 2000}****

The maximum number of simultaneous connections that {{< TransferCFT/axwayvariablesComponentShortName  >}} accepts
to establish on a given network resource.

- Enter
    a value between 0 and 2 times the MAXTRANS
    value
- Recommended value is the same as the MAXTRANS value
- Default = 384

> **Note**
>
> On Unix systems, setting the MAXCNX value to higher than 1020 has no impact.

[Return to Command index](../../)
