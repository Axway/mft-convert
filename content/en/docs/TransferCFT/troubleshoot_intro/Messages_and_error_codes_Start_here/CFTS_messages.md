---
title: "Transfer CFT messages: CFTS"
linkTitle: "CFTS messages"
weight: 360
--- This topic lists the CFTSxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS01W"></span>CFTS01W Synch. response time- out _ Waitresp increased to &amp;ns (&amp;str)<br/> CFTS01W Synch. response time- out _ Waitresp increased to &amp;ns (&amp;str) |
| --- | --- |
| Explanation | The message placed in a synchronous queue has received no response (time limit has expired).<br/> Where:<br/> • &amp;n is the value in seconds of the new WAITRESP<br/><br/> • &amp;str is either CFTTCOM or CFTTFIL, the process who wrote the message<br/> |
| Consequence | Another attempt will be made. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS02E"></span>CFTS02E PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt DIRECT=&amp;direct &amp;fname not found<br/> CFTS02E _ &amp;fname not found &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt DIRECT=&amp;direct&gt; |
| --- | --- |
| Explanation | The &amp;fname procedure was not found for a given transfer (&amp;idt).<br/> This procedure was requested after a file or message transfer or subsequent to an error (see the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation, EXEC parameters). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS03I"></span>CFTS03I PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm]IDT=&amp;idt _ &amp;fname submitted<br/> CFTS03I _ &amp;fname submitted&lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; (&amp;n sec) |
| --- | --- |
| Explanation | The procedure (&amp;fname) was submitted for a given transfer (&amp;idt).<br/> This procedure was requested at the end of a file or message transfer, or in the event of an error (see the EXECxxx parameters). |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS04W"></span>CFTS04W Action file &amp;fname is empty<br/> CFTS04W Action file &amp;fname is empty |
| --- | --- |
| Explanation | An action is envisaged at the end of a transfer or in the event of an error (see the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation, EXECxxx parameters); the file to be submitted is empty. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS05E"></span>CFTS05E Error code &amp;scs _ Trying to access &amp;str<br/> CFTS05E Error code &amp;scs _ Trying to access &amp;str |
| --- | --- |
| Explanation | An access error was detected on a file.<br/> This file (&amp;fname) corresponds to an action requested at the end of a transfer or in the event of an error (see the Transfer CFT Online documentation EXECxxx parameters).<br/> The &amp;str variable can have the following values:<br/> &amp;fname |
| Consequence | The procedure will not be executed. If the action was requested at the end of a transfer, the transfer ends normally. |
| Action | Check that the characteristics of the file to be submitted are correct (attributes and length) and inform Product Support if necessary. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS06E"></span>CFTS06E Error code &amp;scs _ Trying to access temporary file<br/> CFTS06E Error code &amp;scs _ Trying to access temporary file |
| --- | --- |
| Explanation | An action was requested at the end of a transfer or in the event of an error. This action is submitted (see the EXECxxx parameters) through a buffer file (&amp;fname).<br/> An access error was detected on this buffer file. |
| Consequence | The procedure will not be executed. If the action was requested at the end of the transfer, the transfer ends normally. |
| Action |  • Check that the characteristics of the buffer file are correct (attributes and length).<br/> • Check that it exists (created or defined logically in the {{< TransferCFT/axwayvariablesComponentShortName  >}} startup procedure).<br/> • Contact Product Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS07E"></span>CFTS07E Insufficient space for temporary file<br/> CFTS07E Insufficient space for temporary file |
| --- | --- |
| Explanation | An action was requested at the end of a transfer or in the event of an error. This action is submitted (see the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation, EXECxxx parameters) through a buffer file (&amp;fname). The space reserved for this file proves insufficient. |
| Consequence | The procedure will not be executed. If the action was requested at the end of the transfer, the transfer ends normally. |
| Action | Increase the file size, inform Product Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS08E"></span>CFTS08E Error code &amp;scs _ Executing temporary file<br/> CFTS08E Error code &amp;scs _ Executing temporary file |
| --- | --- |
| Explanation | The procedure (&amp;fname) could not be executed for a given transfer (&amp;idt).<br/> This procedure was requested at the end of a file or message transfer or in the event of an error (see the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation EXECxxx parameters). |
| Consequence | The procedure will not be executed. If the action was requested at the end of the transfer, the transfer ends normally. |
| Action | Analyze the &amp;scs code and inform Product Support if necessary. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS10E"></span>CFTS10E File communication task error (&amp;str1) _ &amp;str2<br/> CFTS10E File communication task error (&amp;str1) _ &amp;str2 |
| --- | --- |
| Explanation | Following a synchronous message queue time- out, the communication task aborted.<br/> The str1 and str2 values are:<br/> • str1: Associated str2 value<br/><br/> • define sem: terminating<br/><br/> • open: terminating<br/><br/> • memory: terminating<br/><br/> • post: terminating<br/><br/> • invalid sem: terminating<br/><br/> • catalog full: terminating<br/><br/> • &lt;cs code&gt;: terminating<br/><br/> • read: continue<br/><br/> • delete shut: continue<br/> |
| Action | Contact Axway Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS11E"></span>CFTS11E Allocation error _ Trying to access temporary file<br/> CFTS11E Allocation error _ Trying to access temporary file |
| --- | --- |
| Explanation | An action was requested at the end of a transfer or in the event of an error.<br/> This action is submitted (see the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation, EXECxxx parameters) through a buffer file (&amp;fname). An allocation error was detected on this buffer file. |
| Consequence | The procedure will not be executed. If the action was requested at the end of the transfer, the transfer ends normally. |
| Action |  • Check that the characteristics of the buffer file are correct (attributes and length).<br/> • Check that it exists (created or defined logically in the {{< TransferCFT/axwayvariablesComponentShortName  >}} startup procedure).<br/> • Contact Product Support. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS12W"></span>CFTS12W Error code &amp;scs _ CFT write messages to output stream<br/> CFTS12W Error code &amp;scs _ CFT write messages to output stream |
| --- | --- |
| Explanation | The Transfer CFT logging process can no longer write messages in the log file(s) (problem adding to the file, system incident). |
| Consequence | Transfer CFT messages will be written to the standard output (screen for example). |
| Action | Analyze the &amp;scs code and inform Product Support if necessary. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS13E"></span>CFTS13E Semaphore failure &amp;cs_CFTTPRO aborted<br/> CFTS13E Semaphore failure &amp;cs_CFTTPRO aborted |
| --- | --- |
| Explanation | Problem receiving an internal {{< TransferCFT/axwayvariablesComponentShortName  >}} message by the PROTOCOL task. |
| Consequence | A message has perhaps been lost; the reactions are unpredictable. |
| Action | Analyze the &amp;scs code and inform Product Support if necessary. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS14E"></span>CFTS14E ID=&amp;id error initializing process<br/> CFTS14E Unknown synchronization message CLASS=&amp;class TYPE=&amp;type |
| --- | --- |
| Explanation | Cannot run the end of transfer exit task. This message follows the {{< TransferCFT/axwayvariablesComponentShortName  >}} message CFTI01. |
| Consequence | No effect on the actual transfer (catalog not updated). The exit is not executed. If an end of transfer procedure was defined, it is run. |
| Action | Inform Product Support. |

