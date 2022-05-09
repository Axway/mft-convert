{
    "title": "Character set conversion - CFTXLATE  ",
    "linkTitle": "Character set conversion - CFTXLATE",
    "weight": "280"
}This topic describes how to define a translation table to be used during
a transfer. There are two ways to generate a translation table, either using the FNAME parameter or the TABLE parameter as described below.

****Related
topics****

- Command syntax
    [CFTXLATE](../../../command_summary#CFTXLATE)
- Object concepts
    [Conversion
    tables](../../../../concepts/transfer_command_overview/using_transcoding/translation_table_concepts)

Use this object to define translation tables between 2
alphabets for:

- Transfer
    direction
- File
    data code type (FCODE: ASCII or EBCDIC)
- Data
    network code type (NCODE: ASCII or EBCDIC)


| Parameter  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id)  | Translation table identifier.<br/> Several CFTXLATE commands may have the same identifier, if the values of DIRECT, FCODE or NCODE are different. |
| [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction for which the table applies:<br/> • SEND: translation table for send transfers<br/> • RECV: translation table for receive transfers<br/> • BOTH: translation table which can be used for send transfers and receive transfers<br/> If the value of the parameter is BOTH, the data read in the file allows a translation table for send transfers (SEND) to be created. The translation table for receive transfers (RECV) is deduced automatically.<br/> To provide for bijection (i.e. any character of the source alphabet translated into the target alphabet, and then re-translated from the target alphabet into the source alphabet, takes up its initial value again), the table has to contain 256 different values. It is not essential to strictly comply with this principle for transfer applications using reduced alphabets. |
| [NCODE](../../../command_summary/parameter_intro/ncode) | Code of data sent over the network. |
| [TABLE]()  | A digital representation of the table as a hexadecimal string.  |
| [FCODE](../../../command_summary/parameter_intro/fcode)  | Data code of the file sent. |
| [FNAME](../../../command_summary/parameter_intro/fname)  | Name of the file containing the description of the translation table. This file must have a sequential organization. Examples of such files are given with the various products (refer to the Operations Guide specific to each system). |


**Example using FNAME**

In this example, the Transfer CFT internal translation tables (identifier
DEFAULT) are replaced by the user tables, which can be used in both transfer
directions.

For send transfers, the Transfer CFT translates
from ASCII into EBCDIC and for receive transfers, from EBCDIC into ASCII.
The ASCII to EBCDIC table is defined in the file ATOE and the EBCDIC to
ASCII table is deduced automatically.

```
CFTXLATE
ID = DEFAULT,
DIRECT = BOTH,
FCODE = ASCII,      Dft:
OS
FNAME = ATOE,
NCODE = EBCDIC
 
CFTPARM
DEFAULT = DEFAULT,
DIRECT = BOTH,
FCODE = ASCII,      Dft:
OS
FNAME = ATOE,
NCODE = EBCDIC
```

****Example of generating a translation table using the TABLE parameter****

You can use the following `CFTXLATE `command with the TABLE parameter, instead of FNAME, to generate an ASCII CP437 to EBCDIC CP1047 translation table.

```
CFTXLATE ID=CP437TOCP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE=00010203372D2E2F1605250B0C0D0E0F-
10111213B6B5322618191C27071D1E1F-
405A7F7B5B6C507D4D5D5C4E6B604B61-
F0F1F2F3F4F5F6F7F8F97A5E4C7E6E6F-
7CC1C2C3C4C5C6C7C8C9D1D2D3D4D5D6-
D7D8D9E2E3E4E5E6E7E8E9ADE0BD5F6D-
79818283848586878889919293949596-
979899A2A3A4A5A6A7A8A9C04FD0A13F-
68DC5142434447485253545756586367-
719C9ECBCCCDDBDDDFECFC4AB1B2BFFF-
4555CEDE49699A9BABAFB0B8B7AA8A8B-
2B2C092128656264B438313433708024-
22172906202A46661A35083936303A9F-
8CAC7273740A757677231514046A783B-
EE59EBEDCFEFA08EAEFEFBFD8DBABCBE-
CA8F1BB93C3DE19D90BBB3DAFAEA3E41
```

Or alternatively, you can use the following `CFTXLATE `command to generate an ASCII CP850 to EBCDIC CP1047 translation table.

```
CFTXLATE ID=CP850T0CP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE=00010203372D2E2F1605250B0C0D0E0F-
101112133C3D322618191C27071D1E1F-
405A7F7B5B6C507D4D5D5C4E6B604B61-
F0F1F2F3F4F5F6F7F8F97A5E4C7E6E6F-
7CC1C2C3C4C5C6C7C8C9D1D2D3D4D5D6-
D7D8D9E2E3E4E5E6E7E8E9ADE0BD5F6D-
79818283848586878889919293949596-
979899A2A3A4A5A6A7A8A9C04FD0A13F-
68DC5142434447485253545756586367-
719C9ECBCCCDDBDDDFECFC70B180BFFF-
4555CEDE49699A9BABAFB0B8B7AA8A8B-
2B2C092128656264B4383134334AB224-
22172906202A46661A35083936303A9F-
8CAC7273740A757677231514046A783B-
EE59EBEDCFEFA08EAEFEFBFD8DBABCBE-
CA8F1BB9B6B5E19D90BBB3DAFAEA3E41
```
