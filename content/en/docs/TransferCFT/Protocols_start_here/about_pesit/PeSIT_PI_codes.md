---
    "title": "PeSIT PI code descriptions ",
    "linkTitle": "PeSIT PI codes",
    "weight": "150"
---
This topic provides a description for each of the PeSIT parameter identifiers, PI,
codes used with {{< TransferCFT/axwayvariablesComponentShortName  >}}.

About PeSIT PI codes
--------------------

During the file transfer phase, as seen in [How PeSIT works](), messages called FPDU (File
transfer Protocol Data
Units) are exchanged between partners. Each FPDU comprises a
header (six bytes) that specifies the type of data transferred. The body of the FPDUs comprises a series of parameter identifiers (PI) used to specify
and negotiate all the transfer elements.

![](/Images/TransferCFT/temp_fpdu.png)

Each parameter PI has an assigned a number and value per the PeSIT protocol
specifications. These PI codes are used for the session negotiation between partners.

Furthermore, each parameter group unit is identified by a numeric code named PGI
(Parameters Group
Identifier).

********Parameter identifiers (PI) in the message body negotiate the session********

![](/Images/TransferCFT/temp_PI_format.png)

For more information on the PeSIT protocol, refer to
documentation published by the GSIT (Groupement pour un Système Interbancaire
de Télécompensation).

Descriptions
------------

<span id="PI_01"></span>

### PI 01 Use of a CRC (deprecated)

This field indicates the implementation of transmission error detection.

<span id="PI_02"></span>

### PI 02 PeSIT diagnostic code

Refer to *[Protocol
diagnostic codes.](../../../troubleshoot_intro/about_error_codes/about_diagnostic_codes/general_protocol_diagnostics)*

<span id="PI_03__Requester_identification_PeSIT_E"></span>

### PI 03 Requester identification 

This field specifies the name of the Partner requesting the connection:

- Establishing the
    protocol connection (FPDU CONNECT)

Format: 16 characters.

In requester mode,
this field is defined using the local partner (NSPART) parameter of the partner object (CFTPART). It identifies the network name by which the Transfer CFT
identifies itself to its partner.

In server mode,
this field identifies the network name of the remote partner. It provides
the local Transfer CFT with a search criterion for the remote user definition
in the PARTNERS file, the equivalent to the remote partner (NRPART) parameter of
the partner object (CFTPART).

- Creating a file
    (FPDU CREATE)

Format: 24 characters

- bytes 1 to
    8: sending the application name
- bytes 9 to
    16: sending the user name
- bytes 17 to
    24: free field

In requester mode, the
first two fields are respectively defined using the SAPPL and SUSER
parameters of the CFTSEND or SEND command. If the transfer
command does not specify these parameters, the PI 3 code is not sent.
The last free field is never defined by Transfer CFT.

In server mode, Transfer
CFT recovers the PI 3 code in order to be able to define the &SAPPL
and &SUSER symbolic variables. The free field is not used by
Transfer CFT.

- Selecting a file
    (FPDU SELECT)

As the PeSIT protocol imposes no constraints as to the contents
of this PI code, Transfer CFT does not define or use it.

- Sending a message
    (SEND TYPE = MESSAGE, ...)

The PI 3 code contains the same value as the one conveyed
by the protocol connection request (FPDU CONNECT) previously required
to send this message.

- Sending an acknowledgement
    (SEND TYPE = REPLY, ...)

The PI 3 code contains the value of the PI 4 code conveyed
by the file creation request (FPDU CREATE) whose receipt is acknowledged.

<span id="PI_04___Server_identification_PeSIT_E"></span>

### PI 04 Server identification 

This field specifies the name of the connection server partner. It takes
various formats depending on the PeSIT service used.

- Establishing the
    protocol connection (FPDU CONNECT)

Format: 16 characters

In requester mode, this
field is defined using the remote partner (NRPART) parameter of the partner object (CFTPART). It identifies the Partner's network name.

