---

    title: DIAGI - Diagnostic  codes
    linkTitle: DIAGI - Diagnostic codes
    weight: 430

---
<span id="Internal_diagnostic_codes"></span>

## {{< TransferCFT/axwayvariablesComponentShortName  >}} internal diagnostic codes

Diagnostic codes provide general information on the cause of the error. It
is independent of the operating system and of the network access method
used for the transfer. Some codes are specific to a protocol. If so, this
is indicated in the label.

## Diagnostic code values

DIAGI codes with a value between 001 and 499 indicate a local issue; values
between 501 and 999 correspond to a fault reported by the partner.

This means that when you troubleshooting, if the DIAGI code is greater than 500 it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 916, for example, the issue is a remote problem corresponding to DIAG 416 (Maximum number of transfers reached).

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

<!-- -->

- SSL: Incident
    detected by the secured protocol SSL 

<span id="Behavior_column_in_diagnostic_codes"></span>

## Consequence column in diagnostic codes

The Consequence column provides
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
    -   EXECE: the procedure is executed. This is the procedure
        defined by CFTPARM EXEC\*E parameters. If these parameters are not set,
        the procedure is not executed.

The "No CAT" indication specifies that no
catalog entry had been created for the transfer when the error occurred.
The transfer request is rejected.<span id="Internal_diagnostic_codes_table"></span>

<span class="autonumber"></span>DIAGI - Internal diagnostic codes
table

The following table makes references to DIAGP. For details, please see the [DIAGP section.](../general_protocol_diagnostics)

QQQ\_QQQ\_QQQ flattened the table

Codes with a value between 001 and 499 indicate a local issue; values
between 501 and 999 correspond to a fault reported by the partner.

Therefore when troubleshooting if the code is greater than 500 it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 916, for example, the issue is a remote problem corresponding to DIAG 416 (maximum number of transfers reached).

#### Diagi code : 0

The transfer has terminated correctly

**Consequence:**Execution of normal EXECRF or EXECSF end of transfer procedures

#### Diagi code : 001

SYS: Error creating the message queue or allocating the
memory

**Consequence:**H status - ABORT, EXECE

#### Diagi code : 002

Context definition error

**Consequence:** H status - ABORT, EXECE

#### Diagi code : 003

SYS - Context allocation error

**Consequence:** H status - ABORT, EXECE

#### Diagi code : 004

MQCONN

#### Diagi code : 005

MQOPEN

#### Diagi code : 006

MQPUT

#### Diagi code : 100

FILE - File input/output error

**Consequence:**

H status - ABORT, EXECE

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 101

FILE - Error creating the receive file

**Consequence:**

H status - ABORT, EXECE

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 102

1.FILE - Error allocating the transfer
file

QQQ\_QQQ\_QQQ

**Consequence:**

H status - ABORT, EXECE in requester mode

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

2.FILE - The receive
file cannot be allocated (FDISP=OLD case)

**Consequence:**

H status - ABORT, EXECE  
The file is deleted.

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 103

1.FILE - The file cannot be deleted before
the receive file is created (FDISP = DELETE case)

**Consequence:**

H status - ABORT, EXECE in requester mode

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

2.FILE - Error
deleting the sent file, if a deletion has been requested (FACTION = DELETE)

**Consequence:**

H status - ABORT, EXECE in requester mode

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 104

1\. FILE - Error opening the transfer file

2\. FILE - The
receive file cannot be erased (FDISP = ERASE case): file opening problem

3\. FILE - Prior
to reception, the receive file could not be opened to check that it was
empty (FDISP = VERIFY case)

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 105

1. FILE - Error closing the transfer
file

