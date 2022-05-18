---
title: "DIAGI - Diagnostic  codes"
linkTitle: "DIAGI - Internal diagnostic codes"
weight: 300
--- <span id="Internal_diagnostic_codes"></span>

## {{< TransferCFT/axwayvariablesComponentShortName  >}} internal diagnostic codes

Diagnostic codes provide general information on the cause of the error and are independent of the operating system and of the network access method
used for the transfer.

## DIAGI diagnostic code values

DIAGI codes with a value between 001 and 499 indicate a local issue; values
between 501 and 999 correspond to a fault reported by the partner.

This means that when you troubleshooting, if the DIAGI code is greater than 500 it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 916, for example, the issue is a remote problem corresponding to DIAG 416 (the maximum number of transfers reached).

<span id="Event_column_in_diagnostic_codes"></span>

## Event column in diagnostic codes

The Event column explains the
possible cause of the transfer error. Brief information on the type of error which caused
the transfer failure:

- SYS: System error
- NET: Error detected
    by the network layers ({{< TransferCFT/axwayvariablesComponentShortName >}} layers, manufacturer or network layers)
- PROT: Fault
    detected by the file transfer protocol
- FILE: Transferred
    file access error returned by the operating system
- DATA: Error
    accessing {{< TransferCFT/axwayvariablesComponentShortName >}} basic data: parameter, partners, catalog, communication,
    log, statistics and secondary indirection files (lists of partners, files,
    and so on)
- PARAM: Transfer
    execution error following a parameter setting error
- AUTH: Transfer
    denied following an authorization check by {{< TransferCFT/axwayvariablesComponentShortName >}}
- OVER: The
    transfer cannot be executed because the monitor's resources are saturated
    or a parameter setting limit has been exceeded
- OUT: The transfer
    request is aborted after the maximum number of retries
- USER: The
    transfer is interrupted following an action by the operator

<!- - - - >

- SSL: Incident
    detected by the secured protocol SSL 

<span id="Behavior_column_in_diagnostic_codes"></span>

## Consequence column in diagnostic codes

The **Consequence** column provides
information on the {{< TransferCFT/axwayvariablesComponentShortName  >}} behavior following a transfer failure.
The resulting status of the transfer D, H or K:

- D: the transfer
    can still be executed using the RESTART, NEXT, RETRY or COMMUT mechanisms
- H or K: the transfer
    is aborted, the error procedure

****Further transfer attempts****

For the D status, the following are possible to execute
the transaction:

- RESTART: the transfer
    has been interrupted. The monitor waits for a period fixed by the WSCAN
    parameter (CFTCAT command) before trying to restart the transfer with
    the same access data. It increments the restart counter for the protocol,
    the counter limit being determined by the RESTART parameter (CFTPROT command).
- NEXT: the transfer
    has been interrupted. The monitor waits for a period fixed by the WSCAN
    parameter (CFTCAT command) before trying to restart the transfer with
    the same access data. It does not increment the restart counter. There
    is therefore no limit to the number of retries following this error.
- RETRY: the transfer
    has been interrupted. Transfer CFT waits for a period fixed by the RETRY\*
    parameters before trying to restart the transfer, without changing the
    partner access data (same protocol, same network address. It increments
    the retry counter specific to the partner access data, the counter limit
    being determined by the RETRYM parameter (CFTnetwork command).
- COMMUT (switching):
    the transfer has been interrupted. The monitor waits for a period fixed
    by the WSCAN parameter (CFTCAT command) before trying to restart the transfer.
    It ignores the transfer access data and tries a "switching"
    path to reach the partner: another IP address, another protocol or a
    backup partner.

> **Note**
>
> Protocol switching also means
> communication system (network) switching. The switching mechanism does
> not provide for use of other network resources (CFTNET commands associated
> via the CLASS) for a given protocol (CFTPROT command). Problems associated
> with network resources are masked by the common network access method
> which manages them.

In the case of an H or K status the transfer is aborted, the error procedure can be executed:

- ABORT: the transfer
    is aborted
- Execution
    of the error procedure if the transfer switches to the K or H status:
    - EXECE: the procedure is executed. This is the procedure
        defined by CFTPARM EXEC\*E parameters. If these parameters are not set,
        the procedure is not executed.

"No CAT" indicates that no
catalog entry was created for the transfer when the error occurred.
The transfer request is rejected.<span id="Internal_diagnostic_codes_table"></span>

DIAGI - Internal diagnostic codes
table

The following table makes references to DIAGP. For details, please see the DIAGP section.](../general_protocol_diagnostics)

QQQ_QQQ Table

