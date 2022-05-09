{
    "title": "Using transcoding",
    "linkTitle": "Using transcoding",
    "weight": "340"
}This page describes transcoding concepts with Transfer CFT.

Transcoding is the conversion of one type of encoding to another. With Transfer CFT there are two ways to transcode:

- Basic transcoding (FCODE/NCODE) uses a translation table between 2 computers that use a different alphabet. Each table consists of a file containing a unique 256-character record, where each character defines the correspondence between two alphabets via its position and value.
- Extended transcoding (FCHARSET/NCHARSET) uses iconv libraries to convert one character encoding to another.

You can use either a CFTXLATE table or extended transcoding to implement transcoding with {{< TransferCFT/axwayvariablesComponentLongName  >}}, but you can not use both methods simultaneously.

Using CFTXLATE tables
---------------------

<span id="Create"></span>

### Default translation tables

By default, {{< TransferCFT/axwayvariablesComponentShortName  >}} provides 4 internal ASCII/EBCDIC translation
tables, two for each transfer direction. These tables are bijective and correspond loosely with a translation between code pages EBCDIC 1047 and ASCII 437, with some small differences.

### Define a translation table

Use the CFTXLATE object to define translation tables between 2 alphabets. A definition includes:

- Transfer direction (DIRECT: SEND, RECV, or BOTH)
- File data code
    type (FCODE: ASCII or EBCDIC)
- Data network
    code type (NCODE: ASCII or EBCDIC)
- Translation table file name (FNAME)

See [Define translation tables](translation_table_concepts) for step instructions.

Using extended character sets
-----------------------------

Character transcoding (using extended character sets) defines how data are encoded during the transfer process. This is important when transferring files that do not have the same coding requirements on the sending and receiving systems.

### What is extended transcoding?

Typical transcoding, using either XLATE or internal transcoding (ASCII/EBCDIC/BINARY) methods, extends in Transfer CFT to include multi-bytes encoding. Note that using XLATE consumes less CPU than the NCHARSET/FCHARSET method, however XLATE is restricted to single-byte character sets.

The FCHARSET parameter defines the local file encoding, and the NCHARSET
parameter defines the remote and network data encoding. These parameters, FCHARSET and NCHARSET, are available for the following objects:

- SEND/RECV
- CFTSEND/CFTRECV
- CFTPART
- CFTPROT

See [Using extended character sets](use_extended_character_sets) for step instructions.
