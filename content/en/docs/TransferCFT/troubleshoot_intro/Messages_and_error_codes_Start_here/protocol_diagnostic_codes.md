---

    title: Protocol  diagnostic codes
    linkTitle: Protocol diagnostic codes
    weight: 500

---
<span id="PeSIT_Protocol_Diagnostic_Codes"></span>

## PeSIT protocol diagnostic codes

The PeSIT protocol uses two PIs to carry diagnostic messages: PI 2 and
PI 29.

{{< TransferCFT/axwayvariablesComponentShortName  >}} is responsible for PI 29, which is valid only in
version E. {{< TransferCFT/axwayvariablesComponentShortName  >}} uses PI 29 to carry a clearform message describing
the error. This message is not seen by the user.

## Diagnostic protocol field format

This section presents the diagnostic protocol fields format for the PeSIT
protocol. In PeSIT protocol, the DIAGP or Diag Protocol
fields can be organized in several ways:

- "HHHHHHHH"
    format

H represents a hexadecimal digit.

In general for {{< TransferCFT/axwayvariablesComponentShortName  >}}, this format represents
an error code specific to the operating system of the host computer and
only relates to NON network resources (file access, task management, system
services, etc.).

For PeSIT, this field can also represent an error
code specific to the method of access to the network of the system concerned.
In this case, the code is associated with the following internal diagnostics:

- 301: network
    addressing error in connection phase
- 302: network
    error (cut, timeout) during the data exchange phase
- 303: network
    parameter error in connection phase
- "eNNsNN"
    format

N represents a decimal digit. An unexpected event
has arisen in the protocol controller.

This value should be given to the Technical support
in the event of unexplained transfer difficulties.

- "PDU
    iNN" format

N represents a decimal digit. Reception of an FPDU
not conforming to the protocol specifications. The meanings associated
with this message are explained further on in this book. For example,
DIAGP = "PDU i16" means that the header of an FPDU received
does non conform, because its size is greater than the length of the network
message received.

- "XXX
    NNN" format

X represents an alphabetical character.

N represents a decimal digit. Reception of an FPDU
featuring an error diagnostic (PI 2) conforming to the PeSIT protocol.
XXX represents the FPDU received. NNN represents the PeSIT
reason code (the two least significant bytes sent in the PI 2 code
of FPDUs). The possible values for the PeSIT reason code are given
further on in this appendix. The possible values for XXX are given
in the following table.

- "XXX
    iNN" format

X represents an alphabetical character.  
N represents a decimal digit.  
Reception of an FPDU having a parameter not conforming to the protocol
specifications. As for the previous format, XXX represents the
FPDU received. NN indicates an error code which is used to refine
the problem detected. The possible values for XXX are given in
the following table.  
For example, DIAGP = "CRE i12" means that a CREATE FPDU
has been received with an unknown space reservation unit (PI 41).

<span class="autonumber"></span> "XXX
iNN" format values


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


- "Vxxxxxxx"
    format

The mnemonic Vxxxxxxx represents a protocol
event.

This value should be given to the Technical
support in the event of unexplained transfer difficulties.

<span class="autonumber"></span>Vxxxxxxx format: possible
protocol events


| Vxxxxxxx  | Definition  |
| --- | --- |
| "VFxxxxx"  | File transfer event (eg: VFCAND)  |
| "VLOGxxxx"  | Event relative to the pre-connection message (eg: VLOGRP)  |
| "VNxxxxxx"  | Network event (eg: VNRELI)  |
| "VRxxxxxx"  | FPDU reception event (eg: VRABORT)  |
| "VVxxxxxx"  | Internal event (eg: VVTIMO)  |
| "VIxxxxxx"  | Induced internal event (eg: VIABORT)  |


<span class="autonumber"></span>Error code descriptions


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


### PeSIT reason code

QQQ\_QQQ\_QQQ just split big table into 3

#### Diagnostics imposing a re-synchronization 


| PeSIT reason code&lt;/th&gt;  | Description&lt;/th&gt;  | {{< TransferCFT/axwayvariablesComponentShortName  >}} internal diagnostic  | Service item concerned&lt;/th&gt;  |
| --- | --- | --- | --- |
| 100  | Transmission error  | 720  | F.RESTART  |


#### Diagnostics imposing a restart 


