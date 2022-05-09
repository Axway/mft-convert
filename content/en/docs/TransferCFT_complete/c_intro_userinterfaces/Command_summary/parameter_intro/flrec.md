{
    "title": "flrecl",
    "linkTitle": "flrecl",
    "weight": "1190"
}<span id="flrecl"></span>

### flrecl

****[FLRECL =  0  &#124; {0...32767} ]   ****

The file attribute check option rejects an incoming
transfer if local file attributes do not match the virtual file attributes
(logical record length and record format attributes).

File transfer protocols such
as PeSIT and OFTP use the virtual file concept, which provides a common file organization model suitable
for all computers. Each
side involved in a file transfer, the sender and the receiver, manages
translations between real computer files and the virtual files transferred
by the protocol.

The {{< TransferCFT/axwayvariablesComponentShortName  >}} sender maps the virtual file attributes according to the SEND,
CFTSEND cards and the physical file attributes. For example, if the FLRECL
attribute is not set in the SEND/CFTSEND cards, the logical record length
of the local file is used to set the logical record length of the virtual
file (on a platform where logical record length of a local file has no
meaning, such as UNIX or Windows, the arbitrary value of 512 is used).

The {{< TransferCFT/axwayvariablesComponentShortName  >}}
receiver maps the physical file attributes according to the RECV,
CFTRECV and the virtual file attributes. For example, if the FLRECL attribute
is set the logical record length of the local file will be the FLRECL
value. If it does not match the logical record length of the virtual file,
records are truncated or padded.

<span id="flrecl_CFTRECV"></span>

#### CFTRECV, RECV

****[FLRECL = { 0
&#124; n} ]    {0...32767}****

For records in:

- Fixed
    format (FRECFM = F): length (in bytes) of the records of the receiver
    file
- Variable
    format (FRECFM = V): maximum length (in bytes) of the records of this
    file
- Undefined
    format (FRECFM = U) : maximum length (in bytes) of the records of this
    file

During a receive transfer, any record LARGER
than the one declared by FLRECL, is truncated to the value of FLRECL.

For fixed format records (FRECFM = F), if the size of the records received
is LESS THAN the file record length, these records are padded up to the
nominal value:

- By binary zeros
    (x00) when the local data is declared in binary  
    (FCODE = BINARY),
- By spaces when
    the local data is declared as alphanumeric, with:
    -   FCODE =
        EBCDIC : the space character is then equal to x‘40’ (hexadecimal)
    -   FCODE =
        ASCII : the space character is then equal to x‘20’

Default record lengths implemented on some
systems, length taken into account if the corresponding information is
not supplied either by the file sender or by the local parameters:

- 512 for text files (FTYPE= T, O, or X)
- 4096 for binary files (FTYPE=B)
- 4096 for stream text files (FTYPE=J)

<span id="flrecl_CFTSEND"></span>

#### CFTSEND, SEND

****[FLRECL =  {0...32767} &#124; n} ]   ****

For records in:

- Fixed
    format (FRECFM = F): length, in bytes, of the records of the local file
    to be sent
- Variable
    format (FRECFM = V): maximum length, in bytes, of the records of this
    file
- Undefined
    format (FRECFM = U) : maximum length, in bytes, of the records of this
    file

The use of FLRECL is optional:

- The
    specific *Operations Guides* specify whether this facility is supported
    for each system
- Some
    systems generate implicit record lengths in place of this feature:
    -   512 for text files (FTYPE= T, O, or X)
    -   4096 for binary files (FTYPE=B)
    -   4096 for stream text files (FTYPE=J)

[Return to Command index](../../)
