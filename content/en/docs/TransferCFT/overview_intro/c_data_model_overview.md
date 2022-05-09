{
    "title": "Data models and partners",
    "linkTitle": "About data models and partners ",
    "weight": "170"
}The purpose of {{< TransferCFT/axwayvariablesComponentShortName  >}} is to exchange and
manage files transfers, meaning a file
exchange between two or more computers. In any file exchange, one computer is the sender,
and another is the receiver. The two computers are linked together by
a network (TCP/IP). A file transfer consists of sending:

- A file, group of
    files, or message
- Technical parameters
    associated with the file

Prior to creating new data flows with {{< TransferCFT/axwayvariablesComponentShortName  >}}, you should first familiarize yourself with a few key terms and concepts. This topic introduces the partner definitions, network and protocol concepts, and basic send and receive command parameters.

<span id="Defining_partners__Start_here"></span><span id="CFTPART___Definition_of_a_Partner"></span>

What is a {{< TransferCFT/axwayvariablesComponentShortName  >}} partner?
-----------------------------------------------------------------------------

In {{< TransferCFT/axwayvariablesComponentShortName  >}} a partner is
a logical entity such as a bank, a government agency, or trading partner, which can
be the origin or destination of a data exchange, and corresponds
to a remote file transfer controller.

When you define a partner you require two sets of information to create a minimal configuration, the partner and the network definitions - the CFTPART and CFTxxx objects - respectively.

> **Note**
>
> Note: For the Transfer CFT network definition, the typical usage is CFTTCP.

### Partner definition

The CFTPART/CFTTCP commands describe the following required characteristics of each partner's environment:

- Network
- Protocol
- Security

<span id="Associated_configuration_parameters"></span>

### Associated parameters

For each partner definition some of the parameters in the partner description
refer to the local environment, while others refer to the remote environment. This means that each partner definition consists of a mix of parameters, some of which relate expressly to your remote partner, and others that are specific to your local environment.

### Basic definition

The partial list of parameters described in this section are required for a new partner, but the default values for most other parameters typically are sufficient for a new user.

You can use the CFTPART command to create, modify, or delete a partner. Within the CFTPART command, you define the following parameters:

