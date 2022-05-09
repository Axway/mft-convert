{
    "title": "Requester  transfer",
    "linkTitle": "Requester transfer",
    "weight": "180"
}A requester transfer involves the following steps:

- [Registering
    the request](#Registering_the_request)
- [Activating
    the transfer](#Activating_a_transfer)
- [Connecting
    to the network](#Connecting_to_the_network)
- [Exchanging
    protocol information](#Exchanging_protocol_information)
- [Creating
    the file](#Creating_the_file)
- [Transferring
    data](#Transferring_data)
- [Disconnecting](#Disconnecting)

<span id="Registering_the_request"></span>

### Registering the request

Transfer CFT executes the transfer commands that are saved
in the catalog. It sends and receives the files through the commands:

- SEND TYPE = FILE
    for sending
- RECV TYPE = FILE
    for receiving

It sends messages through the commands:

- SEND TYPE = MESSAGE
    for a simple message
- SEND TYPE = REPLY
    for a reply to a previous transfer type message

In the catalog, a transfer request made with the parameter STATE=HOLD
is marked with an H (Hold) state.
This transfer cannot be activated by Transfer CFT without operator action
(START command) or without a request from the remote partner. For more
information refer to Activating a transfer below.

A transfer requested by the command STATE = DISP is marked with a D,
at Disposal, state. This transfer
is activated as soon as Transfer CFT resources and the partner’s access
rights permit. The transfer may be refused in certain cases.

<span id="Activating_a_transfer"></span>

### Activating a transfer

Transfer CFT scans the catalog at regular intervals to activate pending
transfers, entries in the D state. For each of these entries, Transfer CFT
determines whether it can be activated as a function of the parameter
setting, and selects the exchange protocol in the CFTPROT object.

The transfer may be held if, for example:

- Partner access
    is inhibited by time slots, parameters of the \*MINTIME/\*MAXTIME type in
    the commands CFTPART, etc.
- The maximum number
    of simultaneous transfers is exceeded

In these cases, the entries remain in the D state.

The transfer may be aborted if:

- The file to be
    sent is not present, or the receiving file does not exist as required
    by the parameter setting (FDISP, FACTION parameters)
- The IDF is refused
    for this transfer in the CFTAUTH object
- The partner is
    not defined

Particular case: the EXITs and directory allow a partner
description to be dynamically complemented or created.

- The IDF or file
    is not authorized for the user

The security system is activated and does not provide the required authorizations.

In these cases, the entries change to the H (Hold) or K (Keep) state.

<span id="Connecting_to_the_network"></span>

### Connecting to the network

When Transfer CFT has the resources for an At Disposal (D) transfer,
it attempts to make the connection to the communication system attached
to the partner of this transfer.

The transfer remains in the D
state throughout this connection attempt period.

The transfer may fail as a result of a network incident, line disconnected,
call number unknown, etc., after all the attempts and use of the various
call numbers and protocols. The catalog entry changes to the H
or K state.

In general, Transfer CFT uses a SAP sent at the time the network
connection is made, to inform the partner of the dialog protocol. For
more information see the [SAP](../../../c_intro_userinterfaces/command_summary/parameter_intro/sap)
parameter of the CFTPART object.

<span id="Exchanging_protocol_information"></span>

### Exchanging protocol information

When the network connection is established, the transfer changes to
the in process C state. From then on, the partners participate in an exchange
of messages in accordance with the chosen protocol, forming the protocol
reciprocal recognition and parameter negotiation phase, during which the
transfer may fail, in particular if:

- The partner detects
    a problem during the recognition phase, name and password verification
- The partner refuses
    the transfer as it is not within the authorized time slot
- The associated
    operating parameters or protocols are incompatible (no protocol unit reciprocal
    recognition or negotiation impossible)
- A directory type
    EXIT generates a refusal

In these cases, the catalog entry changes to the H or K state, according
to the severity of the error. The data exchanged during this phase differs
according to the protocol. See [Protocols.](../../../protocols_start_here)

<span id="Creating_the_file"></span>

### Creating the file

Once the partner is recognized and the protocol is defined, the type
of transfer is determined:

- File send or receive,
    and the file is designated
- Message send

The transfer may fail locally if:

- The file to
    be sent is not known or protected
- There is an
    inconsistency between the parameter setting of the CFTSEND command and
    the file to be sent (FORG incorrect, for example)
- There is an
    inconsistency between the parameter setting of the CFTRECV command and
    the file to be received. FDISP = NEW and the file already exists, for
    example
- The file is
    not authorized for the user, operating security system parameter setting

The transfer may fail remotely if:

- The transfer
    of such a file is not authorized by the partner

In these cases, the corresponding catalog entry changes to the H or
K state.

<span id="Transferring_data"></span>

### Transferring data

The data in the file is transferred item by item, broken down into or
concatenated with the data units conveyed over the network, as necessary.
The exchange protocol may, depending on the type, include re-synchronization
and compression functions.

The file type EXIT allows the user to:

- Handle the data
    at any time during the transfer
- Initiate transfer
    refusals

<span id="Disconnecting"></span>

### Disconnecting

On completion of data transfer, the files are closed, the network connection
is cut after the expiration of the associated hold time-out. According
to the protocol, the monitor parameter setting allows a network session
to be held open to permit several transfers to be made in sequence. Network
disconnection is at the initiative of the requesting Transfer CFT or the server
Transfer CFT. See the [DISCTD](../../../c_intro_userinterfaces/command_summary/parameter_intro/disctd) and [DISCTS](../../../c_intro_userinterfaces/command_summary/parameter_intro/discts) parameters of the CFTPROT command.