| Information | <span id="CFTS15I"></span>CFTS15I PART = &amp;part Kill Session Reference &amp;ctx:&amp;ctx<br/> CFTS15I PART = &amp;part Kill Session Reference &amp;ctx:&amp;ctx |
| --- | --- |
| Explanation | Internal message to {{< TransferCFT/axwayvariablesComponentShortName  >}} giving information on the transfer context killed. The &amp;ctx values specify the context concerned. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS16E"></span>CFTS16E Synch. response time- out_End transfer exit<br/> CFTS16E Synch. response time- out _ End of transfer exit |
| --- | --- |
| Explanation | The exit task is run but does not respond to the {{< TransferCFT/axwayvariablesComponentShortName  >}}. This corresponds to the initial phase establishing communications between the {{< TransferCFT/axwayvariablesComponentShortName  >}} and the exit task. |
| Consequence | None on the actual transfer (catalog not updated). The exit is not executed. If an end of transfer procedure has been defined, it is run. |
| Action | Inform Product Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS17E"></span>CFTS17E Error code &amp;scs _ Trying to access End transfer exit<br/> CFTS17E Error code &amp;scs _ Trying to access End of transfer exit |
| --- | --- |
| Explanation | Error posting a {{< TransferCFT/axwayvariablesComponentShortName  >}} message to the exit task during inter- task communications. |
| Consequence | None on the actual transfer (catalog not updated). The exit is does not receive any directives from the {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| Action | Inform Product Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS18W"></span><span id="CFTS18E"></span>CFTS18E PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt _ Catalog record Update Error: &amp;scs<br/> CFTS18E _ Catalog Update Error: &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Transfer CFT catalog update issue with error code = &amp;scs.  |
| Consequence | The transfer record is not updated.  |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS18W"></span>CFTS18W PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt _ Catalog record label<br/> CFTS18W _ Catalog record &amp;label &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Label equals "State not updated x - &gt; y" (x = current state, y = requested state) or "not deleted".<br/> Transfer CFT catalog update issue. |
| Consequence | The catalog entry corresponding to the transfer is not updated. |
| Action | Inform Product Support. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS19I"></span>CFTS19I PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm]IDT=&amp;idt _ Catalog record label<br/> CFTS19I _ Catalog record &amp;str &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Information message label equals "updated x to y" (x = current state, y = requested state) or "deleted". The Transfer CFT catalog is updated. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS20I"></span>CFTS20I Communication file row number deleted: nnnnnnnn (&amp;str)<br/> CFTS20I &amp;str", &amp;str = Communication file row number deleted: nnnnnnnn<br/> or File communication task INIT (&amp;n,WSCAN=&amp;wscan,RET=&amp;ret) |
| --- | --- |
| Explanation | Communication file related message where the 8 digits (n) represent the record number.<br/> If the jobname is set in the catalog, then the information &lt;JOBNAME=PID&gt; (all platforms except z/OS) or &lt;JOBNAME=jobname jobid&gt; (z/OS platforms) displays in the message in place of (&amp;str). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS21I"></span>CFTS21I PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm]IDT=&amp;idt Exit request ID=&amp;id<br/> CFTS21I Exit request ID=&amp;id &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Information message prior to sending information to the end of transfer exit. |
| Consequence | None. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS22I"></span>CFTS22I Task time out End of transfer exit<br/> CFTS22I Task time out End of transfer exit |
| --- | --- |
| Explanation | Exit task re- entry time- out. The task is stopped automatically. |
| Consequence | The exit task will be rerun by the next call. |
| Action | If necessary, increase the value of the WAITTASK parameter in the CFTEXIT card. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS23E"></span>CFTS23E Bad user return code &lt;details&gt;<br/> CFTS23E &amp;str PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt ", &amp;str = Bad End transfer exit version : &amp;ver / &amp;ver<br/> or<br/> CFTS23E &amp;str &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt &gt;") Invalid state transit '&amp;state'- &gt;'&amp;state' or Unknown state &amp;state<br/> or<br/> Unknown action &amp;action or Bad User return code : &amp;scs |
| --- | --- |
| Explanation | Error message specific to the end- of- transfer user exit. The details that display in the message depend on the CFTLOG format (v23 or v24).<br/> **Example**<br/> V24 format:<br/> <code>CFTS23E Bad User return code: 4 &lt;IDTU=idtu PART=part1 IDF=idf1 IDT=idt &gt;</code><br/> V23 format:<br/> <code>CFTS23E Bad User return code : 4 PART=part1 IDF=idf1 IDT=idt</code> |
| Consequence | None. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS26E"></span>CFTS26E XTRK task error &amp;str <br/> CFTS26E XTRK task error &amp;str |
| --- | --- |
| Explanation | Error initializing the Sentinel monitoring task. |
| Action | Take note of the complete text of the message (&amp;str) and contact Axway Product Support.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS27E"></span>CFTS27E Synchronous communication task error CR=&amp;cr &amp;str<br/> CFTS27E Synchronous communication task error CR= &amp;cr &amp;str |
| --- | --- |
| Explanation | Synchronous communication task error.<br/> CR=&amp;cr &amp;str CFT<br/> • Internal synchronization error.<br/> • Password authentication error due to invalid user or password. |
| Consequence | Depending on the severity of the problem, the synchronous communication task can be stopped or continued (and either Terminating or Continue is displayed in the accompanying message &amp;str). |
| Action | Notify Technical Support, if necessary. Furnish the complete message as well as the synchronous communication media parameters. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS29I"></span>CFTS29I Cannot acces XTRK task _ &amp;str<br/> CFTS29I Cannot acces XTRK task _ &amp;str |
| --- | --- |
| Explanation | Problem in communication with the Sentinel monitoring task. |
| Action | Take note of the complete text of the message and contact Axway Product Support.  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS30I"></span>CFTS30I XTRK Information &amp;str<br/> CFTS30I XTRK Information &amp;str |
| --- | --- |
| Explanation | Elements of information concerning the Sentinel monitoring task.<br/> The field "&amp;str" is an explicit string:<br/> “Buffer File Info : Current = nn , Max=mm”<br/> where:<br/> • number of current records = nn<br/> • maximum number of records = mm |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS31W"></span>CFTS31W XTRK Warning &amp;str Error Code = &amp;cr<br/> CFTS31W XTRK Warning &amp;str Error Code = &amp;cr |
| --- | --- |
| Explanation | The user is warned of a problem in communication with the Sentinel Server or the Sentinel Agent. The message is stored in the overflow file for the Sentinel monitoring task.<br/> The text of the &amp;str message specifies the possible type of warning:<br/> sendMessage : send of a message to Sentinel |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS32W"></span>CFTS32W TCOMS Connection refused (address=nnn.nnn.nnn.nnn, name=userid)<br/> CFTS32W TCOMS Connection refused |
| --- | --- |
| Explanation | An incoming connection from name=userid and address=nnn.nnn.nnn.nnn was rejected because this address is not authorized for Synchronous Communication.<br/> See the CFTCOM object ADDRLIST parameter. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS33I"></span>CFTS33I CFTLOG current file before switch<br/> CFTS33I CFTLOG current file before switch :&amp;fname |
| --- | --- |
| Explanation | For SWITCH LOG or SWITCH ACCNT:<br/> • Transfer CFT start- up<br/> • SWITCH operator<br/> • SWITCH cache command<br/> • SWITCH if file is full during a write<br/> • If no switch procedure is defined |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS34I"></span>CFTS34I CFTLOG executed switch proc<br/> CFTS34I CFTLOG execute switch procedure : &amp;fname |
| --- | --- |
| Explanation | For SWITCH LOG or SWITCH ACCNT:<br/> • Transfer CFT start- up<br/> • SWITCH operator<br/> • SWITCH cache command<br/> • SWITCH if file is full during a write<br/> • If no switch procedure is defined |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS35I"></span>CFTS35I CFTLOG current file after switch<br/> CFTS35I CFTLOG current file after switch :&amp;fname |
| --- | --- |
| Explanation | For SWITCH LOG or SWITCH ACCNT:<br/> • Transfer CFT start- up<br/> • SWITCH operator<br/> • SWITCH cache command<br/> • SWITCH if file is full during a write<br/> • If no switch procedure is defined |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS36I"></span>CFTS36I CFTACCNT current file (no switch executed)<br/> CFTS36I CFTACCNT current file (no switch executed): &amp;fname |
| --- | --- |
| Explanation | For SWITCH LOG or SWITCH ACCNT:<br/> • Transfer CFT start- up<br/> • SWITCH operator<br/> • SWITCH cache command<br/> • SWITCH if file is full during a write<br/> • If the two CFTLOG files are full and are not performing the switch:<br/> • CFTS36I CFTLOG current file (no switch executed): CFTOUT<br/> • If the two CFTACCNT files are full and are not performing the switch:<br/> • CFTS36I CFTACCNT current file (no switch executed): Files full |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS37I"></span>CFTS37I CRONJOB ID=&amp;idcron, CRONTAB=&amp;cronname &amp;exec executed, NEXT=&amp;date<br/> CFTS37I CRONJOB: ID=&amp;idcron, CRONTAB=&amp;cronname ACT DONE<br/> CFTS37I CRONJOB: RECONFIG type=CRON DONE |
| --- | --- |
| Explanation | Job defined by &amp;exec is executed correctly; the next submit is indicated in NEXT=date time. |

[ ](../cfti_messages)

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS39E"></span>CFTS38W CRONJOB NEXT TIME CALCUL FAILED, RETURN &amp;ret<br/> CFTS38W CRONJOB NEXT TIME CALCUL IS AFTER 2035, ABORTING, RETURN &amp;ret |
| --- | --- |
| Explanation | Cronjob next time calcul failed. No time corresponding to filter was found. Cronjob is not executed.<br/> or<br/> Cronjob next time calculated is after 2035. Cronjob is not executed. |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS39E"></span><br/> CFTS39E CRONJOB ID=&amp;id, CRONTAB=&amp;cronname exec &amp;fname failed<br/> CFTS39E CRONJOB: ID=&amp;id INACT FAILED: CRONJOB Not Found<br/> CFTS39E CRONJOB: ID=&amp;id ACT FAILED: CRONJOB Not Found |
| --- | --- |
| Explanation |  Job failed. Could not file the file to submit. Check file properties. |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS39E"></span><br/> CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx<br/> CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx |
| --- | --- |
| Explanation | Reserved for z/OS systems. |

| V23 format<br/> V24 format<br/> Fatal | <span id="CFTS40F"></span>CFTS40F CFTACCNT FORMAT=(V23/V24) not available for &amp;fname<br/> CFTS40F CFTACCNT FORMAT=(V23&#124;V24) not available for &amp;fname |
| --- | --- |
| Explanation | An error occurred in the FORMAT=V23/V24 (V23 default) parameter of CFTFILE TYPE=ACCNT. When using the V23 format, the saved description (for ACCOUNT files) is the same as in previous versions. However when using the V24 format, the length for saving is 2048, and the saved description takes into account the new longer field lengths.<br /> <br/> <blockquote> **Note**<br/> The FORMAT parameter for the CFTACCNT command must be the same setting as for CFTFILE TYPE=ACCNT. If not, a message displays in the LOG and Transfer CFT doesnot start.<br/> </blockquote> <div> The message is either:<br /> CFTS40F CFTACCNT FORMAT=V24 not available for CFT.ACCNT <br/> CFTS40F CFTACCNT FORMAT=V23 not available for CFT.ACCNT<br /> Followed by the message: CFTI17F Init error _ Account file .CFT.ACCNT<br/> </div>  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTS41I"></span>CFTS41I Catalog Alert exec &amp;fname executed<br/> CFTS41I Catalog Alert exec &amp;fname executed |
| --- | --- |
| Explanation | The procedure &amp;FNAME for a catalog alert was executed.<br/> • When the critical fill threshold is reached, a message CFTC29W is recorded in the Transfer CFT log.<br/> A batch, which is defined by the CFTCAT TLVWEXEC parameter, is submitted.<br/> • When the alert ceases, a message CFTC30W is recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} log.<br/> A batch, which is defined by the CFTCAT TLVCEXEC parameter, is submitted. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS42E"></span>CFTS42E Catalog Alert exec &amp;fname &amp;str" where &amp;str= "not found" or "failed"<br/> CFTS42E Catalog Alert exec &amp;fname &amp;str |
| --- | --- |
| Explanation | The procedure &amp;FNAME for a catalog alert was not found or failed on access producing this error.<br/> • When the critical fill threshold is reached, a message CFTC29W is recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} log. • The batch, which is defined by the CFTCAT TLVWEXEC parameter, is not executed<br/> <br/> • When the alert ceases, a message CFTC30W is recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} log • The batch, which is defined by the CFTCAT TLVCEXEC parameter, is not executed.<br/>  |

