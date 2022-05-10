---
    "title": "Shared file system prerequisites",
    "linkTitle": "Shared file systems",
    "weight": "200"
---
This section provides general information concerning the prerequisites for shared file systems for the following types of files used with {{< TransferCFT/suitevariablesTransferCFTName  >}} in a UNIX environment.

- Transfer CFT data files: This refers to all files managed by {{< TransferCFT/suitevariablesTransferCFTName  >}} other than transferable application files (including database files), which are stored in the {{< TransferCFT/suitevariablesTransferCFTName  >}} runtime directory.
- Transferable application files: This refers to the files transferred by Transfer CFT.

<span id="Standalo"></span>

Standalone installation
-----------------------

You can use any POSIX compliant shared file system for both Transfer CFT data files and transferable application files.

The following table lists the file systems that are supported and tested with Transfer CFT.

****<span id="Supported_fs_ux_standalone"></span>Supported file systems****


| Operating system  | File system  |
| --- | --- |
| AIX  | GPFS, NFSv3, NFSv4*  |
| HP-UX  | NFSv3, NFSv4*  |
| Linux-x86  | GFS2, GPFS, GlusterFS, NFSv3, NFSv4*  |
| Solaris  | NFSv4*  |
| Windows-x86  | GPFS, SMB  |


\*References to NFSv4 imply any version of NFSv4. All NFSv4 minor versions are supported, for example version 4.2.

Active/passive cluster
----------------------

You can use any POSIX compliant shared file system for both Transfer CFT data files and transferable application files. Please see the [Supported file systems](#Supported_fs_ux_standalone) in the Standalone installation section.

Active/active cluster
---------------------

{{% TransferCFT/snippets/sharedfilesystems_data_files_AA%}}
{{% TransferCFT/snippets/sharedfilesystems_app_files_AA%}}

Please see the [Supported file systems](#Supported_fs_ux_standalone) in the Standalone installation section.

NFS prerequisite
----------------

When implementing a multihost, multi-node architecture, the Transfer CFT user must have read and write access to any folder and files on all hosts. Across all hosts in the implementation, you should ensure that they are using the same UID number.

{{% TransferCFT/snippets/nfs_doc%}}
