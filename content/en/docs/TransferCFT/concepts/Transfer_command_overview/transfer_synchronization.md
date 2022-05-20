---
title: "Transfer synchronization"
linkTitle: "Transfer synchronization"
weight: 280
---Transfer CFT provides the following synchronization services:

* Set synchronization points during transfer
* Resume an interrupted transfer from a synchronization point
* Dynamically resynchronization during transfer

## Services

### Set synchronization points

The partner sending the file data can mark the transfer by means of sequentially numbered synchronization points. The receiving partner acknowledges a synchronization point to indicate the correct reception of the data sent up to that point. The synchronization points and their frequency and the acknowledgement window, are options which can be negotiated between the two partners at the time the logical connection is established.

### Resume a transfer

This function lets the requesting user resume a transfer that was interrupted before completion. In case of incident, the synchronization points mechanism allows a transfer to be resumed from a point other than the beginning of the file. It is possible to resume both a write transfer and a read transfer.

A transfer may be interrupted by an error, such as a network issue, or as a result of an operator action like the HALT command. At the requester end, the transfer may be reactivated manually (CFT START command) or, in certain cases, automatically. The Transfer CFT authorizes the transfer to restart at the last synchronization point set before interruption. If both partners do not succeed in resynchronizing with each other, the transfer begins again from the start of the file.

### Dynamically resynchronize

This function allows the exchange to be resumed from the last synchronization point acknowledged, in case of a remediable incident occurring during data transfer, in particular in the event of a transmission error being detected. Unlike transfer resumption, this mechanism is involved when the transfer has not been interrupted.

## PeSIT synchronization

#### Parameters

The SPACING, RPACING, SCHKW and RCHKW parameters
are used to negotiate the use of the Synchronization function. This function
comprises the synchronization points
and acknowledgement anticipation window
items. The setting of synchronization points allows a transfer to be resumed,
following an incident, from a point other than the start of the file.
The resumption point is the last synchronization point acknowledged by
the file receiver.

The synchronization interval indicates
the maximum volume of data that the sender can send between two consecutive
synchronization points. Only the characters of the file to be transferred
are taken into account in this volume. If the file is compressed, the
interval between synchronization points must be calculated using the volume
of data obtained after compression.

The interval between synchronization points must be selected taking
the following factors into account:

* Exchange of synchronization
    points: a small interval penalizes performance
* Spacing of synchronization
    points: a large interval penalizes restarts
* Spacing of synchronization
    points assumes a large "buffering" capability, since this parameter
    is related to the acknowledgement anticipation
    window (see below) and consequently to the availability of memory
    resources
* Synchronization
    point can only be set between file articles, even if segmentation has
    taken place

This means that the negotiated value of the interval between synchronization
points must always be equal to or greater than the size of the largest
of the articles of the file to be transferred. If compression is used,
this then corresponds to the size of the largest article after compression.
The default value (36 kilo-bytes) has been chosen such that articles of
up to 32 kilo-bytes in size can be sent without problems, even though
this would only be necessary under exceptional circumstances.

A zero value of the interval between synchronization points indicates
that the Synchronization function is not to be used. Although this value
allows the transfer time to be decreased, the resumption point is the
start of the file.

The acknowledgement window determines
the number of synchronization points that the sender of the file can set
without waiting for an explicit response from the receiver partner for
each point. A zero value for the acknowledgement window indicates that
no synchronization point will be acknowledged.

#### In requester mode

When the SROUT
parameter of the CFTPROT command takes the value BOTH, the synchronization
interval negotiated by Transfer CFT, whether the transfer type is write (SEND command) or read (RECV command), is the smallest value of the SPACING
and RPACING parameters.

When the SROUT
parameter of the CFTPROT command takes the value SENDER (resp. RECEIVER),
the synchronization interval negotiated by Transfer CFT corresponds to
the SPACING (or RPACING respectively) parameter for a write
(or read) transfer type.

The value sent
may be reduced by the server partner. It is essential for the final value
negotiated to be greater than the maximum record size of the file. This
maximum size may also be increased by the compression function on the
basis of 1 byte every 32 bytes (for example: 32 bytes for a size of 1024,
128 bytes for a size of 4096).

#### In server mode

If the remote Transfer CFT
opens a one-way write (or read) session, Transfer CFT negotiates, as synchronization
interval, the smallest value between what the partner proposes and the
value of the RPACING parameter of the CFTPROT command for reception
(or the SPACING parameter for transmission).

For a two-way session,
Transfer CFT then negotiates the smallest value between the one proposed
by the partner and the values of the SPACING and RPACING
parameters.  

The acknowledgement window is negotiated in the same way as the synchronization
interval, using the SCHKW and RCHKW parameters of the CFTPROT
command.