| V23 format<br/> <br/> V24 format<br/> Information | <span id="CFTS43I"></span>CFTS43I &amp;str", &amp;str = RECONFIG PARM CACHE CFTMAIN or RECONFIG &amp;task : Keep &amp;key : &amp;value or Parameter &amp;task &amp;value - - &gt; &amp;value or RECONFIG &amp;task <br/> CFTS43I &amp;str |
| --- | --- |
| Explanation | Displays information about CFTUTIL RECONFIG TYPE=UCONF.<br/> Wwhere &amp;str = "RECONFIG PARM CACHE CFTMAIN "<br/> • &amp;str = "RECONFIG &amp;task : Keep &amp;key : &amp;value"<br/> • &amp;str = "Parameter &amp;task &amp;value - - &gt; &amp;value or RECONFIG &amp;task" |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS44W"></span>CFTS44W Unexpected message Class &amp;n &lt;TASK=&amp;str&gt;<br/> CFTS44W Unexpected Message Class &amp;n &lt;TASK=&amp;task&gt; |
| --- | --- |
| Explanation  | An internal message received by the &amp;str task has an unexpected class value (&amp;n).  |
| Consequence  | The message is ignored.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS45W"></span>CFTS45W Unexpected Message Class &amp;n &lt;TASK=&amp;task&gt;<br/> CFTS45W Unexpected Message Type &amp;n &lt;TASK=&amp;task CLASS=&amp;n&gt; |
| --- | --- |
| Explanation  | An internal message received by the &amp;str task with CLASS=&amp;str has an unexpected type value (&amp;n).  |
| Consequence  | The message is ignored.  |

