{
    "title": "Model and Physical file concepts",
    "linkTitle": "Model and physical file concepts",
    "weight": "230"
}This topic introduces the concepts of Model File Identifiers, IDF, and
Physical Files.

### Types of transfers modes

For each transfer, the Transfer
CFT can operate in one of four modes:

- In
    requester mode: it is then the initiator of the transfer

In PeSIT protocol, the initiator of a network connection
keeps the requester role, for the transfer(s) performed over this connection.

- In
    server mode: it accepts transfer requests

In PeSIT protocol, it is also the acceptor of the incoming
connection.

A Transfer
CFT can perform four roles:

- Sender requester
- Receiver requester
- Sender server
- Receiver server

According to the protocol used, two transfer modes can be managed:

- Transfers between
    a sender/requester monitor and a receiver/server monitor: sender/requester
    transfer or write transfer, for all protocols
- Transfers between
    a receiver/requester monitor and a sender/server monitor: receiver/requester
    transfer or read transfer

This second mode can only be used in the following protocols:

- PeSIT D EXTERN
    profile
- PeSIT D CFT profile
- PeSIT E

Each of these modes applied to a file transfer is represented in the
following figure.

**File transfer mode**

![](/Images/TransferCFT/File_transfer_mode.gif)

The processes described in this topic are only applicable for file transfers (sending
or receiving).

### What is a model file identifier?

During a transfer, {{< TransferCFT/axwayvariablesComponentShortName  >}} assigns a file identifier. The file
is identified with a model file identifier called an IDF. An IDF can be
seen as a logical class containing one or more physical files.

A sales department may, for example, send to a production unit its orders
on a daily basis in an ORDERS model file. ORDERS is a logical identifier
agreed between the two parties; the physical files containing the orders
sent and received respectively are independently managed by the two partners,
each party in particular being able to centralize or distribute the data
as required.

The IDF is at the basis of the organization of the local operation:
control of transfers, connection convention between applications (upstream/downstream)
and the {{< TransferCFT/axwayvariablesComponentShortName  >}}.

Although the local operation IDF and the IDF agreed with the remote
partner generally have the same value, it is possible to use correspondence
mechanisms between local identifiers and remote identifiers to avoid any
naming constraints.

The mechanisms, which are described in this topic, assume that the
local operation IDF and the IDF agreed between the partners are equal.

The mechanisms used to establish a correspondence between local identifiers
and remote identifiers are described in the *NIDF/IDF correspondence*
topic.

#### Implementing a Sender/Requester Transfer

The environment for implementing a write transfer is as follows:

*At the sender/requester site,* the
send transmission is activated by a SEND transfer command.

The following commands are associated with this command:

- A CFTSEND parameter
    setting command with an identifier equal to the IDF defined in the SEND
    transfer command

The parameters specified in the CFTSEND command may
take priority over those defined in the SEND command (FORCE parameter
= YES).  
In this case, a message informs the user that his command has been partially
taken into account.

- Or, if no CFTSEND
    command has been defined, the CFTSEND ID=&lt;*default*&gt; command,
    where &lt;*default*&gt; is the default value defined in the DEFAULT
    parameter of CFTPARM

This transmission relates to the partner (server) indicated in the PART
parameter (see **P*arameter setting
example*, in *Partner*).

*At the receiver/server site*, the
reception characteristics are managed using a CFTRECV parameter setting
command with an identifier equal to the IDF sent by the requester in the
SEND command.  
If no CFTRECV command has been defined with an identifier corresponding
to this IDF, the characteristics of this reception are managed using the
command CFTRECV ID = &lt;*default*&gt;, where &lt;*default*&gt;
is the default value defined in the DEFAULT parameter of CFTPARM.

Example of the mechanisms to use for this
type of transfer:

 

**Implementing a sender/requester transfer
(write) - Explicit parameter setting**

![](/Images/TransferCFT/Imp_send_rec_write_explicit.gif)

****Implementing a sender/requester transfer
(write) - Default parameter setting****

![](/Images/TransferCFT/Impl_send_rec_write_default.gif)

#### Implementing a Receiver/Requester Transfer

