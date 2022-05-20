---
title: "execpre"
linkTitle: "execpre"
weight: 940
---### execpre

#### CFTDEST

****[ EXECPRE = { <u>DEST</u> &#124; PART &#124; CHILDREN ] ]****

Preprocessing procedure submit mode type.

When a transfer is terminated, a preprocessing procedure is submitted. The symbolic variables are substituted on the fly. For example, the &PART variable is substituted with the CFTDEST command identifier for the generic, and the partner identifier for each transfer on the list.

* <u>DEST</u>: only the generic executes the script
* CHILDREN: only the children execute the script
* PART: any transfer execute the script

[Return to Command index](../../)