| V23 format<br/> V24 format<br/> Fatal  | <span id="CFTS46F"></span>CFTS46F CFTPRX error _ &amp;str<br/> CFTS46F CFTPRX error _ &amp;str |
| --- | --- |
| Explanation  | A fatal error occurred in the proxy task (CFTPRX). The error details are in &amp;str.  |
| Consequence  | {{< TransferCFT/axwayvariablesComponentShortName  >}} stops.  |
| Action  | If necessary, contact Axway support.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS47E"></span>CFTS47E CFTPRX error _ &amp;str<br/> CFTS47E CFTPRX error _ &amp;str |
| --- | --- |
| Explanation  | A significant error occurred in the proxy task (CFTPRX). The error is detailed in &amp;str.  |
| Consequence  | The concerned transfer goes into error.  |
| Action  | If necessary, contact Axway support.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS48W"></span>CFTS48W CFTPRX _ &amp;str<br/> CFTS48W CFTPRX _ &amp;str |
| --- | --- |
| Explanation  | An anomaly occurred in the proxy task (CFTPRX). The anomaly details are in &amp;str.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS49I"></span>CFTS49I CFTPRX _ &amp;str<br/> CFTS49I CFTPRX _ &amp;str |
| --- | --- |
| Explanation  | Information message from the Proxy task (CFTPRX). The &amp;str value gives additional details.  |