| PeSIT reason code&lt;/th&gt;  | Description&lt;/th&gt;  | {{< TransferCFT/axwayvariablesComponentShortName  >}} internal diagnostic  | Service item concerned&lt;/th&gt;  |
| --- | --- | --- | --- |
| 200  | File characteristics insufficient  | 730  | F.CREATE<br /> F.SELECT  |
| 201  | System resources temporarily insufficient  | 916  | F.CREATE<br /> F.SELECT  |
| 202  | User resources temporarily insufficient  | 730  | F.CREATE<br /> F.SELECT  |
| 203  | Transfer not overriding  | 730  | F.CREATE<br /> F.SELECT  |
| 204  | File already exists  | 613  | F.CREATE<br /> F.SELECT  |
| 205  | File does not exist  | 610  | F.CREATE<br /> F.SELECT  |
| 206  | Reception of the file will cause an overflow of the disk quota  | 611  | F.CREATE<br /> F.SELECT  |
| 207  | File occupied  | 635  | F.CREATE<br /> F.SELECT  |
| 208  | File too old  | 730  | F.CREATE<br /> F.SELECT  |
| 209  | Message of this type not accepted on the reference installation  | 920  | F.CREATE<br /> F.SELECT  |
| 210  | Presentation context negotiation failure  | 730  | F.OPEN  |
| 211  | Not possible to open file  | 604  | F.OPEN  |
| 212  | Not possible to close file normally  | 605  | F.CLOSE  |
| 213  | Inhibiting input/output error  | 600  | F.READ<br /> F.WRITE<br /> F.DATA.END<br /> F.CANCEL  |
| 214  | Restart point negotiation failure  | 730  | F.READ<br /> F.DATA.END<br /> F.CANCEL  |
| 215  | Specific system error  | 730  | F.DATA.END<br /> F.CANCEL  |
| 216  | Intentional premature halt  | 621  | F.DATA.END<br /> F.CANCEL  |
| 217  | Too many synchronization points not acknowledged  | 730  | F.DATA.END<br /> F.CANCEL  |
| 218  | Re-synchronization not possible  | 730  | F.DATA.END<br /> F.CANCEL  |
| 219  | File space full  | 614  | F.DATA.END<br /> F.CANCEL  |
| 220  | Article longer than expected  | 626  | F.DATA.END<br /> F.CANCEL  |
| 221  | Expected end of transmission time  | 730  | F.DATA.END<br /> F.CANCEL  |
| 222  | Too much data without synchronization points  | 730  | F.DATA.END<br /> F.CANCEL  |
| 223  | Abnormal end of transfer  | 730  | F.TRANSFER.END<br /> F.DESELECT  |
| 224  | The size of the file sent is greater than the one announced in F.CREATE  | 600  | F.TRANSFER.END<br /> F.DESELECT  |
| 225  | Workstation application congested: the file has effectively been received but SCRS has not been able to give it to the workstation application  | 600  | F.TRANSFER.END<br /> F.DESELECT  |
| 226  | Transfer refusal  | 904  | F.CREATE<br /> F.SELECT  |
| 299  | Other  | 730  | F.CREATE<br /> F.SELECT<br /> F.OPEN<br /> F.CLOSE<br /> F.READ<br /> F.WRITE<br /> F.DATA.END<br /> F.CANCEL<br /> F.TRANSFER.END<br /> F.DESELECT<br /> F.RESTART  |


#### Diagnostics imposing a reconnection 


| PeSIT reason code&lt;/th&gt;  | Description&lt;/th&gt;  | {{< TransferCFT/axwayvariablesComponentShortName  >}} internal diagnostic  | Service item concerned&lt;/th&gt;  |
| --- | --- | --- | --- |
| 300  | Local communication system congested  | 730  | F.CONNECT  |
| 301  | Identification requested not known  | 909  | F.CONNECT  |
| 302  | Requested system not attached to a SSAP  | 730  | F.CONNECT  |
| 303  | Remote communication system congested<br /> (too many connections)  | 730  | F.CONNECT  |
| 304  | Requested identification not authorized (security)  | 730  | F.CONNECT<br /> F.ABORT<br /> F.CREATE<br /> F.ABORT  |
| 305  | Negotiation failure (SELECT)  | 730  | F.CONNECT  |
| 306  | Negotiation failure (RESYN)  | 730  | F.CONNECT  |
| 307  | Negotiation failure (SYNC)  | 730  | F.CONNECT  |
| 308  | Version number not supported  | 730  | F.CONNECT  |
| 309  | Too many connections already in process for this processing centre  | 916  | F.CONNECT<br /> F.ABORT  |
| 310  | Network incident  | 802  | F.ABORT  |
| 311  | Remote PeSIT protocol error  | 730  | F.ABORT  |
| 312  | Service closure requested by the user  | 730  | F.RELEASE<br /> F.ABORT  |
| 313  | Connection broken at the end of the TD inactivity interval  | 730  | F.RELEASE<br /> F.ABORT  |
| 314  | Connection used to host a new connection  | 730  | F.RELEASE<br /> F.ABORT  |
| 315  | Negotiation failure  | 730  | F.ABORT  |
| 316  | Connection broken as a result of an administration command  | 730  | F.RELEASE<br /> F.ABORT  |
| 317  | Time-out  | 740  | F.ABORT  |
| 318  | Mandatory PI absent or illegal PI contents  | 722  | F.ABORT  |
| 319  | Number of bytes or articles incorrect  | 620  | F.ABORT  |
| 320  | Excessive number of resynchronizations for a transfer  | 730  | F.ABORT  |
| 321  | Call the backup number  | 730  | F.CONNECT<br /> F.CREATE<br /> F.ABORT  |
| 322  | Call back later  | 730  | F.CONNECT<br /> F.CREATE<br /> F.ABORT  |
| 399  | Other  | 730  | F.CONNECT<br /> F.RELEASE<br /> F.CREATE<br /> F.ABORT  |


<span id="ODETTE_Protocol__Diagnostic_Codes"></span>

## ODETTE Protocol Diagnostic Codes

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