The RESYNC parameter is used to negotiate the dynamic resynchronization
of exchanges option during transfer. The only dynamic resynchronization
case possible between two Transfer CFTs is in the event of a CRC
error (PAD = YES).

****Example****

`CFTPROT  ID = INTRAN,         ...         SCHKW   = 2,           SPACING = 4096,         ...`

The sender is able to send up to 4 Mbytes before each synchronization
point set. The sender can also send up to 8 Mbytes before waiting for
the last synchronization points to be acknowledged.

## SFTP synchronization

### Restart a transfer

Transfers are activated by the client, so a restart only works from the client side. Between two Transfer CFTs, interrupted transfers are restarted as on other protocols. Note that:

* There is only one entry in the catalog for the transfer.
* You should use the file name as the transfer identifier during the restart.

When a send is restarted by the client, Transfer CFT checks that the file is still available on the server. This is possible only when the transfer is configured in Open Mode, as otherwise the client does not know the server's name.

### Configuration parameters

Specify the following parameters:

* rpacing: when CFT is receiving a non negotiated, internal file, the size in Kbytes between two synchronization points for the receiver. 0= no synchronization point and the default is 32,000 (32 MB).
* spacing: when CFT is sending the non negotiated internal file, the size in Kbytes, between two synchronization points for the sender. 0= no synchronization point and the default is 32,000 (32 MB).
* srusize:When sending a file, the block size. Note that the larger the block size the more you optimize the I/O process with a maximum 32,700 bytes.
* rrusize: When receiving a file, the block size. Note that the larger the block size the more you optimize the I/O process with a maximum 32,700 bytes.

****Example****

`CFTPROT  ID = SFTP,         ...           SPACING = 32000,         ...`

## OFTP synchronization

To
specify the synchronization interval, set the SCREDIT and RCREDIT parameters.

These parameters correspond to the maximum number of network messages
(NSDU) consecutively sent by the requester before the server acknowledges
their reception. Once the maximum number of network messages has been
sent, the requester has to wait for permission from the server (reception
of a FPDU CDT = FPDU from CREDIT) before sending other data.

These parameters are negotiated with the partner’s symmetrical parameter
during the connection phase. After negotiation, the smallest value is
selected by Transfer CFT as the CREDIT window (=synchronization interval).

In all cases, the end of the transfer phase (characterized by the sending
of a FPDU EFID) sets the local value of the authorized CREDIT to zero
(which is equivalent to a data transfer inhibition). Authorization to
transfer data again is granted at the next application connection request
(sending of a FPDU SFID). This means that the sending of a FPDU SFID sets
the local CREDIT value to the CREDIT value negotiated during the connection
phase.

> **Note**
>
> In accordance with the operation
> of the Transfer CFT, a transfer in process is acknowledged
> by determining synchronization points. These points acknowledge the data
> which have just been transferred. With the Odette protocol, the synchronization
> points are taken each time a CREDIT is received or sent. All the data
> sent are consequently acknowledged when the requester receives a CREDIT.
> Conversely, the previously received data are acknowledged when the Server
> sends a CREDIT. The CREDIT value ranges from 1 to 999. The recommended
> value is \*CREDIT = 4 .

****Example****

`CFTPROT  ID = OFTP,         ...         SCREDIT   = 4,           SRUSIZE = 20000,         ...`

The sender is able to send up to 80,000 bytes (4 x 20000) before each synchronization
point set.

## PeSIT synchronization PIs

<span id="PI_07_Synchronization_points_option"></span>

### PI 07 Synchronization points option

This field indicates the interval between synchronization points and
the acknowledgement window. The values negotiated by Transfer CFT depend
on the type of access to the protocol connection (see PI22).

If a connection is required
with a mixed type of access, Transfer CFT negotiates the smallest
value of the **Data transferred between sync points**(S/RPACING) parameters of the of the protocol object (CFTPROT).

In server mode, Transfer CFT cannot increase the value proposed by the
requester partner. The **Acknowledgement window size** parameters negotiate in the same way as the synchronization
interval (S/RCHKW of the protocol object (CFTPROT)).

<span id="PI_18_Restart_point"></span>

### PI 18 Restart point

This is the synchronization point number used to locate the position
in the file from which the transmission is to be resumed. Transfer CFT
manages this field internally and sets its value in accordance with the
specifications of the PeSIT protocol.

<span id="PI_20_Synchronization_point_number"></span>

### PI 20 Synchronization point number

This numerical value expresses the synchronization point number. Transfer
CFT manages this field internally and sets its value in accordance with
the specifications of the PeSIT protocol.

<span id="PI_23_Resynchronization"></span>

### PI 23 Resynchronization

This parameter indicates the use of the resynchronization functional
unit as required. It is defined using the RESYNC parameter of the
CFTPROT command and is negotiated by both partners at the time
the protocol connection is established.
