{
    "title": "CFTSEND - Send template",
    "linkTitle": "Send templates - CFTSEND ",
    "weight": "200"
}This topic describes how to define the CFTSEND
template. Use this command to SEND a file or files to a partner.

****Related
topics****

- Command syntax
    [CFTSEND](../../../command_summary#CFTSEND)
- Object concepts
    [Default
    send templates](../../../../concepts/cft_configuration_concepts_start_here/default_send_template_concepts)


| Parameter  | Description  |
| --- | --- |
| [ACKEXEC](../../../command_summary/parameter_intro/ackexec)  | Name of the file describing the procedure to be executed when receiving an acknowledgement reply for the transfer.  |
| [ARCHIVEFNAME]()  | The archived source file name after transfer completion if FACTION=ARCHIVE.<br/> <blockquote> **Note**<br/> Note: The fname and archivefname must be on the same volume (all platforms).<br/> </blockquote>  |
| [COMMENT](../../../command_summary/parameter_intro/comment) | Local alphanumeric comment associated with the send transfer. |
| [CYCDATE](../../../command_summary/parameter_intro/cycdate) | Upper final date for activating the first transfer of a cycle. |
| [CYCLE](../../../command_summary/parameter_intro/cycle) | Number of units defining the transfer cycle period. |
| [CYCTIME](../../../command_summary/parameter_intro/cyctime) | Upper limit time for activating the first transfer of a cycle. |
| [DELETE](../../../command_summary/parameter_intro/delete)  | Automatic deletion of the catalog entries in the &quot;X&quot; phase (done) for the corresponding IDF. |
| [EXEC](../../../command_summary/parameter_intro/delete) | Name of the file describing the procedure to be executed on completion of the transfer. |
| [EXECSUB](../../../command_summary/parameter_intro/execsub) | Submission policy of the end-of-transfer procedure, when sending a group of files.  |
| [EXECSUBA](../../../command_summary/parameter_intro/execsuba)  | Submission policy of the procedure to launch receiving acknowledgement, when sending a group of files.  |
| [EXECSUBPRE](../../../command_summary/parameter_intro/execsubpre)  | Submission policy of the preprocessing procedure, when sending a group of files.  |
| [EXIT](../../../command_summary/parameter_intro/exit) | Identifier of the CFTEXIT command associated with this transfer.<br/> Used to activate a file EXIT user task. The value of this parameter may be equal to the symbolic variable &amp;IDF. |
| [FACTION](../../../command_summary/parameter_intro/faction) parameter specifics. |
| [FBLKSIZE](../../../command_summary/parameter_intro/fblksize) | Block size of sent file. |
| [FCODE](../../../command_summary/parameter_intro/fcode) | Code of the data to be sent. |
| [FDISP](../../../command_summary/parameter_intro/fdisp) | File sharing option. |
| [FILTER](../../../command_summary/parameter_intro/filter)  | Defines the filter value to be used when filtering using method defined in filtertype.  |
| [FILTERTYPE](../../../command_summary/parameter_intro/filtertype)  | Defines the type of filter to be applied when sending a generic transfer.  |
| [FKEYLEN](../../../command_summary/parameter_intro/fkeylen)  | Key length of an indexed file. |
| [FKEYPOS](../../../command_summary/parameter_intro/fkeypos) | Key position relative to 0 in the record of an indexed file. |
| [FLOWNAME](../../../command_summary/parameter_intro/flowname)  | The local flow descriptor.  |
| [FLRECL](../../../command_summary/parameter_intro/flrec)  | File record length in bytes. |
| [Additional filename information](../../../command_summary/parameter_intro/fname) |
| [FORCE](../../../command_summary/parameter_intro/force) | Determines the priority with which the parameters set in CFTSEND are taken into account relative to the parameters set in an associated SEND command. |
| [FORG](../../../command_summary/parameter_intro/forg)  | Organization of the file to be sent:<br/> • SEQ: sequential<br/> • INDEXED: indexed<br/> • DIRECT: relative direct access |
| [FRECFM](../../../command_summary/parameter_intro/frecfm) | Record format of the file to be sent:<br/> • F: fixed<br/> • V: variable,<br/> • U: undefined |
| [FSPACE](../../../command_summary/parameter_intro/fspace)  | Size of the file to be sent, in K-bytes (1 K-byte = 1024 bytes). |
| [FTYPE](../../../command_summary/parameter_intro/ftype) | Type of local file to be sent. Also see the NTYPE parameter. |
| [GROUPID](../../../command_summary/parameter_intro/groupid) | Information completing the USERID of the CFTSEND command. |
| [ID](../../../command_summary/parameter_intro/id)  | Local identifier of the model file (IDF) to be sent. |
| [IDA](../../../command_summary/parameter_intro/ida)  | Local transfer identifier assigned by the user or user application.  |
| [IMPL](../../../command_summary/parameter_intro/impl) | Implicit send. |
| [MAXDATE](../../../command_summary/parameter_intro/maxdate) | Transfer validity final date. |
| [MAXTIME](../../../command_summary/parameter_intro/maxtime) | Transfer validity limit time for the final date (MAXDATE). |
| [MINDATE](../../../command_summary/parameter_intro/mindate) | Minimum transfer validity date.<br/> Only taken into account in requester mode |
| [NBLKSIZE](../../../command_summary/parameter_intro/nblksize) | File block size, in protocol terms. |
| [NCODE](../../../command_summary/parameter_intro/ncode)  | Sent data code. |
| [NCOMP](../../../command_summary/parameter_intro/ncomp) | Compression of on-line data required by the sender. |
| [NETBAND](../../../command_summary/parameter_intro/netband) | Select the outgoing port range. |
| [NFNAME](../../../command_summary/parameter_intro/nfname) | Name of the physical file sent to the receiver partner. |
| [NKEYLEN](../../../command_summary/parameter_intro/nkeylen) | Length (in bytes) of the key of an indexed file. |
| [NKEYPOS](../../../command_summary/parameter_intro/nkeypos) | Location (in bytes) of the key in the records of an indexed file. |
| [NLRECL](../../../command_summary/parameter_intro/nlrecl) | For records of:<br/> • Fixed format (FRECFM = F): size in bytes of the records of the receiver file<br/> • Variable format (FRECFM = V): maximum size in bytes of the records |
| [NOTIFY](../../../command_summary/parameter_intro/notify) | Defines the destination of the messages associated with the send transfer selected from the log file messages, by the value of the OPERMSG parameter. |
| [NRECFM](../../../command_summary/parameter_intro/nrecfm) | Format of the records of the file defined in protocol terms. |
| [NTYPE](../../../command_summary/parameter_intro/ntype)  | File type, in protocol terms. |
| [OPERMSG](../../../command_summary/parameter_intro/opermsg)  | Defines the categories of transfer information messages intended for the operator, with all the messages also being written in the log file. |
| [PARM](../../../command_summary/parameter_intro/parm)  | User parameter sent to the receiver. |
| [PREEXEC](../../../command_summary/parameter_intro/preexec)  | Name of the file describing the procedure to be executed before the transfer, as per the preprocessing phase.  |
| [PRI](../../../command_summary/parameter_intro/pri) | Send request selection priority. |
| [RUSER](../../../command_summary/parameter_intro/ruser) | Identifier of the file receiver user. |
| [SELFNAME](../../../command_summary/parameter_intro/selfname)  | Name of the file that contains the list of files selected for sending.<br/> <blockquote> **Note**<br/> Note: When using SELFNAME and FACTION=DELETE, the FNAME must be a directory and not a MASK. For example, #dir is deleted, whereas #dir/* is ignored.<br/> </blockquote>  |
| [SOURCEAPPL](../../../command_summary/parameter_intro/sourceappl)  | The identifier of the local file sender application.  |
| [SPART](../../../command_summary/parameter_intro/spart)  | Network designation by which the local Transfer CFT monitor identifies itself to its partner. |
| [STATE](../../../command_summary/parameter_intro/state) | Defines the transfer request state. |
| [SUSER](../../../command_summary/parameter_intro/suser)  | Identifier of the file sender user. |
| [TARGETAPPL](../../../command_summary/parameter_intro/targetappl)  | Identifier of the local file receiver application.  |
| [TRK](../../../command_summary/parameter_intro/trk) | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [USERID](../../../command_summary/parameter_intro/userid) | Identifier of the transfer owner. |
| [WFNAME](../../../command_summary/parameter_intro/wfname)  | Name of the temporary file used to send a group of files selected in line with the generic name specified in FNAME. |
| [XLATE](../../../command_summary/parameter_intro/xlate)  | Identifier of the translation table used for send transfers relative to this model file. |


<span id="Additional_filename_information"></span>

### Additional filename information

In the sender server with implicit send operating mode, the use
of the FNAME parameter is mandatory in CFTSEND.

In sender requesteror sender server mode with a hold for sending request, the filename can
be defined in the send transfer request (SEND command or CFTAPI call)
rather than in the CFTSEND object. Click on the links in the following
table for examples and details.


| Format  | Processing  |
| --- | --- |
| [FNAME = filename](../../../command_summary/filename_conventions)  | Sends a file or a version of a file  |
| FNAME = {mask &#124; dirname}  | Lists a directory  |
| [FNAME = #filename]()  | Sends a group of files, the list of which is located in the specified file  |
| FNAME = {#mask &#124; #dirname}  |  • Sends a group of files selected in line with the generic name specified (#mask)<br/> • Sends all files in the directory specified (#dirname)  |


<span id="Example"></span>

Examples
--------

This section provides examples on how to define the CFTSEND template.

<span id="Implicit_send_example"></span>

### Implicit send

```
CFTSEND
MODE = REPLACE,
ID = SNDIMPL,     /\* IDF implicit send transfers \*/
IMPL = YES,
FCODE = EBCDIC,      /\* EBCDIC data in file \*/
FNAME = ’JSTATI’     /\* called ... \*/
```

Only used if:

- The value of the
    transfer IDF is SNDIMPL
- Transfer CFT responds
    to a send request from the partner, where Transfer CFT is the sender server
- There is no SEND
    request for this IDF pending in the catalog
- An implicit send
    mechanism is involved where IMPL = YES

### Default description of the model file

```
CFTSEND
MODE = REPLACE,
ID = IDFDEF,  /\* default IDF \*/
IMPL = NO,
FCODE = ASCII /\* EBCDIC data in file \*/
```

Corresponds to a send transfer when the SEND command specifies an IDF
not described by a CFTSEND object. This is the default description of
the model file to be sent. The CFTPARM object must specify:

```
CFTPARM DEFAULT = IDFDEF, ...
```

The SEND command specifies the name of the file to be sent, the FNAME
parameter.

### Cyclic send

```
CFTSEND
MODE = CREATE,
 ID = STAT,                   /\* File identifier \*/
FLRECL = 128, /\* of max length 128 bytes \*/
FACTION = DELETE /\* Delete after send \*/
FCODE = ASCII, /\* File coding \*/
MINDATE = 19920703, /\* From 03/07/92 \*/
MINTIME = 1000, /\* at 10:00 (Monday) \*/
MAXDATE = 19921231, /\* Until 31/12/92 \*/
MAXTIME = 2000, /\* at 20:00 \*/
CYCLE = 7 /\* Every week \*/
TCYCLE = DAY, /\* CYCTIME takes the value 1000 \*/
 CYCDATE = 19920705, /\* Activate possible \*/ /\* first 3 days of \*/ /\* week, before \*/ /\* Wednesday 10:00 \*/
PARM = ’Day statistic’ /\* Associated parameter.for.\*/ /\*PeSIT CFT profile \*/
```

Meets a specific cyclic send requirement.
