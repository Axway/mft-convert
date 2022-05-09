{
    "title": "Unix-specific  values",
    "linkTitle": "Unix specifics",
    "weight": "330"
}This topic summarizes {{< TransferCFT/axwayvariablesComponentShortName  >}} characteristics that differ from
other operating systems:

- specific values
- default files

Specific values tables
----------------------


| Notation  | Object  | Value  |
| --- | --- | --- |
| char_file  | Prefix to logical names  | _ (underlined)  |
| char_mask  | Wild card character  | ?  |
| char_unit  | Separator (volume)  | none  |
| char_symb  | Prefix to symbolic variables  | &amp;  |
| char_directory  | Character introduced in the path name of the FNAME parameter (CFTRECV) from which a tree structure can be created  | +  |
| file_symb  | Character introducing a file name sent to CFTUTIL in parameter form  | @  |


Names of default files used by CFTUTIL
--------------------------------------


| **Objet**  | ****Default name****  |
| --- | --- |
| Parameters file  | _CFTPARM  |
| Partners file  | _CFTPART  |
| Catalogc file  | _CFTCATA  |
| Log file  | _CFTLOG  |
| Communication media  | _CFTCOM  |
| statistics file  | _CFTACNT  |
| Preferred media  | File  |

