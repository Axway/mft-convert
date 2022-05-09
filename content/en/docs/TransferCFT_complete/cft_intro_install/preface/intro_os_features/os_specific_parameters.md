{
    "title": "Operating system specific parameters",
    "linkTitle": "OS specific parameters",
    "weight": "250"
}Platform specific characters
----------------------------

********{{< TransferCFT/axwayvariablesComponentLongName  >}} Guardian specific values********


| Notation | Object | Value |
| --- | --- | --- |
| char_file | Prefix for logical names | = |
| char_mask | Wildcard character | ? |
| char_symb | Prefix for symbolic variables | ^ |
| file_symb | Prefix for a file name passed to CFTUTIL as a parameter | @ |


********File properties automatically retrieved for send operations********


| Notation | Object |
| --- | --- |
| FSPACE | YES |
| FLRECL | YES |
| FBLKSIZE | YES |
| FRECFM | YES |
| FTYPE | YES |


********FTYPE values and associated implicit FCODE default values for send operations********


| FTYPE | FCODE |
| --- | --- |
| ' ' | BINARY |
| E | ASCII |


********FTYPE, FRECFM, and FORG combinations for send operations********

The last 3 columns in the following table provide the implicit values of each Guardian type for FTYPE, FRECFM, and FORG.


| Guardian<br /> type | Guardian<br /> code | File type | FTYPE  | FRECFM  | FORG  |
| --- | --- | --- | --- | --- | --- |
| U | # 101 | Binary stream | ' ' | U | SEQ  |
| U | = 101 | Edit file | E | U | SEQ  |
| E | # 1 | Fixed sequential | ' ' | F | SEQ  |
| E | = 1 | Sequential, variable emulation | ' ' | V | SEQ  |
| R |   | Direct fixed | ' ' | F [1] | DIR  |
| K |   | Fixed indexed sequential | ' ' | F [1] | IDX  |


> **Note**
>
> Note: Empty cells indicate that the information is not significant.

> **Note**
>
> Note: [1] You can also send variable length record files by setting FRECFM = V.

********FTYPE, FRECFM, and FORG values for receive operations********

FTYPE

FRECFM

FORG

File Type

Guardian Type

Guardian Code

 

U

 

Binary stream

U

0

E

 

 

Edit file

U

101

 

F

SEQ

Fixed sequential

E

0

 

V

 

Sequential, variable emulation

E

1

 

F

DIR[2]

Direct fixed

R

0

 

V

DIR [2]

Direct fixed, variable emulation

R

1

 

F

IDX [2]

Fixed indexed sequential

K

0

 

V

IDX [2]

Indexed sequential, variable emulation

K

1

> **Note**
>
> Note: Empty cells indicate that the information is not significant.

> **Note**
>
> Note: [2] Read the file organization from the network, explicitly set FORG to FORG= ‘ ‘. Otherwise, FORG is always sequential (FORG = SEQ).

Platform specific parameters and values
---------------------------------------

****ATTSUSER****

Use the ATTSUSER parameter to set specific attributes for receiving native files. This parameter can take three values, FCODE, FORMAT, and BUFFERED.

****FCODE****

Forces the file CODE attribute. This value should be consistent with the file structure and the restrictions of the system. If FCODE is not specified, Transfer CFT sets the code per the table iah,n Figure 5.

****Example****: `ATTSUSER    = 'FCODE=180'`

The received file is created with 180 as the code.

****FORMAT****

Forces the file FORMAT attribute. This value is either 1 or 2. If FORMAT is not specified, Transfer CFT sets the format according to the estimated size of the received file. A file whose size is greater than 2GB has the format 2, while a file having a size less than 2GB has the format 1.

****Example****: `ATTSUSER    = 'FORMAT=2'`

The received file is created with the format 2.

****Use multiple values****

Additionally, you can use multiple values:

****Example****: `ATTSUSER    = ' FCODE=180,FORMAT=1'`

The received file is created with the format 1, and 180 as the code.

****BUFFERED****

If specified, this parameter forces the BUFFERED attribute for the Guardian file. The possible values are 0 (NO BUFFERED) and 1 (BUFFERED). If not specified, Transfer CFT does not force the attribute. You can find more information on this attribute in the *File Utility Program (FUP) Reference Manual*.

**Example**: `ATTSUSER = 'BUFFERED=1'`

The received file is created with a BUFFERED attribute set.

> **Note**
>
> Note: If a received file has the BUFFERED option, the file write cache is flushed while the PeSIT synchronization points are set. This means that there is no data loss (the file is not altered) if the file transfer is restarted. This applies whether BUFFERED is set on Transfer CFT or by an explicit file creation operation.