| DIAGI Code  | Event  | Consequence  |
| --- | --- | --- |
| 0 | The transfer has terminated correctly | Execution of normal EXECRF or EXECSF end of transfer procedures |
| 001 | SYS: Error creating the message queue or allocating the memory | H status - ABORT, EXECE |
| 002 | Context definition error | H status - ABORT, EXECE |
| 003 | SYS - Context allocation error | H status - ABORT, EXECE |
| 004  | MQCONN  |   |
| 005  | MQOPEN  |   |
| 006  | MQPUT  |   |
| 100 | FILE - File input/output error | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 101 | FILE - Error creating the receive file | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 102 | 1.FILE - Error allocating the transfer file | H status - ABORT, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 102  | 2.FILE - The receive file cannot be allocated (FDISP=OLD case) | H status - ABORT, EXECE&lt;br/&gt; The file is deleted. &lt;/p&gt;<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. &lt;/p&gt; |
| 103 | 1.FILE - The file cannot be deleted before the receive file is created (FDISP = DELETE case) | H status - ABORT, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 103  | 2.FILE - Error deleting the sent file, if a deletion has been requested (FACTION = DELETE) | H status - ABORT, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 104 | 1. FILE - Error opening the transfer file<br/> 2. FILE - The receive file cannot be erased (FDISP = ERASE case): file opening problem<br/> 3. FILE - Prior to reception, the receive file could not be opened to check that it was empty (FDISP = VERIFY case) | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 105 | 1. FILE - Error closing the transfer file<br/> 2. FILE - The receive file cannot be erased (FDISP = ERASE case: file closing problem<br/> 3. FILE - Prior to reception, the receive file could not be closed after checking that it was empty (FDISP = VERIFY case)<br/> 4. FILE - The sent file cannot be deleted following an erase request (FACTION = ERASE case) | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 106 | FILE - Error recording the current position in the transfer file (synchronization point setting) | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 107 | FILE - Error setting the pointer to a re- synchronization point in the file (for a transfer restart) | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 108 | 1. FILE - Send file read error in data transfer phase<br/> 2. FILE - Prior to reception, the receive file could not be read to check that it was empty (FDISP = VERIFY case) | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 109 | FILE - Data write error in the receive file | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 110 | 1. FILE - The send file does not exist<br/> 2. FILE - The receive file to be created does not exist, even though the FDISP parameter requires it (FDISP=OLD). DIAGP is then set to NO OLD | H status - ABORT, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 111 | FILE - Insufficient space to create the file | H status - ABORT in requester mode, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 112  | Nonexistent unit  |   |
| 113 | FILE - The file to be created already exists, even though the FDISP parameter prohibits it (FDISP = NEW). DIAGP is then set to NO NEW | H status - ABORT, EXECE in requester mode<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 114 | FILE - Data write error in the receive file: file space full | H status - ABORT, EXECE<br/> If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information. |
| 115 | 1. FILE - The transfer owner is not authorized to access the file<br/> 2. FILE - The file cannot be deleted before the receive file has been created (FDISP = DELETE case)Protected file | H status - ABORT, EXECE in requester mode<br/> Please see the [Access management sections for more information. |
| - "- | 3. FILE - The sent file cannot be deleted following a deletion request<br/> (FACTION = DELETE case) Protected file | H status - ABORT, EXECE<br/> Please see the Access management sections for more information. |
| 120 | PROT - Counter check error | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 121 | USER - Interruption by the operator | H status - ABORT, EXECE |
| 122 | SYS - Error allocating memory when the transfer is executed | D status - RESTART |
| 123 | FILE - Error setting the pointer to a resynchronization point in the file: the restart point requested by the partner is incorrect.<br/> This can occur when the synchronized points are not flushed from the catalog (if the CFTCAT's <code>updat </code>parameter is greater than 1, the UCONF<code>cft.server.catalog.sync.enable</code> is set to <code>No</code>, or both of these are true) due to an unexpected event such as a process, system, or disk crash.<br/> <br/> You should set the following values to ensure that a restart is possible:<br/> <code>cft.server.catalog.sync.enable = Yes</code><br/> <code>CFTCAT id=xxxx,updat=1</code><br/> <code>CFTPROT id=xxx, schkw=1,rchkw=1</code> | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 124 | PROT - Error: transfer aborted | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 126 | FILE - LRECL error (record length) | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 127 | FILE - The receive file is not empty (FDISP = VERIFY case) | H status - ABORT, EXECE |
| 128 | 1. FILE - Error deselecting the file<br/> 2. FILE - Error deselecting the receive file | H status - ABORT, EXECE<br/> H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| - " - | 3. FILE - Error accessing the send file (allocating or opening), subsequent to a file erase request (FACTION = ERASE) | H status - ABORT, EXECE unless there is an allocation error in sender server mode<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 129 | FILE - Error during file decompression | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 130 | FILE - Error during file compression | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 131 | PROT - IDF different on ODETTE SELECT | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 132 | PARAM - Error accessing the parameter setting indirection file (list of files, list of partners) | K status - ABORT<br/> If the file does not exist, no transfer is executed. Only the generic request remains in the catalog. If an error occurs while reading the indirection file, the transfers generated for the items (files or partners) that have already been read are executed |
| 133 | PARAM - The FOR parameter in the CFTDEST command is invalid<br/> 1. FOR=LOCAL when in the switching mode<br/> 2. FOR=COMMUT when not in the switching mode | No catalog entry. Check the CFTDEST object in the configuration. |
| 134 | FILE - CFTEXIT call error | H status - ABORT, EXECE<br/> Check the information in the cftlog and the cft.out files. |
| 135 | 1. FILE - The send file is locked<br/>  |  • D status - RESTART&lt;br/&gt; Check that the file is not locked by another process. &lt;/p&gt;<br/><br/> • H status - ABORT, EXECE<br /> Check that the file is not locked by another process.<br/> |
| - " - | 2. FILE - The receive file is locked |  • D status - RESTART &lt;br/&gt; Check that the file is not locked by another process. &lt;/p&gt;<br/><br/> • H status - ABORT, EXECE &lt;br/&gt; Check that the file is not locked by another process. &lt;/p&gt;<br/> |
| 136 | FILE - Duplication of the temporary file | H status - ABORT, EXECE<br/> Check the WFNAME parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) command. |
| 137 | FILE - The file exists, the "rename" operation is therefore impossible | H status - ABORT, EXECE<br/> Check the WFNAME parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) command. |
| 138 | 1. FILE - No temporary file has been defined in the send mode and the transfer requires the COPY mechanism. No transfer is triggered and the generic request remains in the catalog | State K - ABORT<br/> Check the WFNAME parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) command.<br/>  |
| - " - | 2. FILE - No temporary file has been defined in the receive mode for a file with versions or for a transfer requiring deconcatenation (COPY) | State K - ABORT, EXECE<br/> Check the WFNAME parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) command.<br/>  |
| 139  | Incorrect attributes file  | Check the cftlog file and the diagp/diagc in the catalog record.<br/> Also, check the FLRECL/FRECFM parameters in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) object. |
| 142 | FILE - The "rename" operation failed | H status - ABORT, EXECE<br/> Check the WFNAME parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command. |
| 144 | MVS transfer busy | State D - Retry<br/> Specific to z/OS environments. |
| 145  |  • The SEND file is outside of the workingdir tree. &lt;/li&gt;<br/> • The temporary SEND file is outside of the workingdir tree. | K status - ABORT<br/> Check the WORKINGDIR parameter in the [CFTSEND](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) object. |
| 148  | 1. The RECV file is outside of the workingdir tree. &lt;/p&gt;<br/> 2. The temporary RECV file is outside of the workingdir tree. | K status - ABORT<br/> Check the WORKINGDIR parameter in the [CFTRECV](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) object. |
| 150 | PARAM - Could not access a file. This error may be due to one of several issues, for example the directory is empty, or the file's name does not correspond to an applied filter. | State K - ABORT. If the directory does not exist, no transfers are triggered and only the generic request remains in the catalog. If an error is detected when reading the directory, the transfers generated for the items (directory menus) that have already been read are triggered. |
| 152 | FILE - Error during the concatenation phase at the start of the transfer or during the de- concatenation phase at the send of the transfer. | State H - ABORT, EXECE<br/> Problem on homogeneous transfer issue. See [Sending a group of files](../../../../concepts/send_command/send_group_of_files_cl)<br/> Check the configuration - FNAME and WFNAME on the CFTSEND or CFTRECV object. |
| 153 | FILE - Error during the transfer. The file has been overwritten. | State K - Short transfer cannot be restarted. |
| 154  | An error occurred when trying to transcode a file using NCHARSET parameter in the CFTSEND command before a send.  | Generates a DIAGI=730, DIAGP=ABO 399 on the remote side as the sender cannot transcode before sending.<br/> Check the diagp/diagc in the catalog to have more information. |
| 155  | This preprocessing error occurs when a preexec is specified, but launching the exec file failed (usually because the file was not found, but could be due to a busy or migrated file on z/OS [MVS]).  | Generates:<br/> • DIAGP: NO EXEC<br/> • DIAGC: PREEXEC LAUNCH ERROR<br/> Check the PREEXEC parameter in the SEND, RECV, CFTSEND, and CFTRECV commands. |
| 156  | This error occurs if the maximum number of retries is reached when FACTION=RETRYRENAME.  | Generates:<br/> • DIAGP: RETRYRENAME<br/> • Phasestep=K<br/> Check these uconf values:<br/> • cft.server.transfer.rrename.retry_delay<br/> • cft.server.transfer.rrename.max_retries<br/> File defined in the FNAME parameter in the CFTRECV object should not exist when the retry occurs. Transfer can be restarted. |
| 158  | There was an error while replacing Transfer CFT variables.<br/> The script referenced by <code>&amp;fname</code> in the CFTS68E message is not executed. | In the script &amp;fname, modify the variable &lt;var&gt; indicated in the CFTS68E message.  |
| 160 | Handles the event 'no outstanding transfer or implicit declaration'. | No catalog entry is created. |
| 200  | An error occurred on a child transfer of this parent (generic) transfer. Refer to the child transfer in error for more information.  | State K - ABORT,EXECE  |
| 203  | PROF=SIT non- priority transfer  |   |
| 220 | PROT - FPDU reception | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 221 | PROT - Int/ext type match error: the type of a PI is not consistent with its conversion type in the external format (for example, a PI in DATE format to be converted to the STRING format) | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 222 | PROT - Mandatory PI missing in FPDU | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 223 | PROT - Invalid PI length | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 224 | PROT - Invalid PGI length | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 225 | PROT - PGI missing from the FPDU | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 226 | PROT - PGI embedded in another PGI | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 230 | PROT - Protocol error. A protocol error has been detected: DIAGP is set to the PeSIT or ODETTE code of the error detected | H status - ABORT, EXECE<br/> Contact the Support team for this type of unexpected error. Prior to doing so though, please collect as much information as possible regarding the transfer in error, including the remote partner information (product, configuration,...).<br/> Note that the 230 diagnostic codes may have different diagnostic protocols formats:<br/> • diagp = eEEsSS: An unexpected internal error when the event "EE" occurs during the "SS" state.<br/> • diagp = PDU iNN: The PDU parameter "NN" is not correct. |
| 231 | PROT - Invalid action | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 232 | PROT - Event not found. An interaction not recognized by the protocol mechanisms has been received in a given transfer context | H status - ABORT, EXECE<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 233  | PROT - Message send operation refused by the protocol used. The following protocols do not support message send operations: PESIT SIT, PESIT EXT, ODETTE | K status - ABORT<br/> This may require a trace. Please see [How to use ATM traces](../../../atm_traces). |
| 240 | PROT - Time- out expired (RTO parameter) | D status - RETRY/COMMUT<br/> For more information, see [Troubleshoot 240 or 740 RTO](../../../admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_rto). |
| 241  | Transfer time- out - requester  |   |
| 241  | Transfer time- out - server  |   |
| 243  | Network connection time- out  |   |
| 244  | Pre- connection time- out  |   |
| 260 | SSL - Security problem | K state - ABORT<br/> DIAGP=PKI with I, E or S nnn<br/> nnn = SSL code alert<br/> See [Troubleshoot security errors](#SSL). |
| 261 | SSL - Error linked to an internal PKI | K state - ABORT<br/> DIAGP=PKI I nnn<br/> nnn = SSL code alert<br/> See [Troubleshoot security errors](#SSL). |
| 262 | SSL - Error linked to the PKI system | K state - ABORT<br/> DIAGP=PKIS nnn<br/> nnn = SSL code alert<br/> See [Troubleshoot security errors](#SSL). |
| 263 | SSL - Error linked to an external PKI | K state - ABORT<br/> DIAGP=PKIE nnn<br/> nnn = SSL code alert<br/> See [Troubleshoot security errors](#SSL). |
| 278 | SSL - Invalid security profile | K state - ABORT |
| 301 | NET - Network addressing error (IP address) at the time of connection | D status - COMMUT<br/> The transfer will be retried for a minimum period equal to the WSCAN parameter of the CFTCAT command. The next partner address in the HOST parameter list (CFTTCP command) will be used for the next retry.<br/> • If the invalid address is the last one in the list, the next protocol in the PROT parameter list (CFTPART command) will be used for the next retry.<br/> • If the protocol used is the last in the list, the transfer is either switched to the backup partner (IPART parameter of the CFTPART command) or aborted (K status) with code 405, while maintaining the diagnostic code of the last retry |
| 302 | NET - Network link broken (cut- off, time- out) outside the connection phase. DIAGP is then set to VNRELI | D status - RETRY/COMMUT<br/> Up to "RETRYM" retries are performed for the transfer and the access data. If the number of retries reaches the value in the RETRYM parameter, {{< TransferCFT/axwayvariablesComponentShortName  >}} switches the access data.<br/> The partner access data for the next retry will relate to the next HOST parameter (CFTNET command), or the next PROT parameter (CFTPART command). The restart counter is reset to 0.<br/> If the protocol used is the last in the list, the transfer is either switched to the backup partner (IPART parameter of the CFTPART command) or aborted with code 405, while maintaining the diagnostic code ([Troubleshooting common transfer issues](../general_protocol_diagnostics) for more information. |
| 303 | NET - Network parameter error at the time of connection | D status - COMMUT<br/> The transfer is retried using the next protocol in the PROT parameter list (CFTPART command) as the new partner access point.<br/> If the protocol used is the last in the list, the transfer is either switched to the backup partner (IPART parameter of the CFTPART command) or aborted (K status) with code 405, while maintaining the diagnostic code of the last retry.<br/> Cannot connect to the remote partner due to a local connection rejection. When using SFTP, check that the remote SAP value in CFTPART object is defined and is correct. |
| 350 | The user requesting the transfer is not authorized to perform it | State H |
| 351 | The remote requester is not authorized to use the transfer. The transfer was in the H state. The monitor is running in the server/sender mode | State H |
| 352 | The remote requester is not authorized to create a transfer. The monitor is running in the server/sender mode and the transfer was to be created via a CFTSEND IMPL=YES | State H |
| 401 | PARAM - Embedded broadcast list explicitly refused | K status - ABORT<br/> For an explicit multi- partner request (PART parameter in the CFTDEST command), a single partner, itself defined as a partner list, aborts the request; only the generic list request set to the K status remains in the catalog. No transfer is executed.<br/> For a multi- partner request via an indirection file (FNAME parameter in the CFTDEST command), only the requests prior to the error are executed |
| 402 | PARAM - The PROT parameter of the CFTPART command does not belong to the active protocol list (PROT parameter of the CFTPARM command) | K status - ABORT |
| 403 | PARAM: Invalid password | No CAT<br/> The diagi = 403 is displayed only in the cftlog file<br/> CFTH22E NPART=PARTSSL rejected DIAGI=403 &lt;HOST=@IP_address&gt;<br/> On the requester the transfer status is D with diagi = 909 and the diagp = RCO 301, therefore the transfer is retried up to RETRYM times.<br/>  |
| 404 | PARAM: Open mode not authorized | No CAT (in server mode)<br/> Check that the FNAME=&amp;NFNAME in the CFTSEND or CFTRECV object. |
| 405 | OUT: Transfer CFT has tried all possible partner access points: HOST, PROT, IPART | K status - ABORT, EXECE<br/> Check the configuration of the CFTPART and CFTTCP objects.<br/> Before reaching 405 diagi, multiple retries were executed. Check the cftlog file for informtion related to this transfer.<br/> After correcting the transfer, you can restart the transfer. |
| 406 | 1. OUT - Maximum number of retries reached (RESTART parameter).<br/> DIAGP is set to MAXRST<br/> 2. AUTH - The required start time for execution of the transfer is outside the authorized time slot (OMINTIME / OMAXTIME); there is no other possible protocol for this partner. DIAGP is set to CALL OUT<br/> 3. AUTH - The network resource associated with the protocol does not accept outgoing calls; there is no other possible protocol for this partner<br/> DIAGP is set to L 0B 022<br/> 4. PARAM - There is no CFTN command for the partner and for the last protocol in the list (CFTPART PROT parameter). DIAGP is set to MAXRST<br/> 5. OVER - Transfer CFT has reached the limit (RESTART parameter) of authorized retries for the last partner protocol (CFTPART PROT parameter). DIAGP is set to MAXRST<br/> 6. PARAM - The SROUT parameter of the protocol cannot be used to execute the transfer; there is no other possible protocol for this partner. DIAGP is set to SROUT |  <br/> <br/> K status - ABORT, EXECE<br/> <br/> <br/>  |
| 408 | PARAM - PART parameter not described by a CFTPART command | K status - ABORT<br/> If a single CFTPART command is missing from an explicit multi- partner request (PART parameter in the CFTDEST command), the request is aborted. Only the generic list request, set to the K status, remains in the catalog.<br/> No transfer is executed.<br/> For a multi- partner request via an indirection file (FNAME parameter in the CFTDEST command), only transfers with no partner defined are halted<br/> The other transfers are executed<br/> Complete broadcasting (or collection) is unsuccessful without operator intervention (partner definition and transfer retry). The end of transfer procedure is not executed.  |
| 409 | PARAM - Unknown NPART parameter | H status - ABORT<br/> Check the correspondence between the PROT, NSPART, and NRPART parameters in the [CFTPART](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) object. |
| 410 | PARAM - Unknown CFTPROT command | K status - ABORT<br/> Check that the PROT value in the [CFTPARM](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) object. |
| 411 | AUTH - File identifier (IDF) not authorized | K status - ABORT<br/> If the IDF for one of the partners in an explicit multi- partner request (PART parameter in the [CFTDEST](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftdest) command) is not authorized, the request is aborted; only the generic list request set to the K status remains in the catalog<br/> No transfer is executed<br/> For a multi- partner request via an indirection file (FNAME parameter in the CFTDEST command), only transfers, the IDFs of which are not authorized for the partner (CFTAUTH command) are set to halted<br/> The other transfers remain active.<br/> However, complete broadcasting (or collection) is unsuccessful without operator intervention (grant authorization to the partners and retry the transfer); the end of transfer procedure will not be executed |
| 412 | DATA - Catalog access error | As the file could not be accessed, there is no change in the status or in the catalog DIAGI.<br/> The CFTT21E should display in the log and contain GCS and GCR information that may be helpful for Axway support to diagnos the issue.<br/> For example, this type of error could be caused by a corrupted catalog. If so, you could import and export the records to repair this. |
| 413 | AUTH - File identifier not authorized | H status - ABORT<br/> Check the SAUTH/RAUTH parameters in the [CFTAUTH](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) objects. |
| 414 | 1. AUTH - The start time for execution of the transfer is outside the authorized time slot (MAXTIME / MAXDATE of the SEND / RECV command)<br/> DIAGP is then set to OUT TIME<br/> 2. PARAM - The outgoing time slot of the partner is null (OMINTIME / OMAXTIME)<br/> There is no intermediate partner<br/> DIAGP is then set to CALL OUT<br/> 3. AUTH - No outgoing call authorized for the network resource (CFTnetwork CNXOUT=0)<br/> DIAGP is then set to NO CALL | K status - ABORT<br/> See [Network resources](../../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftnet) for more information on network resource depletion prevention. |
| 415 | OVER - Maximum number of partners reached | D status - NEXT |
| 416 | 1. OVER - Maximum number of transfers reached (MAXTRANS parameter)<br/> The transfer cannot be executed<br/> DIAGP is then set to MAXTRANS | D status - NEXT<br/> <br/> Please see [Network resource depletion prevention (NRDP)](../../../../concepts/about_parallel_transfers/prevent_network_depletion). |
| - " - | 2. OVER - Maximum number of connections reached for the network resource<br/> The transfer cannot be executed<br/> DIAGP is then set to MAXCNX | D status - NEXT<br/> This status corresponds to a transfer refusal by the {{< TransferCFT/axwayvariablesComponentLongName  >}} protocol task, even though the scheduler has not reached the MAXTRANS limit.<br/> This occurs when the protocol task maintains active connections after transfers have ended. Please see [UCONF](../../../../concepts/about_parallel_transfers/prevent_network_depletion) <code>cft.unix.stcp.afunix</code> parameter according to the operating system limit as shown below:<br/> • AIX: 1023<br/> • HP- UX: 92<br/> • Linux: 108<br/> • Solaris: 108 |
| 417 | 1. OVER - Maximum number of file tasks reached (MAXTASK parameter)<br/> The transfer cannot be executed<br/> 2. SYS- Insufficient system resources available to execute an EXIT task<br/> The transfer cannot be executed | D status - NEXT |
| 418 | OVER - The total number of transfers in progress exceeds one of the CNXIN, CNXOUT or CNXINOUT parameters for the partner<br/> The transfer cannot be executed | D status - RESTART<br/> If the number of retries exceeds the value of the RESTART parameter (CFTPROT command), Transfer CFT switches to the access data of the next protocol for this partner. See [Configure multi- node simultaneous transfers](../../../../concepts/about_parallel_transfers/multi_node_simultaneous_transfers). |
| 419 | DATA - The transfer to be retried is not in the catalog at the server end | ABORT<br/> Check the information in the cftlog file. |
| 420 | DATA - On reception of a REPLY- type message, the original transfer concerned by this reply is not found in the catalog at the server end | ABORT |
| 421 | 1. SYS - Error executing a {{< TransferCFT/suitevariablesTransferCFTName  >}}file task | D status - NEXT |
| - " - | 2. SYS - Error executing a {{< TransferCFT/suitevariablesTransferCFTName  >}} EXIT task | K status - ABORT |
| 423 | SYS or PARAM - EXIT task creation error | H status - ABORT<br/> Check the diagp and the cftlog file for more information on the context. |
| 424 | PARAM - CFTXLATE command not found for this transfer direction and the source and target alphabets | H status - ABORT<br/> Check the XLATE value in the CFTSEND/CFTRECV object and the associated CFTXLATE object. |
| 426 | USER (Directory Exit) - Error in the Directory Exit task | No CAT (server mode)<br/> State K - ABORT (requester mode) |
| 428  | Incoming address verified by the VERIFY parameter in the CFTTCP object was not accepted.  | No CAT<br/> Check the incoming address in the cftlog file and compare it to the host value in the CFTTCP object; the check is done on the VERIFY value (in terms of digits). |
| 430 | PARAM - Transfer is inactive on the requester side | State D - ACT<br/> Check the partner's state. |
| 431 | USER (Security) - CFTAPPL card is absent | No CAT<br/> Transfer is not started and there is no entry in the catalog. Additionally, the diagi is displayed only in cftlog file. |
| 432  | Duplicate transfer error  | Check the DUPLICAT parameter value in the CFTSEND or CFTRECV object.  |
| 433  | User/password error  | Check the SPASSWD/RPASSWD parameter values in the CFTSEND or CFTRECV object.  |
| 434  | AUTH - File identifier (default IDF) is not authorized  | K status - ABORT<br/> When cft.default_idf.enable = Yes , the default IDF is not authorized. |
| 435  | WORKDIR  | The CFTSEND and CFTRECV models with the same ID have different working directories defined.<br/> SFTP issue. Check the workingdir value for both the CFTSEND id=&lt;ID&gt; with IMPL=YES and CFTRECV id=&lt;ID&gt;. |
| 436  | KEY  | The client key does not correspond to the server key.<br/> SFTP issue.<br/> Check the CLIPUBKEY value in the CFTSSH direct=Client setting. This should correspond to an identifier in the local PKI database. |
| 451 | 1. PROT - (PeSIT) Reception of a protocol connection refusal (AckCONNECT FPDU). (Odette) Reception of a protocol connection refusal (ESID). DIAGP is then set to RELEASE<br/> 2. PROT - (PeSIT) (Odette) Violation of the protocol specifications (unknown FPDU, or invalid contents for example). DIAGP is then set to ACO in or RCO in ennsnn<br/> 3. PROT - (PeSIT)(Odette) Connection time- out reached without response (DISCTC parameter of the CFTPROT command). DIAGP is then set to TIMEOUT | D status - RESTART |
| 452 | PROT - (PeSIT) (Odette) Reception of a negative message confirmation FPDU | H status - ABORT, EXECE |
| 453 | PROT - (PeSIT) (Odette) Reception of a negative create confirmation FPDU | H status - ABORT, EXECE |
| 454 | PROT - (PeSIT) Reception of a negative select confirmation FPDU | H status - ABORT, EXECE |
| 455 | PROT - (PeSIT) (Odette) Reception of a negative deselect confirmation FPDU | H status - ABORT, EXECE |
| 456 | PROT - (PeSIT) Reception of a negative open confirmation FPDU | H status - ABORT, EXECE |
| 457  | Reception of a negative closed confirmation  |   |
| 458  | Reception of a negative read confirmation  |   |
| 459 | PROT - (PeSIT) Reception of a negative write confirmation FPDU | H status - ABORT, EXECE |
| 460 | PROT - (PeSIT) Reception of a negative end of transfer confirmation FPDU | H status - ABORT, EXECE |
| 461  | Received an abort with diagnostics  |   |
| 462  | No data sent on network  |   |
| 463  | Logon entry not recognized  |   |
| 499  | ( ODETTE) CD okay  |   |

Codes with a value between 001 and 499 indicate a local issue; values
between 501 and 999 correspond to a fault reported by the partner.

Therefore when troubleshooting if the code is greater than 500 it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 916, for example, the issue is a remote problem corresponding to DIAG 416 (maximum number of transfers reached).

| 500  | Constant to add to a remote code  |   |
| --- | --- | --- |
| 600 | FILE - (PeSIT) (Odette) Transfer aborted by the remote end: file input/output error - PeSIT / Odette code: See [DIAGP.](../general_protocol_diagnostics) | H status- ABORT, EXECE |
| 604 | FILE - (PeSIT) Transfer aborted by the remote end: file opening error | H status - ABORT, EXECE |
| 605 | FILE - (PeSIT) Transfer aborted by the remote end: file closing error | H status - ABORT, EXECE |
| 610 | FILE - (PeSIT) (Odette) Transfer aborted by the remote end: the file to be read does not exist | H status - ABORT, EXECE |
| 611 | FILE - (PeSIT) Transfer aborted by the remote end: insufficient space to create the file | H status - ABORT, EXECE |
| 613 | FILE - (PeSIT) Transfer aborted by the remote end: the file to be created already exists | H status - ABORT, EXECE |
| 614 | FILE - (PeSIT) Transfer aborted by the remote end: file space full | H status - ABORT, EXECE |
| 620 | PROT - (PeSIT) Transfer aborted by the remote end: counter control error | H status - ABORT, EXECE |
| 621 | PROT - (PeSIT) Transfer aborted by the remote end: interruption by the operator | H status - ABORT, EXECE |
| 626 | PROT - (PeSIT) (Odette) Transfer aborted by the remote end: error in record length | H status - ABORT, EXECE |
| 635 | FILE - (PeSIT) Transfer aborted by the remote end: file access conflict | H status - ABORT, EXECE |
| 657  | Errno  | Too many open files in the CFTSFTP process.  |
| 660 | REC (PeSIT) - Error 660, ASE 205 on the requester side | H state - Transfer aborted by the remote end: no outstanding transfer |
| 720 | 1. PROT - (PeSIT) Protocol abort by the remote end: incorrect FPDU (transmission error)<br/> 2. PROT - (Odette) Protocol abort by the remote end: negotiation error | H status - ABORT, EXECE |
| 722 | PROT - (PeSIT) Protocol abort by the remote end: missing PI | H status - ABORT, EXECE |
| 730 | 1. PROT - Protocol error<br/> 2. PROT - (PeSIT) Transfer aborted by the remote peer (i.e. the Partner Side) due to protocol error - PeSIT code: See [DIAGP.](../general_protocol_diagnostics)<br/> 3. PROT - (Odette) Reception of an ESID FPDU | H status - ABORT, EXECE |
| 740 | NET - (PeSIT) Transfer aborted by the remote end: time- out - PeSIT code: 317 | D status - RETRY |
| 850 | PROT - (PeSIT) Protocol rejection by the remote end: authorization problem | H Status - ABORT, EXECE |
| 904 | PROT - (PeSIT) Protocol rejection by the remote end: transfer denied (open mode, authorizations for example) | H status - ABORT, EXECE |
| 909 | PROT - (PeSIT only) Protocol rejection by the remote end: requester identifier unknown | D status - RESTART |
| 916 | PROT - (PeSIT only) Maximum number of transfers reached at the partner end (MAXTRANS parameter) | D status - NEXT<br/> See [Configure multi- node simultaneous transfers](../../../../concepts/about_parallel_transfers/multi_node_simultaneous_transfers). |
| 919 | Restart context not available | H status - ABORT, EXECE |
| 920 | PROT - (PeSIT) Protocol rejection by the remote end: on reception of a REPLY- type message, the partner does not find the transfer concerned by this reply in its catalog | D status - RESTART |
| 925 | Call collect refused by the remote system | No CAT |
| 928 | Invalid caller number | H status - ABORT, EXECE |
| 930 | PROT - Partner is inactive on the server side | ACT status |
| 933  | Error in password management parameter RPASSWD or SPASSWD: non- authorized requester identification  |   |
| 963 | PROT - Protocol pre- connection phase rejected by the remote end (PeSIT LOGON): LOGON string rejected | K status - ABORT, EXECE |
| 970 | PROT - Protocol pre- connection phase rejected by the remote end (PeSIT LOGON): password expired | K status - ABORT, EXECE |

<span id="SSL"></span>

## SSL alert errors

| Code | Description |
| --- | --- |
| 0 | Close notify |
| 10 | Unexpected message |
| 20 | Bad record MAC |
| 21 | Decryption failed |
| 22 | Record overflow |
| 30 | Decompression failure |
| 40 | Handshake failure |
| 41 | No certificate |
| 42 | Bad certificate |
| 43 | Unsupported certificate |
| 44 | Certificate revoked |
| 45 | Certificate expired |
| 46 | Certificate unknown |
| 47 | Illegal parameter |
| 48 | Unknown CA ([Certificate authority](http://en.wikipedia.org/wiki/Certificate_authority)) |
| 49 | Access denied |
| 50 | Decode error |
| 51 | Decrypt error |
| 60 | Export restriction |
| 70 | Protocol version |
| 71 | Insufficient security |
| 80 | Internal error |
| 90 | User canceled |
| 100 | No renegotiation |
| 110 | Unsupported extension |
| 111 | Certificate unobtainable |
| 112 | Certificate unobtainable |
| 113 | Bad certificate status response |
| 114 | Bad certificate hash value |