This feature is only available with the PeSIT protocol and the SIT profile.

The environment for implementing a read transfer is as follows :

*At the receiver/requester site,* the
reception is activated by a RECV transfer command. The following commands
are associated with this command:

- A CFTRECV parameter
    setting command with an identifier equal to the model file identifier
    defined in RECV (IDF)
- If no CFTRECV command
    has been defined, the command CFTRECV ID = &lt;*default*&gt;, where
    &lt;*default*&gt; is the default value defined in the DEFAULT parameter
    of CFTPARM

This reception relates to the partner (server) indicated in the PART
parameter (see *Parameter setting example*
in *Partner*).

*At the sender/server site,* the transmission
characteristics associated with this request may be managed in two ways,
according to operating constraints:

- Either using a
    locked-for-sending command previously saved in the server catalog

This mode is activated by the transfer command SEND STATE=HOLD, the
parameter STATE=HOLD indicating a delayed transfer ("H" state
of the corresponding catalog entry).

The following are associated with this command in local mode:

- A CFTSEND parameter
    setting command with an identifier equal to the model file identifier
    defined in SEND (IDF)
- Or, if no CFTSEND
    command has been defined, the CFTSEND ID = &lt;*default*&gt; command,
    where &lt;*default*&gt; is the default value defined in the DEFAULT
    parameter of CFTPARM

This transmission relates to the partner (REQUESTER) indicated in the
PART parameter (see *Parameter setting example* in *Partner*).

- Or using an "implicit
    send" defined by a parameter setting command CFTSEND IMPL = YES

This implicit mode is used to send in server mode any authorized file
without requiring a locked for sending command.

The default setting of a CFTSEND command is explicit (IMPL = NO).

On receiving a transfer request, the monitor:

- First selects the
    locked for sending commands (if any)
- And then the implicit
    send commands

The following figures summarize the mechanisms
to be used for this type of transfer according to the send control mechanism
implemented at the server end.

**Implementing
a read transfer with locked for sending at the server end**

![](/Images/TransferCFT/Read_transfer_locked_send_servr.gif)

**Implementing
a read transfer with implicit send at the server end**

![](/Images/TransferCFT/Read_transfer_w_implicit_send_servr.gif)

### File locations

The locations of physical files are normally managed locally, the transfer
characteristics being defined for each partner using a common IDF naming
and data type convention. This file management mode is known as the closed
mode.

In PeSIT D CFT profile and PeSIT E protocol, the {{< TransferCFT/axwayvariablesComponentShortName  >}}
also allows a partner to remotely manage this physical file location.
This management mode is known as the "open mode".

The requirements for implementing these two modes is described, for
each of the mechanisms described in the previous paragraph, in the *Location
of physical files* topic.

On MVS and VMS systems, it is possible to manage several versions of
a given logical file. These files are commonly referred to as GDG type
files, which is a purely MVS designation meaning Generation Data Group.

A file of this type can be accessed either by an absolute name, or by
a relative name that consists of a stem and a version number.

Transferring a file designated by an absolute name involves no particularities.
On the other hand, when the sent or received file is designated by a relative
name, {{< TransferCFT/axwayvariablesComponentShortName  >}} systematically looks for the corresponding absolute
name, to make sure to be able to return to the same data in the event
of an incident. Indeed, if the initial relative name is used when restarting
a transfer, there is a risk of accessing different data (a changeover
of versions may have occurred in the meantime).

At the sender end, the absolute name is detected as soon as the send
request is made.

### Request to transfer several physical files

As explained in the *Use of the model file identifier* paragraph,
several physical files may be designated by the same IDF. The mechanisms
below, already described in the previous topics, are used to manage
this possibility.

*For sender/requester transfers*, the
activation of SEVERAL SEND commands with the same IDF. The reception characteristics
are then managed at the server end by a single CFTRECV parameter setting
command with the same IDF as the one sent by the requester (or with the
default IDF) (see the previous chapter).

*For receiver/requester transfers*,
the registering by the server in locked for sending mode of SEVERAL SEND
STATE = HOLD commands with the same IDF. To receive all the files associated
with the same IDF, the requester has to activate as many RECV commands
as there are locked for sending mode commands at the server end; the activation
of a RECV command identified by an EXPLICIT IDF unlocks the FIRST transfer
at the server end.  