| V23 format<br/> V24 format<br/> Fatal  | <span id="CFTS50F"></span>CFTS50F CFTJRE error _ &amp;str<br/> CFTS50F CFTJRE error _ &amp;str |
| --- | --- |
| Explanation  | A fatal error occurred when starting the CFT Java task (CFTJRE). The error is detailed in &amp;str.  |
| Consequence  | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopping.  |
| Action  | If necessary, contact {{< TransferCFT/axwayvariablesPlatformorSuiteShortName  >}} support.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS51E"></span>CFTS51E CFTJRE error _ &amp;str<br/> CFTS51E CFTJRE error _ &amp;str |
| --- | --- |
| Explanation  | A significant error occurred in the {{< TransferCFT/axwayvariablesComponentShortName  >}} Java task (CFTJRE). The error is detailed in &amp;str.  |
| Consequence  | The concerned transfer goes in error.  |
| Action  | If necessary, contact {{< TransferCFT/axwayvariablesPlatformorSuiteShortName  >}} support.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS52W"></span>CFTS52W CFTJRE _ &amp;str<br/> CFTS52W CFTJRE _ &amp;str |
| --- | --- |
| Explanation  | An anomaly occurred in the {{< TransferCFT/axwayvariablesComponentShortName  >}} Java task (CFTJRE). The anomaly is detailed in &amp;str.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS53I"></span>CFTS53I CFTJRE _ &amp;str<br/> CFTS53I CFTJRE _ &amp;str |
| --- | --- |
| Explanation  | Information message from the {{< TransferCFT/axwayvariablesComponentShortName  >}} Java task (CFTJRE). The &amp;str value gives additional details.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS54F"></span>CFTS54F CFTACC task fatal CR=&amp;cr &amp;str<br/> CFTS54F CFTACC task fatal CR= &amp;cr &amp;str |
| --- | --- |
| Explanation  | A fatal error occurred in the accelerator task (CFTACC). The error is detailed in &amp;str.  |
| Consequence  | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopping.  |
| Action  | If necessary, contact Axway support.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS55I"></span>CFTS55I Acceleration &amp;str<br/> CFTS55I Acceleration &amp;str |
| --- | --- |
| Explanation  | Information message from the Accelerator task (CFTACC). the &amp;str value gives more detail.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS56E"></span>CFTS56E Central Governance error (&lt;error_code&gt;) &lt;error_msg&gt;<br/> CFTS56E Central Governance &amp;str", &amp;str = &amp;type error (send): (&amp;code) &amp;message<br/> CFTS56E Central Governance &amp;str", &amp;str = &amp;type error (recv): (&amp;code) &amp;message |
| --- | --- |
| Explanation  | An error occurred when executing a &lt;request&gt; on Central Governance. |
| Consequence  | The {{< TransferCFT/axwayvariablesComponentShortName  >}} instance does not display the correct status.  |

