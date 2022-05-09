{
    "title": "Backup files ",
    "linkTitle": "Backup files",
    "weight": "230"
}The records in this type of file (SAVF) have a specific format. They contain backed-up sequencing and parity data controlled by{{< TransferCFT/PrimaryForOS400  >}}. SAVF files can generally only be used on {{< TransferCFT/PrimaryForOS400  >}} systems. Consequently, backup files are predominantly transferred between two {{< TransferCFT/PrimaryForOS400  >}} partners.

Another system can be used as an intermediate partner, in which it must process the SAVF file as a sequential file with a fixed format and a length of 528 (bytes).

Implementing SAVF file support
------------------------------

The value Z for the CFTSEND / CFTRECV command FTYPE parameter designates SAVF files. It is mandatory in receive mode (CFTRECV) and optional in send mode (CFTSEND).

The only variable property of an SAVF file is its allocation size in Kilobytes (FSPACE parameter). FSPACE must be between 1 and 65535.

The configuration of the other properties (FORG, FRECFM, FLRECL and so on) is ignored. Consequently, they do not need to be specified in the CFTSEND / CFTRECV commands. However, if FLRECL is specified, it must be set to 528 (bytes).

The SAVF file access method, implemented in Transfer CFT {{< TransferCFT/PrimaryForOS400  >}}, does not support synchronization points in write mode. This mechanism is however supported in read mode. Consequently, a SAVF file transfer with a receiver {{< TransferCFT/PrimaryForOS400  >}} system will fail during a restart (abort on the receiver side). If the file is sent to a non-{{< TransferCFT/PrimaryForOS400  >}} (intermediate) system, resynchronization (restart points) can be performed in read mode with the sender Transfer CFT {{< TransferCFT/PrimaryForOS400  >}}.

Using spool files
-----------------

In addition to the possibilities available in the SNDCFTSPLF interactive command (see OS-Integrated Commands), spool files can be used in batch mode or in the Transfer CFT configuration.

The syntax is as follows:

> `SEND FNAME = Spool file/File n°/Job/Job n°, PART ...`

Example:

> `SEND FNAME = QPRINT/2/CFTLOG/074556, IDF = SPLF, PART ...`

You can transfer spool files from an IBM i system to any other system.  
The receive file is a fixed format sequential file. To use it as a spool file on a receiver IBM i system, you can add the commands in the following example to a receive procedure:

> `OVRPRTF FILE(QPRINT) CTLCHAR(*FCFC)CPYF FROM FILE(CFTPROD/R_SPLF) TO FILE (QPRINT) `

The Qprint file is an exact copy of the initial spool file.
