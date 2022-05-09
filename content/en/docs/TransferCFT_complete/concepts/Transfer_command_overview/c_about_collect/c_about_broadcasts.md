{
    "title": "Broadcasting concepts",
    "linkTitle": "Broadcasting processes ",
    "weight": "330"
}<span id="Broadcasting_through_a_store_and_forward_site"></span>

### Broadcasting using a store and forward site

A file can be sent to several partners through
a single SEND command, making reference to a broadcasting list defined
in the CFTDEST command.

The figure below shows the relationships
to be established.

**Defining a list
of partners**

![](/Images/TransferCFT/Define_list_of_partner_SEND.gif)

The broadcasting list can be described in a file. In this case, the
name of this file is indicated in CFTDEST, FNAME parameter. The corresponding
partners must be described using the CFTPART command as in the case where
the list is explicit (see CFTDEST).

The following figure indicates the parameter
setting implemented to reach a partner on a broadcasting list by an intentional
store and forward. Broadcasting to the other two partners is accomplished
by direct connection.

**Broadcasting through
a store and forward site**

![](/Images/TransferCFT/Broadcast_thr_store_and_forward.gif)

<span id="Broadcasting_notations"></span>

### Broadcasting notations

*The notations
below are the ones used in the parameter setting example shown in the
following figure.*

To broadcast a file from a store and forward site, the following conditions
are then required and sufficient:

- the initial sender
    defines a virtual partner with an ID corresponding to the ID of the CFTDEST
    command managed on the store and forward site. The values of the OMINTIME
    and OMAXTIME parameters of the CFTPART command are set to zero to force
    the routing of transfers to the store and forward site (intermediate partner
    IPART : ID_B). A file to be broadcast from the store and forward site
    is sent by the command SEND PART=ID_CD ...
- the store and forward
    node must have a CFTDEST command, the identifier of which (ID=ID_CD) is
    the network name of the broadcasting list indicated by the initial partner
    (SEND PART=ID_CD)
- the CFTDEST object
    must comply with the syntax imposed by {{< TransferCFT/axwayvariablesComponentShortName  >}} ([FOR](../../../../c_intro_userinterfaces/command_summary/parameter_intro/for)=COMMUT
    parameter).
- the final receivers
    C and D know the initial sender of the file (CFTPART ID=ID_A,...) and
    the store and forward partner B
- the connections
    established between partners must be complied with at each of the network
    nodes (CFTPART ID=...)

**Broadcasting activated on a store and
forward site**

![](/Images/TransferCFT/Broadcast_activated_on_store_and_forward.gif)

<span id="Broadcasting_processes"></span>

### Broadcasting processes

*On the store and forward site:* on
effectively transferring the file to be broadcast, {{< TransferCFT/axwayvariablesComponentShortName  >}} does not
activate an end of transfer procedure or an error procedure.

The transfer IDF is set to the value COMMUT: a command CFTRECV ID =
COMMUT must consequently be defined on this site as the transfer is saved
in the catalog under the file identifier IDF = COMMUT

If the transferred data code (NCODE) is different from the data code
of the store and forward site, e.g. NCODE = EBCDIC and ASCII internal
code on the store and forward computer, the data is not translated on
the store and forward computer (in store and forward mode, the data is
only translated end to end and not next computer to next computer)

When all the transfers have been correctly completed, the generic transfer
(virtual) associated with the broadcasting (entry designated in the catalog
by a DIAGP code equal to DIFFUS) changes to the T state. The
monitor then activates the end of transfer procedure, if any, associated
with this generic transfer, see the *Broadcasting* paragraph.

Unlike a simple transfer in store and forward mode, the file created
on the intermediate site is not deleted. This deletion may be handled
by the end of transfer procedure, since the &DIAGP variable is used
to determine whether the transfer is a broadcast (DIAGP = DIFFUS).

*On the final receiver site:* the Transfer
CFT monitor receives the same application parameters as those indicated
for a simple transfer in store and forward mode. The store and forward
site does not affect the transfer mode, either open or closed.

On completion of file or message reception, an acknowledgement message
(SEND TYPE = REPLY) may be sent to the initial sender. See below.

*On the initial sender and final receiver
sites:* end of transfer or error procedures may be initiated during
a transfer.

The usual symbolic variables for these types of procedures may be used.
See *[Symbolic variables](../../../../c_intro_userinterfaces/command_summary/symbolic_variables)*.

<span id="Broadcasting_list_acknowledgement_"></span>

### Broadcasting list acknowledgements

*The notations
below are the ones used in the parameter setting example shown in the
following figure.*

To acknowledge a store and forward file or message transfer, the final
partner(s) sends a TYPE=REPLY message to the initial partner (the zero
values of the OMINTIME and OMAXTIME parameters of the associated CFTPART
command force the routing of the REPLY via the intermediate partner B
IPART=B).

To acknowledge a broadcasting list, the following conditions are then
required to route the acknowledgement to the initial partner:

- the connections
    established between partners must be complied with at each of the network
    nodes (CFTPART ID =...)
- at the store and
    forward node, all the catalog records corresponding to the previously
    performed broadcasting, are present
- at the store and
    forward node, all the catalog records corresponding to the previously
    performed broadcasting, indicate that the receiving partners have correctly
    received the file or message (SFT transfer state)

If these conditions are fulfilled, the initial sender of the file or
message receives a single acknowledgement message. The message comes arbitrarily
from one of the final partner, which is the last one to have sent a REPLY
message.

**Broadcasting list acknowledgement mechanism**

![](/Images/TransferCFT/Broadcast_ack_mechanism.gif)