- SAP: The remote port (the partner's listening port)
- NRPART: The name of the remote partner, which must be the same identifier as the partner's NSPART
- NSPART: The local partner - your local name, which must match the remote partner's NRPART
- You also need to refer to a protocol definition, the CFTPROT object in your local environment, which you and the partner have agreed upon

> **Note**
>
> Tip  
> Think of the NSPART partner as having an "S" for self, and NRPART having an "R" for the remote partner.

For your network connection, CFTTCP, you should define the:

- ID: Partner id
- Host: Hostname or IP address of the remote partner
- Class: this refers to the network used, as defined in your local environment by a CFTNET object

### {{< TransferCFT/axwayvariablesComponentShortName  >}} protocols

{{< TransferCFT/axwayvariablesComponentShortName  >}} supports the following standard protocols:

- PeSIT
- OFTP
    (ODETTE)

For each protocol used, there are corresponding values for the
parameters controlling the size of messages, the compression
of data, the possibility of resuming a transfer after an incident, etc.

Data flow modes
---------------

Transferring files consists of defining characteristics for partners and your flows. The previous section provided details on basic partner parameters and concepts. This section defines the concepts you will need in order to create data flows.

There are 4 {{< TransferCFT/axwayvariablesComponentShortName  >}} modes that relate to data transfer. Two modes relate to uploading or downloading data, and the other two relate to who instigates the flow. We'll begin by discussing who begins the data flow conversation.

- ****Requester mode****: This is the partner that initiates the connection. It is important to note that this is the beginning of the protocol connection and not the start of a transfer. The requester waits for the server (remote partner) to accept the connection. They can then mutually identify, and depending on the definition, exchange data transfer attributes.
- ****Server mode****: This partner is available for transfer requests coming from the network. If it recognizes the partner and protocol, it can accept and proceed with recording the request in the catalog.

The next two mode definitions refer to which partner is sending the data, either a message or a file. The terms used in {{< TransferCFT/axwayvariablesComponentShortName  >}} are:

- ****Sender mode****: Transfers or uploads the available data to a remote partner.
- ****Receiver**** **mode**: Downloads available data, either a file or message, from the partner.

Additional parameters define attributes for scanning folders for files, updating the catalog, creating transfer records, and so on.

Model files
-----------

Every file transfer requires a model as part of the data flow definition. By default, {{< TransferCFT/axwayvariablesComponentShortName  >}} provides a model so that you do not have to create this object, the IDF (logical file identifier), in order to perform your first file transfers. Once you feel comfortable with {{< TransferCFT/axwayvariablesComponentShortName  >}} commands and configuration concepts, you can modify the model attributes, such as the file name, translation, compression, end-of-transfer scripts, and so on.

- When sending, the CFTSEND ID describes the characteristics of the local file to send
- When receiving, the CFTRECV ID describes the characteristics of the file to receive locally

File transfer concepts
----------------------

Most of the tasks that {{< TransferCFT/axwayvariablesComponentShortName  >}} performs
involve configuring and executing file transfers, as well as processing
information if necessary.

### Commands for sending and receiving data

{{< TransferCFT/axwayvariablesComponentShortName  >}} provides models for sending and receiving data:

- CFTSEND: Defines the model for sending transfers
- CFTRECV: Defines the model for receiving transfers

From there you can use SEND and RECV commands to perform file and message transfers, using default settings or new object definitions.

> **Note**
>
> Tip  
> Transfer CFT delivers configuration samples with each installation that allow you to begin performing transfers with a minimum of information. Refer to the dedicated sections in this User's Guide for information on the more complex parameter settings for your flows.

### {{< TransferCFT/axwayvariablesComponentShortName  >}} to application communication

This paragraph overviews how communication between {{< TransferCFT/axwayvariablesComponentShortName  >}} and applications occurs.

Application to
{{< TransferCFT/axwayvariablesComponentShortName  >}}:

- Requests
    submitted through the programming interface
- Requests
    submitted through the {{< TransferCFT/axwayvariablesComponentShortName  >}} utility or GUI

{{< TransferCFT/axwayvariablesComponentShortName  >}} to application:

- An end-of-transfer
    or transfer error procedure is activated
- A log file
    or statistics file switching procedure is activated
- The catalog
    is queried by the programming interface

{{< TransferCFT/axwayvariablesComponentShortName  >}} needs certain objects to be
defined to enable a file transfer. The objects that you define supply
the following types of information:

- Name of file to
    transfer
- Transfer owner
- Physical file attributes
- Protocol used for
    the transfer
- Compression or
    encryption options
- Behavior in case
    of transfer failure
- Transfer scheduling,
    restarting and routing

<span id="Creating_the_default_SEND_template__Start_here"></span>

About model templates for sending and receiving
-----------------------------------------------

### Send models

In {{< TransferCFT/axwayvariablesComponentShortName  >}} you create the
CFTSEND template object, used to specify the default values for:

- The name and local
    physical characteristics of the file to send
- The network characteristics
    of the file to send to the partner
- The actions to
    perform locally during and after a transfer (translation, compression,
    call to a user EXIT, an end-of-transfer procedure...)
- An authorized time
    slot and default user associated with the transfers

<span id="Receive_file_parameter_summary"></span>

### Receive models

In {{< TransferCFT/axwayvariablesComponentShortName  >}} you create the
CFTRECV template object, used to specify the default values for:

- Give the default
    name and local physical characteristics of the file to receive
- Define the default
    actions to perform locally during and after the transfer (translation,
    compression, call to a user EXIT, an end-of-transfer procedure...)
- Authorize the default
    time slot and default user associated with the transfers
