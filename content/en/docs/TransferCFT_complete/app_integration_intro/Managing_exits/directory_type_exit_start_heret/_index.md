{
    "title": "About Directory  exits ",
    "linkTitle": "Directory exit",
    "weight": "310"
}What is a directory exit?
-------------------------

You can use the directory exit
task to modify the parameters used to establish an exit connection with
a remote partner in server or requester mode. A user subprogram can
be called each time a session with a partner, or a set of partners, is
opened depending on the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter setting.

Use a directory exit to:

- Server mode -
    to replace the standard checks performed by {{< TransferCFT/axwayvariablesComponentShortName  >}} when a remote
    partner requests a connection
- Requester mode -
    to provide, supplement or modify the parameters {{< TransferCFT/axwayvariablesComponentShortName  >}} requires
    to establish network and protocol connections with a remote partner

The directory exit parameters can include:

- Network name
- Password
- Remote partner
    address
- Remote SAP - service
    access point
- Final protocol
    to be used

The directory EXIT can only be called once during the protocol connection
phase, or start of protocol session.

<span id="Server_mode"></span>

Server mode
-----------

When a connection is indicated, {{< TransferCFT/axwayvariablesComponentShortName  >}} knows the network name
and the calling partner’s password as well as the protocol to use for
the communication. If the corresponding CFTPROT object contains the identifier
of a directory EXIT task, the monitor gives control to this task that
performs user checking, calling partner’s network name and password, call
time slots, and so on.

You can:

- Request Transfer
    CFT to perform your own standard checks
- Accept the call
- Refuse the call

In this mode, regardless of the protocol used, the EXIT task takes control
at each protocol connection, even in the case of a transfer sequence over
a single physical connection. In the case of a transfer sequence over
a protocol connection, the EXIT task does not take control.

The EXIT can perform checks on a partner:

- Known to Transfer
    CFT and defined in a CFTPART object
- Unknown to Transfer
    CFT when there is no CFTPART object

It can accept or refuse network and protocol connections.

Even if all partners are unknown to {{< TransferCFT/axwayvariablesComponentShortName  >}}, the monitor needs
the partner file to operate.

<span id="Requester_mode"></span>

Requester mode
--------------

In requester mode, you can:

- Request Transfer
    CFT to perform your standard checks before establishing connections
- Accept to establish
    connections
- Refuse to establish
    connections

The partner may be known, defined in a CFTPART object, or unknown to
{{< TransferCFT/axwayvariablesComponentShortName  >}}.

Even if all partners are unknown to {{< TransferCFT/axwayvariablesComponentShortName  >}}, the {{< TransferCFT/axwayvariablesComponentShortName  >}} needs
the partner file to operate.

At the time of a connection request, {{< TransferCFT/axwayvariablesComponentShortName  >}} knows the CFTPART
object associated with the PART parameter of the SEND or RECV command.
The {{< TransferCFT/axwayvariablesComponentShortName  >}} uses the first protocol of the CFTPART object as communication
protocol, the other protocols defined in this object being used as backup
protocols.

The first protocol choice criterion is:

- The partner is
    known to {{< TransferCFT/axwayvariablesComponentShortName  >}}: there is a CFTPART object corresponding to the
    partner. The first protocol of the CFTPART object containing an EXIT directory
    identifier is chosen. If no protocols of this command contain an EXIT
    directory identifier, the first protocol is chosen
- The partner is
    unknown to {{< TransferCFT/axwayvariablesComponentShortName  >}}: there is no CFTPART object corresponding to the
    partner. The first protocol of the CFTPARM object that contains an EXIT
    directory identifier is chosen. If none of the protocols of this command
    contains an EXIT directory identifier, the transfer is abandoned, and
    the request changes to the "K" state
