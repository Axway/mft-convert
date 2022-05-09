{
    "title": "Defining CFTPROT in PeSIT",
    "linkTitle": "Defining CFTPROT in PeSIT",
    "weight": "180"
}The CFTPROT object defines a file transfer protocol application. This topic describes the CFTPROT parameter
setting object as it applies to the PeSIT protocol. It mainly defines
the parameters negotiated with the remote partner at the time the protocol
connection is established, the F.CONNECT service of the PeSIT protocol.

The values configured in the CFTPROT parameter setting are not necessarily
those which are effective once the PeSIT connection is activated. The
term negotiated is used for any parameter whose local value, sent
during the protocol exchange establishing the PeSIT connection, can be
reduced or disabled according to the type of parameter, by the remote
partner.  

Alternatively, when a parameter is non-negotiable,
the server partner must either:

- Submit to the decision
    of the Transfer CFT having initiated the connection
- If the parameter
    value is not acceptable, refuse the connection request with RCONNECT or
    ABORT FPDU

<span id="Mandatory_parameters_in_PesIT_External"></span>

### Mandatory parameters in PeSIT

The TYPE = PESIT parameter must be specified to allow the CFTPROT
object to describe an application of the PeSIT protocol.

The PROF parameter must take one of the three values EXTERN,
CFT or ANY to request the external to SIT profile of the
PeSIT protocol. The PROF parameter also determines the functional level
to be negotiated: D version, E version, with or without Transfer CFT extensions.

For additional information concerning the PROF parameter value, refer
to PROF.

<span id="Time_out_parameters"></span>

### Time-out parameters

Transfer CFT provides the possibility of configuring several time-out
parameters.

PeSIT allows several successive transfers to be concatenated over the
same protocol connection. Transfer CFT consequently defines the maximum
wait time-out between transfers using the parameters DISCTD (wait
time-out in requester mode) and DISCTS (server mode). The protocol
connection is freed on expiration of this time-out.

When not in the protocol connection/disconnection/interrupt phase, the
RTO (read time-out) parameter
is used to monitor the activity of the corresponding PeSIT. For example,
if the local Transfer CFT has not received an acknowledgement of an FPDU (File
Protocol Data Unit) by the time this time-out has expired, it interrupts
the connection with a diagnostic code indicating that the monitoring time-out
has been exceeded.

The DISCTC parameter corresponds to the wait time-out for a response
to the protocol connection request (FPDU CONNECT).

The DISCTR parameter corresponds to thenetwork disconnection wait
time-out of the PeSIT standard. Once the local Transfer CFT has
sent an abrupt protocol interruption (ABORT FPDU), the partner has this
time limit to break the network connection. Beyond this time, the local
Transfer CFT itself sends the network disconnection request.

<span id="Defining_the_transfer_direction"></span>

### Defining the transfer direction

****PeSIT E, CFT/CFT****

The Transfer CFT extensions together with the E version functionality, provide the possibility
to perform a succession of read and write transfers over the same logical
connection. The option of reusing a protocol connection to concatenate
transfers in different directions is defined by the REVERSE parameter.

This parameter is only taken into account in requester mode. For this
reason, when associated with the REVERSE=YES option, Transfer CFT is only
able to concatenate several transfers in different directions during the
same PeSIT session provided that SROUT = BOTH is specified.

The SROUT parameter defines the authorized transfer direction
when the Transfer CFT initiates the protocol connection. The possible values
for this parameter are as follows:

- SENDER: only send
    transfers are authorized
- RECEIVER: only
    receive transfers are authorized
- BOTH: the transfer
    direction is unrestricted
- NONE: no transfer
    is possible

Likewise, the SRIN parameter defines the authorized transfer
direction when the Transfer CFT accepts the protocol connection.

<span id="Compression_options"></span>

### Compression options

The SCOMP and RCOMP parameters are used to negotiate the
maximum authorized compression for file transmission and reception respectively.
The value specified for each of these parameters is the arithmetic sum
of one or more desired types of compression:

- 01: compression
    of a character string "space" EBCDIC, PeSIT E,  PeSIT
    E CFT/CFT
- 02: horizontal
    compression
- 04:
    compression of characters, PeSIT E,  PeSIT
    E CFT/CFT
