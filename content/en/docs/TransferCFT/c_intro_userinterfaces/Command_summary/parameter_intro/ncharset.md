{
    "title": "ncharset",
    "linkTitle": "ncharset",
    "weight": "2130"
}<span id="ncharset"></span>

### ncharset

#### SEND, RECV, CFTSEND, CFTRECV, CFTPROT, CFTPART

******[ NCHARSET = { None / CFT_EBCDIC-FR / CFT_ UTF-8 / CFT_ISO8859-1 / ASCII / EBCDIC / UTF-8 / UTF-16BE / UTF-16LE / ISO8859-1 / . . . }]******

The ncharset is the destination file encoding that is used on a file
to encode or decode network data.

- On the sender side, fcharset is used to read a file. You then use the ncharset to encode this into network data when sending a file.
    &lt;/li&gt;
- On the receiver side, fcharset is the local file encoding used to write a file that has this type of encoding. The ncharset is used to decode network data when receiving a file.
    &lt;/li&gt;

**Example 1**

To translate a local text file before sending it, for example from UTF-8 to UTF-16 using Transfer CFT mapping:

```
CFTUTIL SEND PART = NEWYORK,
IDF = TEST_UTF-8,
FCHARSET = CFT_UTF-8,
NCHARSET = CFT_UTF-16,
FTYPE = T
```

**Example 2**

To translate a local text file before sending it, for example from UTF-8 to UTF-16:

```
CFTUTIL SEND PART = NEWYORK,
IDF = TEST_UTF-8_2,
FCHARSET = UTF-8,
NCHARSET = UTF-16,
FTYPE = T
```

If an error occurs while trying to transcode a file, this results in a DIAGI=154 (DIAGP=ERRTRCOD).

See also [fcharset](../fcharset) and *[Using character sets for transcoding](../../../../concepts/transfer_command_overview/using_transcoding/use_extended_character_sets)*.

[Return to Command index](../../)
