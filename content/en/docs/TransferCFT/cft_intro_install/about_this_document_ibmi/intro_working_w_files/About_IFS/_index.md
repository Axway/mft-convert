{
    "title": "Using IFS hierarchical files ",
    "linkTitle": "Using IFS hierachial files",
    "weight": "200"
}This topic describes the Integrated File System, ****IFS****, functions available in Transfer CFT {{< TransferCFT/PrimaryForOS400  >}}.

It includes:

- [Configure send mode (IFS)](send_files_ifs)
- [Configure receive mode (IFS)](receive_files_ifs)
- [Copyfile (IFS)]()

See also [IFS access error codes.](../../os400_support_tool/ifs_access_errors)

IFS overview
------------

The IFS provides a common interface to another system on the IBM i. After installing {{< TransferCFT/PrimaryForOS400  >}} you can:

- Transfer IFS files
- Receive and store IFS files
- Copy IFS files to a native {{< TransferCFT/PrimaryForOS400  >}} system and vice versa (using CFTUTIL COPYFILE)

### Naming conventions

Respect the following naming conventions:

- The file name must be prefixed by the slash character /
    -   For example: `/home/filename`
- You cannot replace environmental variables in the file name
    -   For example:` $HOME/filename` is not a recognized filename
- You cannot precede filenames by a relative path
    -   For example: `../filename` is not a recognized filename

### Encoding IFS data

IFS file data can be in an ASCII, EBCDIC, or BINARY format. The CCSID, Code Character Set Identifier, associated with the file determines the encoding for the data.

- Transfer CFT can read and write IFS files in these three formats: ASCII, EBCDIC, or BINARY.  
    When using the ASCII or EBCDIC formats, the data translation for a Transfer CFT transfer, if necessary, is managed by the Transfer CFT translation tables (CFTXLATE).
- When Transfer CFT receives an IFS file, the CCSID for the file is set by default. This identifier is set to the Transfer CFT {{< TransferCFT/PrimaryForOS400  >}} job CCSID value.
- The CFTRECV (or RECV) command FCODE=ASCII parameter creates an ASCII file with an associated CCSID code value of 819 (ISO 8859-1 common use default Internet code).

### IFS file rights and authorizations

In a {{< TransferCFT/PrimaryForOS400  >}} environment, files are subject to two types of control, data authorities and object authorities.

By default, the Transfer CFT users and other general users are given the following:

- The RWX options for data authority
- The OBJMGT, OBJEXIST, OBJALTER and OBJREF options for object authority

The minimum IFS data authorities required to perform transfers with any user are:

- RX: readable and executable permission for any object in the IFS directory
- RWX: readable, writable and executable permission for any object in the IFS directory