> **Note**
>
> Note: You cannot use this option in implicit send mode, as the implicit send mode is defined using a parameter setting command with
> a unique identifier.

To facilitate the use of the facility, the monitor provides the possibility
of:

- Receiving all the
    files with the same IDF in locked for sending mode at the server level
    with ONE command at the receiver/requester end
- Sending several
    files with the same IDF with ONE command
- Listing a remote
    directory

#### Receiving a Set of Files with the Same IDF in Receiver/Requester Mode

**PeSIT D EXTERN profile, PeSIT
D CFT profile, PeSIT E**

This facility is only possible with the protocols mentioned above.

This mechanism is used to receive all the files with the same IDF only
in locked for sending mode at the server end with a single activation
of the RECV command.

To implement this mechanism, the value of the FILE parameter of the
RECV command, at the requester end, must be equal to the pre-defined value
ALL (FILE = ALL).

Transfers do not take place simultaneously.

**PeSIT D CFT profile, PeSIT**

The transfer requests are processed in sequence as soon as one transfer
is terminated.

This type of transfer generates, in the requester catalog, a generic
entry (virtual) in the K state which is held in the catalog until all
files have been received. The diagnostic code for this transfer is set
to RECV ALL.

Note: The explicit IDF specified at the requester end (RECV command)
must not correspond, at the server end, to a CFTSEND command in implicit
send mode - as this will trigger continuous repetition of transfers.

An example of this mechanism is indicated in the figure below. In this
example, the names of the files received are defined using the symbolic
variable &IDT which makes it possible to recover the identifier associated
with the transfer in process (in PeSIT protocol for example) for a given
partner.

**Example of receiving a set of files with
the same IDF**

![](/Images/TransferCFT/receive_set_of_files_with_the_same_IDF.gif)

#### Sending a Set of Files with the Same IDF in Sender Mode

This mechanism allows a group of files with the same IDF to be sent
using a single SEND command. This mechanism and the parameter setting
used are described in the *Sending of a group of files topic*.

#### Listing a Remote Directory

To list a remote directory, a receiver/requester transfer must be implemented
with the following definitions:

- On the server side,
    open mode send  
    (CFTSEND IMPL = YES, FNAME = &NFNAME)
- On the receiver
    side, physical location, NFNAME parameter in the RECV command, by entering
    the name of a remote directory (*dirname*) or a generic name, or
    mask, using wildcard characters (*mask*)  
    The syntax convention used corresponds to the one recognized by the
    sender {{< TransferCFT/axwayvariablesComponentShortName  >}}.

In response, the sender/server selects the files, creates an entry in
the catalog (with FNAME= *dirname* or *mask*) and send a series
of records. Each record corresponds to a selected file name.

In response, the received file inherits the attributes defined in the
associated CFTRECV or RECV command. This file contains the list of files
selected by the sender. To edit it, you should specify the attributes
associated with the text file type on your system.

**Example listing a remote directory**

![](/Images/TransferCFT/listing_remote_directory.gif)

### Request to receive several model files

{{< TransferCFT/axwayvariablesComponentShortName  >}} provides the possibility of activating the following
at the requester end:

- The reception of
    a physical file identified by a *generic IDF* partially defined using
    a character string and a wildcard character at the end of this string

> **Note**
>
> Note: This wildcard character is specific to each system ; in the rest of
> this manual it is generically designated by the character ‘\*’. Such an
> IDF value is called a mask and is referred to as &lt;mask&gt;.

- *Selective
    receptions* of several physical files associated with generic IDFs
    identified using a mask
- *Global
    receptions* of IDFs of value not specified by the requester. This possibility
    is the generalization of a partially specified IDF. The value used for
    this IDF is, for all the systems, the character ‘\*’ (the character indicated
    here is not the wildcard character defined for the generic IDF)

To implement these mechanisms, the transmissions at the server end must
be in locked for sending mode (SEND STATE = HOLD).

#### Generic IDF

**PeSIT D CFT profile, PeSIT E**

