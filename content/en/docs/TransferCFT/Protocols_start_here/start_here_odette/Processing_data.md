{
    "title": "Processing  data",
    "linkTitle": "Processing data",
    "weight": "170"
}This topic describes compression functions in Transfer CFT when using
the OFTP (ODETTE) protocol.

- [Negotiating
    compression](#Negotiating_compression)
- [Change
    direction](#Change_Direction)
- [End-to-end
    messages](#End_to_end_messages)
- [Online
    translation](#Online_Translation)

<span id="Negotiating_compression"></span>

Negotiating compression
-----------------------

The COMPRESSION option is negotiated during the connection phase, on
sending the Start File Session IDentification
FPDU.

> **Note**
>
> Note: The SCOMP and RCOMP parameters of CFTPROT are used as a basis for
> this compression option negotiation.

This only involves a COMPRESSION OPTION. This means that while the partners
have the possibility of COMPRESSING files, it is not a MANDATORY REQUIREMENT.

This Compression Option is negotiated as indicated in the figure below.

********Compression option negotiation********

![Compression Option is negotiated between the initiator (Requester) ad the acceptor (Server)](/Images/TransferCFT/Image1692.gif)

<span id="Formatting"></span>

### Formatting

Within the specific framework of the OFTP protocol, the data FPDU (DTF)
is constituted in a particular way. This DATA FPDU comprises n SUBRECORDS
(n &gt;= 1); where the term SUBRECORD means "sub-part of a record".

Each record to be sent is first formatted by breaking it down into SUBRECORDs;
the size of each SUBRECORD is 63 characters or less and is preceded by
a HEADER. This header is a byte the bit by bit meaning which is explained
in the general data formatting diagram.

#### Special Text Type Files (Format = T)

SENDING files

At the end of each record, Transfer CFT sends the 2 control characters
&lt;CR&gt; &lt;LF&gt; (0x0D, 0x0A), so as to comply with the ODETTE format
for ‘T’ type files.

RECEIVING files

Transfer CFT checks whether the record ends with the 2 characters CR/LF.
If it does, Transfer CFT deletes these characters.

#### Structure of the Data Exchanged

********Structure of the data exchange buffer********

![View of structure including the initial Byte, header, and sub-record](/Images/TransferCFT/Image1693.gif)

 

********HEADER structure********

![Header structure defining bits 0 through 7, which is the last bit of the record](/Images/TransferCFT/Image1694.gif)

<span id="Compression"></span>

### Compression

The OFTP (ODETTE) protocol uses a HORIZONTAL compression. Horizontal
compression works by compressing repetitive characters, including the
blank character. Compression is implemented at the HEADER level by setting
the N6 bit to 1. Where the compression is implemented, bits N0 to N5 no
longer indicate the sub-record size, as shown in the above diagram, but
the number of times a single byte is repeated; the byte in question immediately
follows the HEADER.

********SUBRECORD example********

![](/Images/TransferCFT/Image1755.gif)

The character ‘44’ (&lt;=&gt; ‘D’ character) is repeated 13 times in
succession in the record.

<span id="Change_Direction"></span>

Change direction
----------------

The Change direction,
CD, concept is specific to the OFTP (ODETTE) protocol. CD can be thought
of as a token which is exchanged by the partners; the rule associated
with this exchange between partners being that only the partner which
has the "token" has the right to send a file.

Once the session opening phase has been completed, the initiator of
the connection is the sender, where the connector initiator has the communication
token.

The receiver of the file has the possibility of clearly stating its
desire to receive the CD, in order to send a file in its turn. This desire
is stated by the receiver at the time the satisfactory completion of transfer
acknowledgement message is sent to the sender, sending the EFPA message
specifying CD = YES in the message parameters.

When one of the partners has the CD but no longer has anything to send,
the CD can be given to the distant partner unless the distant partner
has just given up the CD.

The Change Direction is sent in THREE specific CASES:

- After a correctly
    completed transfer, the SENDER of the file receives an EXPLICIT CD REQUEST
    from its partner; WITHOUT closing the OFTP session (RELEASE phase), the
    SENDER sends the "Change Direction" to the RECEIVER,
- When the RECEIVER
    of the file does not specify its desire to receive the CD, in order to
    send a file, at the time the EFPA protocol message is sent. There are
    two possibilities on completion of transfer, either:

<!-- -->

- The SENDER
    has something else to send and in this case, it sends another file to
    its partner,
- The SENDER
    has nothing more to send and in this case it sets the DISCTD disconnection
    time-out. On expiration of this time-out, if:  
    -   the negotiated transfer direction is BOTH  
    -   The sender sends the CD to its partner
    -   The transfer direction does not permit this  
    -   The session is interrupted
    -   All transfer requests are ignored until the next F_CONNECT_RQ

<!-- -->

- Transfer CFT calls
    its partner following a RECV command, a request to receive one or more
    files: the Transfer CFT transfer monitor sends a Change Direction directly
    to its partner. When the connection phase is completed, the Requester
    sends the CD.

<span id="End_to_end_messages"></span>

End-to-end messages
-------------------

<span id="Sending_an_end_to_end_message"></span>

### Sending an end-to-end message

The OFTP (ODETTE) protocol has an additional service called END-TO-END
RESPONSE or END-TO-END CONTROL, EERP, for acknowledging an end-to-end
transfer.

This is a message sent by the RECEIVER of the file. This message, still
called EERP, is sent to the SENDER of the file in order to inform that
the data sent has arrived correctly.

The EERP message contains parameters identical to those of the application
connection FPDU, except that these parameters are interpreted by the transfer
monitor slightly differently.

Note :according to the
EERP parameter value:

- if EERP = 86 (first
    version of the protocol):
- the ORIGINATOR
    protocol field corresponds to the sender of the file,
- the DESTINATOR
    protocol fields corresponds to the receiver of the file,
- if EERP = 91 (second
    version of the protocol):
- the ORIGINATOR
    protocol field corresponds to the EERP sender (i.e. to the receiver of
    the file),
- the DESTINATOR
    protocol field corresponds to the EERP receiver (i.e. to the sender of
    the file).

> **Note**
>
> Note: Check the consistency of the end-to-end parameter value settings.
> If a sender has a different version from a receiver, it will not be possible
> to acknowledge the transfer.

In addition, the exchanges of EERP messages are not checked by the negotiation
of the SENS transfer parameter (SRIN/SROUT).

#### Implementing the EERP in Transfer CFT

The sending of the EERP is activated by passing a Transfer CFT MESSAGE
send command. However, the message passed is of a particular type in that
it is a REPLY type message. See also [Sending
replies.](../../../concepts/send_command/send_replies)

EERP TRANSMISSION example:

`SEND part=x1, type= REPLY,idf=IDF1, msg=".......", idm=xxxxxxxx,idt=yyyyyyyy`

<span id="Responding_to_an_end_to_end_message"></span>

### Responding to an end-to-end message

To prevent overcrowding of EERP messages between two sites, during an
uninterrupted flow of crossed EERP messages in particular, an FPDU RTR,
Ready To
Receive, has been defined. Although
this "FPDU" is an acknowledgement of the partner receiving the
EERP message, it does not have an end-to-end meaning. There is a corresponding
"RTR" message for each EERP message.

After sending an EERP message, the sender which is the receiver of the
file transfer, has to wait for the FPDU RTR to be received before sending
any other "FPDU" (whether another EERP or a command such as
SFID).

If the first character of the CFTPARM COMMENT parameter is ‘\*’, the
received EERP is not processed.

<span id="Online_Translation"></span>

Online translation
------------------

For transmission, the online translation
mechanism depends on the values of the FCODE and NCODE parameters of the
CFTSEND object.

Although the OFTP protocol does not convey the "file code"
information over the network, the standard establishes that a file is
always coded in ASCII. In this case, the Transfer CFT monitor performs
an "on line" translation on reception if the value of the FCODE
parameter, of the CFTRECV object, is EBCDIC.
