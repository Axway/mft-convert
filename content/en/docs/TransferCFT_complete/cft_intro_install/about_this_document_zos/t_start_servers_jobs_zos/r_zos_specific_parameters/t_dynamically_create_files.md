{
    "title": "Dynamically create files",
    "linkTitle": "Dynamically create files",
    "weight": "290"
}File characteristics overview
-----------------------------

Dynamically created Transfer CFT z/OS files have the following characteristics:

- All files that are created by Transfer CFT are cataloged
- SAM files are created by DYNALLOC
- VSAM files are created by dynamic calling IDCAMS
- PDS files are created with BLKPDS directory blocks. BLKPDS is an option of the Transfer CFT installation JOB A12OPTS. Its default value is 150
- GDG files can only be created if the PATTERN DSCB exists

The primary space is calculated from the size in Kbytes indicated in the parameters or defined by the partner. The secondary space is equal to 10% of the primary space.

The BLKSIZE can be:

- Calculated depending on the value indicated in the customization parameters
- Announced by the partner (in PeSIT profile CFT and ANY only)
- Calculated from the BLKSIZE defined in the JOB A12OPTS

When DSNTYPE=EXTPREF/EXTREQ/LARGE is specified in JOB A12OPTS, the BLKSIZE value is decreased if possible by at least 32, to compensate for the DF/SMS 32 bytes added to every physical block.

The file allocation is in the form SPACE=(BBBB,(PPPP,SSSS)),DCB=(BLKSIZE=BBBB), with:

- BBBB: Value of the calculated BLKSIZE
- PPPP: Primary allocation, which is updated with the % factor defined in JOB A12OPTS ALLPRIM= or ALLONE=
- SSSS: 10 % of PPPP, which is computed as a % of the primary allocation defined in ALLSEC=, for a single volume allocation, or updated with the % factor defined in ALLNEXT=

Transfer CFT z/OS may also create multi-volume files through the DF/SMS 'ACS ROUTINES', using a DATACLASS that describes striped files. For more information, refer to the IBM documentation DFSMS Storage Administration Reference [Document Number SC26-7402-xx].

<span id="Dynamically creating multi-volume files"></span>

### Dynamically creating multi-volume files

The dynamic creation of multi-volume files is a feature that enables you to receive very large files. It is complementary to existing support through SMS.

This mechanism is called automatically when:

- File creation fails because of lack of space or request for more than 65535 tracks in a NON EXTENDED FORMAT volume
- The VOLUME parameter is not completed
    -   You can deactivate this device when this parameter is present and set. The maximum number of volumes, 20 by default, is customizable in the JOB A12OPTS VOLNUM= .

DYNALLOC is called up to VOLNUM= additional times with:

- The request for one extra volume
- The corresponding reduction in the PPPP primary allocation

Transfer CFT reserves the requested space on the assigned volumes. If this reservation fails for one volume, file creation is rejected and the error code ‘xxxxxE37’ returned.

To request the creation of a multi-volume file, use ‘fname=’%+nnn%DSNAME’, where nnn is a numerical value between 2 and 127:

- If nnn is less than VOLNUM, allocation will be retried up to the limit of 20 volumes
- If nnn is greater than VOLNUM, a single allocation attempt is performed

### DF/SMS extended, large and EAV file support

#### About large file support functionality

Transfer CFT supports the DF/SMS features introduced by IBM for z/OS environments. Additionally, the extended format files and the DFSMS BLOCKTOKENSIZE=LARGE option are automatically managed by Transfer CFT. The JCL parameter DSNTYPE=EXTPREF, indicating a preference for an extended format, can be added when applicable and supported. The EATTR parameter introduced with z/OS 1.11 is also supported.

Overall large file support in Transfer CFT z/OS includes:

- A maximum record size for storing USS text files of 32,756 bytes, and a maximum number of records for a USS text file of 2\*\*31.
- The number of volumes for a file is 255. The maximum supported file size is 54 gigabytes, or 4 terabytes per EAV volume, times 255 volumes. Nevertheless, the maximum file size is limited to 4 terabytes.
- The maximum catalog size is 328,000 saved entries for a non SMS managed volume, 5,000,000 entries for an SMS managed volume, or 10,000,000 with EATTR=OPT.
- Support for the DFSMS BLOCKTOKENSIZE=LARGE parameter, which was introduced with z/OS 1.7.
- Support for VSAM files larger than 4 gigabytes.
- Support for HFS “text” files containing more than 16 million records.
- Support for files created with DSNTYPE=EXTPREF or the LARGE parameter for create, read, or write operations. Files created on the extended address volume (EAV) section of the 3390-A units are also supported for create, read or write operations.
- For Transfer CFT to use the described EXTENDED or LARGE file features, you should specify these using the DFSMS ACS routines. Transfer CFT also provides a certain number of alternate options to specify DFSMS parameters when creating files.
- Transfer CFT will discard unsupported options when it is running on a back-level z/OS system.

#### Specifying a default DSNTYPE value in the SGINSTAL options

SGINSTAL option provides a global default value for DSNTYPE. For more information, refer to the Selecting Transfer CFT functional options section in Customizing the JCL installation files.

#### Specifying additional DFSMS parameters

In two specific cases certain DFSMS parameters can be specified when creating a file, using one of the following:

- CFTRECV WFNAME=’value’ **(\*)**
- CFTFILE FNAME=’value’

Where 'value' is in the format:

WFNAME=‘AN.MVS.DSNAME,keword1=value1,keyword2=value2’

*or*

FNAME=‘AN.MVS.DSNAME,keword1=value1,keyword2=value2’

> **Note**
>
> Note: Keywords are separated with comma, and must be enclosed in quotes.

Supported keywords and values include:

- DATACLAS=class
- DSNTYPE=BASIC/EXTPREF/EXTREQ/LARGE - to override a predefined value in the SGINSTAL option
- EATTR=OPT - valid only with z/OS 1.11 and higher, otherwise it is discarded
- MGMTCLAS=class
- STORCLAS=class
- RETPD=nnnn
- EXPDT=yyyyddd *or* yyddd

> **Note**
>
> Note: Concerning expiration dates:

> -   Expiration dates of 1999365 and 1999366 are considered “never-scratch”.  
>     Or
> -   Expiration dates 99365 and 99366 are considered “never-scratch” dates.
> -   For expiration dates of January 1, 2000 and later, you MUST use the EXPDT=yyyyddd format.

**(\*)** For the CFTRECV command (only), you can specify DFSMS parameters in ATTSUSER.

****Example 1
&lt;/b&gt;&lt;/p&gt;****

To force the creation of the received file in the EAV section of a volume:

`CFTRECV FNAME=A.GDG(+1), WFNAME=’WORK.&PART.&IDTU,DSNTYPE=EXTREQ,EATTR=OPT’`

*Or*

`CFTRECV FNAME=A.GDG(+1),ATTSUSER=’DSNTYPE=EXTREQ,EATTR=OPT’, WFNAME=’WORK.&PART.&IDTU’`

> **Note**
>
> Note: The ATTSUSER field is presently not managed by Central Governance.

Example 2
&lt;/p&gt;

To create a large (greater than 215,000 records) Transfer CFT catalog:

`CFTFILE TYPE=CAT,MODE=CREATE,FNAME=’cftv2.CATALOG,EATTR=OPT’,RECNB=7123456`

****Related topics****

- [Coding Transfer CFT file names](../file_access_and_coding)
- [Working with files](../t_delete_and_rename_files_zos)
- [HFS hierarchical files](../c_hfs_hierarchical_files_zos)