A Generic IDF scenario is possible between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s using either
the PeSIT D profile, or PeSIT E.

This mechanism is used to receive ONE physical file identified at the
server end by a model file identifier corresponding to the mask of the
IDF parameter sent by the requester.

The server unlocks the first transfer pending for the requester partner
and for an IDF parameter value corresponding to this mask.

In the following example, the RECV IDF = IDF\*, ... command unlocks,
at the server end, the transfer SEND IDF = IDF1, STATE = HOLD, FNAME =
Y... , the first transfer pending.

**Example of the first transfer pending
unlocked by {{< TransferCFT/axwayvariablesComponentShortName  >}}**

![](/Images/TransferCFT/First_tx_pending_unlocked_by_CFT.gif)

#### Selective Reception

**PeSIT E, PeSIT D CFT profile**

This mechanism is used to receive all the files whose IDF at the server
corresponds to the "mask" sent by the requester.

To implement this mechanism, the value of the FILE parameter of the
RECV command at the requester end must be equal to the pre-defined value
ALL (FILE = ALL).

The server unlocks all pending transfers concerning the requester and
the IDFs corresponding to the mask sent.

The transfer requests are processed in sequence as soon as one transfer
is terminated. Files are not transferred simultaneously.

In the requester catalog, this type of transfer generates a generic
entry (virtual) in the K state which is held in the catalog until the
files have been received. The diagnostic code for this transfer is set
to RECV ALL.

An example of this mechanism is displayed in the figure. In this example,
the names of the files received are defined using the following symbolic
variables:

- &IDT which
    is used to recover the identifier associated with the transfer in process
    for a given partner
- &IDF which
    is used to recover the IDF sent by the server during a transfer

**Example of selective reception using a
generic IDF**

![](/Images/TransferCFT/select_reception_generic_IDF.gif)

#### Global Receptions


| **ODETTE, PeSIT D CFT profile, PeSIT E**  | Available only with the protocols mentioned above.  |
| --- | --- |


This mechanism allows the requester to receive all the files pending
at the server end.


| **ODETTE**  | Only the following receive command is valid:<br /> RECV IDF = *<br /> Although sequencing is at the sender’s initiative, the receiver end catalog will contain a record corresponding to the global reception request and a record for each reception, in the same way as for the other protocols. This command provides the possibility to change direction and hence globally receive all the files pending at the remote partner end.  |
| --- | --- |
| **PeSIT D CFT profile**  | To activate a reception from a requester {{< TransferCFT/axwayvariablesComponentShortName  >}} to a server monitor, only the following commands are valid:<br /> RECV IDF = * and RECV IDF = *, FILE = ALL |
| **PeSIT D CFT profile, PeSIT E**  | Possible between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s, using one of these two protocols.<br /> Between two CFTs, this is a special case of selective reception, the command being RECV IDF = *, FILE = ALL  |


### Protection of the model file identifier

The CFTAUTH command is used to define the list of IDFs authorized for
a given partner according to the transfer direction: sending or receiving.

Before activating a SEND command, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks that the receiving
partner has the right to receive the specified IDF. This mechanism is
implemented through the parameter setting relationships indicated in the
following figure.

**IDF sending protection**

![](/Images/TransferCFT/IDF_send_protection.gif)

Similarly, when a RECV command is activated, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks that
the sending partner is authorized to send the requested IDF.

This mechanism is implemented through the parameter setting relationships
indicated in the following figure.

**Checking that a partner is authorized
to send the requested IDF**

![](/Images/TransferCFT/Check_partner_authorizd_send_reqd_IDF.gif)

 

These mechanisms may, in particular, be implemented to check whether
partners are authorized or not to access or to write/create a file in
open mode.

The list of authorized or prohibited IDFs, indicated in the CFTAUTH
command, may be explicit:

`          CFTAUTH       ID     =      AUTH,                           IDF     =     (...,FI,...)`

The number of IDFs defined is then limited.

This list may also be defined in an indirection file with a generic
name &lt;*filename*&gt;:

`          CFTAUTH       ID          =       AUTH2,                           FNAME     =     <filename>`

The authorized or prohibited IDFs are indicated in this file.[ ](#)
