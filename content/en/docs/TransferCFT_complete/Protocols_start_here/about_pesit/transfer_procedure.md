{
    "title": "Transfer  procedure ",
    "linkTitle": "Transfer procedure",
    "weight": "210"
}This topic describes the following PeSIT transfer procedure processes and concepts:

- [Establishing a connection](#Establishing_a_connection)
- [Transfer concatenation](#Transfer_Concatenation)
- [Transfer identifiers](#Transfer_Identifier)
- [Transfer retries](#Transfer_Retries)

For a detailed example of the transfer settings for a server and requester site, see the Example of transfer parameters (PeSIT) topic.

<span id="Establishing_a_connection"></span>

Establishing a connection
-------------------------

Following a transfer request, the requesting Transfer CFT:

- Opens a network
    session
- Sends a protocol
    connection frame

At the server end, Transfer
CFT always positively acknowledges the pre-connection message. Another
Transfer CFT may perform an initial access control on the basis of the identifiers
provided by the requester in the LOGON message.

If the pre-connection message is acknowledged positively, the requester
then sends a protocol connection request (FPDU CONNECT of the PeSIT protocol).
As for the LOGON message, the requester identification protocol fields
are set to the values NPART1 and NPASSW1.

On receiving the connection indication, the server Transfer CFT
looks in the PARTNER file for the CFTPART parameter setting command for
which NRPART = CENTER. Transfer CFT then checks that the requester’s password
corresponds to the NRPASSW parameter of the CFTPART command found.

If the protocol connection is rejected as a result of an incorrect identification,
the requester receives a RCONNECT FPDU, with the protocol diagnostic 301.

For further information concerning the interpretation of error messages
or diagnostics from transfer incidents or problems, refer to [Messages
and error codes](../../../troubleshoot_intro/messages_and_error_codes_start_here).

<span id="Transfer_Concatenation"></span>

Transfer concatenation
----------------------

The figure below shows a concatenation of one-way send transfers. If
the time interval between two requests remains below the values of the
time-out parameters DISCTD (at the requester end) and DISCTS
(at the server end), the transfers are able to be concatenated over the
same protocol connection.

Once these time-outs have expired, the protocol connection is broken
by the first partner whose time-out limit is reached. Another session
therefore has to be established to respond to a further transfer request.
It is consequently best to correctly set up the wait time-outs between
transfers to optimize the use of network resources and avoid performance
being degraded by the systematic establishing of a protocol connection.

#### Time-out role

![Protocol session time out role betweeen a requester and server ](/Images/TransferCFT/Timeout_role3.gif)

#### PeSIT E

The following two figures show how the REVERSE command is used to concatenate
transfers in different directions over the same protocol connection. This
parameter is only taken into account in requester mode. If REVERSE = YES
is specified, concatenation is possible provided that the time interval
between two successive transfer requests does not exceed the configured
wait time-outs. If REVERSE = NO is specified, a protocol connection is established
for each transfer.

It should be noted that a transfer only uses an established connection
for the same pair of requester and server identifiers. In particular,
a requester cannot request a transfer with its partner over a connection
for which it was initially server.

#### One-way protocol connection

![One way protocol session connection between requester and server](/Images/TransferCFT/One_way_protocol_connection.gif)

#### Two-way protocol connection

![Two way protocol session connection between requester and server](/Images/TransferCFT/Two_way_protocol_connection.gif)

<span id="Transfer_Identifier"></span>

Transfer identifiers
--------------------

The transfer identifier (IDT) is a label associated by Transfer CFT
to each transfer. The IDT uniquely identifies a transfer for a given partner
and direction by time stamping. The IDT is assigned to the transfer as
soon as it is saved in a Transfer CFT catalog entry. It is used as a search
criterion for the commands associated with the different transfer operations
(acknowledgement, interruption, resumption, deletion, etc.). It appears
under the name Transfer Id when the catalog is viewed by the LISTCAT
command.

### PeSIT E

Caution:
In the E version of the PeSIT protocol, the IDT
is not the transfer identifier used by the protocol (PI 13). Indeed, the
IDT is represented as 8 alphanumerical characters whereas PI 13 is a binary
value stored as 3 bytes at the most. The PI 13 identifier, which is used
during a retry in particular, is internally managed by Transfer CFT.

### PeSIT E CFT/CFT

Between two Transfer CFTs having negotiated the E version of
PeSIT, the IDT is nevertheless sent via a field the use of which is left
free by PeSIT (PI 99 of FPDU CREATE and SELECT). The two partner Transfer CFTs
can consequently adopt the same IDT value to identify their respective
transfers. A transfer command, in particular the making available of a
file, can consequently change identifiers when the partner establishes
the connection.

Example

`SEND PART=CENTER, STATE=HOLD, ...`

In the case of a sender server, this send hold command makes available
a file for the CENTER partner. The transfer adopts an IDT which remains
local at the time the command is registered. This command is released
by the CENTER partner, requester, when it sends a reception request. The
requester Transfer CFT then adopts the IDT sent by the server
partner.

<span id="Transfer_Retries"></span>

Transfer retries
----------------

A transfer is resumed, after an interruption, by means of a manual command
at the initiative of the requesting partner or automatically by Transfer
CFT.

As explained above, Transfer CFT uniquely identifies a transfer with
a given partner by means of a IDT identifier. To locally select a transfer,
in case of a retry in particular, it is consequently only necessary to
supply the PART partner identifier and IDT transfer identifier information.

****Example****

`START PART=SITE1, IDT=A2517435`

A transfer retry request, identified by the number A2517435, is requested
as regards the SITE1 partner.

For the two PeSIT partners in communication, the following common protocol
information is used to uniquely identify a transfer and is consequently
saved in the catalog with the associated context:

- File type (PI 11)
- File identifier
    (PI 12)
- PeSIT transfer
    identifier (PI 13)

The following four modes should be distinguished when processing a retry
request:

- In requester/receiver
    mode, the retry request is of local origin.  
    Using the PART and IDT parameters of the START command, Transfer CFT
    looks for the transfer context in the catalog. It then actualizes PI 11,
    12 and 13 found in the reception request (FPDU SELECT).

<!-- -->

- In server/sender
    mode, on receipt of an FPDU SELECT indicating a retry request, the
    associated PI 11, 12 and 13 fields serve as search criteria in the catalog.  
    If the context of the transfer to be restarted is not found, Transfer
    CFT sends a negative acknowledgement (FPDU AckSELECT) with a protocol
    diagnostic with a value of 205 (file nonexistent) or 214 (negotiation
    failure at retry point).

<!-- -->

- In requester/sender
    mode, the retry request is of local origin  
    Using the PART and IDT parameters of the START command, Transfer CFT
    looks for the transfer context in the catalog. It then actualizes PI 11,
    12 and 13 found in the send request (FPDU CREATE).

<!-- -->

- In server/receiver
    mode, on receipt of an FPDU CREATE indicating a retry request,
    the associated PI 11, 12 and 13 are searched for in the catalog.  
    If no corresponding transfer context is found, Transfer CFT sends a
    negative acknowledgement (FPDU AckCREATE) with a protocol diagnostic with
    a value of 214 (negotiation failure at retry point).

For protocol fields PI 11,
12 and 13 details refer to [PeSIT PI codes](../pesit_pi_codes).
