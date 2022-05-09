{
    "title": "Work with files",
    "linkTitle": "Work with files",
    "weight": "300"
}> **Note**
>
> Note: Do not use a dash "-" as a special character in z/OS (MVS) file names. This is an invalid system character.

About files
-----------

When renaming a file, Transfer CFT z/OS only releases the unused space if authorized to do so (APF), and if the file is allocated on a single volume. All files deleted by Transfer CFT are removed from the catalog.

- SAM files are deleted/renamed by CAMLST

<!-- -->

- PDS members are deleted/renamed by STOW

<!-- -->

- VSAM files are deleted/renamed by dynamic calling to IDCAMS

### Overwrite PDS members

The CFTRECV MACTION=REPLACE parameter controls the overwriting of any PDS file members received.

The CFTRECV command MACTION=REPLACE parameter controls the files that are received from the ADRDSSU utility DUMP.

<span id="Share Transfer CFT files"></span>

Share Transfer CFT files
------------------------

File sharing characteristics include:

- Transfer CFT VSAM files are allocated as DISP=SHR

<!-- -->

- Read transfer files are allocated as DISP=SHR

<!-- -->

- Write files are allocated as DISP=OLD

### GRS multi-system protection

Transfer CFT uses QNAME CFTSHARE to protect various resources. QNAME CFTSHARE must be propagated in all members of a GRS configuration. This is the only QNAME to be propagated.

Transfer CFT uses the following other QNAMEs:

- CFTFILES to protect transferred files
- CFTSUBM to serialize JOBS submitted on the Internal Reader

<!-- -->

- CFTPUTS for general internal serialization

<!-- -->

- CFTCATSH to share the catalog dataspace

The CFTFILES ENQs do not need to be broadcast to all systems in the GRS RING.

****Related topics****

- [File access and coding](../file_access_and_coding)
- [HFS hierarchical files](../c_hfs_hierarchical_files_zos)
