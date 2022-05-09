{
    "title": "Glossary",
    "linkTitle": "Glossary",
    "weight": "200"
}##### A

<span id="Acceptance_by_CFT_"></span>

#### Acceptance by {{< TransferCFT/axwayvariablesComponentShortName  >}}:

Process whereby a transfer request is taken into account by Transfer
CFT when it is recorded in the catalog.

<span id="Accounting"></span>

#### Accounting file:

File containing the descriptive and quantitative properties of all successfully
completed transfers (defined via CFTACCNT, file created via CFTFILE TYPE=ACCNT).

<span id="Acknowledgement_anticipation_window_"></span>

#### Acknowledgement anticipation window:

Parameter determining the number of synchronization points that the
file sender can set without waiting for an explicit response from the
receiver for each point.

This mechanism is used by the PeSIT protocols.

<span id="Acknowledgement__EERP__"></span>

#### Acknowledgement (EERP):

See EERP (End to End Response) acknowledgement.

<span id="Authorized_identifiers_"></span>

#### Authorized identifiers:

List of model file identifiers (IDFs) governed by the same transfer
authorizations or denials (user-defined in the CFTAUTH command).

<span id="Availability_for_the_monitor_"></span>

#### Availability for {{< TransferCFT/axwayvariablesComponentShortName  >}}:

Process whereby a request is made available to the monitor when it is
processed in the communication medium.

<span id="Availability_for_the_partner_"></span>

#### Availability for the partner:

Process whereby a file is made available to a partner so that it can
be sent on reception of the request from the partner. The file can be
made available implicitly (see Implicit send) or explicitly, in which
case the transfer is put on hold in the catalog (SEND STATE=HOLD).

##### B

<span id="Broadcast"></span>

#### Broadcast:

Sending of a file or group of files to a set of partners defined in
a list via a single request (partner list defined in CFTDEST).

<span id="Broadcast_list_"></span>

#### Broadcast list:

List defined in parameter or file form to send a file or group of files
to one or more partners via a single request (user-defined in the CFTDEST
command).

##### C

<span id="Card__parameter__"></span>

#### Card (parameter):