| V23 format<br/> <br/> V24 format<br/> Warning  | <span id="CFTS57W"></span>CFTS57W Synchronous communication _ Authentication ignored - authentication_enable=yes but authentication_method=&amp;auth_method<br/> CFTS57W Synchronous communication _ Authentication ignored - authentication_enable=yes but authentication_method=&amp;auth_method |
| --- | --- |
| Explanation  | Possible scenarios include:<br/> • Authentication_enable=yes but authentication_method=none<br/> In the unified configuration authentication is enabled, but no mode is defined: uconf: cft.server.cftcoms.authentication_enable=yes<br/> uconf: cft.server.authentication_method=none<br/><br/> • User=user01 Group=group01 provided a password but authentication_enable=no<br/> In the CONFIG command the PASSWORD parameter is set, but in the unified configuration it is disabled: uconf:cft.server.cftcoms.authentication_enable=no<br/> |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS59E"></span>CFTS59E Multi- node error _ &amp;str<br/> CFTS58F Multi- node error _ &amp;str |
| --- | --- |
| Explanation  | An important error occurred during the transfer recovery. The error is detailed in &amp;str.  |
| Consequence  | The transfer in question is not recovered.  |
| Action  | Contact the support team if necessary.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS60W"></span>CFTS60W Multi- node _ &amp;str<br/> CFTS60W Multi- node _ &amp;str |
| --- | --- |
| Explanation  | An anomaly occurred during the transfer recovery. The anomaly is detailed in &amp;str.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS61I"></span>CFTS61I Multi- node _ &amp;str<br/> CFTS61I Multi- node _ &amp;str |
| --- | --- |
| Explanation  | Displays information about the multi- node transfer recovery phase. The &amp;str value provides additional details.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS62I"></span>CFTS62I &amp;str<br/> CFTS62I &amp;str |
| --- | --- |
| Explanation  | Displays information about UCONF scheduling.<br/> Where:<br/> • &amp;str = "SCHEDULE : Id &amp;id is not a valid uconf entry", or<br/> • &amp;str = " SCHEDULE &amp;id : Parameter &amp;parm '&amp;value' - - &gt; '&amp;value'", or<br/> • &amp;str = "RECONFIG AM &amp;task or SCHEDULED CONFIG &amp;task" |

