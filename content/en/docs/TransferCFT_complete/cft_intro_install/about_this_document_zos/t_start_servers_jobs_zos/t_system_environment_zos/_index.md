{
    "title": "System environment for z/OS",
    "linkTitle": "About the system configuration ",
    "weight": "230"
}This section presents system environment functions that are specific to Transfer CFT z/OS and provides information about:

- Creating files in z/OS
- File-sharing options in z/OS
- Submitting JOBs

Create files in z/OS
--------------------

Transfer CFT z/OS creates files:

- Using the CFTFILE TYPE=CREATE function

<!-- -->

- Using the COPYFILE function

<!-- -->

- During reception of transferred files

SAM files are created by SVC99(DYNALLOC). The main options for creating SAM files are as follows:

- If the F/V/U format is received, it is processed; otherwise the U format is taken by default

<!-- -->

- If the BLKSIZE is omitted, three cases are possible:

<!-- -->

- In F format, the closest value to MAXBLKSIZE is taken (MAXBLKSIZE is an installation option, with a default value of 27,920)

<!-- -->

- In V format, MAXBLKSIZE is taken

<!-- -->

- In U format, 32760 is taken

<!-- -->

- The secondary space allocated is equal to 10 percent of the primary space

<!-- -->

- For a PDS, 150 DIRECTORY blocks are allocated (this value is modifiable during installation of Transfer CFT/z/OS)

<!-- -->

- The file is destroyed in event of error

<!-- -->

- If VOLUME and/or UNIT are not specified, SVC99 uses the SVC99 default value

DETAILS: z/OS/ESA SPL - Dynamic allocation

This default value can conflict with the installation options or EXITS, and the DF/SMS options.

The VSAM files are created by dynamic calls to IDCAMS. In this case, the parameter VOLUME is mandatory, unless DF/SMS assumes a default value.

File-sharing options in z/OS
----------------------------

Transfer CFT z/OS allows file-sharing with the operating system, with the following options:

- A file to be sent is allocated with DISP=SHR

<!-- -->

- A file to be received is allocated with DISP=OLD
- Depending on the PDSE installation option (see PDSESHARING), you can allow simultaneous writing to a PDSE file type

Transfer CFT z/OS prohibits:

- Sending and receiving simultaneously on the same file, including two members of a PDS

<!-- -->

- Simultaneous receiving of two members of a given PDS file

### Delete files in z/OS

Transfer CFT z/OS does not permit deleting:

- The “minus n” versions of a GDG file

<!-- -->

- A file allocated by another user

Manage files by DF/SMS
----------------------

Transfer CFT z/OS creates files compatible with DF/SMS, where:

- DCB is always specified in the format DCB=(RECFM=xx,LRECL=111,BLKSIZE=bbb)

<!-- -->

- The SPACE is always specified in the format SPACE=(bbb,(nnn,sss))

<!-- -->

- The ACS ROUTINES, in certain cases, must be adapted to the Transfer CFT operating mode

Submit JOBs 
------------

Transfer CFT constructs and submits JOBs from any type of file that can be read by Transfer CFT. By default, the JOB is submitted with the USERID of the user requesting the transfer.

If the last card in the JOB is a JCL card beginning with ‘/\*’ or ‘//’, Transfer CFT adds an additional comment card with the following format:

```
//\* SUBMITTED BY:jjjjjjjj AT hh:mm:ss, USERID=uuuuuuuu ,CARDS= nnnnnnnn
```

Where:

- jjjjjjjj: Transfer CFT JOBNAME

<!-- -->

- hh:mm:ss: SUBMIT time

<!-- -->

- uuuuuuuu: USERID used

<!-- -->

- nnnnnnnn: Number of cards submitted

> **Note**
>
> Note: The SUBOPT parameter in the SGINSTAL macro (A12OPTS member) may affect the automation.

### Support for the SHUT RESTART=YES command

Transfer CFT supports the CFTUTIL SHUT RESTART=YES command. First, a SHUT FAST=YES is performed, and then the CFTMAIN program is restarted using the same parameter string, but with a new copy of all modules including CFTMAIN itself.

### Support for the RECONFIG TYPE=CAT command

Transfer CFT supports the CFTUTIL RECONFIG TYPE=CAT command, expanding the currently opened catalog with the following limits:

- Addition of EXTENTS to the current catalog is limited to the initial volume, and its available space.
- A catalog smaller than 215,000 records (the RECNB parameter of the CFTFILE command (physical records total 248 970), a VSAM file smaller than 4 GBs that cannot be expanded over 4 GBs if it was not defined as VSAM-extended. In this case, you must create a new catalog in VSAM-extended format, and copy the old one to it using the Transfer CFT utility.
- Users connected to the catalog must disconnect/reconnect to access the newly created records.
- The catalog cache is not expanded, and access to the newly created records will be slower. An ordered SHUTDOWN is suggested to recover the full Transfer CFT performances.
