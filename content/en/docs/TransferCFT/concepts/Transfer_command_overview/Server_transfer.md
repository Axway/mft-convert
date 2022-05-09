{
    "title": "Server  transfer",
    "linkTitle": "Server transfer",
    "weight": "190"
}This topic describes the steps involved in a server transfer. These
steps include:

- [Receiving
    an incoming call](#Receiving_an_incoming_call)
- [Exchanging
    protocol information](#Exchanging_protocol_information)
- [Creating
    or releasing the file](#Creating_or_releasing_the_file)
- [Transferring
    data](#Transferring_data)
- [Disconnecting](#Disconnecting)

<span id="Receiving_an_incoming_call"></span>

### Receiving an incoming call

An incoming connection on the communication system is always accepted
by the Transfer CFT . There is no need to negotiate a network connection
for a server transfer, as no transfer is yet associated with the open
session.

The Transfer CFT then decides if it can continue the transfer. This depends
on the limitations set in terms of connections to the requested network
resource, the MAXCNX parameter of the CFTNET object, or number of simultaneous
transfers, the MAXTRANS parameter. If Transfer CFT cannot continue due
to parameter limitations, the network connection is closed.

<span id="Exchanging_protocol_information"></span>

### Exchanging protocol information

The messages exchanged during this phase are part of the reciprocal
partner recognition (authorization checks: passwords, time slots, etc.).
The messages also help in negotiating protocol parameters. The data exchanged
during this phase differs according to the protocol. See also [Protocols.](../../../protocols_start_here)

Transfer CFT may refuse the transfer if:

- The partner is
    unknown, does not give the right password, or is not authorized at that
    time, time slot, and so on
- The requested protocol
    is not supported for this partner
- The number of connections
    with this partner is exceeded
- A directory type
    EXIT generates a refusal
- Negotiation is
    impossible

A catalog entry has not yet been created.

<span id="Creating_or_releasing_the_file"></span>

### Creating or releasing the file

Once the recognition phase is complete, Transfer CFT receives a request
to create a file (receive), open a file (send), or receive a message.
This is followed by one of the two following events:

- Transfer release

There is already a catalog entry, in the H state,
corresponding to the request received. This transfer may be in the H state
as a result of:

- A SEND ...
    STATE=HOLD command - this is a request by the requester to release a transfer
    previously put on hold at the server end
- A HALT command - this is then a request to resume an intentionally halted transfer
- An accidental
    transfer interruption; the request is then a resumption request

Transfer CFT is able to link the transfer request
with the existing entry.

- Transfer creation

There is no catalog entry corresponding to this transfer
and one is then created.

The Transfer CFT may refuse the transfer,
in the following cases in particular:

- The partner
    is not authorized to send or receive the file (CFTAUTH command, internal
    or external security system, etc.)
- It is impossible
    to create or open the file (file not known or inaccessible, characteristics
    incompatible with the Transfer CFT description, for example)

No entry is created in the catalog.

<span id="Transferring_data"></span>

### Transferring data

The data in the file is transferred item by item, broken down, or concatenated
with, the data units conveyed over the network. The exchange protocol
may include re-synchronization and compression functions.

<span id="Disconnecting"></span>

### Disconnecting

When the data transfer is complete, the files are closed, and the network
connection is cut after the expiration of the associated hold time-out.
Depending on the protocol, the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter setting allows a network
session to be held open to permit several transfers to be made in sequence.

Network disconnection is at the initiative of the requesting {{< TransferCFT/axwayvariablesComponentShortName  >}}
or the server {{< TransferCFT/axwayvariablesComponentShortName  >}} (see the DISCTD and DISCTS parameters of the CFTPROT
command).