- 08: vertical compression

<span id="Synchronization_options"></span>

### Synchronization options

{{% TransferCFT/snippets/synchronization_options%}}
<span id="NSDU_size"></span>

### NSDU size

The RRUSIZE and SRUSIZE parameters are used to negotiate
the maximum NSDU (Network Service Data Unit) size for reception and transmission
respectively. To transfer an article longer than (SRUSIZE-6), a segmentation
operation is mandatory.

<span id="Concatenating__regrouping_and_segmenting"></span>

### Concatenating, regrouping and segmenting

Three parameters are used to manage and optimize the filling of an NSDU,
Network Service Data Unit, or an FPDU, File Protocol Data Unit, in the
file data exchange phase:

- CONCAT
    parameter specifies whether several FPDUs can be concatenated in the same
    NSDU
- MULTART
    parameter is option used to group several records of the file sent in
    the same FPDU; this is then referred to as a multi-article FPDU
- SEGMENT
    parameter is the option used to segment file articles into two or more
    FPDUs

These three options are independent from each other and make for no
restrictions as regards a transfer, whatever their value.

These three parameters cannot be transported by the PeSIT protocol and
consequently cannot be negotiated with the remote partner.

- In
    sender mode, it is necessary to know whether the partner supports
    the required options.
- In
    receiver mode, Transfer CFT ignores the value of these parameters
    and equally accepts concatenated, segmented or multi-article FPDUs.

The concatenation option is incompatible with error checking (PAD =
YES). If error checking is active, Transfer CFT then inhibits the sending
of concatenated FPDUs and rejects the reception of concatenated FPDUs.

For additional information, refer to the topic Data unit structure.

<span id="Pre_connection_phase"></span>

### Pre-connection phase

The connection of the PeSIT protocol may be preceded by a pre-connection
phase, independent of the PeSIT protocol. Its role is to inform Transfer CFTs
of the type of transfer protocol used, and the identifier of the caller,
as soon as the connection of the lower communication layers is established.

To meet this requirement, the PeSIT standard defines two messages in
the pre-connection phase. The first message, sent by the requesting partner,
consists of 24 bytes, split into three blocks of 8 bytes:

- bytes 1 to 8: protocol
    used
- bytes 9 to 16:
    requester identification
- bytes 17 to 24:
    requester password

Transfer CFT documents these blocks on the basis of the following values:

- bytes 1 to 8: standard
    string
- bytes 9 to 16:
    value of the CFTPART NSPART parameter
- bytes 17 to 24:
    value of the CFTPART NSPASSW parameter

The second message is sent by the server partner by way of an acknowledgement
(positive "ACK0" or negative "NAK0").

#### PeSIT E CFT/CFT

- In requester mode, the pre-connection phase is
    mandatory. In
    PeSIT E CFT profile, the SSERV parameter is used to particularize
    this string.

- In server mode, Transfer CFT waits for a preconnection
    message prior to any connection.

#### PeSIT E

Use of the SSERV parameter, unless otherwise specified, the SSERV parameter can only be set to two
values with PROF=ANY:

- SSERV=PESIT default
    value
- SSERV=GSIT special
    value

In all cases, the SSERV value is only applied when Transfer CFT is the
requester.

SSERV=GSIT
special value

GSIT is a local conventional value specific to Transfer CFT and impacting
Transfer CFT operations over a TCP/IP network. In this case TCP segments
(packets) must be built in compliance with the latest "PeSIT E on
TCP/IP" standard published by the GSIT.

Operations over a TCP/IP network are only controlled when Transfer CFT
is the requester. When it is the server, it adapts automatically to the
mode required by the requester.

The GSIT value is never used as the pre-connection message initialization
value. When Transfer CFT is the requester and SSERV is set to GSIT, the
first eight bytes of the pre-connection message are set to the standardized
"PESIT---" value.

#### Other SSERV values

The SSERV value is only used to initialize the first eight bytes of
the preconnection message. As the PESIT default value is the only protocol-compliant
value, it is recommended that you only use a different value if so required
by the partner.

#### The LOGON parameter

In PeSIT E, the LOGON parameter can be used to force Transfer CFT to
skip the preconnection phase when it is the requester and therefore cut down
on the number of protocol exchanges. To do so, use the following command:

