---
    title: " Transfer CFT messages: CFTW"
    linkTitle: "CFTW messages"
    weight: 380
---This topic lists the CFTWxx and CFTXxx messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW01W"></span>CFTW01W PART=&amp;part IDF=&amp;idf IDT=&amp;idt Temporary file &amp;file deleted<br/> CFTW01W File &amp;fname deleted &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &gt; |
| --- | --- |
| Explanation | The &amp;file temporary file was deleted at the end of the transfer. The name of this file is declared in the WFNAME parameter of the CFTSEND and CFTRECV commands. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW02W"></span>CFTW02W CFTSEND &amp;idsend override SEND parameters<br/> CFTW02W CFTSEND &amp;id override SEND parameters |
| --- | --- |
| Explanation | The parameters of the SEND command are overridden by the parameters in the associated CFTSEND command. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW03W"></span>CFTW03W _ Send command: Unauthorized usage of IDF = &amp;idf<br/> CFTW03W _ Send Command : Unauthorized usage on IDF = &amp;id |
| --- | --- |
| Explanation | The &amp;idf IDF is not authorized for the SEND command. Check your software key restrictions. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW04W"></span>CFTW04W _ Recv command: Unauthorized usage on IDF = &amp;idf<br/> CFTW04W _ Recv Command : Unauthorized usage on IDF = &amp;id |
| --- | --- |
| Explanation | The &amp;idf IDF is not authorized for the RECV command. See the restrictions concerning the value of your software key. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW05W"></span>CFTW05W PART=&amp;part IDF = &amp;idf Temporary file unknown, WFNAME not defined in SEND<br/> CFTW05W PART=&amp;part IDF=&amp;idf Temporary file unknown, WFNAME not defined in SEND |
| --- | --- |
| Explanation | The WFNAME was not set in the CFTSEND command when preparing a transfer requiring additional processing and sending a group of files. |
| Action | Modify the parameter settings using a different IDF for this type of transfer. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW07W"></span>CFTW07W PART=&amp;part IDF = &amp;idf _ SELFNAME not authorized for COPY<br/> CFTW07W PART=&amp;par IDF=&amp;idf _ SELFNAME not authorized for COPY\n |
| --- | --- |
| Explanation | You cannot use a selection file when implementing additional processing prior to a transfer (IEBCOPY with MVS for example). |
| Consequence | The transfer is not triggered. |
| Action | Do not use a selection file; you can, however, specify a generic file name (FNAME= #FIL1*, FNAME= #TFILM*). |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTW08W"></span>CFTW08W CFTRECV &amp;idrecv override RECV parameters<br/> CFTW08W CFTRECV &amp;id override RECV parameters |
| --- | --- |
| Explanation | The RECV command parameters are overridden by the parameters set in the associated CFTRECV command. |


 


| V23 format<br/> <br/> V24 format<br/> <br/> Information | <span id="CFTW09I"></span>CFTW09I PART=&amp;part IDF=&amp;idf1 &amp;str IDT=&amp;idt CFTSEND &amp;idf2 NIDF=&amp;nidf XLATE=&amp;xlate<br/> CFTW09I PART=&amp;part IDF=&amp;idf1 &amp;str IDT=&amp;idt CFTRECV &amp;idf2 NIDF=&amp;nidf XLATE=&amp;xlate<br/> CFTW09I CFTSEND &amp;idf1 &amp;str&lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf2 IDT=&amp;idt NIDF=&amp;nidf XLATE=&amp;xlate<br/> CFTW09I CFTRECV &amp;idf1 &amp;str &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf2 IDT=&amp;idt NIDF=&amp;nidf XLATE=&amp;xlate |
| --- | --- |
| Explanation | Indicates the ID that was used.<br/> • &amp;idf1 indicates the CFTSEND or CFTRECV ID to use<br/> • &amp;idf2 indicates the SEND or RECV ID (in requester mode), or CFTSEND or CFTRECV ID (in server mode) provided to Transfer CFT<br/> • If &amp;idf1 differs from &amp;idf2, then &amp;str = "default" indicates that {{< TransferCFT/axwayvariablesComponentLongName  >}} is using the default template<br/> ****Example 1****<br/> The IDF is not defined in a CFTSEND command, for example: SEND PART=SERVER,IDF=UNKN but no CFTSEND ID=UNKN<br/> Defined: CFTW09I CFTSEND BIN default &lt;IDTU=A0000024 PART=SERVER IDF=UNKN IDT=I0714504 NIDF=TRTR XLATE=TAB&gt;<br/> **Example 2**<br/> IDF is defined in a CFTSEND command, for example: SEND PART=SERVER,IDF=TRTR and CFTSEND ID=TRTR<br/> Defined: CFTW09I CFTSEND TRTR &lt;IDTU=A0000024 PART=SERVER IDF=TRTR IDT=I0714504 NIDF=TRTR XLATE=TAB&gt; |