See [Command](#Command__parameter__) (parameter).

<span id="Catalog_"></span>

#### Catalog:

File containing the control and monitoring data associated with transfers
(usage mode defined via CFTCAT, file created via CFTFILE TYPE=CAT).

<span id="Catalog"></span>

#### Catalog entry:

Record corresponding to a transfer in the catalog file.

<span id="Catalog_identifier_"></span>

#### Catalog identifier:

See [IDTU](#IDTU__catalog_identifier__) (catalog identifier).

<span id="Catalog_sharing_"></span>

#### Catalog sharing:

Option available in some environments, whereby the user can monitor
several instances of {{< TransferCFT/axwayvariablesComponentShortName  >}} via a single catalog file.

<span id="CD__Change_Direction__"></span>

#### CD (Change Direction):

Odette protocol service whereby a token is exchanged between partners.
Only the partner in possession of the token can send a file.

<span id="CFT_API_"></span>

#### CFT API:

Programming interface used to integrate {{< TransferCFT/axwayvariablesComponentShortName  >}} into applications
developed in either C or COBOL.

<span id="CFTACCNT"></span>

#### CFTACCNT:

Command used to define the accounting data logging mode for successfully
completed transfers.

<span id="CFTAPI"></span>

#### CFTAPI:

See [CFT API](#CFT_API_).

<span id="CFTAUTH"></span>

#### CFTAUTH:

Command used to define a list of file identifiers governed by the same
transfer authorizations or denials.

This list can be stored in a file or defined explicitly via the IDF
parameter in the command.

<span id="CFTCAT"></span>

#### CFTCAT:

Command used to set transfer catalog management parameters.

<span id="CFTCOM"></span>

#### CFTCOM:

Command used to set parameters controlling communications between user
programs and the {{< TransferCFT/axwayvariablesComponentShortName  >}}. See also [Communication
medium](#Communication_medium_).

<span id="CFTDEST"></span>

#### CFTDEST:

Command used to define a list of partners for broadcast/collect operations.
This list can be stored in a file or defined explicitly via the PART parameter
in the command.

<span id="CFTFILE"></span>

#### CFTFILE:

Command used to create or delete {{< TransferCFT/axwayvariablesComponentShortName  >}} files (parameter, partner,
catalog, accounting, log and communication files).

<span id="CFTLOG"></span>

#### CFTLOG:

Command used to set the log file management parameters for the monitor.

<span id="CFTMAIN"></span>

#### CFTMAIN:

Main {{< TransferCFT/axwayvariablesComponentShortName  >}} task used to schedule transfers. It controls
transfer execution and authorizations.

<span id="CFTNET"></span>

#### CFTNET:

Command used to define the properties of a network resource.

<span id="CFTPROT"></span>

#### CFTPROT:

Command used to define a file transfer protocol and the way in which
it is implemented.

<span id="CFTRECV"></span>

#### CFTRECV:

Command used to define the properties of a given type of transfer (storage
of the data received and execution of receive operations).

<span id="CFTSEND"></span>

#### CFTSEND:

Command used to define the properties of a given type of transfer (access
to the data to be sent and execution of send operations).

<span id="CFTTFIL"></span>

#### CFTTFIL:

File access tasks that can be activated concurrently. Depending on the
configuration, a CFTTFIL task can be permanent or will stop on completion
of the transfers for which it was created.

<span id="CFTTPRO"></span>

#### CFTTPRO:

Protocol task giving access to the resources defined via CFTNET commands
and the application-level protocols declared in CFTPROT commands.

<span id="CFTUTIL"></span>

#### CFTUTIL:

Batch or line mode tool giving access to all {{< TransferCFT/axwayvariablesComponentShortName  >}} functions
and parameters.

<span id="CFTXLATE"></span>

#### CFTXLATE:

Command used to define user conversion tables. Files can be converted
in both the send and receive modes, irrespective of the protocol used.

<span id="Ciphering"></span>

#### Ciphering:

Coding or encryption technique whereby information is protected against
illicit read operations.

The information can only be read or processed by entities in possession
of the ciphering "key". See also [Cryptography](#Cryptography).

<span id="Closed_mode_"></span>

#### Closed mode:

Mode whereby only the receiver can determine the physical name (location)
of the received file.

<span id="Collection"></span>

#### Collection:

Reception of a file or group of files from several partners via a single
request (list of partners defined in CFTDEST).

<span id="Command__parameter__"></span>

#### Command (parameter):

Command used to declare a monitor and set up the working environment.

<span id="Commands"></span>

#### Commands:

Instructions used in {{< TransferCFT/axwayvariablesComponentShortName  >}} to configure the monitor and working
environment and to control the monitor and associated transfers.

<span id="Compression"></span>

#### Compression

The types of compression are:

- ****01****:
    compression of a string of characters (compression of blank characters)
- ****02****:
    horizontal compression (repetitive characters are deleted in the record)
- ****04****:
    compression of characters (each alphanumeric character is compressed)
- ****08****:
    vertical compression (only the characters which are different from the
    previous record are transferred)

<span id="Communication_medium_"></span>

#### Communication medium:

File used to transmit the requests submitted to the monitor
(defined via CFTCOM, file created via CFTFILE TYPE=COM).

<span id="Confidentiality"></span>

#### Confidentiality:

Property resulting from ciphering data so that it cannot be read by
an unauthorized third party on the network.

<span id="Conversion"></span>

#### Conversion:

Translation of data, character by character, from one data coding system
to another (EBCDIC to ASCII for example). The conversion can take place
"ON LINE" during transmission, or "OFF LINE" (optionally
defined in the CFTXLATE, SEND/RECV, CFTSEND/CFTRECV and CFTPART commands).

<span id="Conversion_table_"></span>

#### Conversion table:

Table defining the correspondence between two coding systems and used
during conversion. The conversion table, defined for each transfer direction
and conversion mode, can be internal to {{< TransferCFT/axwayvariablesComponentShortName  >}} or created by the
user (CFTXLATE command).

<span id="Copilot"></span>

#### Copilot:

Graphic user interface for {{< TransferCFT/axwayvariablesComponentShortName  >}}.

<span id="Credit"></span>

#### Credit:

For the Odette protocol, maximum number of data packets that the sender
can transmit before the server must acknowledge reception by assigning
a new "credit".  

{{< TransferCFT/axwayvariablesComponentShortName  >}} simulates a synchronization point for each credit message
sent or received.

<span id="Cryptogram"></span>

#### Cryptogram:

Data resulting from a ciphering operation.

<span id="Cryptography"></span>

#### Cryptography:

Discipline comprising data transformation principles and mechanisms
with the aim of masking the meaning of data.

There are two main systems:

- Shared-key or symmetric
    systems for which the ciphering and deciphering keys are the same; see
    [DES](#DES__IBM_Data_Encryption_Standard__) (IBM Data Encryption
     Standard)

<!-- -->

- Dual-key or asymmetric
    systems for which the ciphering and deciphering keys are different; see[RSA](#RSA__Rivest_Shamir_Adelman__) (Rivest Shamir Adelman)

<span id="Cycle"></span>

#### Cycle:

Request to trigger a transfer at regular intervals. The interval can
be expressed in minutes, days or months (defined in CFTSEND, CFTRECV and
RECV).

##### D

<span id="Data_compacting_"></span>

#### Data compacting:

Data compression mode specific to the PeSIT protocol with a CFT profile.
Only the most significant half-bytes in numbers or spaces coded in EBCDIC
are transferred.

<span id="Data_compression_"></span>

#### Data compression:

Action whereby the volume of data sent and therefore the transfer times are
reduced.

<span id="Data_integrity_"></span>

#### Data integrity:

Property whereby the system guarantees that the data has not been accidentally
or intentionally modified or deleted.

<span id="DES__IBM_Data_Encryption_Standard__"></span>

#### DES (IBM Data Encryption Standard):

Cipher algorithm based on a symmetrical key. See also [Cryptography](#Cryptography).

<span id="Direct_transfer_"></span>

#### Direct transfer:

Transfer that does not use the store and forward mechanism.

<span id="Directory_exit_"></span>

#### Directory exit:

In server mode, exit substituting the standard controls applied by Transfer
CFT when a remote partner requests that a session be opened. In requester
mode, it is used to supply all information required to establish a connection
with a remote partner.

<span id="Dynamic_partner_"></span>

#### Dynamic partner:

Partner created when the connection is established, based on a model
partner (defined in CFTPART).

##### E

<span id="EERP__End_to_End_Response__acknowledgement_"></span>

#### EERP (End to End Response) acknowledgement:

Odette protocol service used to generate an end-to-end acknowledgement
for the transfer at application level. In {{< TransferCFT/axwayvariablesComponentShortName  >}}, this acknowledgement
takes the form of a reply-type message (see [Reply
message](#Reply_message_)).

<span id="End_of_transfer_exit_"></span>

#### End of transfer exit:

Exit called at the end of a file or message transfer to automate any
type of processing deemed necessary after the transfer.

<span id="End_of_transfer_procedure_"></span>

#### End of transfer procedure:

Procedure to be submitted by the monitor when a file or message has
been successfully sent or received (defined in CFTPARM, CFTSEND or CFTRECV).

<span id="Environment_variable_"></span>

#### Environment variable:

Information that can be accessed by programs via a predefined symbolic
name and that contains a character string initialized when the execution
environment was set up for the programs (OS-dependent feature).

<span id="Error_procedure_"></span>

#### Error procedure:

Procedure executed when a transfer is aborted or interrupted (defined
in CFTPARM).

<span id="Exit"></span>

#### Exit:

User function, in the form of a subprogram, called directly by the monitor
(defined in CFTEXIT).

##### F

<span id="File_exit_"></span>

#### File exit:

Exit used to take control at various stages of a file transfer.

<span id="File_identifier_"></span>

#### File identifier:

See [IDF](#IDF).

<span id="File_transfer_protocol_or_application_protocol_"></span>

#### File transfer protocol or application protocol:

Set of rules governing dialog and exchange management between two standard
{{< TransferCFT/axwayvariablesComponentShortName  >}}s. These standards are based on a network access mode defined in CFTPROT. See
[PeSIT](#PeSIT__Protocole_d_Echanges_pour_un_Syst_me_Interbancaire_de_T_l___compensation__).

<span id="FPDU__File_Protocol_Data_Unit__"></span><span id="FPDU"></span>

#### FPDU (File Protocol Data Unit):

Data unit for the PeSIT protocol. Protocol-level message supporting
dialog between two PeSIT "protocol layer" entities. It includes
a fixed and a variable part. See also [PI](#PI__Parameter_Identifier__)
(Parameter Identifier).

##### H

<span id="Horizontal compression:"></span>

#### Horizontal compression:

Replacement of characters repeated within a record by a delimiter, a
counter or a single occurrence of the character.

##### I

<span id="Identification"></span>

#### Identification:

Presentation phase at the start of a connection.

The partners submit their names and passwords and the connection is
then either accepted or refused.

<span id="IDF"></span>

#### IDF:

Model file identifier. See also [Model file](#Model_file_).

<span id="IDT"></span>

#### IDT:

Transfer identifier. Label associated with each transfer for a given
partner and in a given direction.

<span id="IDTU__catalog_identifier__"></span>

#### IDTU (catalog identifier):

Unique identifier assigned to a transfer by {{< TransferCFT/axwayvariablesComponentShortName  >}}. It is used
in local transfer monitoring and catalog list commands.

<span id="Implicit_send_"></span>

#### Implicit send:

Mechanism used to make a file available on a permanent basis (CFTSEND
IMPL=YES). See also Availability for the partner.

<span id="Integrity_checking_"></span>

#### Integrity checking:

Process whereby data is protected against intentional deletion or tampering
by an unauthorized third party on the network.

##### L

<span id="List_exit_"></span>

#### List exit:

File exit used by remote partners to query the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog
on a host.

<span id="List_of_model_file_identifiers_"></span>

#### List of model file identifiers:

See [Authorized identifiers](#Authorized_identifiers_).

<span id="Log_file_"></span>

#### Log file:  

File containing all messages recording monitor activity related events
(defined via CFTLOG, file created via CFTFILE=LOG).

##### M

<span id="Mailbox"></span>

#### 

<span id="Message"></span>

#### Message:

Character string specified via a send command and sent by the monitor
in a specific transfer. In {{< TransferCFT/axwayvariablesComponentShortName  >}} and for given protocols, two type
of messages can be sent: simple messages or reply messages.

<span id="Model_file_"></span>

#### Model file:

Each file transfer is processed by {{< TransferCFT/axwayvariablesComponentShortName  >}} in line with a model
file or file identifier (IDF) associated with the data (or physical file)
to be sent. The processing and data file definition parameters are specified
in the CFTSEND configuration command for send operations and in the CFTRECV
command for receive operations.

<span id="Monitor__transfer_or_CFT_monitor__"></span>

#### Monitor ( {{< TransferCFT/axwayvariablesComponentShortName  >}}):

Control programs scheduling transfer requests, file access operations
and transfers using network communication facilities.

<span id="Monitoring"></span>

#### Monitoring:

Process whereby {{< TransferCFT/axwayvariablesComponentShortName  >}} activities are controlled by querying the catalog
(transfer properties and state) and the log file (chronological history
of all events in information or error message form).

##### N

<span id="Network_resource_"></span>

#### Network resource:

Entity via which connections can be established (defined in CFTNET).

<span id="NSDU"></span><span id="NSDU__Network_Service_Data_Unit__"></span>

#### NSDU (Network Service Data Unit):

Data unit for the network service in the PeSIT protocol. It is made
up of the data exchanged between two users and can contain several [FPDUs](#FPDU__File_Protocol_Data_Unit__). See also ****rrusize****
and ****srusize****.

##### O

<span id="ODETTE"></span>

#### ODETTE:

European body in the automotive industry governing transfer exchange
standardization (Organisation de Données Echangées par TéléTransmission
en Europe - European Organization for Data Transmission). This organization
defined the Odette File Transfer Protocol (OFTP) used particularly to
exchange data between car manufacturers and their suppliers. This protocol
is based on X.25-type links (TCP/IP in future versions). The two terms
OFTP and ODETTE are sometimes used interchangeably.

<span id="OFTP__Odette_File_Transfer_Protocol__"></span>

#### OFTP (Odette File Transfer Protocol):

See [ODETTE](#ODETTE).

<span id="Open_mode_"></span>

#### Open mode:

Mode whereby a requester can determine, in send mode, the physical name
to be assigned to the transferred file on the receiver site and in receive
mode, the physical name of the file to be received.

##### P

<span id="Partner"></span>

#### Partner:

Logical entity, to which {{< TransferCFT/axwayvariablesComponentShortName  >}} can send data and from which it
can receive data. A partner generally corresponds to a remote file transfer
monitor or a subset of a remote monitor (defined in CFTPART, file created
in CFTFILE TYPE=PART).

 

<span id="PeSIT__Protocole_d_Echanges_pour_un_Syst_me_Interbancaire_de_T_l___compensation__"></span>

#### PeSIT (Protocole d'Echanges pour un Système Interbancaire de Télé- compensation):

Exchange protocol for an interbank clearing system, first used within
the SIT network and then enhanced by defining broader usage profiles. The non-SIT
and secured profiles can be used for non-banking applications.

<span id="PI__Parameter_Identifier__"></span>

#### PI (Parameter Identifier):

Number identifying the parameter type carried in a PeSIT FPDU.

<span id="Programming_interface_"></span>

#### Programming interface:

Set of rules defining how a service is used by an application. See [CFT API](#CFT_API_) .

##### R

<span id="Reading_or_read_mode_transfer_"></span>

#### Reading or read-mode transfer:

For the PeSIT protocol, triggering by the requester of a file transfer
from the server.

<span id="RECV"></span>

#### RECV:

Command triggering a file receive operation. The transfer properties
must be defined in the RECV command itself or in the associated CFTRECV
command.

<span id="Reply_message_"></span>

#### Reply message:

Character string used in the PeSIT E, PeSIT D (CFT profile) or Odette
protocols and corresponding to a reply to a previous transfer from the
remote partner concerned by the message. Used for example at the end of
a file transfer to inform the sender that all receive-related operations
have been successfully executed.

<span id="Request_"></span>

#### Request:

Local request submitted to the communication medium but not yet recorded
in the catalog file by the monitor.

<span id="Requester"></span>

#### Requester:

Partner initiating the connection.

<span id="Requester_mode_"></span>

#### Requester mode:

Mode whereby the monitor initiates the connection with the remote partner.

<span id="Requester/receiver"></span>

#### Requester/receiver:

Partner or application requesting the connection and wishing to receive
a file.

<span id="Requester/sender"></span>

#### Requester/sender:

Partner or application requesting the connection and wishing to send
a file.

<span id="Resynchronization"></span>

#### Resynchronization:

Protocol-based mechanism whereby an exchange can be restarted from the
last synchronization point acknowledged.

This mechanism is used during data transfer following a recoverable
incident that did not cause the transfer to abort (ODETTE and PeSIT).

<span id="Routing"></span>

#### Routing:

See [Store and forward mode](#Store_and_forward_mode_) and
[Store and forward mechanism](#Store_and_forward_mechanism_).

<span id="RSA__Rivest_Shamir_Adelman__"></span>

#### RSA (Rivest Shamir Adelman):

Cryptographic algorithm based on asymmetric keys. See also [Cryptography](#Cryptography).

##### S

<span id="Secondary"></span>

#### Secondary file:

File used when switching log or accounting files. See [Switching.](#Switching_)

<span id="Secured_mode_"></span>

#### Secured mode:

Means whereby data can be ciphered or sealed using DES- or RSA-type
algorithms. See [Ciphering](#Ciphering), [DES](#DES__IBM_Data_Encryption_Standard__)
(IBM Data Entrcyption Standard) and [RSA](#RSA__Rivest_Shamir_Adelman__)
(Rivest Shamir Adelman).

<span id="Segmentation"></span>

#### Segmentation:

For the PeSIT protocol, possibility of transferring records using successive
data units (NSDU).

<span id="SEND"></span>

####  SEND:

Command used to submit a file, simple message or reply send request.
The transfer properties can be declared in the SEND command itself or
in the associated CFTSEND command.

<span id="SEND__implicit__"></span>

#### SEND (implicit):

See [Implicit send](#Implicit_send_) and [Availability
for the partner](#Availability_for_the_partner_).

<span id="Send_on_hold_"></span>

#### Send on hold:

Explicit provision of a file to be sent to a partner. See also [Availability
for the partner](#Availability_for_the_partner_).

<span id="Sending_of_a_group_of_files_"></span>

#### Sending of a group of files:

Sending of several files in a single request, using either an indirection
file containing the list of files or a wildcard-based selection.

<span id="Server_"></span>

#### Server:

Partner receiving and accepting the connection.

<span id="Server_mode_"></span>

#### Server mode:

Mode whereby the monitor responds to an incoming connection and accepts
the transfer requests.

<span id="Server_receiver_"></span>

#### Server/receiver:

Partner receiving and accepting the connection and then receiving the
file.

<span id="Server_sender_"></span>

#### Server/sender:

Partner receiving and accepting the connection and then sending the
file.

<span id="Simple_message_"></span>

#### Simple message:

Character string used with the PeSIT E protocol
to exchange small volumes of freeform information between partners.

<span id="Software_protection_"></span>

#### Software protection:

Key associated with the contractual conditions in which the software
is used and provided when the product is installed. To use {{< TransferCFT/axwayvariablesComponentShortName  >}},
the key must be specified in the CFTPARM command.

<span id="Store_and_forward_mechanism_"></span>

#### Store and forward mechanism:

Routing technique used by {{< TransferCFT/axwayvariablesComponentShortName  >}}, whereby a file or message is
received on a store and forward site and immediately forwarded to the
next adjacent partner.

<span id="Store_and_forward_mode_"></span>

#### Store and forward mode:

{{< TransferCFT/axwayvariablesComponentShortName  >}} feature whereby files can be routed via one or more intermediary
{{< TransferCFT/axwayvariablesComponentShortName  >}} sites, called store and forward sites. This feature is only
available from a requester/sender (write-mode transfer). See [Store
and forward mechanism](#Store_and_forward_mechanism_).

<span id="Store_and_forward_site_"></span>

#### Store and forward site:

See [Store and forward mode](#Store_and_forward_mode_) and
[Store and forward mechanism](#Store_and_forward_mechanism_).

<span id="Switching_"></span>

#### Switching:

Operation whereby the monitor log or accounting files are swapped so
that two versions are used alternately (operator SWITCH command).

<span id="Symbolic_variable_"></span>

#### Symbolic variable:

{{< TransferCFT/axwayvariablesComponentShortName  >}} variable representing a data item associated with the transfer
and for which the value will only be known when the transfer is executed
and not when the parameters are set.

<span id="Synchronization_interval_"></span>

#### Synchronization interval:

Maximum volume of data that a sender can transmit between two consecutive
synchronization points. If the data is compressed, the interval calculation
is based on the volume of data after compression. This concept is used
for the PeSIT protocols.

<span id="Synchronization_point_setting_"></span>

#### Synchronization point setting:

Means whereby the sender transmits markers assigned sequence numbers
during data transfer, these markers being acknowledged by the receiver
and allowing the transfer to be restarted by either end following an incident.

##### T

<span id="Timeslot_"></span>

#### Timeslot:

Time during which calls are authorized to or from a partner (defined
in CFTPART, CFTSEND or CFTRECV).

<span id="Transfer_"></span>

#### Transfer:

Data transport and exchange of the actions to be taken on the data (read/write/create/delete)
from one computer (partner) to another via a network. One of the partners
is the SENDER and the other the RECEIVER.

<span id="Transfer_identifier_"></span>

#### Transfer identifier:

See [IDT](#IDT).

<span id="Transfer_owner"></span>

#### Transfer owner:

A transfer owner is associated with each transfer. See ****userid****.

<span id="Transfer_restart_"></span>

#### Transfer restart:

Protocol-based mechanism whereby a transfer can be restarted at the
point at which it was interrupted. Depending on the protocol used, the
transfer restarts at the current record (ODETTE) or at the last synchronization
point acknowledged (PeSIT) prior to the transfer interrupt.

<span id="Transfer_resumption_"></span>

#### Transfer resumption:

Means whereby a suspended transfer can be resumed either automatically
or manually.

<span id="Transfer_scheduling_"></span>

#### Transfer scheduling:

Process whereby transfers are executed in line with their priority,
the number of concurrent transfers allowed and the timeslot or period
(cycle) defined. See also [Cycle](#Cycle) and [Timeslot](#Timeslot_).

<span id="Transfer_state_"></span><span id="transfer_states"></span>

#### Transfer state:

Transfers can be set to one of six states.

- ****C****:
    The transfer is in progress (****C****urrent).

<!-- -->

- ****D****:
    The transfer is available (at ****D****isposal)
    and will be triggered automatically as soon as the {{< TransferCFT/axwayvariablesComponentShortName  >}} resources
    and partner access authorizations allow it.
- ****H****:
    The transfer is pending (on ****H****old)
    on the initiative of the transfer requester, an operator (HALT command)
    or the monitor subsequent to an incident. The transfer can be resumed
    by the operator or the remote partner.
- ****K****:
    The transfer is pending (****K****ept)
    on the initiative of the transfer requester, an operator (KEEP command)
    or the monitor subsequent to an incident. The transfer can only be resumed
    by the operator.
- ****T****:
    The transfer has successfully ****T****erminated.
- ****X****:
    All end of transfer operations have been successfully e****X****ecuted
    and the monitor has been notified via the END command.

##### V

<span id="VC_"></span>

#### VC:

Virtual circuit assigned a connection identifier and maintained throughout
the transfer (pipe, window mechanism).

<span id="Vertical_compression_"></span>

#### Vertical compression:

Comparison between the current record and the previous record with the
aim of transferring only those characters that are different.

##### W

<span id="Writing_or_write_mode_transfer_"></span>

#### Writing or write-mode transfer:

For the PeSIT protocol, sending of a file from the requester to the
server.