`CFTPROT TYPE=PESIT, PROF=ANY, LOGON=NO, ... /* (do not send the preconnection   message) */`

As this mode does not comply with the PeSIT external to SIT standard,
it is recommended that you leave the default LOGON value when the partners
concerned do not all use Transfer CFT.

> **Note**

- When Transfer CFT
    is the server, it adapts dynamically to the mode required by the requester
    and the LOGON parameter is ignored.
- When Transfer CFT
    is the requester and LOGON is set to NO, the SSERV parameter is applied
    even if there is no pre-connection message for TCP/IP. The mode is different
    if SSERV=GSIT, as explained in the previous paragraph.

In requester mode and
in PeSIT E, the LOGON=NO option (in the CFTPROT command) may be
specified. In this case, the preconnection message is not sent.  
The use of the SSERV parameter is not relevant since the string designating
the protocol used is always PESIT (5 left-justified characters followed
by 3 blanks).

In server mode and in PeSIT E, Transfer
CFT automatically adapts itself to the partner’s decision to execute a
pre-connection phase or not. Any pre-connection message received is not,
however, used and is always acknowledged positively. Indeed, the PeSIT
connection acceptance or refusal is processed on receipt of the CONNECT
FPDU which has to follow and which contains in particular the requester’s
password.

<span id="Error_detection___PAD_support"></span>

### Error detection - PAD support

**Deprecated in {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.5**

Through an asynchronous access PAD (Packet
Assembler/Disassembler),
certain parameter setting restrictions have to be taken into account at
the level of the CFTPROT command:

- The absence of
    a reliable data link layer requires the two partners to check the integrity
    of the exchanged NSDUs  
    For this purpose, an error detection polynomial CRC (Cyclic Redundancy
    Check) is added to each FPDU
    to allow its integrity to be checked. The CRC length is equal to 2 bytes.  
    The PAD = YES option of the CFTPROT command is mandatory for
    access through a PAD. Indeed, in requester
    mode, this option is used to initiate the use of a CRC and cannot
    be negotiated by the server partner.

<!-- -->

- The implementation
    of the transmission error detection mechanism inhibits the FPDU concatenation
    operation.  
    Each NSDU only transports one FPDU, which improves the efficiency of
    the checks. The CONCAT parameter of the CFTPROT command is set
    to the value NO by Transfer CFT.

<!-- -->

- Performance capabilities
    are not, however, penalized as multi-article FPDUs can be formed (MULTART
    = YES).  
    While the segmentation is certainly less efficient when concatenation
    is inhibited, it is all the same possible (see the SEGMENT parameter).

<!-- -->

- The CRC length
    (2 bytes) must be taken into account in calculating the maximum size of
    NSDUs (SRUSIZE and RRUSIZE parameters).  
    If the length of an article to be sent (after compression if required)
    is greater than the maximum size of an NSDU reduced by 8 bytes, the article
    is segmented.

<!-- -->

- The only dynamic
    resynchronization case possible between two Transfer CFTs is in
    the event of detecting a CRC error.  
    If CRC is used, it is recommended to specify RESYNC = YES to
    mask any transmission errors.

<span id="Negotiated_functional_levels"></span>

### Negotiated functional levels

The table below summarizes the parameter values authorized according
to the functional levels negotiated.


| CFTPROT parameter <br /> for Negotiated functional levels  | PeSIT E  | PeSIT E +<br /> CFT extensions  |
| --- | --- | --- |
| DISCTD  | 0..3600<br /> (dft: 120)  | 0..3600<br /> (dft: 120)  |
| DISCTS  | 0..3600<br /> (dft: 165)  | 0..3600<br /> (dft: 65)  |
| LOGON  | YES, NO  | YES, NO  |
| PAD (*deprecated*) | NO, YES  | NO, YES  |
| RCOMP  | 0, 2, 8, 10<br /> (dft: 10)  | 0..15<br /> (dft: 15)  |
| REVERSE  | NO, YES  | NO, YES  |
| SCOMP  | 0, 2, 8, 10<br /> (dft: 10)  | 0..15<br /> (dft: 15)  |
| SEGMENT  | NO, YES  | NO, YES  |
| SSERV  | &quot;PESIT &quot;  | &quot;PESIT &quot;  |

