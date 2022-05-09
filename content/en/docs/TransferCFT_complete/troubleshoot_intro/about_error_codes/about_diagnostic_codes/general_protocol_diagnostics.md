{
    "title": "DIAGP protocol diagnostic codes",
    "linkTitle": "DIAGP - Protocol diagnostic codes",
    "weight": "310"
}This section presents the protocol diagnostic codes (DIAGP) for {{< TransferCFT/axwayvariablesComponentLongName  >}} transfers. This code provides information on the

transfer status or on the cause of an error depending on the protocol used.

<span id="Diagnostics_format"></span>

Diagnostics format
------------------

If you encounter an error, begin by matching the **Protocol** code with the format in the table below. You can then go to the corresponding section for details.

****Example protocol diagnostic code `L 02 045` (in the format` L HH HHH`)****

![](/Images/TransferCFT/temp_diagp.png)

DIAGP format and description

The table below presents the DIAGP formats.


| Format &lt;/th&gt;  | Meaning &lt;/th&gt;  |
| --- | --- |
| XXXXXXXX | Mnemonic code<br/> A &quot;Mnemonic&quot;-type DIAGP is a &quot;character string&quot; value providing information on the type of catalog entry or the status of the transfer associated with this entry. Some codes are specific to a single protocol. Please see the [Mnemonic DIAGP codes](#%22Mnemoni) table for details. |
| L HH HHH | Local rejection of network connection<br/> These codes correspond to network connection rejection diagnostics. The character L indicates a local rejection. H represents a hexadecimal digit.<br/> The two hexadecimal numbers respectively represent:<br/> • REASON: a reason code according to the network context<br/> • DIAGN: a diagnostic code according to the network context<br/> For the meaning of these codes, refer to the [Network codes](../network_codes) that correspond with the type of network used in the transfer.<br/> This DIAGP format is associated with the following internal diagnostics: |
| R HH HHH | Remote rejection of network connection<br/> These codes correspond to network connection rejection diagnostics. The character &quot;R&quot; indicates a remote rejection. H represents a hexadecimal digit.<br/> The two hexadecimal digits respectively represent:<br/> • REASON: a reason code according to the network context<br/> • DIAGN: a diagnostic code according to the network context<br/> For the meaning of these codes, refer to the section [Network codes](../network_codes) corresponding to the type of network used by the transfer.<br/> This DIAGP format is associated with the following internal diagnostics: |
| HHHHHHHH | In general for {{< TransferCFT/axwayvariablesComponentShortName  >}}, this format represents an error code specific to the operating system of the host computer and only relates to NON network resources (file access, task management, system services, etc.). |
| eNNsNN | (PeSIT) Unexpected event in the automaton<br/> These codes correspond to the diagnostics representing an unexpected event in the protocol automaton (where N represents a digit).<br/> The two numbers respectively represent:<br/> • The event code for all protocols. For the meaning of this code, refer to [Event Codes (All Protocols)](../event_codes#Event_codes_for_all_protocols).<br/> • The status code according to the protocol. For the meaning of this code, see Status Codes for the appropriate transfer protocol.<br/> This value should be given to the Technical support in the event of unexplained transfer difficulties.<br/> <br/>  |
| PDU iNN<br/> <br/> <br/> <br/>  | (PeSIT) FPDU does not conform to the specifications<br/> These codes correspond to the Transfer CFT diagnostics representing the reception of a PeSIT FPDU that does not conform to protocol specifications (where N represents a digit).<br/> The code NN specifies the FPDU build error code.<br/> For example, DIAGP = &quot;PDU i16&quot; means that the header of an FPDU received does non conform, because its size is greater than the length of the network message received.<br/> <br/> The &quot;XXX iNN&quot; format also falls into this category. For example, DIAGP = &quot;CRE i12&quot; means that a CREATE FPDU has been received with an unknown space reservation unit (PI 41).<br/> <br/> Please see the [FPDU Mnemonic Codes - PeSIT Protocol](#FPDU) table for details. |
| XXX NNN | (PeSIT) Received FPDU contains a diagnostic message<br/> These codes correspond to the Transfer CFT diagnostics representing the reception of an FPDU with an error diagnostic code, where X represents a character and N a digit:<br/> • NNN indicates the PeSIT protocol diagnostic in the FPDU.<br/> • XXX represents the mnemonic code of the received FPDU - PeSIT protocol.<br/> <br/> Please see the [PeSIT NNN value descriptions](#XXX%C2%A0NNN) table for details. |
| NNN HHHH | (ODETTE) Received message contains a diagnostic message<br/> These codes corresponding to the reception of an FPDU with an error diagnostic code. H represents a hexadecimal digit and N a digit:<br/> • NNN is a numeric value corresponding to the ODETTE protocol diagnostic code. Please see the [ODETTE NNN value descriptions](#ODETTE_Protocol__Diagnostic_Codes) table for details.<br/> • HHHH is a hexadecimal value corresponding to the {{< TransferCFT/axwayvariablesComponentShortName  >}} numeric code ODETTE protocol. See also{{< TransferCFT/axwayvariablesComponentShortName  >}} Numeric Codes ODETTE Protocol. |
| XXX HHHH | (ODETTE) Negotiation or send error<br/> These codes identifying an FPDU negotiation or send error in the ODETTE protocol (where H represents a hexadecimal digit and X a letter):<br/> • XXX is a Transfer CFT mnemonic code - ODETTE protocol. This is an alphanumeric value describing the origin of the anomaly or phase during which it occurred. See also, {{< TransferCFT/axwayvariablesComponentShortName  >}} Mnemonic Codes ODETTE Protocol.<br/> • HHHH is a {{< TransferCFT/axwayvariablesComponentShortName  >}} numeric code - ODETTE protocol. This is a hexadecimal value corresponding to the internal protocol code. See also &quot;[}} ODETTE](../../../../protocols_start_here/start_here_odette)&quot; |


<span id=""Mnemoni"></span>

"Mnemonic" DIAGP codes
----------------------


| Code  | Protocol  | Meaning  |
| --- | --- | --- |
| ABOI_CD | ODETTE | CD send following reception of an ABORT indication (case of a RECV IDF=* command) |
| ABORT |   | {{< TransferCFT/axwayvariablesComponentShortName  >}} transfer abort request |
| ABORT_I | ODETTE | ABORT caused by the protocol engine, following detection of an error |
| CALL OUT |   | Call not made within the authorized call period (see OMINTIME/OMAXTIME - OMINDATE/OMAXDATE parameters of commands CFTPART, etc.) |
| CATUPDT |   | Catalog update error (at sync point) |
| CD_ODT | ODETTE | (information) Indication of a &quot;generic&quot; entry for file reception |
| CLOSE_RN | ODETTE | Reception of a &quot;negative CLOSE RESP&quot; interaction from the file access task: error closing the file sent |
| CREA_RN | ODETTE | Reception of a &quot;negative CREATE RESP&quot; interaction from the file access task: error creating the receive file |
| COLLECT |   | Indication of a &quot;generic&quot; entry for file collection. This entry does not correspond to an actual transfer |
| CP |   | Transfer performed without the compression option. This can be caused by the REQUESTER or SERVER partner as compression is negotiated |
| NONE |   |   |
| CP nn% |   | Transfer performed with the compression option. The compression rate is then displayed. This rate expresses the number of bytes to be sent in relation to the number of bytes actually sent. The number of bytes to be sent may be different from the number of bytes in the file if the {{< TransferCFT/axwayvariablesComponentShortName  >}} truncates or pads records, depending on the parameter settings |
| DAY CYC |   | Indication of a &quot;generic&quot; entry for cyclic transfers (days). This entry does not correspond to an actual transfer |
| DIFFUS |   | Indication of a &quot;generic&quot; entry for file broadcasting. This entry does not correspond to an actual transfer |
| DSEL_CN | ODETTE | Reception of a &quot;negative DSELECT RESP&quot; interaction from the file access task: error deselecting the sent file |
| END_TFIL | ODETTE | Sending of a &quot;RELEASE IND&quot; interaction to the file access task in order to stop the task (information) |
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
| HOLD |   | Indication of an &quot;on-hold&quot; transfer, waiting for a reception request |
| INACT |   | Transfer refused due to partner inactivity |
| INV XFER |   | Message transfer unauthorized with this protocol |
| LDT_TXT |   | Rusize is greater than MAXRUSIZE in &quot;T&quot; |
| LIST_FI |   | Indication of a &quot;generic&quot; entry for the file list. This entry does not correspond to an actual transfer. |
| MALLOC |   | Cannot allocate the working area required for the transfer dynamically |
| MAXCNX |   | The number of connection(s) for the resource (MAXCNX parameter of the CFTNET command) has already been reached |
| MAXCV |   | The number of connection(s) for the partner has already been reached |
| MAXRETRY |   | Maximum number of retries exceeded (see the RETRY* parameters) |
| MAXRST |   | Maximum number of restarts for a protocol exceeded (see the RESTART parameter of the CFTPROT command) |
| MAXTASK |   | Maximum number of tasks exceeded (see the MAXTASK parameter of the CFTPARM command) |
| MAXTRANS |   | Maximum number of simultaneous transfers exceeded (see the MAXTRANS parameter of the CFTPARM command) |
| MIN CYC |   | Indication of a &quot;generic&quot; entry for cyclic transfers (minutes). This entry does not correspond to an actual transfer |
| MON CYC |   | Indication of a &quot;generic&quot; entry for cyclic transfers (months). This entry does not correspond to an actual transfer. |
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
| RECV ALL |   | Indication of a &quot;generic&quot; entry for global transfer receptions on hold. This entry does not correspond to an actual transfer. |
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


<span id="FPDU"></span>

FPDU Mnemonic Codes - PeSIT Protocol
------------------------------------


| XXX &lt;/th&gt;  | FPDU &lt;/th&gt;  | Definition &lt;/th&gt;  |
| --- | --- | --- |
| ABO  | ABORT  | Sudden connection interruption  |
| ACF  | AckCRF  | File closing confirmation  |
| ACO  | ACONNECT  | Connection confirmation  |
| ACR  | AckCREATE  | File creation confirmation  |
| ADS  | AckDESELECT  | File deselect confirmation  |
| AMG  | AckMSG  | Message confirmation  |
| AOF  | AckORF  | File opening confirmation  |
| ARD  | AckREAD  | Read confirmation  |
| ASE  | AckSELECT  | File selection confirmation  |
| ATE  | AckTRANS.END  | End of transfer confirmation  |
| AWR  | AckWRITE  | Write confirmation  |
| CON  | CONNECT  | Connection request  |
| CRE  | CREATE  | File creation  |
| CRF  | CRF  | File closing  |
| DMG  | MSGDM  | Message start  |
| DSE  | DESELECT  | File deselect  |
| DTE  | TRANS.END  | End of transfer  |
| IDT  | IDT  | Transfer interruption  |
| MSG  | MSG  | Message transmission  |
| RCO  | RCONNECT  | Connection refusal  |
| SEL  | SELECT  | File selection  |


<span id="XXX NNN"></span>

NNN values for the PeSIT protocol
---------------------------------

The PeSIT protocol uses two PIs to carry diagnostic messages: PI 2 and
PI 29.

{{< TransferCFT/axwayvariablesComponentShortName  >}} is responsible for PI 29, which is valid only in
version E. {{< TransferCFT/axwayvariablesComponentShortName  >}} uses PI 29 to carry a clearform message describing
the error. This message is not seen by the user.

The severity and nature of an error is specified by PI 2, which is valid for all profiles.


| Error Code &lt;/th&gt;  | FPDU &lt;/th&gt;  | Meaning &lt;/th&gt;  |
| --- | --- | --- |
| 100 | RESYNC | Transmission error (invalid CRC) |
| 139 |   | Invalid file attributes |
| 200 | AckCREATE AckSELECT | Insufficient file characteristics (insufficient file parameters) |
| 201 | AckCREATE AckSELECT | System resources currently insufficient |
| 202 | AckCREATE AckSELECT | User resources currently insufficient |
| 203 | AckCREATE AckSELECT | Non-priority transfer |
| 204 | AckCREATE | File already exists |
| 205 | AckSELECT | File does not exist |
| 206 | AckCREATE | Available disk space smaller than file size |
| 207 | AckSELECT | File in use |
| 209 | AckMSG | Message type not supported |
| 210 | AckORF | Negotiation failure |
| 211 | AckORF | Cannot open file |
| 212 | AckCRF | Cannot close file |
| 213 | IDT AckWRITE | Fatal file input/output error |
| 214 | AckREAD | Restart point negotiation failure |
| 215 | IDT | Error specific to the system |
| 216 | IDT | Operator-requested premature abort |
| 217 | IDT | Too many synchronization points without acknowledgment |
| 218 | IDT | Cannot re-synchronize |
| 219 | IDT | File space exceeded |
| 220 | IDT | Record length greater than that declared |
| 221 | IDT | Time-out |
| 222 | IDT | Too much data without synchronization point |
| 223 | AckTRANSFER.<br/> END AckDESELECT | Abnormal end of transfer |
| 224 | AckTRANSFER.END | Declared file size smaller than actual size |
| 226 | AckCREATE AckSELECT | Transfer denied |
| 228 |   | File type not supported |
| 229 |   | File type incompatible with transfer direction |
| 230 |   | File type incompatible with application |
| 231 |   | Transfer number not unique |
| 232 |   | Coding incompatible with file type |
| 233 |   | Restart context not available |
| 234 |   | Data entity size inconsistent with record |
| 235 |   | Record format incompatible with file type |
| 236 |   | Record length incompatible with file type |
| 237 |   | Incorrect client identifier |
| 238 |   | Non-authorized client |
| 239 |   | Non-authorized client / requester / file type combination |
| 240 |   | Client not authorized on this server |
| 241 |   | Bank not authorized on this server |
| 242 |   | Old password invalid |
| 243 |   | New password invalid |
| 245 |   | Incorrect file length |
| 246 |   | No file of this type for this client |
| 247 |   | No file of this type on this server |
| 248 |   | Identification type not supported |
| 249 |   | Nominative reference not supported |
| 250 |   | File already transferred |
| 251 |   | Reference type not supported |
| 252 |   | Start date too old |
| 253 |   | Incorrect date(s) |
| 254 |   | File service closed |
| 299 | All acknowledgment FPDUs | Other fatal errors |
| 300 | RelCONNECT | Congestion of local communication system |
| 301 | RelCONNECT | Identification of called party unknown |
| 302 | RelCONNECT | Called party not attached to a Service Access Point (SAP) |
| 303 | RelCONNECT | Maximum number of connections reached |
| 304 | RelCONNECT AckCREATE AckSELECT ABORT | Non-authorized requester identification |
| 305 | RelCONNECT | SELECT negotiation failure |
| 306 | RelCONNECT | RESYN negotiation failure |
| 307 | RelCONNECT | SYNC negotiation failure |
| 308 | RelCONNECT | Version number not supported |
| 309 | RelCONNECT ABORT | Too many connections already in progress |
| 310 | ABORT | Network incident |
| 311 | ABORT | Remote PeSIT protocol error |
| 312 | RelCONNECT (if partner inactive) ABORT/RELEASE | Shutdown of service requested by the user |
| 313 | ABORT/RELEASE | Connection closed due to inactivity |
| 314 | ABORT/RELEASE | Unused connection closed to open a new connection |
| 315 | ABORT | Negotiation failure |
| 316 | ABORT/RELEASE | Connection closed by the administrator |
| 317 | ABORT | Connection time-out |
| 318 | ABORT | Mandatory PI missing or invalid |
| 319 | ABORT | Incorrect number of records or bytes |
| 320 | ABORT | Excessive number of re-synchronizations |
| 321 | RelCONNECT AckCREATE AckSELECT | Call backup number |
| 322 | RelCONNECT AckCREATE AckSELECT | Call back later |
| 323 |   | Incompatible CRC / connection mode |
| 324 |   | Incorrect requester identifier |
| 325 |   | Old password invalid |
| 326 |   | New password invalid |
| 327 |   | Receive access temporarily closed |
| 328 |   | Receive access not supported |
| 329 |   | Send access temporarily closed |
| 330 |   | Send access not supported |
| 331 |   | Excessive time-out value |
| 332 |   | Write not negotiated |
| 333 |   | Read not negotiated |
| 334 |   | Reverse charging refused |
| 335 |   | Invalid calling party number |
| 336 |   | Server date and time refused |
| 399 | All ABORT acknowledgment FPDUs | Other fatal errors |


<span id="ODETTE_Protocol__Diagnostic_Codes"></span>

NNN values for the ODETTE protocol
----------------------------------

These codes are specific to the ODETTE protocol and correspond to the
"ODETTE diagnostic code" transmitted by the protocol.

The values of these codes consist of the diagnostic code (two digits)
to which the {{< TransferCFT/axwayvariablesComponentShortName  >}} adds 100 or 200 depending on the protocol phase concerned.

- Values between 100 and 199 correspond to the "SFNA" (Start
    File Negative Answer) and "EFNA" (End File Negative Answer)
    protocol messages.
- Values between 200 and 299 correspond to the "ESID" (End Session
    IDentification) protocol message.
- This code forms the "NNN NNNN"-type DIAGP protocol diagnostic
    code. Values are expressed in decimal.


| Error code  | Description  |
| --- | --- |
| 101 | File does not exist |
| 102 | Target site does not exist for the file |
| 103 | Source site does not exist for the file |
| 104 | File format not supported |
| 105 | Record length error (length not supported) |
| 106 | File too big |
| 110 | Invalid number of records |
| 111 | Invalid number of characters |
| 112 | Fatal file input/output error (file access method problem) |
| 113 | File already exists |
| 199 | Non-referenced errors |
| 201 | NSDU (Network System Data Unit) not recognized (faulty header) |
| 202 | Protocol error (reception of a invalid NSDU) |
| 203 | Requestee identification not known |
| 204 | Requester identifier not authorized or password incorrect |
| 205 | Congestion of local communication system (sudden shutdown of communication system) |
| 206 | FPDU received with missing parameter or unexpected value |
| 207 | Invalid NSDU size received |
| 208 | User resources currently insufficient |
| 209 | End of time-out |
| 210 | Incorrect number of records. The number transported by the EFID FPDU does not correspond to the number of received records counted by the {{< TransferCFT/axwayvariablesComponentShortName  >}} (F- or V-format files) |
| 211 | Number of characters incorrect. The number transported by the EFID FPDU does not correspond to the number of received characters counted by the {{< TransferCFT/axwayvariablesComponentShortName  >}} (T- or U-format files) |