In server mode, Transfer
CFT does not process this field. Logically, it is defined by the specific
network identification of the local site relative to the requester Partner
(the local partner (NSPART) parameter of the partner object (CFTPART)).

- Creating a file (FPDU CREATE)

Format: 24 characters

- bytes 1 to
    8: receiver application name
- bytes 9 to
    16: receiver user name
- bytes 17 to
    24: free field

In requester mode, the first two
fields are respectively defined using the RAPPL and RUSER
parameters of the CFTSEND or SEND command. If the transfer
command does not specify these parameters, the PI 3 code is not sent.
The last free field is never defined by Transfer CFT.

In server mode, Transfer CFT recovers
the PI 3 code in order to be able to define the &RAPPL and
&RUSER symbolic variables. The free field is not used by Transfer
CFT.

- Selecting a file (FPDU SELECT)

As the PeSIT protocol imposes no constraints as to the contents of this
PI code, Transfer CFT does not define or use it.

- Sending a message (SEND TYPE = MESSAGE, ...)

The PI 4 code contains the same value as the one conveyed by the protocol
connection request (FPDU CONNECT) previously required to send this message.

- Sending an acknowledgement (SEND TYPE = REPLY, ...)

The PI 4 code contains the value of the PI 3 code conveyed by the file
creation request (FPDU CREATE) whose receipt is acknowledged.

<span id="PI_05_Access_control_PeSIT_E"></span>

### PI 05 Access control

- Format: 16 characters
- Bytes 1 to 8: partner
    password
- Bytes 9 to 16:
    the partner's new password

This parameter is exchanged at the time the protocol connection is established.
The Transfer CFT using the F.CONNECT service (request or response)
defines the first field using the local partner password (NSPASSW) parameter of the CFTPART
command. It identifies its own password as regards the transfer partner.
On receipt of an incoming connection (F.CONNECT indication), Transfer
CFT checks the equivalence between this password and the value of the
NRPASSW parameter of the CFTPART command associated with
the partner.

The second field is used to dynamically modify the password. As Transfer
CFT does not support this facility, the area associated with the new password
is not defined and not used.

<span id="PI_06_Version_number"></span>

### PI 06 Version number

This field indicates the version number of the PeSIT protocol.

{{% TransferCFT/snippets/PI07%}}
<span id="PI_11_File_type_PeSIT_E"></span>

### PI 11 File type 

This field conveys the class the file belongs to and contributes to
identifying the file, particularly for resuming transfers.

When a file creation/selection FPDU is sent, Transfer CFT sets this
PI code to zero. On receiving such FPDUs, its value is entered in the
catalog request associated with the transfer. This value cannot be 0xFFFF,
0xFFFE or 0xFFFD or the transfer is refused.

For message transfers (FPDU.MSG), this field characterizes the message
type. Transfer CFT processes this field in accordance with the specifications
of the PeSIT protocol and defines it using the TYPE parameter of
the transfer command:


| TYPE parameter  | Description  |
| --- | --- |
| SEND TYPE = MESSAGE,... | Outgoing message |
| SEND TYPE = REPLY,... |  PI 11 of the file for which the message conveys the acknowledgement |


 Transfer
CFT reserves the 0xFFFD value for subsequent use.

<span id="PI_11__File_type_PeSIT_D"></span>

### PI 11 File type 

Transfer CFT sends and expects to receive the value 0.

<span id="PI_12_File_name"></span>

### PI 12 File name

Format: 76 characters

When sending a file, Transfer CFT
defines this field using the file network identifier, the NIDF
parameter of the transfer command. When receiving a file, Transfer CFT
uses the file network identifier to generate a local file identifier (IDF).
Transfer CFT recovers the PI 12 to set the value of the &NIDF
symbolic variable.

When sending
a file, Transfer CFT defines this field using the message identifier,
the IDM parameter of the transfer command. When receiving a message,
Transfer CFT recovers the PI 12 to set the value of the &IDM
symbolic variable.

When an acknowledgement is sent (SEND TYPE = REPLY, ...), this field
contains the PI 12 of the file whose reception is acknowledged.

