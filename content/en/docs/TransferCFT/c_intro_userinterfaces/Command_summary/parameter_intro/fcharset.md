{
    "title": "fcharset",
    "linkTitle": "fcharset",
    "weight": "1070"
}<span id="fcharset"></span>

### fcharset

#### SEND, RECV, CFTSEND CFTRECV CFTPROT CFTPART

**[FCHARSET = { None / CFT_EBCDIC-FR / CFT_ UTF-8 / UTF-16BE / UTF-16LE / CFT_ISO8859-1 / ASCII / EBCDIC / UTF-8 / ISO8859-1 / . . . }]**

Defines the local file encoding.  

- On the sender side, fcharset is used to read a file. You then use the ncharset to encode this into network data when sending a file.
- On the receiver side, fcharset is the local file encoding used to write a file that has this type of encoding. The ncharset is used to decode network data when receiving a file.

**Example 1**

To translate a local text file before sending it, for example from UTF-8 to UTF-16, using Transfer CFT mapping:

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

See also [ncharset](../ncharset) and *[Using character sets for transcoding](../../../../concepts/transfer_command_overview/using_transcoding/use_extended_character_sets).*

[Return to Command index](../../)
