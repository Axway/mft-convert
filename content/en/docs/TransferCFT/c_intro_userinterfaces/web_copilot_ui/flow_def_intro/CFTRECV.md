---

    title: CFTRECV  - Receive templates
    linkTitle: Receive templates - CFTRECV 
    weight: 220

---
This topic describes the {{< TransferCFT/suitevariablesTransferCFTName  >}}
receive template. You can use the CFTRECV object to:

- Give the default
    name and local physical characteristics of the file to receive
- Define the default
    actions to perform locally during and after the transfer (translation,
    compression, call to a user EXIT, an end-of-transfer procedure...)
- Authorize the default
    time slot and default user associated with the transfers

****Related
topics****

- Command syntax
    [CFTRECV](../../../command_summary#CFTRECV)


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/comment">COMMENT</a> | Local alphanumeric comment associated with receive transfers. |
| <a href="../../../command_summary/parameter_intro/cycdate">CYCDATE</a> | Upper final date for activating the first transfer of a cycle. |
| <a href="../../../command_summary/parameter_intro/cyctime">CYCTIME</a> | Upper limit time for activating the first transfer of a cycle. |
| <a href="../../../command_summary/parameter_intro/delete">DELETE</a>  | Automatic deletion of the catalog entries in the "X" phase (done) for the corresponding IDF. |
| <a href="../../../command_summary/parameter_intro/exit">EXIT</a> | Identifier of the CFTEXIT command associated with this transfer. |
| <a href="../../../command_summary/parameter_intro/faction">FACTION</a> | Action on the file for a receive transfer. |
| <a href="../../../command_summary/parameter_intro/fblksize">FBLKSIZE</a> | This parameter (in bytes) controls the "blocking factor" of the receiver file records: according to the system, it defines the disk block size and/or the file input/output buffer size. |
| <a href="../../../command_summary/parameter_intro/fcharset">FCHARSET</a>  | Defines the local file encoding.  |
| <a href="../../../command_summary/parameter_intro/fcheck">FCHECK</a> | Checks record length attributes. |
| <a href="../../../command_summary/parameter_intro/fcode#fcode_CFTSEND">FCODE</a> | Code of the receiver file data (local data code). |
| <a href="../../../command_summary/parameter_intro/fdisp#fdisp_CFTRECV">FDISP</a> | Presence check indicator of the receiver file used to determine the action of the Transfer CFT monitor. |
| <a href="../../../command_summary/parameter_intro/fkeylen">FKEYLEN</a>  | Key length of an indexed file. |
| <a href="../../../command_summary/parameter_intro/fkeypos#fkeypos">FKEYPOS</a> | Key position relative to 0 in the record of an indexed file. |
| <a href="../../../command_summary/parameter_intro/flowname">FLOWNAME</a>  | The local flow descriptor.  |
| <a href="../../../command_summary/parameter_intro/flrec#flrecl">FLRECL</a>  | File record length in bytes. |
| <a href="../../../command_summary/parameter_intro/fname#fname%20CFTSEND__CFTRECV__CFTISEND">FNAME</a> | Name of the physical receiver file (filename or complete pathname) of the directory. |
| <a href="../../../command_summary/parameter_intro/force">FORCE</a>  | Determines the priority with which the parameters set in CFTRECV are taken into account relative to the parameters set in an associated RECV command. |
| <a href="../../../command_summary/parameter_intro/forg">FORG</a>  | Organization of the file to be sent. |
| <a href="../../../command_summary/parameter_intro/frecfm">FRECFM</a>  | Record format of the receiver file. |
| <a href="../../../command_summary/parameter_intro/fspace">FSPACE</a>  | Size of the receiver file, in K-bytes (1 K-byte = 1024 bytes). |
| <a href="../../../command_summary/parameter_intro/ftype#ftype">FTYPE</a> | Type of the receiver file. |
| <a href="../../../command_summary/parameter_intro/groupid">GROUPID</a> | Information completing the USERID of the CFTRECV command. |
| <a href="../../../command_summary/parameter_intro/id#id_CFTSEND">ID</a>  | Local model file identifier (IDF). |
| <a href="../../../command_summary/parameter_intro/maction">MACTION</a>  | Action on the files transferred by COPY at the time of creation. |
| <a href="../../../command_summary/parameter_intro/maxdate">MAXDATE</a> | Transfer validity final date. |
| <a href="../../../command_summary/parameter_intro/maxtime">MAXTIME</a> | Transfer validity limit time for the final date (MAXDATE). |
| <a href="../../../command_summary/parameter_intro/mindate">MINDATE</a> | Minimum transfer validity date. |
| <a href="../../../command_summary/parameter_intro/mintime">MINTIME</a> | Transfer initial validity time, from the first day (MINDATE). |
| <a href="../../../command_summary/parameter_intro/ncharset">NCHARSET</a>  | Defines the destination file encoding that is used on a file to encode or decode network data.  |
| <a href="../../../command_summary/parameter_intro/ncode">NCODE</a>  | The network data code when receiving transfers. *Available only when using SFTP.*  |
| <a href="../../../command_summary/parameter_intro/ncomp">NCOMP</a> | Compression of on-line data requested by the receiver. |
| <a href="../../../command_summary/parameter_intro/netband">NETBAND</a> | Select the outgoing port range. |
| <a href="../../../command_summary/parameter_intro/notify">NOTIFY</a> | Defines the destination of the messages associated with the send transfer selected from the log file messages, by the value of the OPERMSG parameter. |
| <a href="../../../command_summary/parameter_intro/opermsg">OPERMSG</a>  | Defines the categories of transfer information messages intended for the operator (all the messages also being written in the log file). |
| <a href="../../../command_summary/parameter_intro/pri">PRI</a> | Receive request selection priority. |
| <a href="../../../command_summary/parameter_intro/ruser">RUSER</a> | Identifier of the file receiver user. |
| <a href="../../../command_summary/parameter_intro/sourceappl">SOURCEAPPL</a>  | The identifier of the local file sender application.  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>  | Defines the transfer request state. |
| <a href="../../../command_summary/parameter_intro/suser">SUSER</a>  | Identifier of the file sender user. |
| <a href="../../../command_summary/parameter_intro/targetappl">TARGETAPPL</a>  | Identifier of the local file receiver application.  |
| <a href="../../../command_summary/parameter_intro/tcycle">TCYCLE</a>  | Transfer cycle period unit. |
| <a href="../../../command_summary/parameter_intro/trk">TRK</a>  | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/userid#userid_CFTRECV">USERID</a> | Identifier of the transfer owner. |
| <a href="../../../command_summary/parameter_intro/wfname">WFNAME</a>  | Name of the temporary file used during the transfer. |
| <a href="../../../command_summary/parameter_intro/xlate">XLATE</a>  | Identifier of the translation table used for the receive transfers. |


#### Examples

This section displays examples for a default receive template.

****Example 1****

****CFTRECV****

```
MODE = REPLACE,
 
ID = SRCFILES,
/\* IDF for source files \*/
FDISP = BOTH
/\* already exists or not \*/
FACTION = ERASE,
/\* if exists erase \*/
XLATE = ETOA
/\* with this table  \*/
```

The set of parameters corresponding to this command is used, during
a data receive transfer, if the explicit value of the transfer IDF is
"SRCFILES".  
The monitor thereby has default values for receiver file management. The
FNAME parameter is not defined. The monitor can consequently only operate
if a RECV command specifying FNAME has been entered (Transfer CFT is requester),
or if the partner (Transfer CFT is server) has specified the receiver
file name (open mode for receive transfers).

****Example 2****

****CFTRECV****

```
MODE = REPLACE,
ID = IDFDEF, /\* default IDF \*/
FDISP = BOTH, /\* already exists or not \*/
FACTION = DELETE, /\* if exists - delete \*/
FNAME = &IDT /\* file as per IDT value \*/
```

This command corresponds to the data receive transfer case where the
identifier (IDF) has no explicit description (CFTRECV object). This is
the default receiver model file. The CFTPARM object must specify: CFTPARM
DEFAULT = IDFDEF, ... Transfer CFT then creates a file whose name contains
the unique transfer identifier.

**IBM i:** This command corresponds
to the general database file receive transfer case.
