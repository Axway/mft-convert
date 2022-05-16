---

    title: Transfer CFT messages: CFTR
    linkTitle: CFTR messages
    weight: 360

---
This topic lists the CFTRxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=\[V23,V24\]

For V23: <span class="code">`CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`</span>

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`


| V23 format<br/> V24 format<br/> Error | <span id="CFTR02E"></span>CFTR02E &amp;cmd Failed _ Invalid date or time<br/> CFTR02E &amp;cmd Failed _ Invalid date or time |
| --- | --- |
| Explanation | The &amp;cmd command contains at least one invalid date or time. |
| Consequence | The command is ignored. |
| Action | Check the command syntax. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR03E"></span>CFTR03E &amp;cmd Failed _ No record found<br/> CFTR03E &amp;cmd Failed _ No record found for &amp;str |
| --- | --- |
| Explanation | The &amp;cmd command cannot be associated with any record in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog file (example: deletion of a non-existing record). |
| Consequence | The command is ignored. |
| Action | Check the command syntax. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR04E"></span>CFTR04E &amp;cmd Failed _ Keyword &amp;keyw too large<br/> CFTR04E &amp;cmd Failed _ Keyword &amp;keyw too large |
| --- | --- |
| Explanation | The length of the &amp;keyw keyword is greater than 8. |
| Consequence | The command is ignored. |
| Action | Check the description of this parameter in the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR05E"></span>CFTR05E &amp;cmd Failed _ Illegal separator for keyword &amp;keyw<br/> CFTR05E &amp;cmd Failed _ Illegal separator for keyword &amp;keyw |
| --- | --- |
| Explanation | A parameter separator in the &amp;cmd command is invalid. |
| Consequence | The command is ignored. |
| Action | Check the command syntax (the separator must be a comma), correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR06E"></span>CFTR06E &amp;cmd Failed _ Keyword &amp;keyw, missing quote<br/> CFTR06E &amp;cmd Failed _ Keyword &amp;keyw ; missing quote |
| --- | --- |
| Explanation | A closing quote (') is missing in the value assigned to the &amp;keyw keyword. |
| Consequence | The command is ignored. |
| Action | Check the offending parameter, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR07E"></span>CFTR07E &amp;cmd Failed _ Too many keywords<br/> CFTR07E &amp;cmd Failed _ Too many keywords |
| --- | --- |
| Explanation | There are too many keywords for this command. |
| Consequence | The command is ignored. |
| Action | Check the command syntax, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR08E"></span>CFTR08E &amp;cmd Failed _ Keyword &amp;keyw unknown or duplicate<br/> CFTR08E &amp;cmd Failed _ Keyword &amp;keyw unknown or duplicate |
| --- | --- |
| Explanation | The &amp;keyw keyword is unknown or duplicated in the command. |
| Consequence | The command is ignored. |
| Action | Check the command syntax, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR09E"></span>CFTR09E &amp;cmd Failed _ Keyword &amp;keyw missing<br/> CFTR09E &amp;cmd Failed _ Keyword &amp;keyw missing |
| --- | --- |
| Explanation | The &amp;keyw keyword, which is mandatory for the command, is missing. |
| Consequence | The command is ignored. |
| Action | Check the command syntax, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR10E"></span>CFTR10E &amp;cmd Failed _ Keyword &amp;keyw value out of bounds<br/> CFTR10E &amp;cmd Failed _ Keyword &amp;keyw value out of bounds |
| --- | --- |
| Explanation | The &amp;keyw keyword of the &amp;cmd command is numeric and its value is outside the authorized limits. |
| Consequence | The command is ignored. |
| Action | Check the possible values for this parameter, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR11E"></span>CFTR11E &amp;cmd Failed _ Invalid value for keyword &amp;keyw<br/> CFTR11E &amp;cmd Failed _ Invalid value for keyword &amp;keyw |
| --- | --- |
| Explanation | The value of the &amp;keyw keyword of the &amp;cmd command is not authorized (for example: numeric value for an alphabetic-type parameter). |
| Consequence | The command is ignored. |
| Action | Check the possible values for this parameter, correct the error and then resubmit the command. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTR12I"></span>CFTR12I &amp;cmd PART=&amp;part [IDF=&amp;idf | IDM=&amp;idm]IDT=&amp;idt Treated FOR USER=&amp;user &amp;str<br/> CFTR12I &amp;cmd PART=&amp;part [IDF=&amp;idf | IDM=&amp;idm]IDT=&amp;idt Treated FOR USER=&amp;user &amp;str |
| --- | --- | --- | --- |
| Explanation | The command was executed correctly.<br/> The partner's name (&amp;part), the IDF (&amp;idf) and the IDT (&amp;idt) are only defined if it is a SEND or RECV command.<br/> If the jobname is set in the catalog, then the information &lt;JOBNAME=PID&gt; (all platforms except z/OS) or &lt;JOBNAME=jobname jobid&gt; (z/OS platforms) displays in the message in place of (&amp;str). |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR13E"></span>CFTR13E &amp;cmd Failed _ IDT=&amp;idt not allowed<br/> CFTR13E SEND &amp;cmd PART=&amp;part Failed _ IDT=&amp;idt not allowed |
| --- | --- |
| Explanation | During execution of a command (response to a message or transfer for example) the required transfer identifier (&amp;idt) was not found in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog. |
| Consequence | The command is ignored. |
| Action | Check the parameter settings of the command and the transfer identifier value. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTR14W"></span>CFTR14W &amp;cmd Failed PART=&amp;part _ No transfer found for this request<br/> CFTR14W &amp;cmd PART=&amp;part Failed _ No transfer found for this request |
| --- | --- |
| Explanation | When processing an ACT or INACT command, no transfer in the 'D' state and with DIAGI=430 for ACT or in the 'C' state for INACT was found in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog for the partner(s) designated by &amp;part. |
| Consequence | The command takes effect for any subsequent transfers concerning the partner(s). |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTR15W"></span>CFTR15W &amp;cmd not treated for user &amp;user<br/> CFTR15W &amp;cmd not treated for user &amp;user |
| --- | --- |
| Explanation | The security system has refused to execute the MQUERY or SHUT command. The CFTX03W message is displayed before this message. |
| Consequence | The command is ignored. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTR16I"></span>CFTR16I &amp;message<br/> CFTR16I &amp;message |
| --- | --- |
| Explanation | Information concerning either the TURN command or the WLOG command.<br/> QQQ_QQQ_QQQ_LIST<br/> • TURN command: • PART=&amp;part<br/> • MODE=&amp;mode (&amp;str) &amp;mode: create,replace,delete<br/> • &amp;str: “part not found”,”part inact”,”prot DMZ not found” ,”part not in requester mode","commutation not available”,"see omintime,omaxtime”,”already in command cache”,”not into command cache”<br/> <br/> QQQ_QQQ_QQQ_LIST<br/> • WLOG command: • &amp;message displayed in the {{< TransferCFT/axwayvariablesComponentShortName  >}} LOG<br/>  |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTR17I"></span>CFTR17I &amp;cmd In progress for USER &amp;user &amp;message<br/> CFTR17I &amp;cmd &amp;message In progress for USER &amp;user |
| --- | --- |
| Explanation | This information message displays at the beginning of the processing for the &amp;cmd.<br/> • Cmd =DELETE/END<br/> • Message where PART=&amp;part IDF=&amp;idf [ IDT=&amp;idt IDTU=&amp;idtu IDA=&amp;ida STATE=&amp;state] |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTR18W"></span>CFTR18W &amp;message<br/> CFTR18W &amp;message |
| --- | --- |
| Explanation | Displays warning status in the WLOG command.<br/> • &amp;message displayed in the {{< TransferCFT/axwayvariablesComponentShortName  >}} LOG |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR19E"></span>CFTR19E &amp;message<br/> CFTR19E &amp;message |
| --- | --- |
| Explanation | Displays error status in the WLOG command.<br/> • &amp;message displayed in the {{< TransferCFT/axwayvariablesComponentShortName  >}} LOG |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTR20I"></span>CFTR20I &amp;message<br/> CFTR20I &amp;message |
| --- | --- |
| Explanation | Information concerning the folder monitoring functionality.<br/> • For each configured directory a related message is written to the log.<br/> • When a file is automatically submitted for emission, a log message is also written to the log. |
| V23 format<br/> V24 format<br/> Error | <span id="CFTR20E"></span>CFTR20E &amp;message<br/> CFTR20E &amp;message |
| Explanation | Error messages originating from the folder monitoring functionality.<br/> Usually triggered by error conditions encountered on file manipulation as renaming, deleting. |
| Warning  | CFTR20W &amp;message  |
| Explanation | Warning message related to the folder monitoring feature.<br/> Indicates that a system error concerning the event subsystem occurred (see <a href="../../../app_integration_intro/intro_folder_monitor/configure_folder_monitoring">USEFSEVENT</a>=YES). |
| V23 format<br/> V24 format<br/> Fatal | <span id="CFTR20F"></span>CFTR20F &amp;message<br/> CFTR20F &amp;message |
| Explanation | Fatal messages originating from the folder monitoring functionality.<br/> Indicates that a severe error condition was encountered and is preventing this functionality from proceeding normally. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR21E"></span>CFTR21E &amp;cmd Failed _ No record found &lt;IDTU=&amp;idtu PART=&amp;part IDT=&amp;idt&gt;<br/> CFTR21E &amp;cmd Failed _ No record found &lt;IDTU=&amp;idtu PART=&amp;part IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | During the internal command execution (&amp;cmd) the required unique transfer identifier (&amp;idtu) was not found in the Transfer CFT catalog. |
| Consequence  | The command is ignored.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTR22E"></span>CFTR22E &amp;cmd Failed _ Set FORCE=YES to immediately retry to rename &lt;IDTU=&amp;idtu PART=&amp;part IDT=&amp;idt PHASE=Y PHASESTEP=R&gt;<br/> CFTR22E &amp;cmd Failed _ Set FORCE=YES to immediately retry to rename &lt;IDTU=&amp;idtu PART=&amp;part IDT=&amp;idt PHASE=Y PHASESTEP=R&gt; |
| --- | --- |
| Explanation | A transfer in the YR phase/phasestep was found for the START command, but it has no effect as FORCE is set to NO. |
| Consequence  | The command is ignored.  |


 


| V23 format<br/> V24 format<br/> Information | CFTR23I On &amp;time UserId=&amp;userid, JobName=&amp;jobname ran the command &amp;command<br/> CFTR23I On &amp;time UserId=&amp;userid, JobName=&amp;jobname ran the command &amp;command |
| --- | --- |
| Explanation | This message traces who has cleared what and with which command. |

