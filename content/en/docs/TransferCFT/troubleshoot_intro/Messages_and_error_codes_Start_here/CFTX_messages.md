---
title: " Transfer CFT messages:  CFTW and CFTX"
linkTitle: "CFTX messages"
weight: 390
--- This topic lists the CFTWxx and CFTXxx messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

| V23 format<br/> V24 format<br/> Warning | <span id="CFTX01W"></span>CFTX01W Action &amp;action on object &amp;object not authorized for user &amp;user : ID CFTAPPL=&amp;id CFTX01W Action &amp;action on object &amp;object not authorized for user &amp;user : ID CFTAPPL=&amp;id |
| --- | --- |
| Explanation | The authorization system does not allow the &amp;user user to set the &amp;idf IDF, as indicated in the preceding message (either CFTC10I or CFTC11I). You should check with your security administrator to determine your access rights. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTX02W"></span>CFTX02W owner user is &amp;user, owner group is &amp;group<br/> CFTX02W owner user is &amp;user, owner group is &amp;group |
| --- | --- |
| Explanation | The user and group names used are displayed. This message is displayed after the CFTX01W message. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTX03W"></span>CFTX03W Action &amp;action on object &amp;object not authorized for user &amp;user<br/> CFTX03W+Action &amp;action on object &amp;object not authorized for user &amp;user |
| --- | --- |
| Explanation | The security system does not allow &amp;user to perform the specified action (contact your security administrator to set the required privileges). |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTX04W"></span>CFTX04W With value &amp;value<br/> CFTX04W+with value &amp;value |
| --- | --- |
| Explanation | Warning message. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTX05W"></span>CFTX05W Action &amp;action on object &amp;object not authorized for user &amp;user: FNAME=&amp;fname<br/> CFTX05W Action &amp;action on object &amp;object not authorized for user &amp;user : FNAME=&amp;fname |
| --- | --- |
| Explanation | The security system does not allow &amp;user to specify &amp;fname as the FNAME value (contact your security administrator to set the required privileges). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTX10I"></span>CFTX10I Warning : security file commands were modified<br/> CFTX10I Warning : security file commands were modified |
| --- | --- |
|   | <span id="CFTX11I"></span>CFTX11I+ without a generation was done<br/> CFTX11I+ without a generation was done |
|   | <span id="CFTX12I"></span>CFTX12I+ Some queer comportements will happenned.<br/> CFTX12I+ Some queer comportements will happenned. |
| Explanation | Modifications have been made to the authorization system, but the system has not been regenerated.<br/> Abnormal behavior may arise when {{< TransferCFT/axwayvariablesComponentShortName  >}} is executed. |

