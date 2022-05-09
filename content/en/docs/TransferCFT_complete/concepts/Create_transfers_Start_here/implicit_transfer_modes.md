{
    "title": "Implicit  transfer modes",
    "linkTitle": "Implicit transfer modes",
    "weight": "220"
}<span id="Read_transfer_and_implicit_send_mode"></span>

Read transfer and implicit send mode
------------------------------------

There are two types of read transfer implicit send modes:

- [closed
    mode](#Read_transfer_implicit_send_closed_mode)
- [open
    mode](#Read_transfer_implicit_send_open_mode)

<span id="Read_transfer_implicit_send_closed_mode"></span>

### Read transfer implicit send closed mode

This mode
is only available with the following protocols:

- PeSIT D EXTERN profile
- PeSIT D CFT profile
- PeSIT
    E

A *receiver/requester* requests the reception of a model file identified
by IDF by activating a RECV command.

A CFTSEND IMPL = YES implicit parameter setting command has been set
on the *server* with the same IDF. This IDF corresponds to a physical
file with a location indicated in the FNAME parameter of this command.

The requester stores the file received under the name indicated in the
FNAME parameter of the local RECV command (or by default in the parameter
of the CFTRECV parameter setting command).

The name assigned *at the requester end* may be set explicitly
or be determined using the symbolic variables defined on reception.

*At the server end,* the name of the
file to be sent can also explicitly set or determined using the symbolic
variables defined in the send request.

For the definition of symbolic variables, refer to the section *Symbolic
variables*.

The figures below indicate these possibilities.

**Receiver/requester transfer in implicit
send mode: closed mode - explicit file names**

![](/Images/TransferCFT/Rec_tx_implicit_send_closed_explicit_file.gif)

In the figure above, the name assigned (FNAME = X), at the requester
end and the name of the file to be sent at the server end (FNAME = Y)
are explicit.

**Receiver/requester transfer in implicit
send mode: closed mode - symbolic variable at the requester end**

![](/Images/TransferCFT/Rec_tx_implicit_send_closed_symbolic_variable_at_req.gif)

In the above figure, the name assigned at the requester end is defined
using the symbolic variable &IDT which provides the possibility of
recovering the identifier associated with the transfer in process (transfer
in PeSIT protocol, for example).

**Receiver/requester transfer in implicit
send mode: closed mode - symbolic variable at the server end**

![](/Images/TransferCFT/rec_req_tx_implicit_send_closed_symbolic_var_at_server.gif)

In the above figure, the name of the file to be sent at the server end
is defined using the symbolic variable &PART which provides the possibility
of recovering the partner identity.

<span id="Read_transfer_implicit_send_open_mode"></span>

### Read transfer implicit send open mode

This
mode is only available with the following protocols :

- PeSIT D CFT profile
- PeSIT E

Unlike the locked for sending mode, the open mode allows:

- The sender/server
    to impose the physical location of the file to be received by the receiver/requester
    (open mode at the requester end).
- The receiver/requester
    to impose the physical location of the file to be sent by the server (open
    mode at the server end).

*To implement
the open mode at the requester end:*

- The *server*
    must define a physical name to be used at the requester end to store the
    file received. This name is indicated in the NFNAME parameter of the CFTSEND
    parameter setting command and is sent by the protocol during the transfer.
- The *receiver/requester*
    must be able to make use of the name sent. For this purpose, the value
    of the FNAME parameter of the RECV command (or by default of the parameter
    of the CFTRECV parameter setting command) must be the symbolic variable
    &NFNAME. This symbolic variable is replaced by the file name sent
    by the sender/server. For the definition of the symbolic variables, refer
    to the section *Symbolic variables*.

The physical location of the file to be sent by the sender/server is
defined by the FNAME parameter of the CFTSEND parameter setting command.
The value of this parameter may be explicit or locally defined using symbolic
variables (see the previous paragraph).

> **Note**
>
> Note: If a requester defines FNAME=&NFNAME and the server has not defined
> NFNAME, or vice-versa, the transfer fails and is interrupted. If the server/sender
> defines NFNAME and the file name is preceded with "\*", the requester/receiver
> can use the name of its choice as the FNAME.

**Receiver/requester transfer in implicit
send mode: open mode - at the requester end**

![](/Images/TransferCFT/rec_req_tx_implicit_send_open_at_req.gif)

*To implement
the open mode at the server end:*

- The *requester*
    must know the physical location of the file it wants to receive. This
    name is indicated in the NFNAME parameter of the RECV command activating
    the receive request and is sent by the protocol during this request.
- The *sender/server*
    must be able to make use of the name sent. For this purpose, the value
    of the FNAME parameter of the CFTSEND parameter setting command must be
    the symbolic variable &NFNAME. This symbolic variable is replaced
    by the file name sent by the receiver/requester.

The physical location of the file received by the requester is locally
defined by the FNAME parameter of the RECV command (or by default by the
parameter of the CFTRECV parameter setting command). The value of this
parameter may be explicit or locally defined using symbolic variables
(see previous paragraph).

> **Note**
>
> Note: If a requester defines NFNAME and the server has not defined FNAME
> = &NFNAME, or vice-versa, the transfer fails and is interrupted.

**Receiver/requester transfer in implicit
send mode: open mode at the server end**

![](/Images/TransferCFT/rec_req_tx_implicit_send_open_at_server.gif)

 

These two mechanisms can be implemented simultaneously:

- The *sender/server*
    can impose the physical location of the file to be received by the receiver/requester
    (open mode at the requester end).
- The *receiver/requester*
    can impose the physical location of the file to be sent by the server
    (open mode at the server end).

This possibility is represented in the following diagram.

**Receiver/requester transfer in implicit
send mode: open mode at the requester and server end**

![](/Images/TransferCFT/rec_req_tx_implicit_send-open_at_req_and_server.gif)
