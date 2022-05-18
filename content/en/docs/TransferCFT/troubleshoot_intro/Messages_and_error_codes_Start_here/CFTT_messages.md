---
title: "Transfer CFT messages: CFTT "
linkTitle: "CFTT messages"
weight: 370
--- This topic lists the CFTTxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

| V23 format<br/> V24 format<br/> Error | CFTT00E CFT request warning _ &amp;str<br/> CFTT00E CFT request warning - &amp;str |
| --- | --- |
| Explanation | An error may have occurred during a request sent to the Transfer CFT. The &amp;str message label specifies the possible type of error:<br/> • Unknown protocol request: Internal error of the Transfer CFT, which receives an unexpected protocol event<br/> • Unknown oper request: The operator command received is unknown<br/> • Not computable state: The transfer is not possible (status other than "D" (Available)<br/> • Transfer for myself rejected: The target of the transfer is the local site (CFTPARM PART field) The Transfer CFT therefore refuses to activate a transfer to itself<br/> • Logger currently unreachable: A SWITCH command, switching log files, was attempted while the LOG task (CFT log) was not (or no longer) active<br/> • Ignored (Catalog Full): The catalog was 100% full and so the last SEND (or RECV) command could not be processed; if it was submitted via the Transfer CFT communication file, it is stored in the file and the associated communication process stops (for more information, refer to the CFTC01W message)<br/> • Request syntax error: There is a syntax error in the request; inform Product Support<br/> • Catalog request unknown :The request is invalid<br/> • Unknown process request: An internal Transfer CFT error was detected; inform Product Support<br/> • Unknown file request: An internal Transfer CFT error was detected; inform Product Support<br/> • Transfer already in progress: An attempt was made to restart a transfer in progress; the transfer was not restarted<br/> • File already transferred: An attempt was made to restart a terminated transfer; the transfer was not restarted<br/> • &amp;file: A read error was detected on the authorizations file (&amp;file); inform Product Support<br/> • Access Exit Task unreachable: &amp;diagp: The directory exit task cannot be accessed (DIAGP specifies the cause); inform Product Support<br/> • Request Ignored : time- out: The request was not processed by the Transfer CFT (end time limit exceeded)<br/> • Unable to attach Client mailbox: Attachment to the client application was rejected<br/> • Unable to send data to Client mailbox: Data cannot be sent to the client application |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT01E"></span>CFTT01E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Open mode not allowed<br/> CFTT01E IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Open mode not allowed |
| --- | --- |
| Explanation | A transfer request was made in OPEN mode, but this mode is not supported for the partner concerned. |
| Consequence | The transfer is denied. The corresponding catalog entry is set to the KEEP status. |
| Action | Transfer the files with this partner in closed mode. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT02E"></span>CFTT02E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Transfer Area Full<br/> CFTT02E _ Transfer Area Full &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | A new transfer request was made but the maximum number of transfers allowed at the same time has been reached. |
| Consequence | The transfer is not executed. |
| Action | Wait for a decrease in the number of transfers or increase the maximum number of authorized transfers (this increase can only be made by the technicians responsible for porting and customizing the Transfer CFT product). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT03E"></span>CFTT03E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Max retry Reached<br/> CFTT03E _ Max retry Reached &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The Transfer CFT has performed all retry actions to establish the link with the partner.<br/> • The transfer activation retry counter for this protocol has exceeded the value of the RESTART parameter (CFTPROT command) In this case there have been no other network connection attempts and there are no more protocols in this partner's protocol list In this case the DIAGI code is set to 406<br/> • The physical connection has resulted in an invalid network address. It is the last address for this protocol; there is no backup partner. The internal diagnostic code (DIAGI) is set to 405. When the catalog is displayed, it overrides the value 301 signifying an invalid address<br/> • The physical connection has resulted in a fatal network error. In this case the DIAGI code is set to 303; the DIAGP value defines the source of the error<br/> • The last physical connection attempted according to the values of the RETRYM, RETRYN and RETRYW parameters has failed. It is the last protocol in this partner's protocol list; there is no backup partner<br/> Note: the internal diagnostic code (DIAGI) is set to 405. When the catalog is displayed, it overrides the value 302 signifying a non- fatal network error. |
| Consequence | The transfer is refused. The corresponding catalog entry is set to the KEEP status. |
| Action | Determine the error according to the DIAGI value. Analyze the DIAGP code. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT05E"></span>CFTT05E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Restart Failed<br/> CFTT05E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Restart Failed |
| --- | --- |
| Explanation | A file transfer restart request is not possible (the restart identifier is unknown, for example). |
| Consequence | The transfer is not restarted. |
| Action | Try a new transfer. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT06W"></span>CFTT06W PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Partner switching IPART=&amp;part<br/> CFTT06W PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Partner switching IPART=&amp;part |
| --- | --- |
| Explanation | The partner (&amp;part) cannot be reached within the authorized time slot (OMINTIME, OMAXTIME). |
| Consequence | The transfer will be retried via the intermediate partner IPART. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT07W"></span>CFTT07W Ending Transfer Task &amp;n Failed _ A transfer Running<br/> CFTT07W Ending Transfer Task &amp;n Failed _ A transfer Running |
| --- | --- |
| Explanation | Attempt to delete a transfer task for which not all transfers have been completed. &amp;n designates an internal Transfer CFT task number incremented each time a transfer task is activated. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT08E"></span>CFTT08E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _No prot available<br/> CFTT08E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _No prot available |
| --- | --- |
| Explanation | The maximum number of retries authorized for a transfer using the &amp;prot protocol has been reached (see the Transfer CFT CFTPROT RESTART parameter). |
| Consequence | The entry corresponding to the transfer in the catalog is set to KEEP. |
| Action | Analyze the causes of the broken communication link. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT09E"></span>CFTT09W PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROT=&amp;prot _ Maximum cv affected<br/> CFTT09W PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROT=&amp;prot _ Maximum cv affected |
| --- | --- |
| Explanation | All virtual circuits associated with a partner in server mode have already been allocated |
| Consequence | The transfer is refused locally. |
| Action | Wait for virtual circuits to be freed or increase the maximum number of virtual circuits (see the Transfer CFT Online documentation CNXIN and CNXINOUT parameters). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT10E"></span>CFTT10E PART=&amp;part PROT=&amp;prot _ Protocol not authorized<br/> CFTT10E PART=&amp;part PROT=&amp;prot _ Protocol not authorized |
| --- | --- |
| Explanation | The &amp;prot protocol is not authorized for this partner. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check Transfer CFT parameter settings. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT11E"></span>CFTT11EPART=&amp;part PROT=&amp;prot CLASS=&amp;n _ &amp;net not found<br/> CFTT11EPART=&amp;part PROT=&amp;prot CLASS=&amp;n _ &amp;net not found |
| --- | --- |
| Explanation | The network characteristics associated with the partner and for the &amp;n class of resources have not been found in the Transfer CFT partner file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check the Transfer CFT parameter settings. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT12W"></span>CFTT12WPART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Out of time to call<br/> CFTT12WPART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Out of time to call |
| --- | --- |
| Explanation | A transfer request was made outside the time slot authorized for this partner. |
| Consequence | The transfer is not executed (remains in the D state). |
| Action | Execute a new transfer within the time slot authorized for this partner. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT13I"></span>CFTT13IPART=&amp;part (IDF=&amp;idf IDM=&amp;idm) IDT=&amp;idt _ Session parameters PROT=&amp;prot SAP=&amp;sap DIALNUM(or HOST)= &amp;dialnum (or &amp;host)<br/> CFTT13IPART=&amp;part (IDF=&amp;idf IDM=&amp;idm) IDT=&amp;idt _ Session parameters PROT=&amp;prot SAP=&amp;sap DIALNUM(or HOST)= &amp;dialnum (or &amp;host) |
| --- | --- |
| Explanation | Information message for each dialno (or host) switch and protocol. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT14E"></span>CFTT14E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Not found<br/> CFTT14E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Not found |
| --- | --- |
| Explanation | The &amp;part partner was not found in the Transfer CFT partner file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check the Transfer [CFT parameter settings](../../../c_intro_userinterfaces/command_summary/parameter_intro).<br/> Check the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter settings. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT15E"></span>CFTT15E NPART=&amp;part _ Not found<br/> CFTT15E NPART Not found &lt;NPART=%s&gt; |
| --- | --- |
| Explanation  | The network identifier of the &amp;part partner was not found in the Transfer CFT partner file.  |
| Consequence  | The transfer is not executed. The corresponding catalog entry is set to KEEP.  |
| Action  | Check the Transfer CFT parameter settings.  |
| V23 format<br/> V24 format<br/> Error | CFTT15E NPART=&amp;part _ Not found, possibly truncated to 24 characters<br/> CFTT15E NPART=&amp;part _ Not found, possibly truncated to 24 characters |
| Explanation | The network identifier of the &amp;part partner was not found in the Transfer CFT partner file. A protocol limitation may have truncated the partner to 24 characters. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check the Transfer CFT parameter settings and limit the partner ID to 24 characters.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT16E"></span>CFTT16E PART=&amp;part IDF=&amp;idf _ No implicit send<br/> CFTT16E PART=&amp;part IDF=&amp;idf _ No implicit send |
| --- | --- |
| Explanation | The partner has made a selection request and no file is ready to be sent (SEND on HOLD or implicit SEND). |
| Consequence | The transfer is not executed (no catalog record is created). |
| Action | Prepare a transfer (SEND state=hold) or declare an implicit send in the Transfer [CFT parameter settings](../../../c_intro_userinterfaces/command_summary/parameter_intro).<br/> Prepare a transfer (SEND state=hold) or declare an implicit send in the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter settings. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT17I"></span>CFTT17I PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ STATE=HOLD<br/> CFTT17I PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ STATE=HOLD |
| --- | --- |
| Explanation | A transfer request was voluntarily put on Hold (see Transfer CFT Concepts, Send on Hold). |
| Consequence | The transfer is not executed and is on hold for a possible reception request. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT18E"></span>CFTT18E PART=&amp;part IDF=&amp;idf CFTAUTH id=&amp;id _ Not found<br/> CFTT18E PART=&amp;part IDF=&amp;idf CFTAUTH id=&amp;id _ Not found |
| --- | --- |
| Explanation | The identifier of the list of files authorized for a partner (see [CFTPART](../../../c_intro_userinterfaces/command_summary) ) was not found in the Transfer CFT parameter file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check the [Transfer CFT parameter](../../../c_intro_userinterfaces/command_summary/parameter_intro) settings.<br/> Check the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter setting. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT19E"></span>CFTT19E PART=&amp;part _ Invalid remote password &amp;str *<br/> CFTT19E PART=&amp;part _ Invalid remote password &amp;str * |
| --- | --- |
| Explanation | Transfer request was made and the partner sent an invalid password.<br/> *where &amp;str = supplied password is incorrect |
| Consequence | The connection is refused. |
| Action | Execute a new transfer with a valid password. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT20E"></span>CFTT20E PART=&amp;part _ PVC not allowed<br/> CFTT20E PART=&amp;part _ PVC not allowed |
| --- | --- |
| Explanation | The call collect connection request received is not authorized for this partner. |
| Consequence | The connection is refused.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT21E"></span>CFTT21E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Catalog access failed &amp;scs ,&amp;cr<br/> CFTT21E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Catalog access failed &amp;scs ,&amp;cr |
| --- | --- |
| Explanation | During a transfer (or a transfer request) an undetermined Transfer CFT catalog access error was detected (input/output error for example). |
| Consequence | The transfer is interrupted (or not executed). |
| Action | Analyze the &amp;scs code and inform Product Support if necessary. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT22E"></span>CFTT22E &amp;str PART=&amp;part IDF=&amp;idf IDT=&amp;idt_ &amp;str<br/> CFTT22E _&amp;str &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; &amp;str |
| --- | --- |
| Explanation | Connection refused due to user authentication failure (rpasswd, spasswd error).<br/> The &amp;str provides additional information.<br/> For more information, see [Password management](../../../transport_security_start_here/password_management). |
| Consequence | The transfer is not executed. |
| Action | Check the &amp;str, correct, and retry. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT23E"></span>CFTT23E PART=&amp;part Shutdown in progress _ &amp;str<br/> CFTT23E PART=&amp;part Shutdown in progress _ &amp;str |
| --- | --- |
| Information | A Transfer CFT shutdown is in progress, the request sent is not processed (the &amp;str message specifies the type of request).<br/> Connect in being refused: connection request from one of its partners. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT24E"></span>CFTT24E PART=&amp;part PROT=&amp;prot _ Invalid call number &amp;n<br/> CFTT24E PART=&amp;part PROT=&amp;prot _ Invalid call number &amp;n |
| --- | --- |
| Explanation | The called number received (server end) is not in the list of numbers defined (and authorized) for this partner (&amp;n represents the unauthorized number). |
| Consequence | The connection is refused. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT25E"></span>CFTT25E PART=&amp;part IDF=&amp;idf _ IDF not authorized<br/> CFTT25E PART=&amp;part IDF=&amp;idf _ IDF not authorized |
| --- | --- |
| Explanation | During a transfer request the identifier of the file to be transferred is not authorized for this partner (server or requester end). |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Request that another file be sent or authorize this identifier. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT26E"></span>CFTT26E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Max transfer tasks<br/> CFTT26E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Max transfer tasks |
| --- | --- |
| Explanation | A new transfer request was made but the maximum number of transfer processes has been reached (as has the maximum number of transfers per process). |
| Consequence | The transfer is not executed and remains in the D state. |
| Action | Wait for a decrease in the number of transfers or increase the maximum number of authorized processes or the maximum number of transfers per process. This increase can only be made by the technicians responsible for porting and customizing the Transfer CFT product. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT27E"></span>CFTT27E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Error &amp;scs writing starts<br/> CFTT27E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Error &amp;scs writing starts |
| --- | --- |
| Explanation | The statistics relating to the designated transfer could not be written in the accounting file. |
| Consequence | The accounting file is incomplete. |
| Action | Analyze the file access system code (&amp;scs) to determine the source of the error. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT28E"></span>CFTT28E No outgoing CV configured on Network<br/> CFTT28E No outgoing CV configured on Network |
| --- | --- |
| Explanation | An outgoing call attempt was made on a network resource configured with the CALL = IN parameter (CFTNET command). |
| Consequence | The requester transfer cannot be executed.<br/> The catalog entry is set to the K state with a protocol diagnostic code (DIAGP): "L 0B 22" - 0B meaning that network access is forbidden.<br/> If another protocol (CFTPROT) using another network resource (CFTNET) is declared for this partner (PROT parameter of the CFTPART command), Transfer CFT will make another attempt on this resource. |
| Action | Change the parameter settings so that the protocol designated for the partner points to a network resource available for outgoing calls. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT29E"></span>CFTT29E DEST= &amp;dest - Invalid use _Define for [BOTH/LOCAL/COMMUT] use only<br/> CFTT29E DEST= &amp;dest - Invalid use _Define for [BOTH/LOCAL/COMMUT] use only |
| --- | --- |
| Explanation | A broadcast list can be used (FOR = parameter) with the value:<br/> • LOCAL meaning that the partner list can only be used for a direct transfer.<br/> • COMMUT meaning that the partner list can only be used for store and forward operations.<br/> • BOTH meaning that the partner list is used locally and for store and forward operations. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT30E"></span>CFTT30E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Max Exit tasks<br/> CFTT30E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Max Exit tasks |
| --- | --- |
| Explanation | A new transfer request with an associated EXIT was requested but the maximum number of EXIT processes has been reached. |
| Consequence | The transfer is not executed (remains set to the D state). |
| Action | Wait for a decrease in the number of transfers or increase the maximum number of processes allowed. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT31W"></span>CFTT31W Ending Exit Task &amp;n Failed _ A transfer Running<br/> CFTT31W Ending Exit Task &amp;n Failed _ A transfer Running |
| --- | --- |
| Explanation | Attempt to delete an EXIT task in which not all transfers have been completed. &amp;n designates an internal Transfer CFT task number incremented each time a transfer task is activated. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT32E"></span>CFTT32E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Partner not found<br/> CFTT32E _ Partner not found &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The identifier of the &amp;part partner was not found in the Transfer CFT partner file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is set to KEEP. |
| Action | Check the Transfer CFT parameter settings. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT33E"></span>CFTT33E PART = &amp;dest IDF = &amp;idf IDT = &amp;idt _ Illegal use of CFTDEST<br/> CFTT33E _ Illegal use of CFTDEST &lt;IDTU=&amp;idtu PART=&amp;part &amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The identifier of a partner mentioned in a list (CFTDEST command) is itself a list identifier. As list embedding is not allowed, the transfer with this partner is interrupted.<br/> Transfers with the previous partners in the list are nevertheless activated, but only for an explicit list (FNAME parameter of the CFTDEST command). |
| Consequence | The transfer is interrupted with a DIAGI 401. |
| Action | Change the partners list to show all the partners on one level. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT34E"></span>CFTT34E PART = &amp;part IDF = &amp;idf _ &amp;cause<br/> CFTT34E _ &amp;cause&lt;IDTU=&amp;idtu PART = &amp;part IDF=&amp;idf&gt; |
| --- | --- |
| Explanation | Access error on an external file describing a list of items:<br/> • Partners list: CFTDEST FNAME=#filename,<br/> • File group: SEND FNAME=#filename,<br/> Possible values of &amp;cause are:<br/> • Allocating external file &amp;file<br/> • Opening external file &amp;file<br/> • Reading external file &amp;file<br/> The file processing phase (allocation, opening or reading) is specified in the message. |
| Consequence | The external file cannot be read - the corresponding transfers are not activated. |
| Action | Correct the file access problem. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT35W"></span>CFTT35W PART=&amp;part IDF=&amp;idf IDT=&amp;idt DELETE file &amp;fname Failed _&amp;str<br/> CFTT35W DELETE file &amp;fname Failed _&amp;str &lt;IDTU=&amp;idtu PART = &amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | A DELETE command is executed on a catalog request (in receive mode and in a non- terminated H or K state).<br/> The receive file corresponding to this request could not be deleted.<br/> “ ” (no label): The file cannot be deleted; inform Product Support<br/> • Allocate file error: File allocation error<br/> • Open file error File open error<br/> • Close file error: File closing error<br/> • Free file error: File release error<br/> • Allocate memory error: Memory allocation error |
| Consequence | The request is deleted from the catalog but the user is notified that the &amp;wfname file was not deleted. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT36W"></span>CFTT36W PART=&amp;part IDF=&amp;idf IDT=&amp;idt ERASE file &amp;fname Failed &amp;str<br/> CFTT36W ERASE file &amp;fname Failed &amp;str &lt;IDTU=&amp;idtu PART = &amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | A DELETE command is executed on a catalog request (in receive mode and in a non- terminated state).K or H<br/> The purge of the received file that corresponds with this request could not be carried out:<br/> • Allocate file error: File allocation error<br/> • Open file error : File open error<br/> • Close file error: File clofile close error<br/> • Free file error: File release error<br/> • Allocate memory error: Memory allocation error |
| Consequence | The request is deleted from the catalog but the user is notified that the &amp;fname file has not been purged. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT37I"></span>CFTT37I PART=&amp;part _ Not found and ignored for CFTDEST &amp;id<br/> CFTT37I _ Not found and ignored for CFTDEST &amp;id &lt;IDTU=&amp;idtu PART=&amp;part&gt; |
| --- | --- |
| Explanation | The NOPART parameter for the CFTDEST command can have on of the following values: ABORT (default), CONTINUE, or IGNORE.<br/> • ABORT: Transfer CFT continues functioning as it was before the request for change. No transfer is generated if a partner does not exist in the list of partners defined in the PART parameter. If the list of partners is defined in the PART parameter. If the list of partners is defined in a file (FNAME parameter) the transfers carried out for the only for existing partners and the treatment is identical to that for the NOPART=CONTINUE option.<br/> • CONTINUE: If the partner does not exist, Transfer CFT indicates this in a message in the LOG. CFTT32E PART=idpart Not found. Pass the transfer in SFK diagi 408 and continue the transfers for the other partners. The generic post remains in the K state and the end of transfer procedure is not executed.<br/> • IGNORE: • If a partner does not exist in the list, Transfer CFT ignores the partner (there is no transfer) and moves on to the next partner.<br/> • A message is displayed in the LOG CFTT37I PART=idpart _ Not found and ignored for CFTDEST iddest<br/> • The generic job passes to the T state and is under the end of transfer procedure.<br/>  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT38I"></span>CFTT38I PART=&amp;part _ Dynamic partner: &amp;npart<br/> CFTT38I _Dynamic partner: &amp;npart &lt; PART=&amp;part DIAG=&amp;diag&gt; |
| --- | --- |
| Information | When receiving an incoming call from an unknown source (no local partner description corresponding to the &amp;part network name received), the &amp;npart dynamic partner creation mechanism is triggered. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT39E"></span>CFTT39E PART=&amp;part DIAG=&amp;diag _ Access Exit Connect Reject<br/> CFTT39E _ Access Exit Connect Reject &lt;PART=&amp;part DIAG=&amp;diag&gt; |
| --- | --- |
| Explanation | The connection is refused by the directory EXIT task; &amp;diag contains the field in the communication structure updated by the EXIT. |
| Consequence | The transfer is aborted with the following diagnostics codes: 403, 409, 410, 411, 414, 415, 416, 418, 425, 426. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT40E"></span>CFTT40E PART=&amp;part DIAG=&amp;diag _ Access Exit Error<br/> CFTT40E _ Access Exit Error &lt;PART=&amp;part DIAG=&amp;diag&gt; |
| --- | --- |
| Explanation | An error has been detected in the directory EXIT task. |
| Consequence | The transfer is aborted with the following possible diagnostics codes: 134, 423. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT42E"></span>CFTT42E part&amp;PART=&lt;Partner switching IPART=PART not available<br/> CFTT42E _ Partner switching IPART=PART not available &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | The IPART value must be different than the PART value. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT44E"></span>CFTT44E PART=&amp;part IDF=&amp;idf _ &amp;str directory &amp;file<br/> CFTT44E _ &amp;str directory &amp;file &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf&gt; |
| --- | --- |
| Explanation | The file selection phase is indicated in the message, where &amp;str can be allocating, opening, empty, or reading. A directory access error has been detected when sending a list of file names or a group of files based on a selection.<br/> SEND FNAME = # FIL* or SEND FNAME = # DIR*. |
| Consequence | If the directory could not be accessed (allocating, opening or empty), no transfers are triggered.<br/> If the directory could not be read when selecting the files in the directory, all transfers preceding the error are triggered. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT44W"></span>CFTT44W PART=&amp;part IDF=&amp;idf _ &amp;str directory &amp;file (file not found ignored)<br/> CFTT44W _ &amp;str directory &amp;file &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf&gt; (file not found ignored) |
| --- | --- |
| Explanation | The file selection phase is indicated in the message, where &amp;str can be allocating, opening, empty, or reading.<br/> A directory access error has been detected when sending a list of file names or a group of files based on a selection.<br/> SEND FNAME = # FIL* or SEND FNAME = # DIR*. |
| Consequence | If the directory is empty, Transfer CFT ignores the fact that the file is not found, and passes the transfer to the X phase. |
| Action  | Ignore this error.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT45E"></span><br/> CFTT45E PART=&amp;part _ Partner switching IPART=&amp;ipart not found<br/> CFTT45E _ Partner switching not found &lt;PART=&amp;part IPART=&amp;ipart&gt; |
| --- | --- |
| Explanation | The intermediate partner (IPART) was not found. |
| Consequence | The transfer cannot be performed. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT46W"></span>CFTT46W PART=&amp;part ,IDF=&amp;idf ,IDT=&amp;idt _ Part inactive: mode &amp;str<br/> CFTT46W _ Part inactive: mode &amp;str &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The transfer attempt for partner &amp;part cannot succeed as the partner is inactive in &amp;str mode:<br/> • &amp;str= requester or<br/> • &amp;str = server |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT47E"></span>CFTT47E PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROTOCOL=&amp;id _ Cannot find SSL security profil<br/> CFTT47E _ Cannot find SSL security profil &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROTOCOL=&amp;prot&gt; |
| --- | --- |
| Explanation | The attempted transfer the &amp;part partner cannot be performed because the security profile was not found. For more information on SSL definition for CFTPART, see the [SSL](../../../c_intro_userinterfaces/command_summary/parameter_intro/ssl) parameter description. |
| Consequence | The transfer can not be carried out. |

| V23 format<br /> V24 format<br /> Warning | <span id="CFTT47W"></span>CFTT47W PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROTOCOL=&amp;prot SSLid=&amp;id DIRECT=CLIENT _ Cannot find SSL security profil<br/> CFTT47W _ Cannot find SSL security profil &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROTOCOL=&amp;prot SSLID=&amp;id DIRECT=CLIENT&gt;<br/> CFTT47W PART=&amp;part SSLid=&amp;id DIRECT=SERVER _ No SSL security profile for additional checks<br/> CFTT47W _ No SSL security profile for additional checks &lt;PART=&amp;part SSLID=&amp;id DIRECT=SERVER&gt; |
| --- | --- |
| Explanation | The transfer attempt for partner &amp;part uses a non- existent CFTSSL (&amp;sslid). |
| Action | Check the configuration and add the missing profile. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT48E"></span>CFTT48E PART=&amp;part SSL=&amp;id _ Server Session rejected reason=&amp;reason<br/> CFTT48E _ Server Session rejected reason=&amp;reason &lt;PART=&amp;part SSL=&amp;ssl&gt; |
| --- | --- |
| Explanation | The attempted transfer the &amp;part partner cannot be performed because the security profile is not valid, with as an internal reason (&amp;reason). |
| Consequence | The transfer can not be carried out. |
| Action | Note the REASON (&amp;reason) value and contact the product support team if necessary. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT49W"></span>CFTT49W Unable to send data to Synchronous task<br/> CFTT49W Unable to send data to Synchronous task |
| --- | --- |
| Explanation | Synchronous communication task is inaccessible. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT50E"></span>CFTT50E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Duplicate transfer with IDTU=<br/> CFTT50E _ Duplicate transfer with IDTU=&amp;idtu &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | A duplicate transfer occurred. IDTU=&amp;idtu is the previously performed transfer.<br/> For more information, see the [DUPLICAT](../../../c_intro_userinterfaces/command_summary/parameter_intro/duplicat) field details. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT51I"></span>CFTT51I PART=&amp;part ,&amp;str session opened<br/> CFTT51I PART=&amp;part ,&amp;str session opened |
| --- | --- |
| Explanation | The session is open, the connection request has been accepted (requester side (&amp;str= requester if not secured, &amp;str=SSL requester) or server (&amp;str = server if not secured , &amp;str=SSL server if secured)). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT52I"></span>CFTT52I PART=&amp;part ,&amp;str session closed<br/> CFTT52I PART=&amp;part ,&amp;str session closed |
| --- | --- |
| Explanation | The session is closed: requester side (&amp;str = requester if not secured, &amp;str = SSL requester if secured) or server (&amp;str = server if not secured , &amp;str = SSL server if secured). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT53I"></span>CFTT53I PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;str file &amp;str1<br/> CFTT53I &amp;str file &amp;str1 &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The file is selected (&amp;str1 = selected) or created (&amp;str1 = created) either by the requester (&amp;str = requester) or by the server (&amp;str = server). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT54I"></span>CFTT54I PART=&amp;part IDF=&amp;idf IDT=&amp;idt ,&amp;str file deselected<br/> CFTT54I &amp;fname file deselected &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The file is deselected either by the requester (&amp;str = requester) or by the server (&amp;str = server). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT55I"></span>CFTT55I PART=&amp;part IDF=&amp;idf IDT=&amp;idt ,&amp;str file opened<br/> CFTT55I &amp;fname file opened &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The file is opened either by the requester (&amp;str = requester) or by the server (&amp;str = server). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT56I"></span>CFTT56I PART=&amp;part IDF=&amp;idf IDT=&amp;idt ,&amp;str file closed<br/> CFTT56I &amp;fname file closed &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The file is closed either by the requester (&amp;str = requester) or by the server (&amp;str = server. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT57I"></span>CFTT57I PART=&amp;part IDF=&amp;idf IDT=&amp;idt IDS=&amp;ids,&amp;str transfer started<br/> CFTT57I &amp;str transfer started &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | The transfer has been started either by the requester (&amp;str = requester) or by the server (&amp;str = server).<br/> <blockquote> **Note**<br/> IDS refers to the session identifier.<br/> </blockquote>  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT58I"></span>CFTT58I PART=&amp;part IDF=&amp;idf IDT=&amp;idt IDS=&amp;ids,&amp;str transfer ended<br/> CFTT58I &amp;str transfer ended &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | The transfer has been completed either by the requester (&amp;str = requester) or by the server (&amp;str = server).<br/> <blockquote> **Note**<br/> IDS refers to the session identifier.<br/> </blockquote>  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT59I"></span>CFTT59I PART=&amp;part IDM=&amp;idf IDT=&amp;idt ,&amp;str &lt;message&#124;reply&gt; transferred<br/> CFTT59I &amp;str &lt;message&#124;reply&gt; transferred &lt;IDTU=&amp;idtu PART=&amp;part IDM=&amp;idm IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The message or the reply has been sent either by the requester (&amp;str = requester) or by the server (&amp;str = server). |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT60I"></span>CFTT60I &amp;str<br/> CFTT60I IDTU=&amp;idtu &amp;str |
| --- | --- |
| Explanation | The &amp;str message is an information message, sent by the file EXIT associated with the transfer. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT61E"></span>CFTT61E PART=&amp;part IDM=&amp;idf IDT=&amp;idt local message reject &amp;diagi ,&amp;diagp<br/> CFTT61E local message transfer reject &lt;IDTU=&amp;idtu PART=&amp;part IDM=&amp;idm IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The local partner rejects the message transfer. |
| Consequence | The message transfer is not executed. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT62E"></span>CFTT62E PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT62E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The transfer was interrupted by the operator (&amp;diagp = "OPER") or by the file EXIT (&amp;diagp represents the phase of the EXIT which prompted the interruption = "ALLOC", "OPEN", TRANS or CLOSE). |
| Consequence | The transfer is aborted. The corresponding catalog entry is set to HOLD (after a HALT command from the operator or an interrupt by the EXIT), KEEP (after a KEEP command from the operator). |
| Action | After an interruption by the operator, the transfer can be restarted manually (START command).<br/> After interruption by a file EXIT, the action is to be defined by the engineers responsible for this file EXIT. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT62E"></span><span id="CFTT63E"></span>CFTT63E IDTU=&amp;idtu MODE=&amp;str Transfer refused due to product key limitation until &amp;date<br/> CFTT63E _ Transfer refused due to product key limitation until &amp;date &lt;IDTU=&amp;idtu MODE=&amp;mode&gt; |
| --- | --- |
| Explanation | The license key is limiting the number of transfers in this time frame, where:<br/> • &amp;str : REQUESTER or SERVER |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT64E"></span>CFTT64E PART=&amp;part IDF=&amp;idf _ Flow does not exist and cft.default_idf.enable is set to 'no'<br/> CFTT64E _ Flow does not exist and cft.default_idf.enable is set to 'no' &lt;PART=&amp;part IDF=&amp;idf&gt; |
| --- | --- |
| Explanation | The default IDF functionality is disabled for the command. |
| Consequence | The transfer is not executed and has the status K. The DIAGP also reflects this status. |
| Action | For parameter details, see [UCONF: General unified configuration parameters](../../../admin_intro/uconf/uconf_parameters). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT65E"></span>CFTT65E PART=&amp;part IDF=&amp;idf IDT=&amp;idt PROT=&amp;prot _ Protocol not available<br/> CFTT65E _ PROT=&amp;prot not available &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The &amp;prot protocol is not authorized for this partner.  |
| Consequence | The transfer is not executed, and the corresponding catalog entry is set to KEEP. DIAGI=410 ,DIAGP = NO PROT  |
| Action | Check the PROT value in the SEND or RECV transfer.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT66E"></span>CFTT66E Maximum number of partners authorized by license key reached (using PART=&amp;part)<br/> CFTT66E Maximum number of partners authorized by license key reached (using PART=&amp;part) |
| --- | --- |
| Explanation | {{< TransferCFT/axwayvariablesComponentShortName  >}} license keys support either a limited or unlimited number of partners. The transfer is treated as if the partner does not exist.  |
| Consequence | An error occurred because you have reached the maximum number of partners allowed by your license key.  |
| Action | In a command line window, you can enter the command CFTUTIL ABOUT to check the number of partners that your license key authorizes. For additional information on license keys, contact an {{< TransferCFT/axwayvariablesCompanyName  >}} sales representative.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT66E"></span>CFTT68E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Unexpected AT phase/phasestep at restart, converting T to K<br/> CFTT68E _ Unexpected AT phase/phasestep at restart, converting T to K &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | If Transfer CFT was stopped abruptly when creating one or more child transfers for the current generic transfer, when you restart Transfer CFT you cannot manage these transfers (AT phase/phasestep).  |
| Consequence | The corresponding transfer entry in the catalog now switches to the K phasestep instead of the previous T phasestep.  |
| Action | You can continue to manage the affected transfer (those in K phasestep).  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT69E"></span>CFTT69E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Unexpected YE phase/phasestep at restart, converting E to H<br/> CFTT69E _ Unexpected YE phase/phasestep at restart, converting E to H IDTU=&amp;idtu, PART=&amp;part, IDF=&amp;idf, IDT=&amp;idt |
| --- | --- |
| Explanation | If Transfer CFT was stopped abruptly during the post- processing phase, when you restart Transfer CFT you cannot manage these transfers (YE phase/phasestep).  |
| Consequence | The corresponding transfer entry in the catalog now switches to the H phasestep instead of the previous E phasestep.  |
| Action | You can continue to manage the affected transfer (those in H phasestep).  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTT70I"></span>CFTT70I The user &amp;user is connecting with method &amp;method<br/> CFTT70I The user &amp;user is connecting with method &amp;method |
| --- | --- |
| Explanation  | A client is trying to connect to the server.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTT70E"></span>CFTT70E The user &amp;user is not allowed to connect to the server<br/> CFTT70E The user &amp;user is not allowed to connect to the server |
| --- | --- |
| Explanation  | The user is not allowed to connect to the server, or the password or the authentication method is incorrect.  |
| Action  | Correct the user name or password, or add a public key.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT71E"></span>CFTT71E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote creation reject &amp;diagi ,&amp;diagp<br/> CFTT71E remote creation reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The file was not created, the internal &amp;diagi code explains the reason for the rejection (see the chapter on internal Transfer CFT codes). |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT72E"></span>CFTT72E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote selection reject &amp;diagi ,&amp;diagp<br/> CFTT72E remote selection reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The file was not selected; the internal &amp;diagi code explains the reason for the rejection (see the topic on internal Transfer CFT codes). |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT72W"></span>CFTT72W PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote selection reject (file not found ignored) &amp;diagi ,&amp;diagp,<br/> CFTT72W remote selection reject (file not found ignored) &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp |
| --- | --- |
| Explanation | FILENOTFOUND=IGNORE The file was not selected because the file is not found. |
| Consequence | The transfer is aborted. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT73E"></span><br/> CFTT72E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote selection reject &amp;diagi ,&amp;diagp<br/> CFTT72E remote selection reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp |
| --- | --- |
| Explanation | FILENOTFOUND=ABORT The message was not sent. |
| Consequence | The message transfer is aborted. The corresponding catalog entry is put on HOLD. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT73E"></span>CFTT73E PART=&amp;part IDM=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT73E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The message was not sent. |
| Consequence | The message transfer is aborted. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT74E"></span>CFTT74E PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT74E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The transfer was interrupted by the remote partner. |
| Consequence | The transfer is aborted, the corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT75E"></span>CFTT75E PART=&amp;part IDF=&amp;idf IDT=&amp;idt connect reject &amp;diagi ,&amp;diagp<br/> CFTT75E connect reject &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The connection request was rejected by the partner. |
| Consequence | The transfer failed. If the called number was engaged or a network incident occurred (for example), the transfer will be retried several times (see the RETRYM, RETRYN and RETRYW parameters).<br/> If all retries fail, the transfer is not executed and the corresponding catalog entry is set to KEEP. |
| Action | Analyze the internal &amp;diagi error code for the transfer and try to correct it. |

| V23 format<br/> V24 format<br/> Error | CFTT75E Incorrect user or password &lt;IDTU=&amp;idtuPART=&amp;part IDF=&amp;idf IDT=&amp;idf DIAGI=&amp;diagi&gt;<br/> CFTT75E Incorrect user or password &lt;IDTU=&amp;idtuPART=&amp;part IDF=&amp;idf IDT=&amp;idf DIAGI=&amp;diagi&gt; &lt;/p&gt; |
| --- | --- |
| Explanation | Cannot connect to the SFTP server because the user name or password is incorrect. |
| Action | Correct the user name or password. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT76E"></span>CFTT76E PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT76E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The write transfer request is refused. |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT77E"></span>CFTT77E PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT77E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The read transfer request is refused. |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT78E"></span>CFTT78E PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp<br/> CFTT78E &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | A problem was detected at the end of the transfer. |
| Consequence | The transfer has terminated but is not considered to be valid, the corresponding catalog entry is set to DISP (transfer to be restarted), on HOLD (the transfer may be restarted) or to KEEP. |
| Action | Automatically by Transfer CFT or manually by the user (correct the error, restart or try a new transfer). |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT79E"></span>CFTT79E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote deselect reject &amp;diagi ,&amp;diagp<br/> CFTT79E remote deselect reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The file could not be deselected. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT80E"></span>CFTT80E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote open reject &amp;diagi ,&amp;diagp<br/> CFTT80E remote open reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The file could not be opened. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT81E"></span>CFTT81E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote close reject &amp;diagi ,&amp;diagp<br/> CFTT81E remote close reject &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | The file could not be closed. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT82E"></span>CFTT82E PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt transfer aborted &amp;diagi ,&amp;diagp<br/> CFTT82E transfer aborted &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | A serious error was detected.<br/> ****Example****<br/> 19/02/19 16:01:37 CFTT82E Transfer aborted &lt;IDTU=A0000008 PART=PARIS_KEY IDF=AS3SR IDT=B1916013 DIAGI=110<br/> 19/02/19 16:01:37 CFTT82E+ DIAGP=00000002 DIAGC=File not found: zohs.bat&gt; |
| Consequence | The transfer is interrupted and the corresponding catalog entry is set to KEEP. |
| Action | Correct the error and try again. |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT82W"></span>CFTT82W PART=&amp;part IDF=&amp;idf IDT=&amp;idt transfer aborted (file not found ignored) &amp;diagi ,&amp;diagp<br/> CFTT82W transfer aborted (file not found ignored) &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt &amp;diagi ,&amp;diagp&gt; |
| --- | --- |
| Explanation | File not found error was detected. |
| Consequence | The transfer ignore the file not found problem and pass to X phase. |
| Action | Ignore the error. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT83I"></span>CFTT83I PART=&amp;part IDF=&amp;idf IDT=&amp;idt change direction(CD) for request<br/> CFTT83I change direction(CD) for request &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | This message is only displayed for the ODETTE protocol and a RECV command. It indicates that the remote partner has accepted its turn to transmit. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT86I"></span>CFTT86I PART=&amp;part IDS=&amp;ids Change direction(TURN) sent<br/> CFTT86I Change direction(TURN) sent &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | For a PeSIT protocol in a DMZ profile, the token (TURN) has been sent to the partner &amp;part. |

| V23 format<br/> V24 format<br/> Information | CFTT86I FNAME=&amp;fname S=ByteCount<br/> CFTT86I FNAME=&amp;fname S=ByteCount |
| --- | --- |
| Explanation | Name of the file sent or received and the number of bytes in the file. This message completes the CFTT54I message. Name of the file sent or received and the number of bytes in the file. This message completes the CFTT54I message. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT87I"></span>CFTT87I PART=&amp;part IDS=&amp;ids Change direction(TURN) received<br/> CFTT87I Change direction(TURN) received&lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | For the PeSIT protocol in a DMZ profile, the token (TURN) has been received by the partner &amp;part, where IDS is the reference for the session context. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT88I"></span>CFTT88I+IDT=&amp;idt WORKINGDIR=&amp;workingdir FNAME=&amp;fname NBC=&amp;n DURATION=&amp;time<br/> CFTT88I+&lt;IDTU=&amp;idtu WORKINGDIR=&amp;workingdir FNAME=&amp;fname NBC=&amp;n DURATION==&amp;time&gt; |
| --- | --- |
| Explanation | This message completes the message CFTT54I.<br/> The following fields indicated:<br/> • WORKINGDIR: if the WORKINGDIR is not defined, this field is empty<br/> • fname: name of the file sent<br/> • n: number of bytes in the file<br/> • time: transfer duration in seconds |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT89I"></span>CCFTT89I PART=&amp;part IDF=&amp;idf IDT=&amp;idt [FACTION on FNAME ] or [NACTION on NFNAME]=&amp;fname : &amp;str+"deleted" or "erased"<br/> CFTT89I [FACTION on FNAME ] or [NACTION on NFNAME] =&amp;fname : &amp;str+"deleted" or "erased" &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation |  • [NACTION](../../../c_intro_userinterfaces/command_summary/parameter_intro/faction)=DELETE was used in a RECV command, which deleted the file (NFNAME) at the end of the transfer. This option is only available when using SFTP. |
| Information  | CFTT89I [FACTION on FNAME ] or [NACTION on NFNAME]=&amp;srcfile archived as &amp;archivefile &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;  |
| Explanation  |  • [NACTION](../../../c_intro_userinterfaces/command_summary/parameter_intro/faction)=ARCHIVE was used in a RECV command, which archived the file (NFNAME) at the end of the RECV transfer. This option is only available when using SFTP. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT90W"></span>CFTT90W IDF=&amp;idf IDT=&amp;idt [FACTION on FNAME ] or [NACTION on NFNAME]=&amp;fname : [erase or delete] failed cs<br/> CFTT90W IDF=&amp;idf IDT=&amp;idt [FACTION on FNAME ] or [NACTION on NFNAME]=&amp;fname : [erase or delete] failed cs |
| --- | --- |
| Explanation  | Failed to delete or erase the file (FNAME/NFNAME) and the transfer has completed (T state). This failure might occur if the file to be deleted or erased was being used, for example.<br/> When using the following commands:<br/> • SEND [NACTION](../../../c_intro_userinterfaces/command_summary/parameter_intro/faction)=DELETE, the action occurs after the transfer completed. Only available when using SFTP. |
| Action | Check the following:<br/> • [NACTION](../../../c_intro_userinterfaces/command_summary/parameter_intro/faction): Manually delete the remote file (NFNAME). This option is only available when using SFTP. |
| Warning  | CFTT90W IDF=&amp;idf IDT=&amp;idt NARCHIVEFNAME undefined and NACTION=ARCHIVE  |
| Explanation  | If you have set [NACTION](../../../c_intro_userinterfaces/command_summary/parameter_intro/naction)=ARCHIVE, you must define the NARCHIVEFNAME.  |
| Action  | Define the [NARCHIVEFNAME](../../../c_intro_userinterfaces/command_summary/parameter_intro/narchivename) parameter. This parameter is only available when using SFTP.  |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT91W"></span>CFTT91W PART=&amp;part IDS=&amp;ids Change direction(TURN) not supported by server<br/> CFTT91W Change direction (TURN) not supported by server PART=DMZ1 IDS=&amp;ids |
| --- | --- |
| Explanation | When using the DMZ mode, if the server does not accept the TURN, the session is closed by the requester without message. The IDS is the reference for this session context. |
| Action | This message is edited on LOG file. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTT93W"></span> CFTT92I IDTU=&amp;idtu CTX=&amp;ctx IDT=&amp;idt<br/> CFTT92I &lt;IDTU=&amp;idtu CTX=&amp;ctx IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Additional message for message CFTT57I, and is displayed only if ATM traces are activated.<br/> CTX=&amp;ctx provides the protocol context associated with the transfer. |

| V23 format<br/> V24 format<br/> Warning | <span id="CFTT93W"></span>CFTT93W PART=&amp;part IDS=&amp;ids Negative ack not supported<br/> CFTT93W Negative ack not supported PART=&amp;part IDS=&amp;ids |
| --- | --- |
| Explanation | The final partner signals to the initial file sender that application errors were detected. This occurs via a negative acknowledgments sent in a PeSIT Hors SIT message, where IDS is the reference for the session context. |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTT94I"></span>CFTT94I PART=&amp;part IDF=&amp;idf IDT=&amp;idt FCHARSET=&amp;str NCHARSET=&amp;str<br/> CFTT94I &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt FCHARSET=&amp;str NCHARSET=&amp;str&gt; |
| --- | --- |
| Explanation  | This information message relates to the extended transcoding used for this transfer.  |

| V23 format<br/> V24 format<br/> Error  | <span id="CFTT95E"></span>CFTT95E Incorrect user or password &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idtf DIAGI=&amp;diagi&gt;<br/> CFTT95E Incorrect user or password &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt DIAGI=&amp;diagi&gt; |
| --- | --- |
| Explanation  | Cannot connect to the SFTP server because the user name or password is incorrect.  |
| Action  | Correct the user name or password.  |

| V23 format<br/> V24 format<br/> Information  | <span id="CFTT96I"></span> CFTT96I &amp;str transfer restarted &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt POS=&amp;pos IDS=&amp;ids&gt;<br/> CFTT96I &amp;str transfer restarted &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt POS=&amp;pos IDS=&amp;ids&gt; |
| --- | --- |
| Explanation  | The requester (&amp;str = requester) or the server (&amp;str = server) has restarted the transfer. &amp;pos is the file position during restart.  |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT97E"></span>CFTT97E cmd prefix not allowed in procedure execution for SEND and RECV commands",<br/> CFTT97E cmd prefix not allowed in procedure execution for SEND and RECV commands |
| --- | --- |
| Explanation  |   |

| V23 format<br/> V24 format<br/> Error | <span id="CFTT98W"></span>CFTT98W PART=%- 8.8s IDF=%- 8.8s IDT=%.8s Rename ignored because WFNAME equals FNAME,<br/> CFTT98W Rename ignored because WFNAME equals FNAME &lt;IDTU=%.8s PART=%s IDF=%s IDT=%.8s&gt; |
| --- | --- |
| Explanation  | The configuration has the same [WFNAME](../../../c_intro_userinterfaces/command_summary/parameter_intro/wfname) as the RECV FNAME and so the renaming is ignored.  |

