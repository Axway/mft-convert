---
title: "Transfer CFT messages:  CFTI"
linkTitle: "CFTI messages"
weight: 320
--- This topic lists the CFTIxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

| V23 format<br/> V24 format<br/> Information | <span id="CFTI00I"></span>CFTI00I Snumb of spawned procedure is &amp;str1:&amp;str2<br/> CFTI00I Snumb of spawned procedure is &amp;str1:&amp;str2 |
| --- | --- |
| Explanation | *z/OS only*<br/> Information concerning the submission of a procedure that has its job identifier specified in &amp;str1 and the &amp;str2 (which represent the JOBID and JOBNAME). |

| V23 format<br/> V24 format<br/> Fatal | <span id="CFTI01F"></span>CFTI01F &amp;str <br/> CFTI01F &amp;str  |
| --- | --- |
| Explanation | Internal {{< TransferCFT/axwayvariablesComponentShortName  >}} execution error.<br/> The field "&amp;str" can have the following values:<br/> • CFT error &amp;scs:{{< TransferCFT/axwayvariablesComponentShortName  >}} inter- task communication system problem (waiting for the CFTMAIN scheduler task queue)<br/> • CFT error _ usage expired:The {{< TransferCFT/axwayvariablesComponentShortName  >}} user key (CFTPARM KEY) does not authorize {{< TransferCFT/axwayvariablesComponentShortName  >}} execution beyond the expired period<br/> • CFT error _ CFT usage not authorized:The {{< TransferCFT/axwayvariablesComponentShortName  >}} user key (CFTPARM KEY) does not authorize {{< TransferCFT/axwayvariablesComponentShortName  >}} execution on this operating system or computer<br/> • CFT error _ file keys not available: The {{< TransferCFT/axwayvariablesComponentShortName  >}} user keys are stored in an indirection file (CFTPARM KEY parameter); this file cannot be accessed by {{< TransferCFT/axwayvariablesComponentShortName  >}}<br/> • CFT error &amp;scs _ Common_area allocation failed:Definition of the memory area common to the {{< TransferCFT/axwayvariablesComponentShortName  >}} tasks has failed. This can be caused by insufficient memory<br/> • CFT error &amp;scs _ Mailbox definition failed: {{< TransferCFT/axwayvariablesComponentShortName  >}} is unable to link to a mailbox defined by the *CFTOM command<br/> • CFT error &amp;scs _ CFT semaphore definition failed:{{< TransferCFT/axwayvariablesComponentShortName  >}} is unable to define an inter- task communications queue<br/> • CFT error _ CFTEXIT ID=&amp;id missing: A {{< TransferCFT/axwayvariablesComponentShortName  >}} task dedicated to file EXITs could not be activated (the CFTEXIT command relating to the identifier mentioned (ID) was not found)<br/> • CFT error _ Maximum process CFTEXIT running reached: A {{< TransferCFT/axwayvariablesComponentShortName  >}} task dedicated to file EXITs could not be activated (the maximum number of EXIT processes that can be activated has already been reached)<br/> • CFT error &amp;cs _ Initializing process CFTEXIT: A {{< TransferCFT/axwayvariablesComponentShortName  >}} task dedicated to file EXITs could not be activated (the maximum number of EXIT processes that can be activated has already been reached)<br/> • CFT error _ &amp;Net Network Access Method Option not authorized by license key:The {{< TransferCFT/axwayvariablesComponentShortName  >}} is NOT authorized to use the optional network access method designated by &amp;Net (TCP/IP).<br/> • CFT error _ SSL Protocol Option not authorized by license key:A protocol defined in the CFTPARM object uses the SSL option, but the SSL option is not available with this license key.<br/> • CFT error _ FIPS Compliance Option not authorized by license key:The uconf:cft.fips.enable_compliance parameter is set to Yes, but the FIPS option is not available with this license key.<br/> • CFT error _ File Transfer Acceleration Option not authorized by license key:The uconf:acceleration.enable parameter is set to Yes, but the acceleration option is not available with this license key. |
| Consequence | The transfer concerned by the incident is interrupted, which is the K status. |
| Action | Check parameter settings, analyze the &amp;cs code value to determine, if necessary, the origin of the error:<br/> • CFT error &amp;scs _ LOG stop failed: The message logging task cannot be stopped<br/> • CFT error &amp;scs _ mailbox delete failed: A mailbox defined by a CFTCOM command cannot be deleted |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Analyze the &amp;scs. code to determine the exact origin of the error. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTI01W"></span>CFTI01W &amp;str<br/> CFTI01W &amp;str  |
| --- | --- |
| Explanation | CFT error &amp;scs _ Initializing process CFTTFIL: A {{< TransferCFT/axwayvariablesComponentShortName  >}} task dedicated to transfer file access could not be activated. |
| Consequence  | {{< TransferCFT/axwayvariablesComponentShortName  >}} is not stopped, and transfers are not interrupted.  |
| Action | No action necessary. |

