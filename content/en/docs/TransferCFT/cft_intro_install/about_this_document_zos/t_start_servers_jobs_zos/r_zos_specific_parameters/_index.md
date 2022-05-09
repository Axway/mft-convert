{
    "title": "Working with files and coding",
    "linkTitle": "About files and coding",
    "weight": "260"
}Specific parameters for z/OS
----------------------------

This section describes the parameters and values that are specific to Transfer CFT z/OS and information about:

- Transferable files
- Filename coding

Transferable files
------------------

File characteristics that are found automatically for sending are listed in the following table.

**File characteristics**


| Parameter  | Found automatically for sending  |
| --- | --- |
| FSPACE | YES |
| FLRECL | YES |
| FBLKSIZE | YES |
| FRECFM | YES |
| FTYPE | YES |


**FTYPE and FRECFM combinations for sending**


| Type of file to be sent  | Implicit value of FTYPE  | Implicit value of FRECFMz  |
| --- | --- | --- |
| Disk sequential files |   | F/V/U |
| Members of PDS files (1 transfer per member) |   | F/V/U |
| Designated version of a file in GDG |   | F/V/U |
| Multivolume disk file |   | F/V/U |
| VSAM KSDS or ESDS file |   | F/V |
| Print file with ASA jump codes (z/OS to z/OS) | A | F/V/U |
| Print file with machine jump codes (z/OS to z/OS) | M | F/V |
| Spanned variable format file (z/OS to z/OS) | S | V |


> **Note**
>
> Note: Variable SPANNED files can be routed through an intermediary Transfer CFT for the PeSIT protocol only (ANY profile). In this case, the file received on Transfer CFT z/OS is always in the ‘U’ format.

The PDS files copied by IEBCOPY are also received in the ‘U’ format, which is compatible with IEBCOPY.

**Receiving values for FORG, FTYPE and FRECFM**


| FORG  | FTYPE  | FRECFM  |  Type of receive file  |
| --- | --- | --- | --- |
| SEQ |   | F/V/U | Disk sequential file |
| PART |   | F/V/U | Member of PDS files (1 transfer per member) |
| SEQ |   | F/V/U | Version designated -1 to 0 of a file in GDG |
| SEQ |   | F/V/U | Multivolume disk file |
| DIRECT |   | F/V | VSAM ESDS file |
| INDEXED |   | F/V | VSAM KSDS file (already exists but empty) |
| SEQ | A | F/V/U | Print file with ASA jump code (z/OS to z/OS) |
| SEQ | M | F/V/U | Print file with machine jump code (z/OS to z/OS) |
| SEQ | S | V | File in spanned variable format (z/OS to z/OS) |
| SEQ |   | F/V/U | Magnetic tape or cartridge file in position 1 (with STANDARD LABELS) |


These values are explicit in CFTRECV or are deduced from the received protocol values.
