{
    "title": "CFTRECV  - Receive templates",
    "linkTitle": "Receive templates - CFTRECV ",
    "weight": "210"
}This topic describes the {{< TransferCFT/suitevariablesTransferCFTName  >}}
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
| [COMMENT](../../../command_summary/parameter_intro/comment) | Local alphanumeric comment associated with receive transfers. |
| [CYCDATE](../../../command_summary/parameter_intro/cycdate) | Upper final date for activating the first transfer of a cycle. |
| [CYCTIME](../../../command_summary/parameter_intro/cyctime) | Upper limit time for activating the first transfer of a cycle. |
| [DELETE](../../../command_summary/parameter_intro/delete)  | Automatic deletion of the catalog entries in the &quot;X&quot; phase (done) for the corresponding IDF. |
| [EXIT](../../../command_summary/parameter_intro/exit) | Identifier of the CFTEXIT command associated with this transfer. |
| [FACTION](../../../command_summary/parameter_intro/faction) | Action on the file for a receive transfer. |
| [FBLKSIZE](../../../command_summary/parameter_intro/fblksize) | This parameter (in bytes) controls the &quot;blocking factor&quot; of the receiver file records: according to the system, it defines the disk block size and/or the file input/output buffer size. |
| [FCHARSET](../../../command_summary/parameter_intro/fcharset)  | Defines the local file encoding.  |
| [FCHECK](../../../command_summary/parameter_intro/fcheck) | Checks record length attributes. |
| [FCODE](../../../command_summary/parameter_intro/fcode#fcode_CFTSEND) | Code of the receiver file data (local data code). |
| [FDISP](../../../command_summary/parameter_intro/fdisp#fdisp_CFTRECV) | Presence check indicator of the receiver file used to determine the action of the Transfer CFT monitor. |
| [FKEYLEN](../../../command_summary/parameter_intro/fkeylen)  | Key length of an indexed file. |
| [FKEYPOS](../../../command_summary/parameter_intro/fkeypos#fkeypos) | Key position relative to 0 in the record of an indexed file. |
| [FLOWNAME](../../../command_summary/parameter_intro/flowname)  | The local flow descriptor.  |
| [FLRECL](../../../command_summary/parameter_intro/flrec#flrecl)  | File record length in bytes. |
| [FNAME](../../../command_summary/parameter_intro/fname#fname%20CFTSEND__CFTRECV__CFTISEND) | Name of the physical receiver file (filename or complete pathname) of the directory. |
| [FORCE](../../../command_summary/parameter_intro/force)  | Determines the priority with which the parameters set in CFTRECV are taken into account relative to the parameters set in an associated RECV command. |
| [FORG](../../../command_summary/parameter_intro/forg)  | Organization of the file to be sent. |
| [FRECFM](../../../command_summary/parameter_intro/frecfm)  | Record format of the receiver file. |
| [FSPACE](../../../command_summary/parameter_intro/fspace)  | Size of the receiver file, in K-bytes (1 K-byte = 1024 bytes). |
| [FTYPE](../../../command_summary/parameter_intro/ftype#ftype) | Type of the receiver file. |
| [GROUPID](../../../command_summary/parameter_intro/groupid) | Information completing the USERID of the CFTRECV command. |
| [ID](../../../command_summary/parameter_intro/id#id_CFTSEND)  | Local model file identifier (IDF). |
| [MACTION](../../../command_summary/parameter_intro/maction)  | Action on the files transferred by COPY at the time of creation. |
| [MAXDATE](../../../command_summary/parameter_intro/maxdate) | Transfer validity final date. |
| [MAXTIME](../../../command_summary/parameter_intro/maxtime) | Transfer validity limit time for the final date (MAXDATE). |
| [MINDATE](../../../command_summary/parameter_intro/mindate) | Minimum transfer validity date. |
| [MINTIME](../../../command_summary/parameter_intro/mintime) | Transfer initial validity time, from the first day (MINDATE). |
| [NCHARSET](../../../command_summary/parameter_intro/ncharset)  | Defines the destination file encoding that is used on a file to encode or decode network data.  |
| [NCODE](../../../command_summary/parameter_intro/ncode)  | The network data code when receiving transfers. *Available only when using SFTP.*  |
| [NCOMP](../../../command_summary/parameter_intro/ncomp) | Compression of on-line data requested by the receiver. |
| [NETBAND](../../../command_summary/parameter_intro/netband) | Select the outgoing port range. |
| [NOTIFY](../../../command_summary/parameter_intro/notify) | Defines the destination of the messages associated with the send transfer selected from the log file messages, by the value of the OPERMSG parameter. |
| [OPERMSG](../../../command_summary/parameter_intro/opermsg)  | Defines the categories of transfer information messages intended for the operator (all the messages also being written in the log file). |
| [PRI](../../../command_summary/parameter_intro/pri) | Receive request selection priority. |
| [RUSER](../../../command_summary/parameter_intro/ruser) | Identifier of the file receiver user. |
| [SOURCEAPPL](../../../command_summary/parameter_intro/sourceappl)  | The identifier of the local file sender application.  |
| [STATE](../../../command_summary/parameter_intro/state)  | Defines the transfer request state. |
| [SUSER](../../../command_summary/parameter_intro/suser)  | Identifier of the file sender user. |
| [TARGETAPPL](../../../command_summary/parameter_intro/targetappl)  | Identifier of the local file receiver application.  |
| [TCYCLE](../../../command_summary/parameter_intro/tcycle)  | Transfer cycle period unit. |
| [TRK](../../../command_summary/parameter_intro/trk)  | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [USERID](../../../command_summary/parameter_intro/userid#userid_CFTRECV) | Identifier of the transfer owner. |
| [WFNAME](../../../command_summary/parameter_intro/wfname)  | Name of the temporary file used during the transfer. |
| [XLATE](../../../command_summary/parameter_intro/xlate)  | Identifier of the translation table used for the receive transfers. |


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