| V23 format<br/> V24 format<br/> Fatal | <span id="CFTI02F"></span>CFTI02F Init Error code &amp;scs _ Allocating param. file &amp;fname<br/> CFTI02F Init Error code &amp;scs _ Allocating param. file &amp;fname |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when allocating the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter file. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Check that the file is not already allocated; if it exists, correct the error and then restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI03F"></span>CFTI03F Init Error code &amp;scs _ Opening param. file &amp;fname<br/> CFTI03F Init Error code &amp;scs _ Opening param. file &amp;fname |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when opening the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter file. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Correct the error and then restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI04F"></span>CFTI04F Init Error code &amp;scs _ Allocating partners file &amp;fname<br/> CFTI04F Init Error code &amp;scs _ Allocating partners file &amp;fname |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when allocating the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner file. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Check that the file is not already allocated, correct the error and then restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI05F"></span>CFTI05F Init Error code &amp;scs _ Opening partners file &amp;fname<br/> CFTI05F Init Error code &amp;scs _ Opening partners file &amp;fname |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when opening the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner file. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Correct the error and then restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI06F"></span>CFTI06F Init Error code &amp;scs _ Allocating catalog file &amp;fname<br/> CFTI06F Init Error code &amp;scs _ Allocating catalog file &amp;fname |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when allocating the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog file. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Check that the file is not already allocated, correct the error and then restart {{< TransferCFT/axwayvariablesComponentShortName  >}}. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI08F"></span>CFTI08F Init error _ Protocol process<br/> CFTI08F Init error _ Protocol process |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when activating the {{< TransferCFT/axwayvariablesComponentShortName  >}} protocol process. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Inform Transfer CFT Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI09F"></span>CFTI09F Init error _ Communication process<br/> CFTI09F Init error _ Communication process |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when activating the {{< TransferCFT/axwayvariablesComponentShortName  >}} communication process. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Inform Transfer CFT Support. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI10F"></span>CFTI10F Init error _ Logger process<br/> CFTI10F Init error _ Logger process |
| --- | --- |
| Explanation | During {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization an error was detected when activating the {{< TransferCFT/axwayvariablesComponentShortName  >}} message logging process.<br/> It may be a memory allocation or queue definition type system error (or a problem when submitting a message to the queue). |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Inform Product Support. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTI11I"></span>CFTI11I Init complete _ Logger process<br/> CFTI11I Init complete _ Logger process |
| --- | --- |
| Explanation | Normal end of {{< TransferCFT/axwayvariablesComponentShortName  >}} logging process initialization. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTI12I"></span>CFTI12I Init complete _ Protocol process<br/> CFTI12I Init complete _ Protocol process |
| --- | --- |
| Explanation | Normal end of {{< TransferCFT/axwayvariablesComponentShortName  >}} protocol process initialization. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTI13I"></span>CFTI13I Init complete _ Communication process<br/> CFTI13I Init complete _ Communication process |
| --- | --- |
| Explanation | Normal end of {{< TransferCFT/axwayvariablesComponentShortName  >}} communication task initialization. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI14I"></span>CFTI14I CFT Init complete<br/> CFTI14I CFT Init complete |
| --- | --- |
| Explanation | Normal end of {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI15F"></span>CFTI15F Error code &amp;ncs _ Trying to define resource &amp;id<br/> CFTI15F Error code &amp;ncs _ Trying to define resource &amp;id |
| --- | --- |
| Explanation | Rejection of the define resource request specified by the network interface. The &amp;ncs return code explains the cause of the rejection. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Review Transfer CFT parameter settings (CFTNET commands). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI16F"></span>CFTI16F Error code &amp;ncs _ Register request<br/> CFTI16F Error code &amp;ncs _ Register request |
| --- | --- |
| Explanation | Rejection of the register request specified by the network interface. The &amp;ncs return code explains the cause of the rejection. |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Review Transfer CFT parameter settings (CFTPROT commands). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI17F"></span>CFTI17F Init error _ Account file &amp;fname<br/> CFTI17F Init error _ Account file &amp;fname |
| --- | --- |
| Explanation | During the {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization phase an error was detected when processing the accounting file (CFTACCNT command). |
| Consequence | The Transfer CFT initialization phase has stopped. |
| Action | Check the existence and integrity of the &amp;fname file. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI18I"></span>CFTI18I _ &amp;str<br/> CFTI18I _ &amp;str |
| --- | --- |
| Explanation | This is a {{< TransferCFT/axwayvariablesComponentShortName  >}} welcome message describing the computer environment and the main runtime characteristics, according to the options activated by the software key (KEY parameter):<br/> • Usage of this product is strictly limited to &amp;cpu_id machine: The {{< TransferCFT/axwayvariablesComponentShortName  >}} can only be executed on the computer with the designated CPU<br/> • Usage of this product is strictly limited to &amp;label: The {{< TransferCFT/axwayvariablesComponentShortName  >}} can only be executed within a specific framework, as designated by &amp;label<br/> • Usage of this product is strictly limited until &amp;date: The {{< TransferCFT/axwayvariablesComponentShortName  >}} cannot be executed after the date designated by &amp;date<br/> • &amp;Maxtrans simultaneous transfer(s) is(are) authorized: {{< TransferCFT/axwayvariablesComponentShortName  >}} cannot process more than &amp;Maxtrans simultaneous transfers. This value overrides the MAXTRANS parameter in the CFTPARM command<br/> • &amp;Net Network Access Method Option is authorized: The {{< TransferCFT/axwayvariablesComponentShortName  >}} is authorized to use the optional network access method designated by &amp;Net (TCP/IP) • The information in this message is related to the UCONF setting for server.authentication_method.<br/> • For more information on Access Management parameters, see the Password authentication for synchronous communication media section in [Access Management and PassPort AM parameters](../../../internal_a_m_start_here/about_passport_am/unconf_access_management). |
| Consequence | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped during the initialization phase. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTI18W"></span>CFTI18W+ Version mismatch between Transfer &amp;CFTVersion and the UCONF dictionary &amp;UCONFVersion<br/> CFTI18W+ Version mismatch between Transfer &amp;CFTVersion and the UCONF dictionary &amp;UCONFVersion<span id="CFTI19I"></span> |
| --- | --- |
| Explanation | The UCONF dictionary was created by a different version of Transfer CFT, most likely due to a non- standard installation. |
| Consequence  | There is a risk of failure.  |
| Action  | Properly reinstall the product.  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTI19I"></span>CFTI19I © Copyright AXWAY,....<br/> CFTI19I © Copyright AXWAY,.... |
| --- | --- |
| Explanation | {{< TransferCFT/axwayvariablesComponentShortName  >}} copyright message. |

| V23 format<br/> V24 format<br/> Fatal | <span id="CFTI20F"></span>CFTI20F Semaphore definition failure CR=&amp;cr CS= &amp;scs<br/> CFTI20F Semaphore definition failure CR=&amp;cr CS= &amp;scs |
| --- | --- |
| Explanation | Cannot define the internal communications queue. |
| Consequence | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped during its initialization phase. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI21F"></span>CFTI21F CFTNET=&amp;id Resource define failure CS=&amp;ncs<br/> CFTI21F CFTNET=&amp;id Resource define failure CS=&amp;ncs |
| --- | --- |
| Explanation | Cannot define the resource. The resource identifier for this CFTNET command displays in the message. |
| Consequence | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped during its initialization phase. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI22F"></span>CFTI22F CFTPROT=&amp;id Register request failure CS=&amp;ncs<br/> CFTI22F CFTPROT=&amp;id Register request failure CS=&amp;ncs |
| --- | --- |
| Explanation | Cannot register the protocol defined in this CFTPROT command. |
| Consequence | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped during its initialization phase. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI23F"></span>CFTI23F MAIN synchronization failure CR=&amp;cr CS=&amp;scs<br/> CFTI23F MAIN synchronization failure CR=&amp;cr CS=&amp;scs |
| --- | --- |
| Explanation | Internal synchronization error between the main Transfer CFT task and the protocol task. |
| Consequence | {{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped during its initialization phase. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTI24I"></span>CFTI24I &amp;str<br/> CFTI24I &amp;str |
| --- | --- |
| Explanation | Message displayed when viewing the command cache or the transfer cache: CFTUTIL or CFTINT MQUERY command.<br/> The messages depend on the type of cache concerned (command or catalog):<br/> * TRANSFER CACHE IS EMPTY<br/> The catalog cache is empty.<br/> The messages vary according to the context:Or gives details of the cache information (catalog or command cache) according to the type of information displayed<br/> For a line in the command cache, the information is divided into three parts:<br/> • command execution<br/> • DATE and TIME<br/> • type of command (SWITCH ACCNT, SWITCH LOG or PURGE)<br/> For a transfer, the information is divided into four parts:<br/> • request activation time<br/> • identifier of the partner concerned<br/> • idf identifier and<br/> • IDT value calculated by {{< TransferCFT/axwayvariablesComponentShortName  >}} |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI25I"></span>CFTI25I Init complete _ Security active [&amp;str]<br/> CFTI25I Init complete _ Security active [&amp;str] |
| --- | --- |
| Explanation | The description of the message &amp;str specifies the activated security options:<br/> • HAB: Normal end of initialization with activation of the {{< TransferCFT/axwayvariablesComponentShortName  >}} security system<br/> • SSL: Normal end of initialization with activation of the SSL protocol<br/> The information in this message is affected by the UCONF setting for access management. For more information, see the am.type parameter and access management options in [Access Management and PassPort AM parameters](../../../internal_a_m_start_here/about_passport_am/unconf_access_management). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI26I"></span>CFTI26I Init complete _ Security not active<br/> CFTI26I Init complete _ Security not active |
| --- | --- |
| Explanation | Normal end of initialization without activating the Security option ({{< TransferCFT/axwayvariablesComponentShortName  >}} security system and the SSL protocol). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTI27F"></span>CFTI27F Init Error code &amp;scs _ Opening security file &amp;file<br/> CFTI27F Init Error code &amp;scs _ Opening security file &amp;file |
| --- | --- |
| Explanation | When {{< TransferCFT/axwayvariablesComponentShortName  >}} was initialized, a security system open error was detected. |
| Consequence | The Transfer CFT initialization phase is stopped. |
| Action | Inform Product Support. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI28I"></span>CFTI28I Init complete <br/> CFTI28I Init complete |
| --- | --- |
| Explanation | Message on initialization of CFTMAIN. With following Message CFTI18I FNAME :catalog name. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI34I"></span>CFTI34I PID=&amp;id &amp;task Task started successfully<br/> CFTI34I PID=&amp;id &amp;task Task started successfully |
| --- | --- |
| Explanation | The &amp;task Task whose internal identifier is &amp;pid has been started successfully. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI35I"></span>CFTI35I PID=&amp;id &amp;task Task ended<br/> CFTI35I PID=&amp;id &amp;task Task ended |
| --- | --- |
| Explanation | The &amp;task Task whose internal identifier is &amp;pid has stopped. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI36I"></span>CFTI36I CRONJOB: ID=&amp;idcron, CRONTAB=&amp;cronname &amp;str<br/> CFTI36I CRONJOB: ID=&amp;idcron, CRONTAB=&amp;cronname &amp;str |
| --- | --- |
| Explanation | &amp;Idcron = CFTCRON command identifier<br/> &amp;Cronname = id of the list in CRONTABS in CFTPARM<br/> &amp;Str=<br/> • INSERT OK: NEXT= date time ,TIME=&amp;str - (&amp;str= TIME of CFTCRON command defined by ID)<br/> • INSERT OK: NOACTIVE- The CFTCRON with a STATE=NOACTIVE in the configuration is not activated.<br/> • Not enabled - (cronname not defined in the CRONTABS list) |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTI38E"></span>CFTI38E CRONJOB: ID=&amp;idcron, CRONTAB=&amp;cronname INSERT FAILED<br/> CFTI38E CRONJOB: ID=&amp;idcron, CRONTAB=&amp;cronname INSERT FAILED |
| --- | --- |
| Explanation |  Insert failed due to incorrect entry or unexpected character. Check CRONTAB parameters, such as TIME. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI39I"></span>CFTI39I &amp;str<br/> CFTI39I &amp;str |
| --- | --- |
| Explanation | Displays information about the {{< TransferCFT/axwayvariablesComponentShortName  >}} Heartbeat. Possible states:<br/> • Enable<br/> • Update UCONF parameters<br/> • Disable |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTI40E"></span>CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx<br/> CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx |
| --- | --- |
| Explanation | z/OS only<br/> If the OMVS segment is not defined for Transfer CFT and/or the Transfer CFT Copilot server owner, then Transfer CFT, the Transfer CFT Copilot server, or CFTUTIL (synchronous communication) stops and displays this message.<br/> To disable the display and check option, modify the environment variable in the ..UPARM(CNFENV)target file as follows:<br/> omvs_check_disable=1. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI41I"></span>CFTI41I OMVS information for user=xxxxxx,uid=n,gid=n,home=(/xxxxx)<br/> CFTI41I OMVS information for user=xxxxxx,uid=n,gid=n,home=(/xxxxx) |
| --- | --- |
| Explanation | z/OS only<br/> If the OMVS segment is defined for Transfer CFT and/or the Transfer CFT Copilot server owner, this information message displays.<br/> To disable the display and check option, modify the environment variable in the ..UPARM(CNFENV)target file as follows:<br/> omvs_check_disable=1. |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTI42E"></span>CFTI42E PID=&amp;pid &amp;task Task startup error failed to lock resource '&amp;pid_file_name': resource already locked<br/> CFTI42E PID=&amp;pid &amp;task Task startup error failed to lock resource '&amp;pid_file_name': resource already locked |
| --- | --- |
| Explanation | The pid_file_name that is used to ensure process uniqueness could not be locked, causing the task to fail.<br/> Locate and stop the locked process. If this does not resolve the issue, restart the server using the "force- stop" mode. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTI42E"></span><span id="CFTI43I"></span>CFTI43I Attention: The Transfer CFT license expires in n days<br/> CFTI43I Attention: The Transfer CFT license expires in n days |
| --- | --- |
| Explanation | There are 7 days remaining on the license key for Transfer CFT. |
| Action  | To obtain a new key:<br/> • For an existing installation, use the command **<code>cftutil about</code>** to retrieve your system information. The **ABOUT** command displays the Transfer CFT product, host, and key information, and characteristics of the platform on which Transfer CFT is installed.<br/> • Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key. • For a US key, contact: <code>fulfillment@us.axway.com</code><br/> • For an EMEA or APAC key, contact: <code>product.key@axway.com</code><br/> <br/> • Provide the hostname and system information for the installed or updated Transfer CFT.<br/> To apply the key:<br/> To apply the license key from the Axway Fulfillment team, either:<br/> • Enter the key directly.<br/> • Enter the key(s) in the indirection file.<br/> <blockquote> **Note**<br/> When working in multi- node you must have one key per node and host.<br/> </blockquote> See the *Apply a license key* section in the Transfer CFT Installation Guide that corresponds with your OS for details. |

| V23 format<br/> V24 format<br/> Warning  | <span id="CFTI42E"></span><span id="CFTI43W"></span>CFTI43W Attention: The Transfer CFT license expires in n days<br/> CFTI43W Attention: The Transfer CFT license expires in n days |
| --- | --- |
| Explanation | There are fewer than 7 days remaining on the license key for Transfer CFT. |
| Action  | To obtain a new key:<br/> • For an existing installation, use the command **<code>cftutil about</code>** to retrieve your system information. The **ABOUT** command displays the Transfer CFT product, host, and key information, and characteristics of the platform on which Transfer CFT is installed.<br/> • Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key. • For a US key, contact: <code>fulfillment@us.axway.com</code><br/> • For an EMEA or APAC key, contact: <code>product.key@axway.com</code><br/> <br/> • Provide the hostname and system information for the installed or updated Transfer CFT.<br/> To apply the key:<br/> To apply the license key from the Axway Fulfillment team, either:<br/> • Enter the key directly.<br/> • Enter the key(s) in the indirection file.<br/> <blockquote> **Note**<br/> When working in multi- node you must have one key per node and host.<br/> </blockquote> See the *Apply a license key* section in the Transfer CFT Installation Guide that corresponds with your OS for details. |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTI42E"></span><span id="CFTI43E"></span>CFTI43E Attention: The Transfer CFT license has expired<br/> CFTI43E Attention: The Transfer CFT license has expired |
| --- | --- |
| Explanation | The Transfer CFT license key is no longer valid. |
| Action  | To obtain a new key:<br/> • For an existing installation, use the command **<code>cftutil about</code>** to retrieve your system information. The **ABOUT** command displays the Transfer CFT product, host, and key information, and characteristics of the platform on which Transfer CFT is installed.<br/> • Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key. • For a US key, contact: <code>fulfillment@us.axway.com</code><br/> • For an EMEA or APAC key, contact: <code>product.key@axway.com</code><br/> <br/> • Provide the hostname and system information for the installed or updated Transfer CFT.<br/> To apply the key:<br/> To apply the license key from the Axway Fulfillment team, either:<br/> • Enter the key directly.<br/> • Enter the key(s) in the indirection file.<br/> <blockquote> **Note**<br/> When working in multi- node you must have one key per node and host.<br/> </blockquote> See the *Apply a license key* section in the Transfer CFT Installation Guide that corresponds with your OS for details. |

| V23 format<br/> V24 format<br/> Warning  | CFTI18W Version mismatch between Transfer CFT &amp;CFTVersion and the UCONF dictionary &amp;UCONFVersion<br/> CFTI18W Version mismatch between Transfer CFT &amp;CFTVersion and the UCONF dictionary &amp;UCONFVersion |
| --- | --- |
| Explanation | The UCONF dictionary was created with a different Transfer CFT version than the product, probably due to a non- standard installation. |
| Consequence  | There is a risk of failure.  |
| Action  | Perform a standard installation to reinstall the product.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTS39E"></span><br/> CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx<br/> CFTI40E OMVS SEGMENT NOT DEFINED for user=xxxxxx |
| --- | --- |
| Explanation | Reserved for z/OS systems. |

