{
    "title": "Configure send mode (native)",
    "linkTitle": "Configure send mode (native)",
    "weight": "210"
}Native file type definitions
----------------------------

The following table lists the different types of files that can be used according to the type of data to be sent.

> **Note**
>
> Note: Bold  values indicate a recommended combination. For example, when FTYPE=D and FRECFM=V then RCDLEN-5 is the recommended PF-DTA with variable data.


| FTYPE  | FRECFM  | PF-DTA<br/> Fixed data | PF-DTA<br/> Variable data | PF-SRC  | SAVF  |
| --- | --- | --- | --- | --- | --- |
|   |   | FRECFL  | FRECFL  | FRECFL  |   |
| ‘D’ |  ‘F’ | **RCDLEN** | RCDLEN <sup>1</sup> | RCDLEN | 528 |
| ‘D’  | ‘V’ | RCDLEN | **RCDLEN-5** <sup>2</sup> | RCDLEN | 528 |
| ‘S’ |  ‘F’ | RCDLEN | RCDLEN <sup>1</sup> | **RCDLEN** | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |
| ‘S’  | ‘V’ | RCDLEN | RCDLEN-5 <sup>2</sup> | RCDLEN | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |
| ‘E’ |  ‘F’ | RCDLEN | RCDLEN <sup>1</sup> | **RCDLEN-12** <sup>3</sup> |  Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |
| ‘E’  | ‘V’ | RCDLEN | RCDLEN-5 <sup>2</sup> | RCDLEN-12 <sup>3</sup> | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |
| ‘Z’<br/>  |  ‘F’ | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696  |  Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696  |  **528** |
| ‘Z’  |  ‘V’ | Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |  Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |  Error<br /> DIAGI: 102<br /> DIAGP: 1140850696 |  528 |


****Key****

<sup>1</sup> Truncates the 5 bytes variable header, preserving the original record length.

<sup>2</sup> Truncates the 5 bytes variable header, adjusting the record length accordingly.

<sup>3</sup> Truncates the 12 bytes, adjusting the record length accordingly.

****Default FTYPE or FRECFM value****

The behavior of the values ‘’ and ‘ ’, for FTYPE and FRECFM respectively, are not detailed in the following table. These values correspond to `undefined`, meaning that the transfer in emission takes the value of both the file type and the member content.


| FTYPE | FRECFM | Supported files and data organizations (if applicable). |
| --- | --- | --- |
| ‘D’  | ‘F’  | PF-DTA Member containing fixed data |
| ‘D’  | ‘V’  | PF-DTA Member containing variable data |
| ‘D’  | ‘F’  | PF-SRC  |
| ‘Z’  | ‘F’  | SAVF  |

