{
    "title": "About the PeSIT protocol",
    "linkTitle": "PeSIT protocol",
    "weight": "120"
}What is PeSIT?
--------------

PeSIT is an end-to-end public protocol designed to provide a reliable
and normalized interface for user applications to exchange files and messages
between heterogeneous systems, irrespective of the network communication
type. You can use the PeSIT protocol on TCP/IP as well as some
proprietary communication networks.

In this PeSIT protocol sub-book we'll first review PeSIT features, then discuss how PeSIT works in Transfer CFT, the processes involved in partner interactions, and lastly provide a description of PeSIT PI codes in a {{< TransferCFT/axwayvariablesComponentShortName  >}} context.

<span id="PeSIT"></span>

Why use PeSIT?
--------------

PeSIT features available for {{< TransferCFT/axwayvariablesComponentShortName  >}} include:

- [File
    transmission](#File)
- [File
    reception](#File2)
- [Setting synchronization points during a transfer](#Setting)
- [Restarting interrupted transfers](#Restarti)
- [Dynamic
    synchronization during a transfer](#Dynamic)
- [Temporary
    suspension of a transfer](#Temporar)
- [Data
    compression](#Data)
- [Generic
    file selection](#Generic)
- [Two-way
    transfers over the same logical connection](#Two-way)
- [Acknowledgment/negative acknowledgment messages](#End-to-e)
- [Relay store-and-forward mode](#Store-an)

<span id="File"></span>

### File transmission

File transmission, also known as file-writing, enables you to transfer
data to a remote partner, provided that you have established a logical
link between the two users.

<span id="File2"></span>

### File reception

File reception, also known as file- reading, enables you to transfer
file data from a partner that is represented by a remote partner.

<span id="Setting"></span>

### Setting synchronization points

The data sender can initiate sequentially numbered synchronization points
during the transfer. The data receiver can acknowledge the synchronization
points to confirm that the data transferred up to that point has been
correctly received and saved. This mechanism is used to restart a transfer
after an incident.

When you establish the logical connection between partners, the two
partners can negotiate the following:

- Synchronization
    acknowledgement window
- Interval
    between synchronization points
- Whether
    re-synchronization is allowed

<span id="Restarti"></span>

### Restarting interrupted transfers

Transfer restart enables you to restart a transfer interrupted before
its completion. The interruption closes the file and disables the protocol
and network connections. It is always the Initiator who initiates the
restart and the Receiver who decides from which synchronization point
the transfer is to be restarted.

<span id="Dynamic"></span>

### Dynamic synchronization

Dynamic synchronization allows you to ask the partner to restart the
transfer from a previous synchronization point, if an incident was encountered
during data transfer. The file remains open during this operation, which
can be initiated by either Initiator or Responder.

<span id="Temporar"></span>

### Temporary suspension of transfer

Transfer suspension allows you to suspend the current transfer (close
and deselect the file) but retain the active connection between the
Initiator and the Responder. The suspended transfer will be restarted
later.

<span id="Data"></span>

### Data compression

Data can be compressed in order to reduce the size of data actually
transferred.

Two types of compression are available in PeSIT:

- Horizontal
    compression: deletes repeated characters in a logical record
- Vertical
    compression: transmits only that data which differs from the previous
    record

<span id="Generic"></span>

### Generic file selection

When an existing file is remotely selected for read access, the file
name sent by the Initiator is no longer just a nominative reference. With
PeSIT, it may also be a set of criteria that the server uses to select
files. File selection can also be generic. The partner partners are responsible
for handling the fact that several files may meet the given selection
criteria.

<span id="Two-way"></span>

### Two-way transfers over the same logical connection

The PeSIT protocol allows only one file transfer in a given session at a time. Nevertheless, Transfer CFT can perform several concurrent transfers either as: 

- Consecutive transfers within the same session
- Several simultaneous file transfers on different sessions in both directions

<span id="End-to-e"></span>

### End-to-end response acknowledgment messages

Message
transfer which enables the transmission of application messages of arbitrary
length, or end-to-end response acknowledgment messages.

This operation enables items of relatively small information to be exchanged between the partners. A message is accompanied by a minimum of service information and may, for example, be used to acknowledge the correct processing of a received file. Transfer CFT enables you to transmit 512-character user messages (between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s the limit is 4096 characters).

Additionally, {{< TransferCFT/axwayvariablesComponentShortName  >}} provides negative acknowledgment capabilities.

<span id="Store-an"></span>

### Relay store-and-forward mode

The relay store-and-forward mode is available with automatic routing from one partner to another via intermediate
partners.

PeSIT diagnostic codes
----------------------

provides PeSIT reason and diagnostic code information, error codes, and FPDU values.
