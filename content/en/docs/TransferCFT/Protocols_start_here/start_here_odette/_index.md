{
    "title": "OFTP  (ODETTE): Start here",
    "linkTitle": "OFTP protocol",
    "weight": "130"
}This section describes how to configure the OFTP (ODETTE) protocol for
Transfer CFT. This topic begins
with an overview of the OFTP file transfer protocol describing:

- OFTP
    (Odette) features
- Network
    message structure

This section is comprised of the topics listed in the table below, which
describe the following OFTP (ODETTE) functions in Transfer CFT and how
to configure them:

- File transmission
    and reception
- Transfer restart
- Data compression
- Change direction (CD)


| Topic  | Describes...  |
| --- | --- |
| [Configuring OFTP](configuring_odette) | The Transfer CFT objects that you must configure to use the OFTP (ODETTE) protocol. |
| [Configuring partners](cftpart_parameters) | The Transfer CFT parameters that you must define for a partner when using the OFTP protocol. |
| [Processing data](processing_data) | The compression functions in Transfer CFT when using the OFTP (ODETTE) protocol. |
| [Submitting a transfer request](submitting_a_transfer_request) | The steps that enable you to submit a transfer request. |
| [Receiving transfers](receiving_transfers) | How to configure the OFTP (ODETTE) environment you must define the transfer environment in the RECV object to receive a request. |


<span id="About_OFTP"></span><span id="OFTP__ODETTE__features"></span>

OFTP (ODETTE) features
----------------------

OFTP (ODETTE) protocol standards were defined to facilitate the transmission
of information between partners, or centers, and the motor vehicle industry.
These standards define not only the exchange protocol, but also the content
of the information exchanged and the communication procedure to be established
between the two sites.

Transfer CFT complies with the exchange protocol, but does not check
the information. This enables you to use the protocol outside of the automobile
sector.

The data can be checked by an application initiated by the end-of-transfer
procedures.

### General features

The general characteristics of the OFTP (ODETTE) protocol are as follows:

- Two-way exchanges:
    transmission and reception
- Transfer retry
    in the event of an incident
- Data compression,
    character compression
- Change direction
    possibility between partners
- File reception
    acknowledgement possibility

### Specific features

The particular characteristics of the OFTP (ODETTE) protocol are as
follows:

- Only files supported
    by the protocol have a sequential organization
- Sent files are
    identified by their name and the transfer date and time
- The protocol supports
    four record formats:
    -   F
        for fixed format: all the file records have the same size
    -   V
        for variable format: the file records have a variable length
    -   U
        for unstructured format: the entire file is considered as a single
        character string. In this file format, the concept of record length
        does not exist
    -   T
        for text format: a text file is defined as a sequence of ASCII characters
        containing no control characters, except for CR/LF characters delimiting
        records

These format concepts, in the protocol meaning of the term, correspond
to:

- Various files types
    NTYPE
- Record lengths
    NLRECL
- File formats NRECFM
- Data codes NCODE

<span id="Network_message_structure"></span>

Network message structure
-------------------------

A protocol message is called a FPDU, a File
Protocol Data
Unit. This message has a particular
structure for the OFTP (ODETTE) protocol.

The size of the buffers of the network resources associated with the
OFTP protocol, is deduced from the maximum values of the RRUSIZE and SRUSIZE
parameters of the CFTPROT object.
