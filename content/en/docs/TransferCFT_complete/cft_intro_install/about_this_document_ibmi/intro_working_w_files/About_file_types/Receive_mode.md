{
    "title": " Configure receive mode (native)",
    "linkTitle": "Configure receive mode (native)",
    "weight": "220"
}File type
---------

The behavior of the values ‘’ and ‘ ’, for FTYPE and FRECFM respectively, are not detailed in the following tables. These values correspond to undefined, which means that the transfer in reception takes the value sent through the network. 

****File type when the file does not exist****

The following table lists the different types of files that can be created on an IBM i system if the file to receive does not already exist:


| FTYPE  | FRECFM  | Created file<br /> Type  | Created file<br /> Max record length  |
| --- | --- | --- | --- |
| ‘D’ | ‘F’ | PF-DTA  | FLRECL |
| ‘D’  | ‘V’ | PF-DTA | FLRECL + 5 bytes <sup>1</sup> |
| ‘S’ | ‘F’ | PF-SRC | FLRECL |
| ‘S’  | ‘V’ | PF-SRC | FLRECL |
| ‘E’ | ‘F’ | PF-SRC | FLRECL +12 bytes <sup>2</sup> |
| ‘E’  | ‘V’ | PF-SRC | FLRECL +12 bytes <sup>2</sup> |
| ‘Z’ | - | SAVF | NA |


****File type when the file already exists****

The following table describes the {{< TransferCFT/headerfootervariableshflongproductname  >}} behavior when trying to receive data in an existing file on the native side of an IBM i system.

> **Note**
>
> Note: Bold  values indicate a recommended combination. For example, when FTYPE=D and FRECFM=V then FLRECL+5 / 5 is the recommended PF-SRC.

QQQ_QQQ_CHECK Does **Existing file** apply to col 5?


|  FTYPE  |  FRECFM  | Existing file<br /> PF-DTA<br/> Record length / member header | Existing file<br /> PF-SRC<br/> Record length / member header | Overwriting on a SAVF<br/> with FACTION=ERASE |
| --- | --- | --- | --- | --- |
| ‘D’ | ‘F’ | **FLRECL / No** | FLRECL / No | Yes <sup>3</sup> |
| ‘D’  | ‘V’ | FLRECL+5 / 5 | **FLRECL+5 / 5** | Yes <sup>3</sup> |
| ‘S’ | ‘F’ | FLRECL / 0 OK | **FLRECL / 12** | Error<br/> DIAGI: 102<br/> DIAGP: 1140850696 |
| ‘S’  | ‘V’ | FLRECL+17 / 17 | FLRECL+17 / 17 | Error<br/> DIAGI: 102<br/> DIAGP: 1140850696 |
| ‘E’ | ‘F’ | FLRECL +12 / 0 | **FLRECL +12 / 12** | Error<br/> DIAGI: 102<br/> DIAGP: 1140850696 |
| ‘E’ | ‘V’ | FLRECL+17 /17<br/>  | FLRECL+17 / 5 | Error<br/> DIAGI: 102<br/> DIAGP: 1140850696 |
| ‘Z’ | - | Error<br/> DIAGI: 102<br/> DIAGP: 1140850696 | Error<br/> DIAGI: 101<br/> DIAGP: 11409169 | Yes <sup>3</sup> |


<sup>1</sup> The file is created with a record length corresponding to the record length of the original file, plus 5 bytes corresponding to five header bytes in each record. These 5 bytes indicate the length of useful data.

<sup>2</sup> The file is created with a record length corresponding to the record length of the original file, plus 12 bytes corresponding to five header bytes in each record. These 12 bytes indicate the date and the sequence number.

<sup>3</sup> You must set FACTION to ERASE for the transfer. Otherwise, the transfer fails to overwrite the existing file, and ends in error.