<span id="PI_13_Transfer_identifier"></span>

### PI 13 Transfer identifier

This parameter is used to identify a transfer through a numerical value.
Transfer CFT manages this field internally and sets its value in accordance
with the specifications of the PeSIT protocol. In the case of a retry,
in particular, it must be the same as the one used during the initial
transfer.

When sending an acknowledgement
(SEND TYPE = REPLY, ...), this field contains the PI 13
of the file transfer acknowledged.

<span id="PI_14_Requested_attributes"></span>

### PI 14 Requested attributes

This parameter indicates the file attributes whose values have to be
communicated in response to a file selection request. This involves a
combination of the logical, physical and log attributes. In requester/receiver
mode, Transfer CFT systematically requests all the attributes. In
server/sender mode, Transfer CFT adapts itself to the request made
by the requester partner.

When transferring a message
or acknowledgement, Transfer CFT does not define or use this field.

<span id="PI_15_Restarted_transfer"></span>

### PI 15 Restarted transfer

This parameter indicates that a transfer is a new occurrence of a transfer
which has already been attempted.

- In
    requester mode, Transfer CFT manages this field internally,
    according to the transfer characteristic (new or restarted).
- In server mode, if the remote Partner
    indicates a restart, the search criteria for a catalog entry relative
    to the network values received are as follows:

<!-- -->

- File type (PI 11)
- File identifier
    (PI 12)
- Transfer identifier
    (PI 13)
- Connect Partner
    (PI 3 of FPDU CONNECT)

<!-- -->

- Initial sender
    name (PI 61)
- Final receiver
    name (PI 62)

If no catalog entry corresponds to the restart request, Transfer CFT
rejects the transfer request.

<span id="PI_16_Data_code"></span>

### PI 16 Data code

This parameter indicates the type of coding (ASCII, EBCDIC or BINARY)
used to send the file data or the message.

In sending mode, this field is defined
using the NCODE parameter of the CFTSEND or SEND
transfer command.

In receiving mode, this field supplies
the network code of the data so that they can be used by the local Transfer CFT.

<span id="PI_17_Transfer_priority"></span>

### PI 17 Transfer priority

This parameter provides the relative priority assigned by the requester
of a transfer.

In requester mode, this field is
defined using the PRI parameter of the RECV and CFTRECV
commands (for a read transfer) or of the SEND and CFTSEND
commands (for a write transfer). In {{< TransferCFT/PrimaryCGorUM  >}}, use the Transfer priority parameter.  
As PeSIT only recognizes three priority levels, the following conversions
are performed:

- PRI &gt; 128    
    PI 17 = 0 (high)
- PRI = 128    
    PI 17 = 1 (medium)
- PRI &lt; 128    
    PI 17 = 2 (low)

In server mode, the PI 17 value is
entered for information in the Transfer CFT catalog and under no circumstances
affects the transfers in process. The PI 17 network values 0, 1 and 2
provide a priority of 255, 128 and 0 respectively in the Transfer CFT
catalog.

{{% TransferCFT/snippets/PI18%}}
<span id="PI_19_End_of_transfer_code"></span>

### PI 19 End of transfer code

This code specifies the type of end-of-transfer in the event of a voluntary
interruption (F.CANCEL primitive). Transfer CFT manages this field internally
and sets its value in accordance with the specifications of the PeSIT
protocol.

A HALT command causes an end-of-transfer type of 4.

{{% TransferCFT/snippets/PI20%}}
<span id="PI_21_Compression"></span>

### PI 21 Compression

This field is used to negotiate the use of compression during the transmission
of the file data.

The use of zero, one or several of the following compression
techniques may be negotiated:

- horizontal compression
- vertical compression

In sending mode, Transfer CFT defines this field using the result
of a logical AND between the compression levels specified by the SCOMP
(CFTPROT command) and NCOMP (CFTSEND/SEND
commands) parameters.

In reception mode, Transfer CFT defines this field using the
result of a logical AND between the compression levels specified by the
RCOMP (CFTPROT command) and NCOMP (CFTRECV/RECV
commands) parameters.

