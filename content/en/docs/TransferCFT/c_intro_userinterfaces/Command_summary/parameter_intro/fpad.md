---

    title: fpad
    linkTitle: fpad
    weight: 1290

---
### fpad

#### SEND, CFTSEND, RECV and CFTRECV    TYPE = FILE

****\[ FPAD = string \]****

This parameter defines the padding character at the file level.

- Fixed format: Specifies the character to use for padding using FPAD at file level. If FPAD is not set, the padding character depends on the FCODE used. When FCODE=ASCII it is an ASCII space, FCODE=EBCDIC it is an EBCDIC space, and FCODE=BINARY it is a binary zero.
- Variable format: Defines the character to use to unpad the record. If FPAD is not set, the record is unchanged.

> **Note**
>
> In addition to printable characters, you can also enter a non-printable character for NPAD or FPAD using the hexadecimal syntax: 0xHH. For example: 0xFC, 0xab, 0x00

For information on [padding](../../../../concepts/transfer_command_overview/padding) at the network level see [NPAD](../npad).

[Return to Command index](../../)
