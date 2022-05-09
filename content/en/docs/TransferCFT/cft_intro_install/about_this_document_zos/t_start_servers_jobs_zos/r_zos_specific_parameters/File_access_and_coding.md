{
    "title": "Code Transfer CFT filenames",
    "linkTitle": "Code filenames",
    "weight": "280"
}Working with files and coding

This section describes file properties and how to code Transfer CFT filenames.

- [File access overview](#File%20access%20overview)
- [About coding filenames](#Coding%20file%20names%20zOS)
- [Filename forms](#Filename)
- [Code filenames with DDNAME ](#Coding%20filenames%20with%20DDNAME)
- [Coding PDS filenames](#Coding%20PDS%20filenames)
- [Code GDG filenames](#Coding%20GDG%20filenames)

<span id="File access overview"></span>

File access overview
--------------------

Transfer CFT z/OS has read or write access to the following files:

- Disk sequential files
- Multi-volume disk sequential files
- PDS files
- Files in GENERATION DATA GROUP (GDG)
- VSAM KSDS files
- VSAM ESDS files
- HFS hierarchical files

Transfer CFT z/OS creates, deletes, and renames the following files by calling IDCAMS:

- VSAM KSDS files
- VSAM ESDS files

Transfer CFT z/OS cannot transfer the following files:

- VSAM RRDS files
- VSAM LINEAR files
- Concatenated files
- Files on magnetic tapes
- File for which the LRECL is higher than 32760 (lrecl=x)

<span id="Coding file names zOS"></span>

About coding filenames
----------------------

Transfer CFT z/OS uses the following coding to handle files:

```
FNAME=VOLUME%UNIT%NAME1.NAME2.NAMEX
```

Where:

- VOLUME has the following characteristics:

    > -   Indicates the name of the disk volume where the file will be created or searched
    > -   Is optional, but useful for requesting Transfer CFT to create a VSAM file or other file, except for SMS managed volumes
    > -   Corresponds to the VOLUME parameter of the DEFINE CLUSTER VSAM or VOL=SER= of the JCL
    > -   Should not be used with DF/SMS

- UNIT has the following characteristics:

> -   Indicates the UNITNAME used
>
> <!-- -->
>
> -   Is optional but useful for a tape or disk SAM file
>
> <!-- -->
>
> -   Corresponds to the parameter UNIT= of the JCL
>
> <!-- -->
>
> -   By default, the value SYSALLDA or the value imposed by your DYNALLOC EXIT will be used

- NAME1.NAME2.NAMEx have the following characteristics:

> -   Defines the filename
>
> <!-- -->
>
> -   Mandatory
>
> <!-- -->
>
> -   The initial character in HFS filenames is the slash: ( / )

**Example**

```
CFTVOL%%FILENAME
%3390%FILENAME
%%FILENAME
FILENAME
/path/file.extension
```

> **Note**
>
> Note: The two % signs are mandatory only if the VOLUME parameter or the UNIT parameter has been specified.

<span id="Filename"></span>

Filename forms
--------------

A filename can have different forms:

- A DSNAME or a string coded in form ‘VOLUME%UNIT%DSNAME’ (VOLUME and UNIT are often optional).
- A logical name, associated with a DD card [ JCL ] or with an ALLOC [ CLIST ].
- PDS member name, which is also by completing with the member name between brackets.

****Example ****

A DSNAME or a string to request that a file be sent:

```
SEND FNAME=‘CFT.SEND.FILE’
```

Search the catalog for the file:

```
SEND FNAME=‘CFTRES%3480%CFT.SEND.FILE’
```

Look for the file on the volume CFTRES, unit 3480:

```
SEND FNAME=‘%3480%CFT.SEND.FILE’
```

Look for the file in the catalog (unit type imposed):

```
SEND FNAME=‘CFTRES%%CFT.SEND.FILE’
```

Look for the file on the disk CFTRES:

Using parameters ‘VOLUME’ and/or ‘UNIT’ may conflict with DF/SMS file management.

****Example****

PDS member name to request sending of a member with the file searched for in the catalog:

```
SEND FNAME=‘CFT.SEND.FILE(MEMBER)’
```

****Example****

A logical name to select a PARTNERS file:

```
CFTPARM PARTFNAM=$CFTPART
```

This file is indicated in the JCL that starts Transfer CFT, by:

```
//CFTPART DD DISP=SHR,DSN=... ,
```

Or under TSO:

```
ALLOC FI(CFTPART) SHR DA(’...’) .
```

Transfer CFT and the associated utilities set aside the use of “DD names” beginning with ‘FIL’ for dynamic allocations.

### Coding SMS parameters

Transfer CFT z/OS allows limited coding of certain SMS parameters in the UNIT parameter.

The following values allow you to:

- &gt;STORCLA: specify a value for STORAGE-CLASS

<!-- -->

- &lt;DATACLA: specify a value for DATA CLASS
- \*MGTCLAS: specify a value for MANAGEMENT CLASS

An alternate way to specify full-length DF/SMS parameters is described in [DF/SMS large file support](../t_dynamically_create_files).

<span id="Coding filenames with DDNAME"></span>

Code filenames with DDNAME 
---------------------------

Transfer CFT z/OS uses the following coding to refer to a DDNAME declared in the JCL:

```
FNAME=$DDNAME
```

Example

```
FNAME=$CFTCAT
```
<span id="Coding PDS filenames"></span>

Coding PDS filenames
--------------------

Transfer CFT z/OS handles PDS files one member at a time. Transfer CFT z/OS processes PDS files:

- Member by member in sequence

<!-- -->

- By calling IEBCOPY UNLOAD

<!-- -->

- For the entire PDS, by setting FNAME=\#DSNAME or \#DSNAME(\*)

> **Note**
>
> Note: The second syntax is recommended as it is the syntax to use for heterogeneous transfers.

- For a selected subset of members, using the ’\*’ character to replace a character string or ’?’ to replace one character and by setting FNAME=\#DSNAME(ME?BER\*)

A PDS file is coded as:

```
FNAME=NAME1.NAMEX(MEMBER)
```

Delivered template:

- `..SAMPLE(CFTPDS)`

<span id="Coding GDG filenames"></span>

Code GDG filenames
------------------

A GDG filename is coded as:

```
FNAME=NAME1.NAMEX(0)
FNAME=NAME1.NAMEX(-n)
FNAME=NAME1.NAMEX(+n)
```

> **Note**
>
> Note: Transfer CFT z/OS does not allow the concatenation of all versions of a GDG file.

Delivered templates:

- `..SAMPLE(CFTGDGS) (send GDG)`
- `..SAMPLE(CFTGDGR) (receive GDG)`

****Related topics****

- [Delete rename and share files](../t_delete_and_rename_files_zos)
- [Dynamically create files](../t_dynamically_create_files)
- [HFS hierarchical files](../c_hfs_hierarchical_files_zos)