<span id="PI_22_Access_type_in_PeSIT_E"></span>

### PI 22 Access type 

This field indicates the type of transfers authorized for the required
connection: write, read or mixed.

In requester mode, Transfer CFT opens
a connection type depending on the SROUT parameter of the CFTPROT
command:


| SPROUT value  | Access level  |
| --- | --- |
| SROUT = SEND  | Write access |
| SROUT = RECV  | Read access |
| SROUT = BOTH  | Mixed access |
| SROUT = NONE  | Transfer CFT refuses to perform the transfer. This value has no protocol reality, since no connection request is sent |


In server mode, Transfer CFT only
tolerates sessions in accordance with the value of the SRIN parameter.

{{% TransferCFT/snippets/PI23%}}
<span id="PI_25_Maximum_size_of_a_data_entity"></span>

### PI 25 Maximum size of a data entity

This parameter corresponds to the maximum number of bytes which can
be sent in a data entity of the network service (NSDU).

In sending mode (reception mode), this parameter is defined using the
SRUSIZE (RRUSIZE) parameter of the CFTPROT command.
Its value is negotiated during the file selection phase. The requester
proposes a maximum size and the server responds with the same maximum
value or less.

<span id="PI_27_Number_of_data_bytes"></span>

### PI 27 Number of data bytes

This field indicates the number of file data bytes which have been transferred.
If compression is used, this value includes the size of the headers of
compression strings. Transfer CFT manages this field internally and sets
its value in accordance with the specifications of the PeSIT protocol.

<span id="PI_28_Number_of_articles"></span>

### PI 28 Number of articles

This field indicates the number of file records which have been transferred.
Transfer CFT manages this field internally and sets its value in accordance
with the specifications of the PeSIT protocol.

<span id="PI_31_Article_format"></span>

### PI 31 Article format

This parameter specifies the file article format. It is defined using
the NRECFM parameter of the SEND command, or specify the **Record format** in the send template (CFTSEND).  
The following conversions are performed:

- NRECFM = F    
    PI 31 = 0x00 (fixed)
- NRECFM = V    
    PI 31 = 0x80 (variable)
- NRECFM = U    
    PI 31 = 0x80 (variable)

<span id="PI_32_Article_length"></span>

### PI 32 Article length

This parameter specifies the length in bytes of a file record. It is
defined using the NLRECL parameter of the SEND command, or the Max record length of the send template (CFTSEND).

<span id="PI_33_File_organization"></span>

### PI 33 File organization

This parameter describes the organization of the data within the file
and, consequently, the type of access to its data for the transfer: sequential,
relative or indexed.

It is defined using the NORG parameter of
the CFTSEND or SEND command.

<span id="PI_37_File_label"></span>

### PI 37 File label

Format: 80 characters (maximum length supported by Transfer CFT: 512
characters).

This field can be used to associate a symbolic name to a transferred
file.

In sending mode, Transfer CFT defines
this field using the **File name sent** parameter of the of the send template (CFTSEND), or the (NFNAME) of the SEND command, to designate the name of the file on the receiver
site.

In reception mode, Transfer CFT recovers
the PI 37 in order to be able to define the symbolic variable &NFNAME.
Otherwise, this field is only used if the transfers with the partner can
be performed in open mode. In other words, if the local Transfer CFT authorizes
the sending partner to set the physical name of the receiver file (CFTRECV/RECV
FNAME = &NFNAME, ... ).

<span id="PI_38_Key_length_in_PeSIT_E"></span>

### PI 38 Key length 

Where the organization of the announced file is indexed (see PI 33),
this parameter contains the length in bytes of the file access key.

It
is defined using the NKEYLEN parameter of the CFTSEND or
SEND command.

In
reception mode, Transfer CFT recovers the PI 38 in order to
be able to define the symbolic variable &FKEYLEN as required.

<span id="PI_39_Key_offset_in_PeSIT_E"></span>

### PI 39 Key offset 

