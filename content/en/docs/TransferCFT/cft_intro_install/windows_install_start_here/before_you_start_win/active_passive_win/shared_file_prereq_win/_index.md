---
title: "Shared file system prerequisites"
linkTitle: "Shared file systems"
weight: 210
---This section provides general information concerning the prerequisites for shared file systems for the following types of files used with {{< TransferCFT/suitevariablesTransferCFTName  >}} in a UNIX environment.

- Transfer CFT data files: This refers to all files managed by {{< TransferCFT/suitevariablesTransferCFTName >}} other than transferable application files (including database files), which are stored in the {{< TransferCFT/suitevariablesTransferCFTName >}} runtime directory.
- Transferable application files: This refers to the files transferred by Transfer CFT.

## Standalone installation

You can use any POSIX compliant shared file system for both Transfer CFT data files and transferable application files.

The following table lists the file systems that are supported and tested with Transfer CFT.

****<span id="Supported_fs_win"></span>Supported file systems****


| Operating system  | File system  |
| --- | --- |
| AIX  | GPFS, NFSv3, NFSv4*  |
| HP-UX  | NFSv3, NFSv4*  |
| Linux-x86  | GFS2, GPFS, GlusterFS, NFSv3, NFSv4* |
| Solaris  | NFSv4*  |
| Windows-x86  | GPFS, SMB  |


\*References to NFSv4 imply any version of NFSv4. All NFSv4 minor versions are supported, for example version 4.2.

## Active/passive cluster

You can use any POSIX compliant shared file system for both Transfer CFT data files and transferable application files. Please see the [Supported file systems](#Supported_fs_win) in the Standalone installation section.

## Active/active cluster

#### {{< TransferCFT/suitevariablesTransferCFTName  >}} data files

**Supported shared file systems for multi-node, multi-host architecture (active/active)**

The following non-exhaustive table lists shared file systems that have been tested with Transfer CFT.


| Operating system  | Supported  | Unsupported  |
| --- | --- | --- |
| AIX  | GPFS, NFSv4*  | NFSv3, CXFS, VeritasSF  |
| HP-UX  | NFSv4*  | NFSv3, CXFS, VeritasSF  |
| Linux-x86  | GPFS, GFS2, NFSv4*, AWS EFS  | NFSv3, CXFS, ACFS, OCFSv1, OCFSv2, QFS, VeritasSF  |
| OpenVMS  | RMS  |   |
| Solaris  | NFSv4*  | NFSv3, CXFS, QFS, VeritasSF  |
| Windows-x86  | SMB/CIFS, GPFS  | CXFS, NFS  |
| z/OS  | Sharing DASD across Sysplex  |   |


\*References to NFSv4 imply any version of NFSv4. All NFSv4 minor versions are supported, for example version 4.2.

#### {{< TransferCFT/suitevariablesTransferCFTName  >}} transferable application files

You can use any POSIX compliant shared file system for transferable application files.

Please see the [Supported file systems](#Supported_fs_win) in the Standalone installation section.
