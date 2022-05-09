{
    "title": "HFS hierarchical files",
    "linkTitle": "HFS hierarchical files",
    "weight": "310"
}The [HFS Hierarchical File System]() data architecture is consecutive and non-structured for records and blocks. The HFS files are installed in hierarchical disk spaces, and divided into directories and sub-directories according to UNIX conventions.

HFS files are managed either by applications that work in an UNIX/OMVS environment, or by applications that use the [USS UNIX System Services](), such as the management and access interface.

Transfer CFT only accepts the complete name from the root directory. All file name components are separated by the ‘/’ character. The complete name is limited to 248 characters.

The z/OS file characteristics, such as format (RECFM), record size (LRECL), and block size (BLKSIZE) are meaningless for an HFS file. However, Transfer CFT uses these characteristics as a basis for ensuring the data management and the transmission of file characteristics towards the receiver for a transfer. As a result, parameter settings must take these characteristics into account when characteristics are conveyed between heterogeneous partners.

Transfer CFT z/OS can only access HFS files for a transfer. This excludes any other use in the Transfer CFT. This means that Transfer CFT z/OS:

- Does not support generic sending of HFS in homogeneous mode.
- The CFTUTIL utility COPYFILE function does not work with HFS files.

HFS file names
--------------

File names coded in the FNAME=, WFNAME=, and NFNAME= parameters must follow the UNIX conventions for file identification. This means that the files values for the parameters associated with FNAME=, and WFNAME= parameters must be coded between quotes “…”.

However, as the value for the NFNAME= parameter is not converted to upper case, it should not be coded between quotes “…”.

**Example**

`FNAME ="/home/qualcft/send/SEND.File.Lowercase" `

`NFNAME= '/home/qualcft/recv/Recv.File.Lowercase' `

or

` /home/qualcft/recv/Recv.File.Lowercase`

`To enable recursive processing for a group of files, use the following syntax ** (two asterisks). To select all files in all of the folders, for example: `

```
FNAME =/home/qualcft/\*\*
```

> **Note**
>
> Note: A single asterisk \* only selects the file in the immediate folder.

### HFS file characteristics

HFS files are specified by their type, depending on whether they contain text (structured) or are binary (non-structured). The FTYPE= and NTYPE= parameters convey the file type.

The values allocated to TYPE are:

- T: text file
    -   This file is structured
    -   The logical records are limited
    -   Transfer CFT treats the data in record mode

<!-- -->

- B: binary file
    -   This file is non-structured
    -   Transfer CFT treats the data in continuous flow

<!-- -->

- J: stream text
    -   Enables sending a text file that contains records that are larger than 32 KB

If the parameter is not present, the binary file type becomes the default type.

### RECFM, LRECL, and BLKSIZE characteristics

The RECFM, LRECL and BLKSIZE parameters retain their characteristics when they are conveyed between heterogeneous partners.

Transfer CFT uses these parameter characteristics as a basis for ensuring data management for a transfer. For this reason, parameter settings must take into account the following:

- Transfer CFT interprets the file format (FRECFM= and NRECFM=) to select a data exchange method. If this parameter is not present, V is the default value.
- Transfer CFT interprets the size of a logical record (FLRECL= and NRECL=) to adapt the size of exchanged articles. If this parameter is not present, 4096 is the default value.
- The size of a block is ignored (FBLKSIZE= and NBLKSIZE=) and is set to 0.
- The LRECL for text files is limited to 32,752 bytes. Some JAVA/XML applications create non delimited text files, not supported by Transfer CFT.

### HFS file owner and access rights 

An HFS file owner, or attribute, is characterized by:

- A UID (user number)
- A GID (group number)
- A string that specifies access rights

Transfer CFT z/OS manages the file owner and the access rights in two ways:

- Transfer CFT is an authorized program and the CFTPARM USERCTRL parameter is set to YES.  
    As a result, files are created/read/written with the same rights as the requesting user. This is the recommended operating mode.

<!-- -->

- Otherwise, the HFS files are created/read/written with the rights of the Transfer CFT. It is recommended that you assign a UID to the Transfer CFT that is not 0.

When a received transfer leads to the creation of an HFS file, the file is created with the access right -rwxr-xr-x.

Before Transfer CFT z/OS creates a file, it checks that enough space is available in the File System. If not, Transfer CFT refuses the creation, and returns error code 00F00B37.

Transfer CFT handles HFS files smaller than 4 terabytes.

Changing the name of an HFS file can only be carried out in the same directory. Additionally, the file path that is described in the FNAME= parameter must be identical to the path specified in the WFNAME=parameter.

### HFS-specific error message

For each HFS file access error, Transfer CFT z/OS displays the CFTHF01E message in the SYSLOG.

```
CFHF01E:BPX1mod ,RSN=05F1006C,RC=ENOENT (129)No such file or directory
```

Where:

- BPX1mod: Name of the service module that returned the error

<!-- -->

- RSN=xxxxxxxx: The returned reason code

Refer to the IBM brochure *UNIX System Services Messages & Codes*.

- RC=: The return code in abbreviated mnemonic (numeric) form

****Related topics****

- [File access and coding](../file_access_and_coding)
