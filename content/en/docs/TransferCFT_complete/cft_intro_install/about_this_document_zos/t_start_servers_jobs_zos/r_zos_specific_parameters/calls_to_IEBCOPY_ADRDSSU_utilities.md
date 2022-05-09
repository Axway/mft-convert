{
    "title": "Calls to z/OS utilities from Transfer CFT",
    "linkTitle": "Calls to z/OS utilities IEBCOPY and ADRDSSU",
    "weight": "320"
}The Z/OS utility call function enables you to transfer files that are not directly readable by Transfer CFT. The IEBCOPY and ADRDSSU utilities are supported. The following sections describe this operating mode.

Common rules for utility calls
------------------------------

Utilities are called to:

- Transform data for output in a temporary sequential file. Transformation is done when the file is selected, before the network session is open.

<!-- -->

- Transform the incoming file into a format identical to the sent file(s). Transformation is done on reception of an end of transfer notification. The network session stays open and the message originator receives an indication of reception or transfer failure.

Respect the following rules:

- The Transfer CFT must be and authorized program (APF)

<!-- -->

- The transfer is only possible between two Transfer CFT, one switch via an intermediary Transfer CFT site is allowed

### CFTSEND or SEND command coding

The following parameters are required:

- The \# character is necessary at the head of FNAME (hexadecimal code 7B)

<!-- -->

- WFNAME indicates the intermediary file name that will be written by the utility, and then transferred by Transfer CFT. Files that have DSNTYPE=EXTENDED/LARGE are not supported by IEBCOPY or ADRDSSU.

<!-- -->

- NSPACE indicates the size of the intermediary file.  
    This parameter is necessary if the FNAME value translates to a selection of files or members

<!-- -->

- FACTION=DELETE is not supported (ignored by the utility).

<!-- -->

- FTYPE=’1’ commands the choice of the ADRDSSU utility, associated with one or several ‘ ‘ characters at the end of the FNAME.

### CFTRECV and RECV command coding

The following parameters are required:

- WFNAME indicates the name of the intermediary file that is received by Transfer CFT and then by the utility

<!-- -->

- FACTION is not supported (ignored by the utility)

<!-- -->

- MACTION=REPLACE controls the replacement of members or files
- FTYPE=’_’ creates a PDSE (hexadecimal X’6D’)

Use IEBCOPY
-----------

An IEBCOPY call enables the transfer to all members, or to a selected group of members, of a PDS file. For LOAD MODULES you must use IEBCOPY.

**Example of sending a complete library**

`SEND  ..FNAME=#MY.LOAD.LIBRARY,WFNAME=TEMP.FILE.&PART.&IDTU `

*Or*

`SEND  ..FNAME=#MY.LOAD.LIBRARY(*),`

`WFNAME=TEMP.FILE.&PART.&IDTU`

And the CFTRECV:

`CFTRECV FNAME=A.LOAD.LIBRARY,WFNAME=TEMP.FILE.&PART.&IDTU,MACTION = REPLACE`

**Example of sending a partial library**

`SEND  ..FNAME=#MY.LOAD.LIBRARY(TEST*) `

The received PDS is created, if it does not already exist, with the following characteristics:

- SPACE same as initial file

<!-- -->

- Number of DIRECTORY = ‘BLKPDS’ blocks, 150 by default

Restrictions:

- IEBCOPY compatible utilities are not supported
- You cannot transfer members of a load library from unlike libraries (PDS from/to PDSE)

Using ADRDSSU
-------------

ADRDSSU enables the transfer of one or more files, or you can transfer all files using the joker character ‘_’. It is recommended that you use this utility for all files except the sequential files:

- VSAM

<!-- -->

- PDSE (on reception)

<!-- -->

- HFS

A sub-group of ADRDSSU parameters is used, refer to the examples below.

### CFTSEND or SEND coding FNAME

The general format for CFTSEND is:

`CFTSEND FTYPE=’1’, WFNAME=TEMP.FILE.&PART.&IDTU, FNAME = ‘VOLSER%%DSNGEN_’`

Where:

- VOLSER: optional parameter that can contain a volume name

<!-- -->

- DSNGEN: optional parameter

<!-- -->

- If present it must terminate with ‘_’

<!-- -->

- If not present then VOLSER is mandatory and all files of the ‘VOLSER’ volume are transferred (except SYS1.VVDS and VTOC INDEX)

<!-- -->

- DSNGEN is a file name ending with ‘_’; this is the only file (catalog or not) that is not transferred

<!-- -->

- DSNGEN contains several ‘_’;  in this case it is a model that is interpreted according to the following rules:

#### Rules for interpreting DSNGEN


| Model  | Interpretation  |
| --- | --- |
| DSNGEN_ | DSN |
| DSNGEN__ | DSN* |
| DSNGEN._ | DSN.* |
| DSNGEN_._ | DSN*.* |
| DSNGEN_.__ | DSN*.** |


**Example of sending 1 file**