2. FILE - The
receive file cannot be erased (FDISP = ERASE case: file closing problem

3. FILE - Prior
to reception, the receive file could not be closed after checking that
it was empty (FDISP = VERIFY case)

4. FILE - The
sent file cannot be deleted following an erase request (FACTION = ERASE
case)

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 106

FILE - Error recording the current position in the transfer
file (synchronization point setting)

**Consequence:**

H status - ABORT, EXECE

If there is a DIAGP for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 107

FILE - Error setting the pointer to a re-synchronization
point in the file (for a transfer restart)

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 108

1. FILE - Send file read error in data transfer phase

2. FILE - Prior to reception, the receive file could
not be read to check that it was empty (FDISP = VERIFY case)

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 109

FILE - Data write error in the receive file

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 110

1. FILE - The send file does not exist

2. FILE - The receive file to be created does not
exist, even though the FDISP parameter requires it (FDISP=OLD). DIAGP
is then set to NO OLD

**Consequence:**

H status - ABORT, EXECE in requester mode

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 111

FILE - Insufficient space to create the file

**Consequence:**

H status - ABORT in requester mode, EXECE in requester
mode

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 112

Nonexistent unit

#### Diagi code : 113

FILE - The file to be created already exists, even though
the FDISP parameter prohibits it (FDISP = NEW). DIAGP is then set
to NO NEW

**Consequence:**

H status - ABORT, EXECE in requester mode

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 114

FILE - Data write error in the receive file: file space
full

**Consequence:**

H status - ABORT, EXECE

If there is a [DIAGP](../protocol_diagnostic_codes) for this, it is system specific. For these errors, check the DIAGC or the log for more information.

#### Diagi code : 115

1. FILE - The transfer owner is not authorized to
access the file

2. FILE - The file cannot be deleted before the receive
file has been created (FDISP = DELETE case)Protected file

**Consequence:**

H status - ABORT, EXECE in requester mode

Please see the <a href="" class="MCXref xref">Access management</a> sections for more information.

3. FILE - The sent file cannot be deleted following
a deletion request

(FACTION = DELETE case) Protected file

**Consequence:**

H status - ABORT, EXECE

Please see the <a href="" class="MCXref xref">Access management</a> sections for more information.

#### Diagi code : 120

PROT - Counter check error

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 121

USER - Interruption by the operator

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 122

SYS - Error allocating memory when the transfer is executed

D status - RESTART

#### Diagi code : 123

FILE - Error setting the pointer to a resynchronization point in the file: the restart point requested by the partner is incorrect.

This can occur when the synchronized points are not flushed from the catalog (if the CFTCAT's `updat `parameter is greater than 1, the UCONF<span class="code">`cft.server.catalog.sync.enable`</span> is set to <span class="code">`No`</span>, or both of these are true) due to an unexpected event such as a process, system, or disk crash.

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 124

PROT - Error: transfer aborted

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 126

FILE - LRECL error (record length)

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 127

FILE - The receive file is not empty (FDISP = VERIFY case)

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 128

1. FILE - Error deselecting the file

2. FILE - Error deselecting the receive file

**Consequence:**

H status - ABORT, EXECE

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

3. FILE - Error accessing the send file (allocating
or opening), subsequent to a file erase request (FACTION = ERASE)

**Consequence:**

H status - ABORT, EXECE unless there is an
allocation error in sender server mode

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 129

FILE - Error during file decompression

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 130

FILE - Error during file compression

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 131

PROT - IDF different on ODETTE SELECT

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 132

PARAM - Error accessing the parameter setting indirection
file (list of files, list of partners)

**Consequence:**

K status - ABORT

If the file does not exist, no transfer is executed. Only
the generic request remains in the catalog. If an error occurs while reading
the indirection file, the transfers generated for the items (files or
partners) that have already been read are executed

#### Diagi code : 133

PARAM - The FOR parameter in the CFTDEST command is invalid

1\. FOR=LOCAL when in the switching mode

2\. FOR=COMMUT when not in the switching mode

**Consequence:**

No catalog entry. Check the CFTDEST object in the configuration.

#### Diagi code : 134

FILE - CFTEXIT call error

**Consequence:**

H status - ABORT, EXECE

Check the information in the cftlog and the cft.out files.

#### Diagi code : 135

1. FILE - The send file is locked

**Consequence:**

D status - RESTART

Check that the file is not locked by another process.

H status - ABORT, EXECE

Check that the file is not locked by another process.

2. FILE - The receive file is locked

**Consequence:**

D status - RESTART

Check that the file is not locked by another process.

H status - ABORT, EXECE

Check that the file is not locked by another process.

#### Diagi code : 136

FILE - Duplication of the temporary file

**Consequence:**

H status - ABORT, EXECE

Check the WFNAME parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) or [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

#### Diagi code : 137

FILE - The file exists, the "rename" operation
is therefore impossible

**Consequence:**

H status - ABORT, EXECE

Check the WFNAME parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) or [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

#### Diagi code : 138

1. FILE - No temporary file has been defined in the
send mode and the transfer requires the COPY mechanism. No transfer is
triggered and the generic request remains in the catalog

**Consequence:**

State K - ABORT

Check the WFNAME parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) or [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

Check the WFNAME parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) or [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

2. FILE - No temporary file has been defined in the
receive mode for a file with versions or for a transfer requiring deconcatenation
(COPY)

**Consequence:**

State K - ABORT, EXECE

Check the WFNAME parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) or [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

#### Diagi code : 139

Incorrect attributes file

**Consequence:**

Check the cftlog file and the diagp/diagc in the catalog record.

Also, check the FLRECL/FRECFM parameters in the [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) object.

#### Diagi code : 142

FILE - The "rename" operation failed

H status - ABORT, EXECE

Check the WFNAME parameter in the [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) command.

#### Diagi code : 144

MVS transfer busy

**Consequence:**

State D - Retry

Specific to z/OS environments.

#### Diagi code : 145

vK status - ABORT

Check the WORKINGDIR parameter in the [CFTSEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend) object.

#### Diagi code : 148

1\. The RECV file is outside of the workingdir tree.

2\. The temporary RECV file is outside of the workingdir tree.

**Consequence:**

K status - ABORT

Check the WORKINGDIR parameter in the [CFTRECV](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftrecv) object.

#### Diagi code : 150

PARAM - Could not access a file. This error may be due to one of several issues, for example the directory is empty, or the file's name does not correspond to an applied filter.

State K - ABORT. If the directory does not exist, no transfers
are triggered and only the generic request remains in the catalog. If
an error is detected when reading the directory, the transfers generated
for the items (directory menus) that have already been read are triggered.

#### Diagi code : 152

FILE - Error during the concatenation phase at the start
of the transfer or during the de-concatenation phase at the send of the
transfer.

**Consequence:**

State H - ABORT, EXECE

Problem on homogeneous transfer issue. See <a href="../../../concepts/using_the_send_command/send_group_of_files_cl" class="MCXref xref">Sending
a group of files</a>

Check the configuration - FNAME and WFNAME on the CFTSEND or CFTRECV object.

#### Diagi code : 153

FILE - Error during the transfer. The file has been overwritten.

**Consequence:**

State K - Short transfer cannot be restarted.

#### Diagi code : 154

An error occurred when trying to transcode a file using NCHARSET parameter in the CFTSEND command before a send.

**Consequence:**

Generates a DIAGI=730, DIAGP=ABO 399 on the remote side as the sender cannot transcode before sending.

Check the diagp/diagc in the catalog to have more information.

#### Diagi code : 155

This preprocessing error occurs when a preexec is specified, but launching the exec file failed (usually because the file was not found, but could be due to a busy or migrated file on z/OS \[MVS\]).

**Consequence:**

Generates:

- DIAGP: NO EXEC
- DIAGC: PREEXEC LAUNCH ERROR

Check the PREEXEC parameter in the SEND, RECV, CFTSEND, and CFTRECV commands.

#### Diagi code : 156

This error occurs if the maximum number of retries is reached when FACTION=RETRYRENAME.

**Consequence:**

Generates:

- DIAGP: RETRYRENAME
- Phasestep=K

Check these uconf values:

- cft.server.transfer.rrename.retry\_delay
- cft.server.transfer.rrename.max\_retries

File defined in the FNAME parameter in the CFTRECV object should not exist when the retry occurs. Transfer can be restarted.

#### Diagi code : 158

There was an error while replacing Transfer CFT variables.

The script referenced by <span class="code">`&fname`</span> in the CFTS68E message is not executed.

**Consequence:**

In the script &fname, modify the variable &lt;var> indicated in the CFTS68E message.

#### Diagi code : 160

Handles the event 'no outstanding
transfer or implicit declaration'.

**Consequence:**

No catalog entry is created.

#### Diagi code : 200

An error occurred on a child transfer of this parent (generic) transfer. Refer to the child transfer in error for more information.

**Consequence:**

State K - ABORT,EXECE

#### Diagi code : 203

PROF=SIT non-priority transfer

#### Diagi code : 220

PROT - FPDU reception

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 221

PROT - Int/ext type match error: the type of a PI is not
consistent with its conversion type in the external format (for example,
a PI in DATE format to be converted to the STRING format)

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 222

PROT - Mandatory PI missing in FPDU

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 223

PROT - Invalid PI length

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 224

PROT - Invalid PGI length

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 225

PROT - PGI missing from the FPDU

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 226

PROT - PGI embedded in another PGI

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 230

PROT - Protocol error. A protocol error has been detected:
DIAGP is set to the PeSIT or ODETTE code of the error detected

**Consequence:**

H status - ABORT, EXECE

Contact the Support team for this type of unexpected error. Prior to doing so though, please collect as much information as possible regarding the transfer in error, including the remote partner information (product, configuration,...).

Note that the 230 diagnostic codes may have different diagnostic protocols formats:

- diagp = eEEsSS: An unexpected internal error when the event "EE" occurs during the "SS" state.
- diagp = PDU iNN: The PDU parameter "NN" is not correct.

#### Diagi code : 231

PROT - Invalid action

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 232

PROT - Event not found. An interaction not recognized by
the protocol mechanisms has been received in a given transfer context

**Consequence:**

H status - ABORT, EXECE

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 233 

PROT - Message send operation refused by the protocol used.
The following protocols do not support message send operations: PESIT SIT, PESIT EXT, ODETTE

**Consequence:**

K status - ABORT

This may require a trace. Please see <a href="../../traces_in_cft" class="MCXref xref">How to use ATM traces</a>.

#### Diagi code : 240

PROT - Time-out expired (RTO parameter)

**Consequence:**

D status - RETRY/COMMUT

For more information, see <a href="../../admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_rto" class="MCXref xref">Troubleshoot 240 or 740 RTO</a>.

#### Diagi code : 241

Transfer time-out - requester

#### Diagi code : 241

Transfer time-out - server

#### Diagi code : 243

Network connection time-out

#### Diagi code : 244

Pre-connection time-out

#### Diagi code : 260

SSL - Security problem

**Consequence:**

K state - ABORT

DIAGP=PKI with I, E or S nnn

nnn = SSL code alert

See [SSL alert errors](#SSL) and <a href="../../admin_troubleshooting_server/troubleshoot_security" class="MCXref xref">Troubleshoot security errors</a>.

#### Diagi code : 261

SSL - Error linked to an internal PKI

**Consequence:**

K state - ABORT

DIAGP=PKI I nnn

nnn = SSL code alert

See [SSL alert errors](#SSL) and <a href="../../admin_troubleshooting_server/troubleshoot_security" class="MCXref xref">Troubleshoot security errors</a>.

#### Diagi code : 262

SSL - Error linked to the PKI system

**Consequence:**

K state - ABORT

DIAGP=PKIS nnn

nnn = SSL code alert

See [SSL alert errors](#SSL) and <a href="../../admin_troubleshooting_server/troubleshoot_security" class="MCXref xref">Troubleshoot security errors</a>.

#### Diagi code : 263

SSL - Error linked to an external PKI

**Consequence:**

K state - ABORT

DIAGP=PKIE nnn

nnn = SSL code alert

See [SSL alert errors](#SSL) and <a href="../../admin_troubleshooting_server/troubleshoot_security" class="MCXref xref">Troubleshoot security errors</a>.

#### Diagi code : 278

SSL - Invalid security profile

**Consequence:**

K state - ABORT

#### Diagi code : 301

NET - Network addressing error (IP address) at the time
of connection

**Consequence:**

D status - COMMUT

The transfer will be retried for a minimum period equal
to the WSCAN parameter of the CFTCAT command. The next partner address
in the HOST parameter list (CFTTCP command) will be used for the
next retry.

- If the invalid address is the last one in the list, the next
    protocol in the PROT parameter list (CFTPART command) will be used for
    the next retry.
- If the protocol used is the last in the list, the transfer
    is either switched to the backup partner (IPART parameter of the CFTPART
    command) or aborted (K status) with code 405, while maintaining the
    diagnostic code of the last retry

#### Diagi code : 302

NET - Network link broken (cut-off, time-out) outside the
connection phase. DIAGP is then set to VNRELI

**Consequence:**

D status - RETRY/COMMUT

Up to "RETRYM" retries are performed for the
transfer and the access data. If the number of retries reaches the value
in the RETRYM parameter, {{< TransferCFT/axwayvariablesComponentShortName  >}} switches the access data.

The partner access data for the next retry will relate to the next HOST
parameter (CFTNET command), or the next PROT parameter (CFTPART command).
The restart counter is reset to 0.

If the protocol used is the last in
the list, the transfer is either switched to the backup partner (IPART
parameter of the CFTPART command) or aborted with code 405, while maintaining
the diagnostic code ([DIAGP](../general_protocol_diagnostics)) of the last retry

See <a href="../../common_transfer_issues" class="MCXref xref">Troubleshooting common transfer issues</a> for more information.

#### Diagi code : 303

NET - Network parameter error at the time of connection

**Consequence:**

D status - COMMUT

The transfer is retried using the
next protocol in the PROT parameter list (CFTPART command) as the new
partner access point.

If the protocol used is the last in the list, the
transfer is either switched to the backup partner (IPART parameter of
the CFTPART command) or aborted (K status) with code 405, while maintaining
the diagnostic code of the last
retry.

Cannot connect to the remote partner due to a local connection rejection. When using SFTP, check that the remote SAP value in CFTPART object is defined and is correct.

#### Diagi code : 350

The user requesting the transfer is not authorized to perform
it

**Consequence:**

State H

#### Diagi code : 351

The remote requester is not authorized to use the transfer.
The transfer was in the H state. The monitor is running in the server/sender
mode

**Consequence:** State H

#### Diagi code : 352

The remote requester is not authorized to create a transfer.
The monitor is running in the server/sender mode and the transfer was
to be created via a CFTSEND IMPL=YES

**Consequence:** State H

#### Diagi code : 401

PARAM - Embedded broadcast list explicitly refused

**Consequence:**

K status - ABORT

For an explicit multi-partner request
(PART parameter in the CFTDEST command), a single partner, itself defined
as a partner list, aborts the request; only the generic list request set
to the K status remains in the catalog. No transfer is executed.

For a multi-partner request via an indirection file (FNAME
parameter in the CFTDEST command), only the requests prior to the error
are executed

#### Diagi code : 402

PARAM - The PROT parameter of the CFTPART command does
not belong to the active protocol list (PROT parameter of the CFTPARM
command)

**Consequence:**

K status - ABORT

#### Diagi code : 403

PARAM: Invalid password

**Consequence:**

No CAT

The diagi = 403 is displayed only in the cftlog file

CFTH22E NPART=PARTSSL rejected DIAGI=403 &lt;HOST=@IP\_address>

On the requester the transfer status is D with diagi = 909 and the diagp = RCO 301, therefore the transfer is retried up to RETRYM times.

#### Diagi code : 404

PARAM: Open mode not authorized

**Consequence:**

No CAT (in server mode)

Check that the FNAME=&NFNAME in the CFTSEND or CFTRECV object.

#### Diagi code : 405

OUT: Transfer CFT has tried all possible partner access
points: HOST, PROT, IPART

**Consequence:**

K status - ABORT, EXECE

Check the configuration of the CFTPART and CFTTCP objects.

Before reaching 405 diagi, multiple retries were executed. Check the cftlog file for informtion related to this transfer.

After correcting the transfer, you can restart the transfer.

#### Diagi code : 406

1. OUT - Maximum number of retries reached (RESTART
parameter).

DIAGP is set to MAXRST

2. AUTH - The required start time for execution of
the transfer is outside the authorized time slot (OMINTIME / OMAXTIME);
there is no other possible protocol for this partner. DIAGP is set to
CALL OUT

3. AUTH - The network resource associated with the
protocol does not accept outgoing calls; there is no other possible protocol
for this partner

DIAGP is set to L 0B 022

4. PARAM - There is no CFTN command for the
partner and for the last protocol in the list (CFTPART PROT parameter).
DIAGP is set to MAXRST

5. OVER - Transfer CFT has reached the limit (RESTART
parameter) of authorized retries for the last partner protocol (CFTPART
PROT parameter). DIAGP is set to MAXRST

6. PARAM - The SROUT parameter of the protocol cannot
be used to execute the transfer; there is no other possible protocol for
this partner. DIAGP is set to SROUT

**Consequence:**

K status - ABORT, EXECE

#### Diagi code : 408

PARAM - PART parameter not described by a CFTPART command

**Consequence:**

K status - ABORT

If a single CFTPART command is missing from an explicit
multi-partner request (PART parameter in the CFTDEST command),
the request is aborted. Only the generic list request, set to the K status,
remains in the catalog.

No transfer is executed.

For a multi-partner request via
an indirection file (FNAME parameter in the CFTDEST command), only transfers
with no partner defined are halted

The other transfers are executed

Complete broadcasting (or collection) is unsuccessful without operator
intervention (partner definition and transfer retry). The end of transfer procedure is not executed.

#### Diagi code : 409

PARAM - Unknown NPART parameter

**Consequence:**

H status - ABORT

Check the correspondence between the PROT, NSPART, and NRPART parameters in the [CFTPART](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) object.

#### Diagi code : 410

PARAM - Unknown CFTPROT command

**Consequence:**

K status - ABORT

Check that the PROT value in the [CFTPART](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) matches an active protocol PROT parameter of [CFTPARM](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftparm) object.

#### Diagi code : 411

AUTH - File identifier (IDF) not authorized

**Consequence:**

K status - ABORT

If the IDF for one of the partners in an explicit multi-partner
request (PART parameter in the [CFTDEST](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftdest) command) is not authorized, the
request is aborted; only the generic list request set to the K status
remains in the catalog

No transfer is executed

For a multi-partner request via
an indirection file (FNAME parameter in the CFTDEST command), only transfers,
the IDFs of which are not authorized for the partner (CFTAUTH command)
are set to halted

The other transfers remain active.

However,
complete broadcasting (or collection) is unsuccessful without operator
intervention (grant authorization to the partners and retry the transfer);
the end of transfer procedure will not be executed

#### Diagi code : 412

DATA - Catalog access error

Consequence: As the
file could not be accessed, there is no change in the status or in the catalog DIAGI.

The  CFTT21E should display in the log and contain GCS and GCR information that may be helpful for Axway support to diagnos the issue.

For example, this type of error could be caused by a corrupted catalog. If so, you could import and export the records to repair this.

#### Diagi code : 413

AUTH - File identifier not authorized

**Consequence:**

H status - ABORT

Check the SAUTH/RAUTH parameters in the [CFTPART](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart) object and the associated [CFTAUTH](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftauth) objects.

#### Diagi code : 414

1. AUTH - The start time for execution of the transfer
is outside the authorized time slot (MAXTIME / MAXDATE of the SEND / RECV
command)

DIAGP is then set to OUT TIME

2. PARAM - The outgoing time slot of the partner is
null (OMINTIME / OMAXTIME)

There is no intermediate partner

DIAGP is then set to CALL OUT

3. AUTH - No outgoing call authorized for the network
resource (CFTnetwork CNXOUT=0)

DIAGP is then set to NO CALL

**Consequence:**

K status - ABORT

See <a href="../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftnet" class="MCXref xref">CFTNET
- Network resources</a> and <a href="../../../admin_intro/admin_config_commands/network_resource_concepts" class="MCXref xref">Network
resources</a> for more information on network resource depletion prevention.

#### Diagi code : 415

OVER - Maximum number of partners reached

**Consequence:**

D status - NEXT

#### Diagi code : 416

1. OVER - Maximum number of transfers reached (MAXTRANS
parameter)

The transfer cannot be executed

DIAGP is then set to MAXTRANS

**Consequence:**

D status - NEXT

Please see <a href="../../../concepts/about_parallel_transfers/prevent_network_depletion" class="MCXref xref">Network resource depletion prevention (NRDP)</a>.

2. OVER - Maximum number of connections reached for
the network resource

The transfer cannot be executed

DIAGP is then set to MAXCNX

**Consequence:**

D status - NEXT

This status corresponds to a transfer refusal by the {{< TransferCFT/axwayvariablesComponentLongName  >}} protocol
task, even though the scheduler has not reached the MAXTRANS
limit.

This occurs when the protocol task maintains active connections
after transfers have ended. Please see <a href="../../../concepts/about_parallel_transfers/prevent_network_depletion" class="MCXref xref">Network resource depletion prevention (NRDP)</a>.

WSCAN related behavior

Technically, the next retry is triggered **wscan** minutes after the previous try. However, sometimes you may have a {{< TransferCFT/axwayvariablesComponentLongName  >}} log where the 416 diagnostic codes are not evenly distributed (by the same time intervals). This may occur if the scheduling task believes resources are available and schedules a retry, but in reality the resource is taken.

*Unix only* - If the runtime path is too long, a diag 416 issue occurs and a message similar to the following displays in the <span class="code">`cft.out`</span>:

> `CFTTCPS S_socket : start_soc : bind afunix`
>
> `CFTTCPS Invalid argument`

If this error occurs, modify the path to the socket in the [UCONF](../../../admin_intro/uconf/uconf_directory) <span class="code">`cft.unix.stcp.afunix`</span> parameter according to the operating system limit as shown below:

- AIX: 1023
- HP-UX: 92
- Linux: 108
- Solaris: 108

#### Diagi code : 417

1. OVER - Maximum number of file tasks reached (MAXTASK
parameter)

The transfer cannot be executed

2. SYS- Insufficient system resources available to
execute an EXIT task

The transfer cannot be executed

**Consequence:**

D status - NEXT

#### Diagi code : 418

OVER - The total number of transfers in progress exceeds
one of the CNXIN, CNXOUT or CNXINOUT parameters for the partner

The transfer cannot be executed

**Consequence:**

D status - RESTART

If the number of retries exceeds the value of the RESTART
parameter (CFTPROT command), Transfer CFT switches to the access data of
the next protocol for this partner. See <a href="../../../concepts/about_parallel_transfers/multi_node_simultaneous_transfers" class="MCXref xref">Configure multi-node simultaneous transfers</a>.

See also, [Frequently asked questions](../../../concepts/about_parallel_transfers/faq) and [Configure multi-node simultaneous transfers](../../../concepts/about_parallel_transfers/multi_node_simultaneous_transfers).

#### Diagi code : 419

DATA - The transfer to be retried is not in the catalog
at the server end

**Consequence:**

ABORT

Check the information in the cftlog file.

#### Diagi code : 420

DATA - On reception of a REPLY-type message, the original
transfer concerned by this reply is not found in the catalog at the server
end

**Consequence:** ABORT

#### Diagi code : 421

1. SYS - Error executing a {{< TransferCFT/suitevariablesTransferCFTName  >}}file task

**Consequence:**

D status - NEXT

2. SYS - Error executing a {{< TransferCFT/suitevariablesTransferCFTName  >}} EXIT task

**Consequence:**

K status - ABORT

#### Diagi code : 423

SYS or PARAM - EXIT task creation error

**Consequence:**

H status - ABORT

Check the diagp and the cftlog file for more information on the context.

#### Diagi code : 424

PARAM - CFTXLATE command not found for this transfer direction
and the source and target alphabets

**Consequence:**

H status - ABORT

Check  the XLATE value in the CFTSEND/CFTRECV object and the associated CFTXLATE object.

#### Diagi code : 426

USER (Directory Exit) - Error in the Directory Exit task

**Consequence:**

No CAT (server mode)

State K - ABORT (requester mode)

#### Diagi code : 428

Incoming address verified by the VERIFY parameter in the CFTTCP object  was not accepted.

Consequence: No CAT

Check the incoming address in the cftlog file
and compare it to the host value in the CFTTCP object; the check is done on the VERIFY value (in terms of digits).

#### Diagi code : 430

PARAM - Transfer is inactive on the requester side

**Consequence:**

State D - ACT

Check the partner's state.

#### Diagi code : 431

USER (Security) - CFTAPPL card is absent

**Consequence:**

No CAT

Transfer is not started and there is no entry in the catalog. Additionally, the diagi is displayed only in cftlog file.

#### Diagi code : 432

Duplicate transfer error

**Consequence:**

Check the DUPLICAT parameter value in the CFTSEND or CFTRECV object.

#### Diagi code : 433

User/password error

**Consequence:**

Check the SPASSWD/RPASSWD parameter values in the CFTSEND or CFTRECV object.

#### Diagi code : 434

AUTH - File identifier (default IDF) is not authorized

**Consequence:**

K status - ABORT

When cft.default\_idf.enable = Yes , the default IDF is not authorized.

#### Diagi code : 435

WORKDIR

**Consequence:**

The CFTSEND and CFTRECV models with the same ID have different working directories defined.

SFTP issue. Check the workingdir value for both the CFTSEND id=&lt;ID> with IMPL=YES and CFTRECV  id=&lt;ID>.

#### Diagi code : 436

KEY

**Consequence:**

The client key does not correspond to the server key.

SFTP issue.

Check the CLIPUBKEY value in the CFTSSH direct=Client setting. This should correspond to an identifier in the local PKI database.

#### Diagi code : 451

1. PROT - (PeSIT) Reception of a protocol connection
refusal (AckCONNECT FPDU). (Odette) Reception of a protocol connection
refusal (ESID). [DIAGP](../protocol_diagnostic_codes) is then set to RELEASE

2. PROT - (PeSIT) (Odette) Violation of the protocol
specifications (unknown FPDU, or invalid contents for example). DIAGP
is then set to ACO in or RCO in ennsnn

3. PROT - (PeSIT)(Odette) Connection time-out reached
without response (DISCTC parameter of the CFTPROT command). DIAGP is then
set to TIMEOUT

**Consequence:**

D status - RESTART

#### Diagi code : 452

PROT - (PeSIT) (Odette) Reception of a negative message
confirmation FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 453

PROT - (PeSIT) (Odette) Reception of a negative create
confirmation FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 454

PROT - (PeSIT) Reception of a negative select confirmation
FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 455

PROT - (PeSIT) (Odette) Reception of a negative deselect
confirmation FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 456

PROT - (PeSIT) Reception of a negative open confirmation
FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 457

Reception of a negative closed confirmation

#### Diagi code : 458

Reception of a negative read confirmation

#### Diagi code : 459

PROT - (PeSIT) Reception of a negative write confirmation
FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 460

PROT - (PeSIT) Reception of a negative end of transfer
confirmation FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 461

Received an abort with diagnostics

#### Diagi code : 462

No data sent on network

#### Diagi code : 463

Logon entry not recognized

#### Diagi code : 499

( ODETTE) CD okay

#### Diagi code : 500

Constant to add to a remote code

#### Diagi code : 600

FILE - (PeSIT) (Odette) Transfer aborted by the remote
end: file input/output error - PeSIT / Odette code: See [DIAGP.](../general_protocol_diagnostics)

**Consequence:**

H status- ABORT, EXECE

#### Diagi code : 604

FILE - (PeSIT) Transfer aborted by the remote end: file
opening error

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 605

FILE - (PeSIT) Transfer aborted by the remote end: file
closing error

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 610

FILE - (PeSIT) (Odette) Transfer aborted by the
remote end: the file to be read does not exist

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 611

FILE - (PeSIT) Transfer aborted by the remote end: insufficient
space to create the file

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 613

FILE - (PeSIT) Transfer aborted by the remote end: the
file to be created already exists

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 614

FILE - (PeSIT) Transfer aborted by the remote end: file
space full

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 620

PROT - (PeSIT) Transfer aborted by the remote end: counter
control error

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 621

PROT - (PeSIT) Transfer aborted by the remote end: interruption
by the operator

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 626

PROT - (PeSIT) (Odette) Transfer aborted by the remote
end: error in record length

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 635

FILE - (PeSIT) Transfer aborted by the remote end: file
access conflict

H status - ABORT, EXECE

#### Diagi code : 657

Errno

**Consequence:**

Too many open files in the CFTSFTP process.

#### Diagi code : 660

REC (PeSIT) - Error 660, ASE
205 on the requester side

**Consequence:**

H state - Transfer aborted by the remote end: no outstanding
transfer

#### Diagi code : 720

1. PROT - (PeSIT) Protocol abort by the remote end:
incorrect FPDU (transmission error)

2. PROT - (Odette) Protocol abort by the remote end:
negotiation error

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 722

PROT - (PeSIT) Protocol abort by the remote end: missing
PI

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 730

1. PROT - Protocol error

2. PROT - (PeSIT) Transfer aborted by the remote peer (i.e. the Partner Side)
due to protocol error - PeSIT code: See [DIAGP.](../general_protocol_diagnostics)

3. PROT - (Odette) Reception of an ESID FPDU

**Consequence:**

H status - ABORT, EXECE

#### Diagi code : 740

NET - (PeSIT) Transfer aborted by the remote end: time-out
- PeSIT code: 317

**Consequence:**

D status - RETRY

#### Diagi code : 850

PROT - (PeSIT) Protocol rejection by the remote end: authorization
problem

**Consequence:** H Status - ABORT, EXECE

#### Diagi code : 904

PROT - (PeSIT) Protocol rejection by the remote end: transfer
denied (open mode, authorizations for example)

**Consequence:** H status - ABORT, EXECE

#### Diagi code : 909

PROT - (PeSIT only) Protocol rejection by the remote end:
requester identifier unknown

**Consequence:** D status - RESTART

#### Diagi code : 916

PROT - (PeSIT only) Maximum number of transfers reached
at the partner end (MAXTRANS parameter)

**Consequence:**

D status - NEXT

See <a href="../../../concepts/about_parallel_transfers/multi_node_simultaneous_transfers" class="MCXref xref">Configure multi-node simultaneous transfers</a>.

#### Diagi code : 919

Restart context not available

**Consequence:** H status - ABORT, EXECE

#### Diagi code : 920

PROT - (PeSIT) Protocol rejection by the remote end: on
reception of a REPLY-type message, the partner does not find the transfer
concerned by this reply in its catalog

**Consequence:** D status - RESTART

#### Diagi code : 925

Call collect refused by the remote system

**Consequence:** No CAT

#### Diagi code : 928

Invalid caller number

**Consequence:** H status - ABORT, EXECE

#### Diagi code : 930

PROT - Partner is inactive on the server side

**Consequence:** ACT status

#### Diagi code : 933

Error in password management parameter RPASSWD or SPASSWD: non-authorized requester identification

#### Diagi code : 963

PROT - Protocol pre-connection phase rejected by the remote
end (PeSIT LOGON): LOGON string rejected

**Consequence:** K status - ABORT, EXECE

#### Diagi code : 970

PROT - Protocol pre-connection phase rejected by the remote
end (PeSIT LOGON): password expired

K status - ABORT, EXECE

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
| 48 | Unknown CA (<a href="http://en.wikipedia.org/wiki/Certificate_authority">Certificate authority</a>) |
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