| V23 format<br/> V24 format<br/> Fatal  | <span id="CFTS63F"></span>CFTS63F Secure Relay fatal error _ &amp;str<br/> CFTS63F Secure Relay fatal error _ &amp;str |
| --- | --- |
| Explanation  | Fatal error detected. &amp;str indicates the reason Secure Relay Master agent is stopped.<br/> Secure Relay is not available. |
| Action  | Check the configuration depending on the reason and restart Transfer CFT.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS64I"></span>CFTS64I Secure Relay _ &amp;str<br/> CFTS64I Secure Relay _ &amp;str |
| --- | --- |
| Explanation  | Secure relay information message : &amp;str= Router agent &amp;n is enabled but has no host or ports.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS64I"></span><span id="CFTS65W"></span>CFTS65W Unable to launch another script for 60s due to the cft.server.max_processing_scripts limit<br/> CFTS65W Unable to launch another script for 60s due to the cft.server.max_processing_scripts limit |
| --- | --- |
| Explanation  | The total number of transfers in the AC, YC and ZC state has reached the <code>cft.server.max_scripts</code> limit and has not decreased for 60s.  |
| Consequence  | A script has been waiting to be launched for at least 60s.  |
| Action  | Check that there is an END command in the EXEC scripts.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS64I"></span><span id="CFTS66E"></span>CFTS66E TFIL error _ &amp;str", &amp;str = Regular expression &amp;mask parsing error or List generation error &amp;scs<br/> CFTS66E TFIL error _ &amp;str |
| --- | --- |
| Explanation  | The regular expression (REGEX) &amp;mask configured in fname (SEND or CFTSEND) can not be parsed with error &amp;scs Consequence.<br/> Where:<br/> • &amp;str: "Regular expression &amp;mask parsing error", or<br/> • &amp;str: " List generation error &amp;scs" |
| Consequence  | Transfer is not executed.  |
| Action  | Check and modify the REGEX &amp;mask value.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS67E"></span>CFTS67E Error replacing variable &amp;var &amp;message<br/> CFTS67E Error replacing variable &amp;var &amp;message |
| --- | --- |
| Explanation  | The **&lt;var&gt;** variable contains blacklisted characters.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS68E"></span>CFTS68E PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm]IDT=&amp;idt _ &amp;fname not executed<br/> CFTS68E _ &amp;fname not executed &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt DIRECT=&amp;direct&gt; |
| --- | --- |
| Explanation  | The **&amp;fname** script was not executed because a variable contained blacklisted characters. The file was nonetheless created, and may contain some data.  |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS71W"></span>CFTS71W Command file Alert fill threshold reached: level=&amp;level ID=&amp;id<br/> CFTS71W Command file Alert fill threshold reached: level=&amp;level ID=&amp;id |
| --- | --- |
| Explanation  | &amp;level of the COM file space has been used. &amp;level is the amount set by the CFTCOM TLVWARN parameter. When the critical fill threshold is reached, a message is recorded in the Transfer CFT log.<br/> A batch in response to the alert, the CFTCOM TLVWEXEC parameter, is submitted. |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTS72W"></span>CFTS72W Command file Alert cleared : level=&amp;level ID=&amp;id<br/> CFTS72W Command file Alert cleared : level=&amp;level ID=&amp;id |
| --- | --- |
| Explanation  | This alert stops when the fill level drops below the TLVCLEAR level.<br/> When the alert stops, the message is recorded in the Transfer CFT log and a batch, the CFTCOM TLVCEXEC parameter, is submitted. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTS73I"></span>CFTS73I Command file Alert exec &amp;fname executed<br/> CFTS73I Command file Alert exec &amp;fname executed |
| --- | --- |
| Explanation  | The &amp;FNAME procedure for a catalog alert was executed.<br/> • If the critical fill threshold is reached, the CFTS71W message is recorded in the Transfer CFT log.<br/> • If the alert ceases, the CFTS72W message is recorded in the Transfer CFT log.<br/> Either way a batch is submitted, as by the CFTCOM TLVCEXEC parameter. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS74E"></span>CFTS74E Command file Alert exec &amp;fname &amp;str"<br/> CFTS74E Command file Alert exec &amp;fname &amp;str" |
| --- | --- |
| Explanation  | The &amp;FNAME procedure for a command file alert was not found or failed on access, producing this error.<br/> • If the critical fill threshold is reached, the CFTS71W message is recorded in the Transfer CFT log.<br/> • If the alert ceases, the CFTS72W message is recorded in the Transfer CFT log.<br/> In either case, the batch defined by the CFTCOM TLVCEXEC parameter is not executed.<br/> Where:<br/> • &amp;str: "not found" or "failed" |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTS79W"></span>CFTS79W Certificate (ID=&amp;id ROOTCID=&amp;rootcid TYPE=&amp;type) will expire on &amp;date<br/> CFTS79W Certificate (ID=&amp;id ROOTCID=&amp;rootcid TYPE=&amp;type) will expire on &amp;date |
| --- | --- |
| Explanation  | A warning message displays if a certificate is about to expire and the UCONF<code> pki.expiration_check.* </code>parameters are set and enabled.<br/> Where:<br/> • &amp;id: is the certificate identifier<br/><br/> • &amp;rootcid: is the certificate authority identifier<br/><br/> • &amp;type: is the type of certificate (root, user,...)<br/><br/> • &amp;date: is the date in UTC format YYYY- MM- DD hh:mm:ss<br/> |

| V23 format<br/> V24 format<br/> Error | <span id="CFTS79E"></span>CFTS79E Certificate (ID=&amp;id ROOTCID=&amp;rootcid TYPE=&amp;type) expired on &amp;date<br/> CFTS79E Certificate (ID=&amp;id ROOTCID=&amp;rootcid TYPE=&amp;type) expired on &amp;date |
| --- | --- |
| Explanation  | An error message displays if a certificate has expired and the UCONF<code> pki.expiration_check.* </code>parameters are set and enabled.<br/> Where: • &amp;id: is the certificate identifier<br/><br/> • &amp;rootcid: is the certificate authority identifier<br/><br/> • &amp;type: is the type of certificate (root, user,...)<br/><br/> • &amp;date: is the date in UTC format YYYY- MM- DD hh:mm:ss<br/> |

