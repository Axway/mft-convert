---

    title: General Transfer CFT protocol diagnostics
    linkTitle: General protocol diagnostics
    weight: 460

---
<span id="Diagnostics_format"></span>

## Diagnostics format

The following table defines the DIAGP formats.

<span class="autonumber"></span>DIAGP Formats


| Format &lt;/th&gt;  | Meaning &lt;/th&gt;  |
| --- | --- |
| XXXXXXXX | Mnemonic code |
| L HH HHH | Local rejection of network connection |
| R HH HHH | Remote rejection of network connection |
| HHHHHHHH | Error in the system or network software at operating system level |
| eNNsNN | (PeSIT) Unexpected event in the automaton |
| PDU iNN | (PeSIT) FPDU does not conform to the specifications |
| XXX NNN | (PeSIT) Received FPDU contains a diagnostic message |
| NNN HHHH | (ODETTE) Received message contains a diagnostic message |
| XXX HHHH | (ODETTE) Negotiation or send error |


## "Mnemonic"-Type DIAGP Codes

A "Mnemonic"-type DIAGP is a "character string"
value providing information on the type of catalog entry or the status
of the transfer associated with this entry. Some codes are specific to
a single protocol.

<span class="autonumber"></span>Specific codes


| Code  | Protocol  | Meaning  |
| --- | --- | --- |
| ABOI_CD | ODETTE | CD send following reception of an ABORT indication (case of a RECV IDF=* command) |
| ABORT |   | {{< TransferCFT/axwayvariablesComponentShortName  >}} transfer abort request |
| ABORT_I | ODETTE | ABORT caused by the protocol engine, following detection of an error |
| CALL OUT |   | Call not made within the authorized call period (see OMINTIME/OMAXTIME - OMINDATE/OMAXDATE parameters of commands CFTPART, etc.) |
| CATUPDT |   | Catalog update error (at sync point) |
| CD_ODT | ODETTE | (information) Indication of a "generic" entry for file reception |
| CLOSE_RN | ODETTE | Reception of a "negative CLOSE RESP" interaction from the file access task: error closing the file sent |
| CREA_RN | ODETTE | Reception of a "negative CREATE RESP" interaction from the file access task: error creating the receive file |
| COLLECT |   | Indication of a "generic" entry for file collection. This entry does not correspond to an actual transfer |
| CP |   | Transfer performed without the compression option. This can be caused by the REQUESTER or SERVER partner as compression is negotiated |
| NONE |   |   |
| CP nn% |   | Transfer performed with the compression option. The compression rate is then displayed. This rate expresses the number of bytes to be sent in relation to the number of bytes actually sent. The number of bytes to be sent may be different from the number of bytes in the file if the {{< TransferCFT/axwayvariablesComponentShortName  >}} truncates or pads records, depending on the parameter settings |
| DAY CYC |   | Indication of a "generic" entry for cyclic transfers (days). This entry does not correspond to an actual transfer |
| DIFFUS |   | Indication of a "generic" entry for file broadcasting. This entry does not correspond to an actual transfer |
| DSEL_CN | ODETTE | Reception of a "negative DSELECT RESP" interaction from the file access task: error deselecting the sent file |
| END_TFIL | ODETTE | Sending of a "RELEASE IND" interaction to the file access task in order to stop the task (information) |
| ERR INC | ODETTE | Transfer ABORT - Reason not specified |
| ERRCOMP |   | File compression error - the compression type requested is incompatible with the file data |
| ERR LREC |   | Error sending or receiving the file data. {{< TransferCFT/axwayvariablesComponentShortName  >}} detects an invalid length for the data read or to be written |
| ERR_NCAR |   | Error in the number of characters sent |
| ERR_NREC |   | Error in the number of records |
| ERRPROT |   | Protocol error when switching directions |
| ERR_UFMT | ODETTE | Internal error when deformatting the received FPDU |
| EVT | ODETTE | Reception of an unexpected event in the current phase of the protocol automaton |
| ERRPASSW | ODETTE | Invalid partner LOGON password |
| EXAERR |   | Processing error in the directory EXIT task |
| EXARJT |   | Connection refusal via the directory EXIT task |
| EXATASK |   | Load error for the directory EXIT task |
| EXAWRSP |   | Waiting for a response from the directory EXIT task |
| EXIT |   | EXIT task initialization problem |
| FCON_RN | ODETTE | Session parameter negotiation error, implying a connection rejection |
| FORMAT |   | FPDU formatting error |
| HOLD |   | Indication of an "on-hold" transfer, waiting for a reception request |
| INACT |   | Transfer refused due to partner inactivity |
| INV XFER |   | Message transfer unauthorized with this protocol |
| LDT_TXT |   | Rusize is greater than MAXRUSIZE in "T" |
| LIST_FI |   | Indication of a "generic" entry for the file list. This entry does not correspond to an actual transfer. |
| MALLOC |   | Cannot allocate the working area required for the transfer dynamically |
| MAXCNX |   | The number of connection(s) for the resource (MAXCNX parameter of the CFTNET command) has already been reached |
| MAXCV |   | The number of connection(s) for the partner has already been reached |
| MAXRETRY |   | Maximum number of retries exceeded (see the RETRY* parameters) |
| MAXRST |   | Maximum number of restarts for a protocol exceeded (see the RESTART parameter of the CFTPROT command) |
| MAXTASK |   | Maximum number of tasks exceeded (see the MAXTASK parameter of the CFTPARM command) |
| MAXTRANS |   | Maximum number of simultaneous transfers exceeded (see the MAXTRANS parameter of the CFTPARM command) |
| MIN CYC |   | Indication of a "generic" entry for cyclic transfers (minutes). This entry does not correspond to an actual transfer |
| MON CYC |   | Indication of a "generic" entry for cyclic transfers (months). This entry does not correspond to an actual transfer. |
| MSG_NOAU |   | EERP send not authorized |
| MSG_RN | ODETTE | The EERP message has not been acknowledged by the partner |
| MYSELF |   | The target partner is the local site (CFTPARM PART) |
| NO AUTH |   | Non-authorized partner or file (see the AUTH parameter, CFTAUTH command, and the CFTPART security parameters) |
| NO NEW |   | Receive file already exists for a reception request: (CFT)RECV FDISP=NEW |
| NO OLD |   | Receive file does not exist for a reception request: (CFT)RECV FDISP=OLD |
| NO OPEN |   | Transfer in open mode not authorized (see the OPEN parameter of the partner's CFTPART command) |
| NO PARM |   | Error due to incorrect {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter settings |
| NO PART |   | Partner does not exist (no CFTPART command for this partner identifier) |
| NO PROT |   | Protocol does not exist (no CFTPROT command for this protocol identifier) |
| NO TURN | ODETTE | The {{< TransferCFT/axwayvariablesComponentShortName  >}} cannot hand over to the partner (CD) as the partner handed over during the previous exchange |
| NO XLATE |   | CFTXLATE command not found for this transfer direction, nor for the source and target alphabets |
| NOCALL |   | The partner cannot be called |
| NOCTX |   | Message for a missing or inactive context |
| NOSELECT |   | With the PeSIT protocol, SIT profile, the file selection request (RECV command) is not authorized |
| NO_FILE | ODETTE | There are no more files to be sent (information) |
| NPART |   | REQUESTER partner mismatch with SERVER partner in SIT profile. Strict naming and consistency rules are imposed both by {{< TransferCFT/axwayvariablesComponentShortName  >}} and by the PESIT standard. |
| N_REL_I | ODETTE | Reception of a network outage indication |
| OPER |   | Transfer interrupt request by the operator |
| OUT TIME |   | The transfer has exceeded the authorized time slot (MAXDATE, MAXTIME parameters of the command) |
| RECV ALL |   | Indication of a "generic" entry for global transfer receptions on hold. This entry does not correspond to an actual transfer. |
| RELEASE |   | Unexpected network outage, caused by the remote partner or the network |
| RESTART0 |   | Transfer interruption (it will be restarted at the beginning of the file) |
| RESTARTF |   | Transfer interruption (it will be restarted at the restart point) |
| RTO |   | {{< TransferCFT/axwayvariablesComponentShortName  >}} time-out during the transfer phase. This time-out corresponds to the RTO parameter of the CFTPROT command |
| R_PASSW | ODETTE | The password given by the partner does not correspond to the parameter settings (CFTPART command). |
| SFNA | ODETTE | Reception of an SFNA FPDU corresponding to a session parameter negotiation problem |
| SROUT |   | The partner cannot be called (SROUT parameter of the CFTPROT command) |
| SSY TFIL |   | Error sending data to CFTTFIL |
| SYPOST |   | Communication error between {{< TransferCFT/axwayvariablesComponentShortName  >}} and the directory EXIT task |
| TFIL | ODETTE | Reception of a transfer ABORT request originating from the file access task |
| TIMEOUT |   | Monitoring time-out during the connection phase, due particularly to a missing response to a pre-connection (LOGON) string or a CONNECT FPDU. With the SIT profile, there is no pre-connection phase |
| TSK_EXIT |   | Error initializing a file EXIT task. |
| VRESID | ODETTE | Transfer ABORT caused by the reception of an ESID FPDU (partner session termination request) |
| WF RENAM |   | Cannot rename the temporary file (WFNAME parameter) in FNAME, at the end of the transfer |


## "L HH HHH"-Type DIAGP Codes

"L HH HHH"-type DIAGP codes correspond to network connection
rejection diagnostics. The character L indicates a local rejection. H
represents a hexadecimal digit.

The two hexadecimal numbers respectively represent:

- REASON:
    a reason code according to the network context
- DIAGN:
    a diagnostic code according to the network context

For the meaning of these codes, refer to the Network
Codes that correspond with the type of network used in the transfer.

## "R HH HHH"-Type DIAGP Codes

"R HH HHH"-type DIAGP codes correspond to network connection
rejection diagnostics. The character "R" indicates a remote
rejection. H represents a hexadecimal digit.

The two hexadecimal digits respectively represent:

- REASON:
    a reason code according to the network context
- DIAGN:
    a diagnostic code according to the network context

For the meaning of these codes, refer to the section Network
Codes corresponding to the type of network used by the transfer.

<span id="HHHHHHHH___Type_DIAGP_Codes"></span>

## "HHHHHHHH"-Type DIAGP Codes

"HHHHHHHH"-type DIAGP codes correspond to the error diagnostics
specific to the network or system access software.

They are the "CS" or "NCS" codes.

Refer to
the manufacturer's documentation (system code or network codes, depending
on the type of occurrence).

<span id="eNNsNN___Type_PeSIT_DIAGP_Codes"></span>

## "eNNsNN"-Type PeSIT DIAGP Codes

"eNNsNN"-type PeSIT DIAGP codes correspond to the diagnostics
representing an unexpected event in the protocol automaton (where N represents
a digit).

The two numbers respectively represent:

- The
    event code for all protocols

For the meaning of this code, refer to [Event Codes (All
Protocols)](../event_codes#Event_codes_for_all_protocols).

- The
    status code according to the protocol

For the meaning of this code, see
Status Codes for the appropriate
transfer protocol.

<span id="PDU_iNN___Type_PeSIT_DIAG_Codes"></span>

## "PDU iNN"-Type PeSIT DIAGP Codes

"PDU iNN"-type PeSIT DIAGP codes correspond to the Transfer
CFT diagnostics representing the reception of a PeSIT FPDU that does not
conform to protocol specifications (where N represents a digit).

The code NN specifies the FPDU build error code.

For the meaning of this code, see [FPDU
Build Error Codes (PESIT).](#)

<span id="XXX_NNN___Type_PeSIT_DIAG_Codes"></span>

## "XXX NNN"-Type PeSIT DIAGP Codes

"XXX NNN"-type PeSIT DIAGP codes correspond to the Transfer
CFT diagnostics representing the reception of an FPDU with an error diagnostic
code, where X represents a character and N a digit:

- NNN
    indicates the PeSIT protocol diagnostic in the FPDU
    -   For the meaning of this code, refer to the section
        PESIT Protocol Diagnostic Code.
- XXX
    represents the mnemonic code of the received FPDU - PeSIT protocol
    -   For the meaning of this code, refer to the section
        <span class="italic_in_para">FPDU Mnemonic Codes PeSIT Protocol.</span>

<span id="NNN_HHHH_Type_ODETTE_DIAGP_Codes"></span>

## "NNN HHHH"-Type ODETTE DIAGP Codes

{{< TransferCFT/axwayvariablesComponentShortName  >}} diagnostic codes corresponding to the reception of an FPDU
with an error diagnostic code. H represents a hexadecimal digit and N
a digit:

- NNN
    is a numeric value corresponding to the ODETTE protocol diagnostic code

For the meaning of this code, refer to the section
[Protocol Diagnostic Codes](../protocol_diagnostic_codes).

- HHHH
    is a hexadecimal value corresponding to the {{< TransferCFT/axwayvariablesComponentShortName >}} numeric code
    ODETTE protocol

See also{{< TransferCFT/axwayvariablesComponentShortName  >}} Numeric Codes ODETTE Protocol.

<span id="XXX_HHHH_Type_ODETTE_DIAGP_Codes"></span>

## "XXX HHHH"-Type ODETTE DIAGP Codes

{{< TransferCFT/axwayvariablesComponentShortName  >}} diagnostic code identifying an FPDU negotiation or send
error in the ODETTE protocol (where H represents a hexadecimal digit and
X a letter):

- XXX is a Transfer
    CFT mnemonic code - ODETTE protocol. This is an alphanumeric
    value describing the origin of the anomaly or phase during which it occurred.

See also,
{{< TransferCFT/axwayvariablesComponentShortName  >}} Mnemonic Codes ODETTE Protocol.

- HHHH
    is a {{< TransferCFT/axwayvariablesComponentShortName >}} numeric code - ODETTE protocol. This is a hexadecimal
    value corresponding to the internal protocol code.

See also
"[{{< TransferCFT/axwayvariablesComponentShortName  >}} ODETTE](../../../protocols_start_here/start_here_odette)"
