{
    "title": "Write  transfer modes",
    "linkTitle": "Write transfer modes",
    "weight": "210"
}This
topic presents two transfer modes, which are comprised of the following
sections:

- Write transfer
    mode
- [Read
    transfer and locked-for-sending mode](#Read_transfer_and_locked_for_sending_mode)

<span id="Write_transfer_mode"></span>

Write transfer mode
-------------------

There are two write transfer modes, **Open**
and **Closed**, which are described
in this section.

<span id="Write_transfer_closed_mode"></span>

### Write transfer closed mode

The physical location of the file *sent* by the sender/requester
is defined by the FNAME parameter of the SEND transfer command or by default
by the parameter of the CFTSEND parameter setting command.

The physical location of the file *received* is determined at the
receiver/server end using the FNAME parameter of the local CFTRECV parameter
setting command. The name assigned at the server end may be determined
either:

- Explicitly
- From symbolic variables
    defined on reception

For the definition of symbolic variables, refer to the *Symbolic variables*
sections.

The figures below indicate these possibilities.

**Sender/requester transfer: closed mode
- explicit name at the server end**

![](/Images/TransferCFT/Send_req_tx_closed_explicit_name_server.gif)

 

In the figure above, the name assigned (FNAME = Y) at the server end
is explicit.

**Sender/requester transfer: closed mode
- symbolic variables at the server end**

![](/Images/TransferCFT/Send_req_tx_closed_symbolic_var_server.gif)

In the second figure (transfer in PeSIT E protocol for example), the
name assigned at the server end is defined using the following symbolic
variables:

- &IDT recovers
    the identifier associated with the transfer in process for a given partner
- &IDF recovers
    the IDF sent by the sender during a transfer
- &PART recovers
    the locally defined name of the requester partner

<span id="Write_transfer_open_mode"></span>

### Write transfer open mode

**PeSIT D CFT profile, PeSIT E**

The open mode allows the sender/requester to decide on the physical
location of the file on the receiver/server.

To implement this mode:

- The *requester*
    must consequently define a physical name to be used on the server. This
    name is indicated in the NFNAME parameter of the SEND command (or by the
    parameter of the CFTSEND parameter setting command). This name is sent
    by the protocol
- The *receiver/server*
    must be able to use the name sent. For this purpose, the value of the
    FNAME parameter of the CFTRECV command must be the symbolic variable &NFNAME

This symbolic variable is replaced by the file name sent by the sender/requester.
For the definition of symbolic variables, see [Symbolic
variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables).

The physical location of the file to be sent by the sender/requester
is defined by the FNAME parameter of the SEND transfer command or by default
by the parameter of the CFTSEND parameter setting command.

> **Note**
>
> Note: If the sender/requester defines NFNAME and the server/receiver has
> not defined FNAME = &NFNAME (or vice-versa), the transfer fails and
> is interrupted. If the requester/sender defines NFNAME and the file name
> is preceded with "\*", the server/receiver can use the name of
> its choice as the FNAME. The following figure summarizes the implementation
> in open mode for a sender/requester transfer.

**Sender/requester transfer: open mode**

![](/Images/TransferCFT/Send_transfer_req_open_mode.gif)

<span id="Read_transfer_and_locked_for_sending_mode"></span>

Read transfer and locked-for-sending mode
-----------------------------------------

<span id="Read_transfer_closed_mode"></span>

### Read transfer closed mode

**PeSIT D EXTERN profile, PeSIT
D CFT profile, PeSIT E, ODETTE**

This mode is only possible with the protocols mentioned.

**ODETTE**

Only the following reception command is valid: RECV IDF = \*

A *receiver/requester* requests the reception of a model file identified
by IDF by activating a RECV command.

A locked for sending command was previously activated on the *server*
for this model file identifier. This IDF corresponds to a physical file
with a location indicated in the FNAME parameter of the SEND command (or
by default by the parameter of the CFTSEND parameter setting command).

The requester stores the file received under the name indicated in the
FNAME parameter of the local RECV command (or by default by the parameter
of the CFTRECV parameter setting command).

The name assigned at the requester end
may be set explicitly or determined using the symbolic variables defined
on reception. For the definition of symbolic variables, see [Symbolic
variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables).

For the server, the name of the file to be sent cannot be defined using
a symbolic variable. In ****Locked for sending****
mode the name of the file to be sent, FNAME, must be known when the SEND
is submitted.

The following figures indicate these possibilities.

**Receiver/requester transfer in locked
for sending mode: closed mode - explicit name at the requester end**

![](/Images/TransferCFT/Rec_req_tx__lock_for_send_closd_explicit_req_end.gif)

In the figure above, the name assigned (FNAME = X) at the requester
end is explicit.

**Receiver/requester transfer in locked
for sending mode: closed mode symbolic variable at the requester end**

![](/Images/TransferCFT/Rec_req_tx_lock_send_closed_symbolic_var_on_requester.gif)

In the figure above, the name assigned at the requester end is defined
using the symbolic variable &IDT which provides the possibility of
recovering the identifier associated with the transfer in process (transfer
in PeSIT E protocol, for example).

<span id="Read_transfer_open_mode"></span>

### Read transfer open mode

**PeSIT D CFT profile, PeSIT E** This
mode is possible between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s using one of these
protocols.

In locked for sending mode, the open mode allows a sender/server to
impose the physical location of the file to be received by the receiver/requester
(open mode at requester end).

To implement this mode:

- The *server*
    must define a physical name to be used at the requester end to store the
    file received. This name is indicated in the NFNAME parameter of the SEND
    command activated in locked for sending mode and is sent by the protocol
    during the transfer.
- The *receiver/requester*
    must be able to make use of the name sent. For this purpose, the value
    of the FNAME parameter of the RECV command (or by default the parameter
    of the CFTRECV parameter setting command) must be equal to the symbolic
    variable &NFNAME. This symbolic variable is replaced by the file name
    sent by the sender/server. For the definition of symbolic variables, see [Symbolic variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables).

The physical location of the file to be sent by the sender/server is
defined by the FNAME parameter of the SEND transfer command (or by default
by the parameter of the CFTSEND parameter setting command).

> **Note**
>
> Note: In locked for sending mode, the name FNAME of the file to be sent
> must be known as soon as SEND is submitted. The receiver/requester is
> consequently not able to impose the physical location of the file to be
> sent by the server. The open mode at the sender/server end is not possible.

For the same reason, the file name sent at the server end cannot be
locally determined using a symbolic variable.

If a requester defines FNAME = &NFNAME and the server has not defined
NFNAME, or vice-versa, the transfer does not go through and is interrupted.

If a requester defines FNAME=&FNAME and the server has not defined
NFNAME, or vice-versa, the transfer does not go through and is interrupted.

However, if the server/sender defines NFNAME and the file name is preceded
with "\*", the requester/receiver can use the name of its choice
as the FNAME.

The following diagram displays the open mode for this
transfer.

**Receiver/requester transfer in locked
for sending mode: open mode**

![](/Images/TransferCFT/Rec_req_tx_locked_for_send_open.gif)