Where the organization of the announced file is indexed (see PI 33),
this parameter contains the offset of the file access key in bytes relative
to the start of the article.

It is defined using the NKEYPOS parameter
of the CFTSEND or SEND commands.

In
reception mode, Transfer CFT recovers the PI 39 in order to
be able to define the symbolic variable &FKEYPOS as required.

<span id="PI_41_Space_reservation_unit"></span>

### PI 41 Space reservation unit

This field defines the unit used to express the space reservation value.
The possible units are kilo-bytes or articles.  

In sending mode, Transfer CFT always expresses the space reservation
unit in kilo-bytes.  

In reception mode, Transfer CFT supports both types of possible units.  
The reservation unit can only be the article when the file format is fixed
(see PI 31).

<span id="PI1PI_42_Space_reservation_maximum_value"></span>

### PI 42 Space reservation maximum value

This field defines the size that the file cannot exceed. It is defined
using the NSPACE parameter of the CFTSEND or SEND
command and is always expressed in kilo-bytes.

<span id="PI_51_Creation_data_and_time"></span>

### PI 51 Creation data and time

This field indicates the file creation date and time.

In sending mode, it is defined using the FDATE and FTIME
parameters of the CFTSEND or SEND commands. If these parameters
are not expressed at the time the transfer command is sent, Transfer CFT
assigns them with the date and time the catalog takes them into account.

In reception mode, Transfer CFT recovers
the PI 51 in order to be able to define the &FDATE and &FTIME
symbolic variables.

When an acknowledgement is
sent (SEND TYPE = REPLY, ...), this field contains the PI 51 of
the file whose reception is acknowledged.

<span id="PI_52_Last_extraction_date_and_time"></span>

### PI 52 Last extraction date and time

This field indicates the date and time the last transfer was normally
terminated or interrupted.

In file sending mode, Transfer CFT
sets this field to the PI 51 value. In reception mode, Transfer CFT does
not use this field.

<span id="PI_61_Client_identifier_in_PeSIT_E"></span>

### PI 61 Initial sender name 

Format: 24 characters

This field contains the identifier of the initial sender of the transfer.

In store and forward
mode, it can be distinguished from the requester identifier (PI 3), thereby
allowing relay Transfer CFTs, if any, to route the transfer to the next computer.
For write transfers, this field corresponds to the local partner (NSPART) parameter
of the partner the transfer was originally sent from.

<span id="PI_62_Bank_identifier_in_PeSIT_E"></span>

### PI 62 Final receiver name 

Format: 24 characters.

This field contains the identifier of the final recipient of the transfer.

In store and forward mode, it can be distinguished from the server
identifier (PI 4), thereby allowing relay Transfer CFTs, if any, to route the
transfer to the next computer. For write transfers, this field corresponds
to the remote partner (NRPART) parameter of the final partner the transfer is intended
for.

<span id="PI_91_Message_in_PeSIT_E"></span>

### PI 91 Message 

Format: 4096 characters

This field allows a message to be conveyed from one user to another
by means of the message transfer service.

It is defined using the MSG
parameter of the SEND TYPE = MESSAGE command or SEND TYPE =
REPLY command.

In message reception mode, Transfer
CFT recovers the PI 91 in order to define the &MSG symbolic
variable.

See the [xlate](../../../c_intro_userinterfaces/command_summary/parameter_intro/xlate) parameter for transcoding details.

<span id="PI_99_Free_message"></span>

### PI 99 Free message

Format:

- 254 characters for Transfer CFT to a non-Transfer CFT.
- 512 characters when transferring between two Transfer CFTs.

This parameter allows a message to be conveyed from one user to another
in the free field of the PeSIT service primitives. No control concerning
the coding, structure or semantics of its contents is imposed by the protocol.

During file sending, PI 99 contains the value in the PARM field of the
SEND command. Character type coding with a maximum length of 254/512 bytes is permitted depending on the partner type.

See the [xlate](../../../c_intro_userinterfaces/command_summary/parameter_intro/xlate) parameter for transcoding details.
