---
    "title": "Using extended character sets",
    "linkTitle": "Using extended character sets ",
    "weight": "370"
---
Character transcoding (using extended character sets) defines how data are encoded during the transfer process. This is important when transferring files that do not have the same coding requirements on the sending and receiving systems.

<span id="Using"></span>

Using character sets for transcoding
------------------------------------

You can set the FCHARSET and NCHARSET values using the Transfer CFT character sets mapping (CFT_UTF-8, CFT_UTF-16, CFT_ISO8859-1, ...) or directly using most available iconv charsets (UTF-8, UTF-16, ISO8859-1, ...).

{{% TransferCFT/snippets/uconf_code%}}

For example, if you define an NCHARSET as CFT_ISO8859-1, the parameter is translated as 'iso88591' for HPUX OSes, as '00819' for IBM i, and as 'ISO8859-1' for all other platforms. We strongly recommend that you use this option.

> **Note**
>
> Note: On versions of OS/400 prior to Transfer CFT 3.1.3, use the charset number only without the CFT_ prefix.

### Using the //IGNORE functionality

In some cases you may want to disregard characters either in a file to be sent, or in a file to be received. You can use the //IGNORE functionality to disregard characters that you do not want to be present in the target.

Depending on your operating system, note the following specific //IGNORE behavior:

- Sun: converts the character to a question mark '?'
- AIX and HPUX: converts the character to the substitute character (the control character 1A hex)
- All other systems: no character substitution occurs

**Example 1**

Use the Transfer CFT mapping and the IGNORE functionality to translate a local text file before sending it, for example from UTF-8 to ISO8859-1:

```
CFTUTIL SEND PART = NEWYORK,
  IDF = TEST_IGNORE,
 FCHARSET = CFT_UTF-8,
  NCHARSET = CFT_ISO8859-1//IGNORE,
 FTYPE = T
```

**Example 2**

Use the Transfer CFT mapping and the IGNORE functionality to translate a received file, for example from UTF-8 to ISO8859-1:

```
CFTUTIL RECV PART = NEWYORK,
  IDF = TEST_IGNORE,
  FCHARSET = CFT_ISO8859-1//IGNORE,
  NCHARSET = CFT_UTF-8,
 FTYPE = T
```

See *Adding a character set: transcoding* (in the general unified configuration parameters) for a complete list of the Transfer CFT default charsets.

FCHARSET/NCHARSET value resolution
----------------------------------

The FCHARSET/NCHARSET values for a transfer are defined as follows:

1. SEND/RECV: The transfer uses
    the charset value provided by the SEND/RECV commands if the corresponding
    CFTSEND/CFTRECV permits it.
1. CFTSEND/CFTRECV: If the SEND/RECV
    command is empty, the values come from the corresponding CFTSEND/CFTRECV
    objects.
1. CFTPART: If the charset value
    is still empty, the values come from the corresponding CFTPART object.
1. CFTPROT: If the charset
    value is still empty, the values come from the corresponding CFTPROT object.

The following rules apply:

- FCHARSET and NCHARSET
    can be set by independent sources, be that SEND, CFTSEND, CFTPART, or
    CFTPROT.
- If one or both
    of the FCHARSET and NCHARSET fields are empty, or set to NONE, the extended
    transcoding is disabled and the traditional transcoding applies.
- If you use FCHARSET/NCHARSET, the FCODE/NCODE parameters are ignored.

### Considerations when choosing the file type

It is generally recommended that you use text files in the variable-length format when possible.

When using multibyte encoding for fixed or limited record size files, please pay attention to the following important considerations:

- Shrinking a record
    can cause a fatal error if it occurs in the middle of a multibyte character.
- Padding a record
    can cause a fatal error if the size to be padded is not a multiple of
    the pad character. The pad character is a blank for a text file, and a
    zero for binary files.
- Errors when using binary files are more likely (with the exception of single-byte encoding).
- When using FTYPE=J (stream text), an interrupted transfer restarts at the beginning of the transfer, not at the last synchronization point.

<span id="CHARSET"></span>

### CHARSET mapping

The following table shows the CHARSET mapping. Brackets in the UNIX/Windows column indicate platform exceptions.


| CFT_ charset  | UNIX/Windows  | IBM i  |
| --- | --- | --- |
| CFT_UTF-8  | UTF-8  | 01208  |
| CFT_UTF-16  | UTF-16  | 01204  |
| CFT_UTF-16LE  | UTF-16LE<br/> [AIX] UTF-16le<br/>  | 01202  |
| CFT_UTF-16BE  | UTF-16BE<br/> [AIX] UTF-16<br/> [HPUX] ucs2 | 01200  |
| CFT_UTF-32  | UTF-32<br/> [HPUX] UTF-32BE | 01236  |
| CFT_UTF-32LE  | UTF-32LE  | 01234  |
| CFT_UTF-32BE  | UTF-32BE<br/> [AIX] UTF-32<br/> [HPUX] ucs4 | 01232  |
| CFT_UCS-2  | UCS-2<br/> [HPUX] = UCS-2BE | N/A  |
| CFT_UCS-2LE  | UCS-2LE | N/A  |
| CFT_UCS-2BE  | UCS-2BE<br/> [AIX] UCS-2 | N/A  |
| CFT_CP850  | CP850<br/> [AIX, MVS (z/OS), VMS] IBM-850 | 00850  |
| CFT_BIG5  | BIG5<br/> [AIX, HPUX] big5 | 00947  |
| CFT_ISO8859-1  | ISO8859-1<br/> [HPUX] iso88591 | 00819  |
| CFT_ISO8859-15  | ISO8859-15<br/> [HPUX] iso885915 | 00923  |
| CFT_EBCDIC-FR  | [UNIX] EBCDIC-FR<br/> [AIX, SUN] IBM-297<br/> [HPUX, Windows] cp1147 | 00297  |

