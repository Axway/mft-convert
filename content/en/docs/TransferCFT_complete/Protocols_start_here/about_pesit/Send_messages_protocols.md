{
    "title": "Sending  messages ",
    "linkTitle": "Sending messages",
    "weight": "230"
}This topic describes the concepts related to sending messages using
PeSIT and Transfer CFT profiles.

- Send free message as a new transfer: SEND TYPE=MESSAGE (for PeSIT and SFTP (only CFT/CFT) protocols)
- Send positive acknowledgement as a response to a completed transfer: SEND TYPE=REPLY (for PeSIT , OFTP, and SFTP (CFT/CFT) protocols)
- Send negative acknowledgement as a response to a completed transfer: SEND TYPE=NACK (for PeSIT (CFT/CFT) and SFTP (CFT/CFT) protocols)

Two SEND command examples are provided
below.

<span id="Sending_messages_in_PeSIT"></span>

### Sending messages in PeSIT

A PeSIT service user can send a quantity of information in free format
to another PeSIT service user. Transfer CFT provides for the visibility
of this service through two commands:

- SEND
    TYPE = MESSAGE, ...

This command causes
a message to be sent to a designated Partner.

- SEND
    TYPE = REPLY, ...

This command causes a message to be sent
in response to a previous transfer from the Partner to which the message
was sent. The partner {{< TransferCFT/axwayvariablesComponentShortName  >}} interprets this message as a transfer acknowledgement.

Such messages are sent in a connected state; a protocol connection must
exist between the sender of the message and its recipient. This connection
must not be in the transfer phase and must provide for write transfers.
If such a connection does not exist, Transfer CFT establishes one to allow
the message to be sent.

Transfer CFT does not support message segmentation. The size of the
message to be conveyed cannot exceed 512 bytes in PeSIT, nor the FPDU maximum size which
is minus the 6 header characters.

SEND
TYPE = NACK

This command sends a negative acknowledgement .

<span id="Examples"></span>

Examples
--------

#### Example 1

`SEND TYPE=MESSAGE, PART=OFFICE,   IDM=ANDREW,MSG = ’ANDREW: call PETER’`

A message was sent by an identifier ANDREW to a Partner identified by
OFFICE.

#### Example 2

Site A sends a file to the partner on site B. On completion of this
transmission, an end of transfer procedure is initiated on site B. This
procedure sends a message "PAY file effectively received" to
the Partner on site A, using the &PART transfer originator variable.
The transfer identifier to be acknowledged (IDT) is defined by the symbolic
variable &IDT:

`SEND TYPE=REPLY, PART=&PART,   IDM=MES2, IDT=&IDT,MSG = ’PAY file effectively received’`

<span id="Monitoring_PeSIT_transfer_requests"></span>

Monitoring PeSIT transfer requests
----------------------------------

Each protocol command or response corresponds to a protocol trace message
that you can record in the log file. The protocol trace is an optional
feature that you define for transfer activity for a given partner.
