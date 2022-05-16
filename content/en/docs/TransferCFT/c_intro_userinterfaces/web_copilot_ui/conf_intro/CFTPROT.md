---

    title: CFTPROT  - Transfer protocols
    linkTitle: Protocol - CFTPROT
    weight: 220

---
The CFTPROT command defines a file transfer protocol application. This page describes the parameter setting command, and explains the parameters that are negotiated with the remote partner at the time the protocol connection is established.

The following table lists the parameters which are common to all protocols. The CFTPROT TYPE = xxx commands are explained in the tables following the general parameters.

For network resources of the asynchronous type (not SAP), there
cannot be more than one CFTPROT object per CFTNET object.

****Related
topics****

- Object concepts
    [Transfer protocols](../../../../admin_intro/admin_config_commands/transfer_protocol_concepts)

 
Use this command to define network parameter settings.


| Parameters  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/disctd">DISCTD</a> | Wait time-out (in seconds) before disconnection, in the absence of a new transfer request to the partner, in requester mode.<br/> The session established for a transfer remains active for DISCTD seconds after the completion of this transfer.<br/> If the value is 0, the wait time-out is infinite. |
| <a href="../../../command_summary/parameter_intro/discts">DISCTS</a><br/> <a href="../../../command_summary/parameter_intro/discts">see table</a> | Wait time-out (in seconds) before disconnection, in the absence of a new transfer request from the partner, in server mode.<br/> The session established for a transfer remains active for DISCTS seconds after the completion of this transfer. If at the end of this time-out, no new transfer has been received, the connection is freed by the ABORT FPDU. |
| <a href="../../../command_summary/parameter_intro/dynam">DYNAM</a> | Dynamic partner identifier (8 characters) in server mode. |
| <a href="../../../command_summary/parameter_intro/exit">EXIT</a>  | Identifier describing the directory EXIT.<br/> The value of this identifier corresponds to the CFTEXIT ID parameter.<br/> The identifier may contain the &amp;NPART symbolic variable. |
| <a href="../../../command_summary/parameter_intro/exita">EXITA</a>  | Identifier describing the directory EXIT.<br/> The value of this identifier corresponds to the value of the CFTEXIT ID parameter.<br/> The identifier may contain the &amp;NPART symbolic variable. |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | CFTPROT command identifier.<br/> This name must be referenced in the values taken by the CFTPARM PROT parameter. |
| <a href="../../../command_summary/parameter_intro/idf">IDF</a>  | Used to assign an IDF to a file on receiving an NIDF.<br/> This parameter can be used in:<br/> • server mode (sender or receiver)<br/> • receiver requester following the activation of a RECV IDF=&lt;mask&gt; command |
| <a href="../../../command_summary/parameter_intro/nack">NACK</a>  | ****PeSIT****<br/> Enables or disables the negative acknowledgement feature. |
| <a href="../../../command_summary/parameter_intro/net">NET</a>  | Identifier referring to a CFTNET command associated with this protocol. |
| <a href="../../../command_summary/parameter_intro/rcomp">RCOMP</a> | Maximum compression authorized on receiving a file.<br/> This compression is negotiated between the sender and the receiver.<br/> A zero value corresponds to no compression.<br/> For more information such as usable values etc., see <a href="../../../command_summary/parameter_intro/compression">Compression</a>. |
| <a href="../../../command_summary/parameter_intro/restart">RESTART</a> | Maximum number of transfer restart attempts.<br/> An attempt is taken into account as soon as the physical connection with the remote site is correctly established. |
| <a href="../../../command_summary/parameter_intro/rto">RTO</a>  | Network monitoring time-out (in seconds) excluding the protocol connection/disconnection/break phase.<br/> Corresponds to the wait time-out of a reply to an FPDU before disconnection (READ TIME OUT).<br/> If the value is 0, the wait time-out is infinite. |
| <a href="../../../command_summary/parameter_intro/sap">SAP</a>  | Name of the local SAP, Service Access Point, associated with this protocol.<br/> Used to identify the "access point" at which incoming connection requests for this communication protocol are placed.<br/> The SAP supplied by a requester partner when making its connection request is retrieved by the local Transfer CFT which uses it to deduce the protocol to be used. Each CFTPROT object in a given resource class must include its specific SAP.<br/> The value of this parameter may be expressed in hexadecimal form. In this case, the first character must be "#" (number sign) (for example: #31 is understood as the ASCII character ‘1’). |
| <a href="../../../command_summary/parameter_intro/scomp">SCOMP</a>  | Maximum authorized compression for sending a file.<br/> This compression is negotiated between the sender and the receiver.<br/> A zero value corresponds to no compression. |
| <a href="../../../command_summary/parameter_intro/srin">SRIN</a>  | Controls the direction of transfers authorized for the Transfer CFT when it is server, accepter of the protocol connection. |
| <a href="../../../command_summary/parameter_intro/srout">SROUT</a> | Controls the direction of transfers authorized for the Transfer CFT when it is requester (initiator of the protocol connection). |
| <a href="../../../command_summary/parameter_intro/type#type_CFTPROT">TYPE</a>  | Type of file transfer protocol. |


<span id="Defining_ODETTE"></span>

## Defining ODETTE (OFTP)

The CFTPROT TYPE = ODETTE command is used to describe the OFTP (ODETTE)
transfer protocol.

This command is used to specify the values on which the Transfer CFT is based
during protocol negotiations concerning:

- Credit
- Data compression
- Special logic

The "Credit" value designates the maximum number of data packets
which the sender can send, before the server needs to acknowledge their
receipt by assigning a new "Credit". Transfer CFT simulates
the taking of a synchronization point at each credit message sent or received.

If a transfer is interrupted, the restart point proposed by a sender
Transfer CFT corresponds to the point the send transfer was interrupted,
as detected by the sender. The restart point negotiated by a receiver
Transfer CFT always corresponds to a credit message it previously sent
(except when restarting from the beginning of the file).

The [OFTP (ODETTE)](../../../../protocols_start_here/start_here_odette)
section of the Protocols book contains a detailed explanation of the constraints
and specifics regarding the use of this particular protocol.

QQQ\_QQQ\_QQQ

#### Parameters for CFTPROT TYPE = ODETTE


| <a href="../../../command_summary/parameter_intro/eerp">EERP</a> | Used to interpret the value of the ORIGINATOR and DESTINATOR fields contained in the EERP message, according to the protocol version.<br/> The End to End ResPonse service generates a message called EERP. This message informs the file sender that the data sent arrived correctly.<br/> The first version of the protocol (1986) specifies that:<br/> • the ORIGINATOR protocol field corresponds to the file sender<br/> • the DESTINATOR protocol field corresponds to the file receiver<br/> The second version (1991) specifies that:<br/> • the ORIGINATOR protocol field corresponds to the EERP sender (i.e. the file receiver)<br/> • the DESTINATOR protocol field corresponds to the EERP receiver (i.e. the file sender)<br/> Note: heck the consistency of the customized values from one end to another. If the sender and receiver have different versions, it is not possible to acknowledge the transfer. |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/pad">PAD</a>  | *Deprecated in* {{< TransferCFT/axwayvariablesComponentLongName  >}}**{{< TransferCFT/axwayvariablesReleaseNumber  >}}<br/> Option applying "SPECIAL LOGIC" to the data exchange buffers.<br/> This option is negotiated with the partner when the protocol session is established (in the SSID FPDU). If the option is set to NO for one of the partners, the "special logic" is not applied. |
| <a href="../../../command_summary/parameter_intro/rcredit">RCREDIT</a>  | Value of the "credit" (expressed as a number of "DATA" messages) proposed by Transfer CFT when it is server.<br/> This value is negotiated with the value proposed by the requester (see the SCREDIT parameter) when the protocol session is established. |
| <a href="../../../command_summary/parameter_intro/resync">RESYNC</a>  | Option for restarting a transfer following an interruption.<br/> This option is negotiated with the partner when the connection is established: if the option is set to NO for one of the partners, transfer restarts are not managed. |
| <a href="../../../command_summary/parameter_intro/rrusize">RRUSIZE</a> | Maximum size of NSDUs (Network Service Data Unit) being received.<br/> This parameter is negotiated with the partner (SRUSIZE parameter if Transfer CFT), the smallest value is selected as the size of NSDUs sent.<br/> Refer to the Transfer CFT <a href="../../../../protocols_start_here">Protocol topics</a> to optimize the definition of the value of this parameter. |
| <a href="../../../command_summary/parameter_intro/scredit">SCREDIT</a> | Value of the "credit" (expressed as a number of "DATA" messages) proposed by Transfer CFT when it is the requester.<br/> Transfer CFT is authorized to send a number of "DATA" protocol messages equal to the result of the negotiation (performed when the protocol session is established), before waiting for a new "credit" to be sent by the server. |
| <a href="../../../command_summary/parameter_intro/srusize">SRUSIZE</a> | Maximum size of NSDUs (Network Service Data Unit) being sent.<br/> This parameter is negotiated with the partner (RRUSIZE parameter if Transfer CFT), the smallest value being selected as the size of NSDUs sent.<br/> Refer to the <a href="../../../../protocols_start_here">Protocol topics</a> to optimize the definition of the value of this parameter.<br/> MVS connection, the maximum value of SRUSIZE is equal to the value configured in the NCP (or the equivalent) less (-) 6 bytes. |
| TCP  | Processing method used for protocol messages:<br/> • Transfer CFT: activation of the method specific to Transfer CFT<br/> • OFTP (default value): activation of the standard method (RFC 2204)<br/> This parameter applies in both initiator and responder modes.  |


<span id="Defining_PeSIT"></span>

## Defining PeSIT

The CFTPROT TYPE = PESIT command is used to describe the PeSIT transfer
protocol.

In PeSIT, the user can specify parameters controlling the:

- mechanisms associated
    with the synchronization points
- format of messages
- compression algorithms
- CRC calculation

> **Note**
>
> In certain environments,
> the mechanisms for repositioning in the transferred files are not operational
> with all the files supported: after a transfer interruption, transfers
> then begin again from the start of files (see the Transfer CFT Operations
> Guide specific to your OS).

The four PeSIT variants supported correspond to the four values of the
PROF parameter (CFT, SIT, EXTERN, ANY). Some parameters are only meaningful
for one or more of these variants. For clarity, these specific parameters
are grouped below according to the corresponding value(s) of the PROF
parameter.

For a detailed explanation of the constraints and specifics regarding
the use of each of these variants, refer to the Transfer CFT [Protocols](../../../../protocols_start_here).

QQQ\_QQQ\_QQQ

#### Parameters for CFTPROT TYPE = PESIT


| <a href="../../../command_summary/parameter_intro/concat">CONCAT</a><br/> Only in sender mode | Option to concatenate FPDUs (File Protocol Data Units) in a given NSDU.<br/> This option is not negotiated. |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/cto">CTO</a>  | Minimum duration (in minutes) of the session, Cycle Time Out.<br/> At the end of a transfer, the wait time-out for a nfew transfer is recalculated depending on:<br/> • the time (hour) for opening the session<br/> • the current time<br/> • the wait delay before disconnection (DISCTS for the protocol)<br/> • the duration of the session (CTO)<br/> The session is liberated if no transfer was initiated by the remote partner during the indicated duration. |
| <a href="../../../command_summary/parameter_intro/cycle">CYCLE</a>  | Periodicity (in minutes) for creation of a protocol session:<br/> • 0: PeSIT session open on startup<br/> • n: periodicity |
| <a href="../../../command_summary/parameter_intro/disctc">DISCTC</a>  | Wait time-out (in seconds) for the reply FPDU (ACONNECT), after the sending of a CONNECT FPDU.<br/> If the value is 0, the wait time-out is infinite. |
| <a href="../../../command_summary/parameter_intro/disctr">DISCTR</a>  | Network disconnection wait time-out. Wait time-out (in seconds) for the partner site to cut the connection, after sending an "abort" request (ABORT FPDU).<br/> If the value is 0, the wait time-out is infinite. |
| <a href="../../../command_summary/parameter_intro/hide99">HIDE99</a> | Optional parameter available only to PESIT protocol definition (TYPE=PESIT) using the ANY profile (PROFIL=ANY/CFT).<br/> • NO (Default value): no information inside PI99 (free message PI Code) is hidden<br/> • YES: hide private information carried by the protocol (physical local path of the file) |
| <a href="../../../command_summary/parameter_intro/logon">LOGON</a><br/> Only in requester mode PeSIT E | Implementation of the pre-connection phase.<br/> According to the value of this parameter:<br/> • YES: this phase is implemented. The requester sends a 24-byte EBCDIC message as follows:<br/> • • byte 1 to 8: ‘PESIT ’ (PeSIT followed by 3 blank characters) (corresponding to the protocol used)<br/> • byte 9 to 16: requester identifier (NSPART of CFTPART)<br/> • byte 17 to 24: requester password (NSPASSW of CFTPART)<br/> <br/> • NO: this phase is not implemented: the requester does not send a message<br/> Note: The Transfer CFT server automatically adapts itself to the choice of the requesting partner to send a Logon message or not. |
| <a href="../../../command_summary/parameter_intro/multart">MULTART</a><br/> Only in sender mode | Option to group several records of the file sent in a given FPDU (multi-record FPDUs).<br/> • in sender mode, MULTART = YES is recommended if the partner supports multi-record FPDUs<br/> The value MULTART = YES is PROHIBITED in this profile<br/> • in receiver mode, the Transfer CFT accepts multi-record FPDUs, regardless of the value of this parameter |
| <a href="../../../command_summary/parameter_intro/pad">PAD</a> <br/> Only in requester mode CFT profile | *Deprecated in* {{< TransferCFT/axwayvariablesComponentLongName  >}}**{{< TransferCFT/axwayvariablesReleaseNumber  >}}<br/> Use of the CRC (Cyclic Redundancy Checksum).<br/> This option is not negotiated: in server mode, Transfer CFT always adapts itself to the choice of the requesting partner.<br/> The PAD = YES option is mandatory for an access through a PAD. |
| <a href="../../../command_summary/parameter_intro/part">PART</a>  | List of the partners (maximum of four) for which a PeSIT session, where the transactional turn is cyclically opened..<br/> Inactive partners (result of the command INACT) are not taken into account. |
| PROF  | PeSIT D or E protocol profile.<br/> The profile options are:<br/> • EXTERN profile: corresponds to the standardized definition of the PeSIT version D protocol<br/> • CFT profile: the PeSIT version D protocol, when the partner also has a Transfer CFT<br/> Its functionality level is greater than the PeSIT D EXTERN profile specifications,<br/> • ANY profile: corresponds to the standardized definition of the PeSIT version E protocol<br/> This profile includes the facilities of the CFT profile, as standard.<br/> Additional facilities are provided between two Transfer CFTs, while remaining in conformity with the PeSIT E standard. These facilities are based on the use of the PI 99 (free PI).<br/> • the DMZ profile (DeMilitarized Zone): corresponds to the normalized definition for the PeSIT protocol, version E E (refer to Managing the Turn)<br/> Note: In server mode, the PROF parameter can take either the EXTERN, CFT or ANY values: indeed, in server mode, the Transfer CFT automatically adapts itself to the profile proposed by the requesting partner. |
| <a href="../../../command_summary/parameter_intro/rchkw">RCHKW</a> | Size of the receive mode synchronization point acknowledgement anticipation window, expressed as a number of synchronization points.<br/> Negotiated with the sender partner.<br/> RCHKW=0 means that synchronization points are not acknowledged.<br/> RCHKW=1 is equivalent to operation in half-duplex mode.<br/> On LU6.2 networks all non-null values will be forced to 1 during protocol negotiation. |
| <a href="../../../command_summary/parameter_intro/resync">RESYNC</a><br/> PeSIT D CFT profile, PeSIT D EXTERN profile, PeSIT E | Dynamic resynchronization of exchanges during transfer (without interrupting the data exchange phase).<br/> This option is negotiated with the partner at the time the connection is established: if this option is set to NO for one of the partners, dynamic resynchronization is not managed.<br/> Note: The only dynamic resynchronization possible between two Transfer CFTs is performed when a CRC error is detected (PAD=YES). |
| <a href="../../../command_summary/parameter_intro/reverse">REVERSE</a><br/> Only in requester mode PeSIT D CFT profile, PeSIT E | Option to reuse a connection to perform two transfers in different directions one after the other. |
| <a href="../../../command_summary/parameter_intro/rpacing">RPACING</a> | Value of the interval between synchronization points for receive transfers (in Kbytes) (1 Kbyte = 1024 bytes) (see explanations of the SPACING parameter).<br/> This parameter is negotiated with the partner (SPACING parameter if Transfer CFT); the smallest value is selected as the interval between synchronization points.<br/> A null value (RPACING = 0) means that synchronization points are not set. |
| <a href="../../../command_summary/parameter_intro/rrusize">RRUSIZE</a>  | Maximum size of NSDUs being received and sent.<br/> This parameter is negotiated with the partner (SRUSIZE parameter if Transfer CFT); the smallest value is selected as the size of NSDUs sent.<br/> Refer to the Transfer CFT <a href="../../../../protocols_start_here">Protocol</a> for more information on this parameter. |
| <a href="../../../command_summary/parameter_intro/schkw">SCHKW</a> | Size of the send mode acknowledgement anticipation window for synchronization points, expressed as a number of synchronization points. |
| <a href="../../../command_summary/parameter_intro/segment">SEGMENT</a><br/> Only in sender mode Profile | Option to segment file records in several FPDUs. |
| <a href="../../../command_summary/parameter_intro/spacing">SPACING</a>  | Interval between synchronization points being sent (in Kbytes)<br /> (1 Kbyte = 1 024 bytes). |
| <a href="../../../command_summary/parameter_intro/srusize">SRUSIZE</a> | Maximum size of NSDUs being received and sent. |
| <a href="../../../command_summary/parameter_intro/sserv">SSERV</a> | Identifies the service (protocol variant) required for the incoming partner. |
|   |   |


<span id="SSL_parameter_in_CFTPROT"></span>

## SSL parameter

The CFTPROT object accepts the optional SSL parameter. For the declared
protocol, it sets the:

- Default security
    profile in requester mode
- Security profile
    in server mode

If the SSL parameter is set in a CFTPROT object, transport security
must be implemented in both the server and requester modes.

The security profile is mandatory in server mode. The default security
profile for the requester mode is optional: if it does not exist, it must
then be declared for each CFTPART object, which provides the transfer
partner definition.

The new SSL parameter is used to associate a security profile with a
protocol definition.

For a security profile related to incoming calls (server mode), the
SSL parameter value must correspond to the identifier of a DIRECT=SERVER
SSL command.

For a default security profile related to outgoing calls (client mode),
the SSL parameter value must correspond to the identifier of a DIRECT=CLIENT
SSL command.

If you do not define the CFTPROT SAP parameter when using the SSL protocol,
then the server mode for CFTSSL is not mandatory.

Syntax:  

`CFTPROT[SSL =     identifier,]`

QQQ\_QQQ\_QQQ


| <a href="../../../command_summary/parameter_intro/ssl">SSL</a> | SSL commands Identifier used for security profiles. |
| --- | --- |


<span id="PeSIT_examples"></span>

## PeSIT example

PeSIT protocol, ANY profile, associated
with the data exchange protocol and network resources defined by the CFTNET
ID=ACCEPTOR command.

The time-outs are the default values. Each FPDU contains a single file
record (MULTART=NO).

```
CFTPROT ID = PSITE, /\* PSITE protocol \*/
TYPE = PESIT, PROF=ANY, /\* PeSIT E \*/
NET = ACCEPTOR,
MULTART = NO,
CONCAT = YES
```
