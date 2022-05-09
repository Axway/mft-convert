{
    "title": "Extended character set mapping",
    "linkTitle": "Transcoding character set mapping",
    "weight": "230"
}Character transcoding defines how data are encoded during the transfer process. This is important when transferring files that do not have the same coding requirements on the sending and receiving systems. See the section **Character set transcoding** in the *Transfer CFT User Guide* for more information.

NCHARSET and FCHARSET parameter mapping
---------------------------------------

The following table shows the mapping for the IBM i (OS/400) platform when using the NCHARSET and FCHARSET parameters.


| CFT_ charset  | IBM i  |
| --- | --- |
| CFT_UTF-8  | 01208  |
| CFT_UTF-16  | 01204  |
| CFT_UTF-16LE  | 01202  |
| CFT_UTF-16BE  | 01200  |
| CFT_UTF-32  | 01236  |
| CFT_UTF-32BE  | 01232  |
| CFT_UCS-2  | N/A  |
| CFT_CP850  | 00850  |
| CFT_BIG5  | 00947  |
| CFT_ISO8859-1  | 00819  |
| CFT_ISO8859-15  | 00923  |
| CFT_EBCDIC-FR  | 00297  |