`SEND  ..FTYPE=’1’, FNAME=#A.VSAM.FILE_`

The generated ADRDSSU command has the format:

`DUMP DATASET ( INCLUDE ( A.VSAM.FILE )COMPRESS ALLDATA(*) TOLERATE(ENQFAILURE) SPHERE`

**Example of sending n files**

`SEND  ..FTYPE=’1’, FNAME=#CFTV2.NEW.VERSION.__ , NSPACE=150000`

**Example of ending n files of 1 volume**

`SEND  ..FTYPE=’1’, FNAME=#VOLSER%%CFTV2.NEW.__, NSPACE=1000000`

The ADRDSSU command also uses the parameters:

`EXCLUDE ( SYS1.VVDS.V*  , SYS1.VTOCIX.*  ) LOGINDYNAM ( VOLSER) SELECTMULTI ( ANY ) `

**Notes**

- All of the allocated space is copied (ALLDATA parameter)

<!-- -->

- The VSAM CLUSTERS are copied with alternating indexes (SPHERE parameter)

<!-- -->

- The multivolume files are copied completely (SELECTMULTI parameter)

Restrictions:

- ADRDSSU does not take into account migrated HSM files

<!-- -->

- The PAGE DATASET files and the VSAM catalogs are not supported

#### CFTRECV or RECV coding

The general format for CFTRECV is:

`CFTRECV FNAME = ‘VOLSER%%QUALIF1’WFNAME=TEMP.FILE.&PART.&IDTU`

Where:

- VOLSER: An optional parameter that can contain the name of the volume that receives the files.

<!-- -->

- QUALIF1: An optional parameter that enables you to change the first QUALIFIER of the file. QUALIF1 can also be a RENAME PATTERN of up to 180 bytes in length, and contain 1 or more strings each separated by a comma. The syntax must conform with ADRDSSU rules, and be enclosed in quotes.

##### Examples

Example of a received files RENAME with a pattern:

`CFTRECV FNAME= ‘(QUALIF1),(OLD.PATTERN1,NEW.PATTERN1),(OLD.PATTERN2, NEW.PATTERN2)’`

Example of receiving files under their initial name:

`CFTRECV FNAME= ‘%%’ `

The ADRDSSU command that is generated has the format:

`RESTAURE DATASET ( INCLUDE ( ** ) CATALOG SPHERE `

Example of receiving files on 1 VOLUME, under their initial name:

`CFTRECV FNAME=’VOLSER%%’ `

The ADRDSSU command also uses the parameter:

`OUTDYNAM  ( VOLSER ) `

Example of replacing received files:  

`CFTRECV MACTION = REPLACE, FNAME=’%%’ `

The ADRDSSU command also uses the parameter:

`REPLACEU`

Example of RENAME received files, modifying the first qualifier:  

`CFTRECV FNAME=’%%NEWNAME’ `

The ADRDSSU command also uses the parameter:

`RENAMEUNC ( NEWNAME ) `

The combination RENAMEU+REPLACEU will always replace existing files.

> **Note**

- The created files are identical to the initial file.

<!-- -->

- Transfer CFT forces the RECALL HSM of migrated files to be restored.

Additional messages
-------------------

If processing is correct, Transfer CFT displays two CFTF30W messages containing:

- Utility return code

<!-- -->

- Additional text

For an IEBCOPY, a report file is generated. This report contains a DSNAME (MEMBER) report for each member. This is a temporary MVS or z/OS file that is not cataloged. Its name is stored in the Transfer CFT catalog in the SELFNAME topic.

**Example**

`VOLSER%%SYS03132.T164138.RA000.CFT.R0200169`

This file is automatically deleted by Transfer CFT when the catalog post is deleted.

Error messages
--------------

When an error occurs, a message is displayed for either:

- The ABEND code of the utility

<!-- -->

- The first error message of the utility

Additionally, most error messages are sent to the SYSLOG z/OS.

The list of all files not restored by ADRDSSU is displayed in the SYSLOG z/OS, in the message: CDSS04E: Failed to Restore=DSNAME

Refer to the related IBM documents for explanations, or see the IEBCOPY and ADRDSSU parameters and messages.

The SGTRACE 12 fine control option enables the recording of detailed traces in the SGTRACE file.

Utility performance
-------------------

Transfer CFT z/OS calls a single external utility at a given time. Memory use is as follows:

**Memory use**


| Utility  | 24 bit memory  | 31 bit memory  |
| --- | --- | --- |
| IEBCOPY | 1024K | 0 |
| ADRDSSU | 2048K | 8192K |


The CFTPROT RTO=seconds parameter must contain a value expressed in seconds that is large enough to allow the complete operation by the utility on the receiving side.

For ADRDSSU and RACF, the TOLERATE (ENQFAILURE) parameter requires READ access to the FACILITY class STGADMIN.ADR.DUMP.TOLERATE.ENQF resource, from the sending side.

****Related topics****

****[Transfer CFT z/OS general performance concepts](../../zos_performance)****
