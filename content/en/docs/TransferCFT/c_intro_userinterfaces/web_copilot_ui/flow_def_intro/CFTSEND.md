---

    title: CFTSEND - Send template
    linkTitle: Send templates - CFTSEND 
    weight: 210

---
This topic describes how to define the CFTSEND
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
| <a href="../../../command_summary/parameter_intro/ackexec">ACKEXEC</a>  | Name of the file describing the procedure to be executed when receiving an acknowledgement reply for the transfer.  |
| <a href="">ARCHIVEFNAME</a>  | The archived source file name after transfer completion if FACTION=ARCHIVE.<br/> <blockquote> **Note**<br/> The fname and archivefname must be on the same volume (all platforms).<br/> </blockquote>  |
| <a href="../../../command_summary/parameter_intro/comment">COMMENT</a> | Local alphanumeric comment associated with the send transfer. |
| <a href="../../../command_summary/parameter_intro/cycdate">CYCDATE</a> | Upper final date for activating the first transfer of a cycle. |
| <a href="../../../command_summary/parameter_intro/cycle">CYCLE</a> | Number of units defining the transfer cycle period. |
| <a href="../../../command_summary/parameter_intro/cyctime">CYCTIME</a> | Upper limit time for activating the first transfer of a cycle. |
| <a href="../../../command_summary/parameter_intro/delete">DELETE</a>  | Automatic deletion of the catalog entries in the "X" phase (done) for the corresponding IDF. |
| <a href="../../../command_summary/parameter_intro/delete">EXEC</a> | Name of the file describing the procedure to be executed on completion of the transfer. |
| <a href="../../../command_summary/parameter_intro/execsub">EXECSUB</a> | Submission policy of the end-of-transfer procedure, when sending a group of files.  |
| <a href="../../../command_summary/parameter_intro/execsuba">EXECSUBA</a>  | Submission policy of the procedure to launch receiving acknowledgement, when sending a group of files.  |
| <a href="../../../command_summary/parameter_intro/execsubpre">EXECSUBPRE</a>  | Submission policy of the preprocessing procedure, when sending a group of files.  |
| <a href="../../../command_summary/parameter_intro/exit">EXIT</a> | Identifier of the CFTEXIT command associated with this transfer.<br/> Used to activate a file EXIT user task. The value of this parameter may be equal to the symbolic variable &amp;IDF. |
| <a href="../../../command_summary/parameter_intro/faction">FACTION</a> | Action on the file after a send transfer.<br/> If you are using FACTION=DELETE, see the <a href="../../../command_summary/parameter_intro/faction">FACTION</a> parameter specifics. |
| <a href="../../../command_summary/parameter_intro/fblksize">FBLKSIZE</a> | Block size of sent file. |
| <a href="../../../command_summary/parameter_intro/fcode">FCODE</a> | Code of the data to be sent. |
| <a href="../../../command_summary/parameter_intro/fdisp">FDISP</a> | File sharing option. |
| <a href="../../../command_summary/parameter_intro/filter">FILTER</a>  | Defines the filter value to be used when filtering using method defined in filtertype.  |
| <a href="../../../command_summary/parameter_intro/filtertype">FILTERTYPE</a>  | Defines the type of filter to be applied when sending a generic transfer.  |
| <a href="../../../command_summary/parameter_intro/fkeylen">FKEYLEN</a>  | Key length of an indexed file. |
| <a href="../../../command_summary/parameter_intro/fkeypos">FKEYPOS</a> | Key position relative to 0 in the record of an indexed file. |
| <a href="../../../command_summary/parameter_intro/flowname">FLOWNAME</a>  | The local flow descriptor.  |
| <a href="../../../command_summary/parameter_intro/flrec">FLRECL</a>  | File record length in bytes. |
| <a href="../../../command_summary/parameter_intro/fname">FNAME</a> | Name of the local file, directory, indirection file, selection mask or selection directory to be sent.<br/> See also <a href="#Additional_filename_information">Additional filename information</a> |
| <a href="../../../command_summary/parameter_intro/force">FORCE</a> | Determines the priority with which the parameters set in CFTSEND are taken into account relative to the parameters set in an associated SEND command. |
| <a href="../../../command_summary/parameter_intro/forg">FORG</a>  | Organization of the file to be sent:<br/> • SEQ: sequential<br/> • INDEXED: indexed<br/> • DIRECT: relative direct access |
| <a href="../../../command_summary/parameter_intro/frecfm">FRECFM</a> | Record format of the file to be sent:<br/> • F: fixed<br/> • V: variable,<br/> • U: undefined |
| <a href="../../../command_summary/parameter_intro/fspace">FSPACE</a>  | Size of the file to be sent, in K-bytes (1 K-byte = 1024 bytes). |
| <a href="../../../command_summary/parameter_intro/ftype">FTYPE</a> | Type of local file to be sent. Also see the NTYPE parameter. |
| <a href="../../../command_summary/parameter_intro/groupid">GROUPID</a> | Information completing the USERID of the CFTSEND command. |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | Local identifier of the model file (IDF) to be sent. |
| <a href="../../../command_summary/parameter_intro/ida">IDA</a>  | Local transfer identifier assigned by the user or user application.  |
| <a href="../../../command_summary/parameter_intro/impl">IMPL</a> | Implicit send. |
| <a href="../../../command_summary/parameter_intro/maxdate">MAXDATE</a> | Transfer validity final date. |
| <a href="../../../command_summary/parameter_intro/maxtime">MAXTIME</a> | Transfer validity limit time for the final date (MAXDATE). |
| <a href="../../../command_summary/parameter_intro/mindate">MINDATE</a> | Minimum transfer validity date.<br/> Only taken into account in requester mode |
| <a href="../../../command_summary/parameter_intro/nblksize">NBLKSIZE</a> | File block size, in protocol terms. |
| <a href="../../../command_summary/parameter_intro/ncode">NCODE</a>  | Sent data code. |
| <a href="../../../command_summary/parameter_intro/ncomp">NCOMP</a> | Compression of on-line data required by the sender. |
| <a href="../../../command_summary/parameter_intro/netband">NETBAND</a> | Select the outgoing port range. |
| <a href="../../../command_summary/parameter_intro/nfname">NFNAME</a> | Name of the physical file sent to the receiver partner. |
| <a href="../../../command_summary/parameter_intro/nkeylen">NKEYLEN</a> | Length (in bytes) of the key of an indexed file. |
| <a href="../../../command_summary/parameter_intro/nkeypos">NKEYPOS</a> | Location (in bytes) of the key in the records of an indexed file. |
| <a href="../../../command_summary/parameter_intro/nlrecl">NLRECL</a> | For records of:<br/> • Fixed format (FRECFM = F): size in bytes of the records of the receiver file<br/> • Variable format (FRECFM = V): maximum size in bytes of the records |
| <a href="../../../command_summary/parameter_intro/notify">NOTIFY</a> | Defines the destination of the messages associated with the send transfer selected from the log file messages, by the value of the OPERMSG parameter. |
| <a href="../../../command_summary/parameter_intro/nrecfm">NRECFM</a> | Format of the records of the file defined in protocol terms. |
| <a href="../../../command_summary/parameter_intro/ntype">NTYPE</a>  | File type, in protocol terms. |
| <a href="../../../command_summary/parameter_intro/opermsg">OPERMSG</a>  | Defines the categories of transfer information messages intended for the operator, with all the messages also being written in the log file. |
| <a href="../../../command_summary/parameter_intro/parm">PARM</a>  | User parameter sent to the receiver. |
| <a href="../../../command_summary/parameter_intro/preexec">PREEXEC</a>  | Name of the file describing the procedure to be executed before the transfer, as per the preprocessing phase.  |
| <a href="../../../command_summary/parameter_intro/pri">PRI</a> | Send request selection priority. |
| <a href="../../../command_summary/parameter_intro/ruser">RUSER</a> | Identifier of the file receiver user. |
| <a href="../../../command_summary/parameter_intro/selfname">SELFNAME</a>  | Name of the file that contains the list of files selected for sending.<br/> <blockquote> **Note**<br/> When using SELFNAME and FACTION=DELETE, the FNAME must be a directory and not a MASK. For example, #dir is deleted, whereas #dir/* is ignored.<br/> </blockquote>  |
| <a href="../../../command_summary/parameter_intro/sourceappl">SOURCEAPPL</a>  | The identifier of the local file sender application.  |
| <a href="../../../command_summary/parameter_intro/spart">SPART</a>  | Network designation by which the local Transfer CFT monitor identifies itself to its partner. |
| <a href="../../../command_summary/parameter_intro/state">STATE</a> | Defines the transfer request state. |
| <a href="../../../command_summary/parameter_intro/suser">SUSER</a>  | Identifier of the file sender user. |
| <a href="../../../command_summary/parameter_intro/targetappl">TARGETAPPL</a>  | Identifier of the local file receiver application.  |
| <a href="../../../command_summary/parameter_intro/trk">TRK</a> | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/userid">USERID</a> | Identifier of the transfer owner. |
| <a href="../../../command_summary/parameter_intro/wfname">WFNAME</a>  | Name of the temporary file used to send a group of files selected in line with the generic name specified in FNAME. |
| <a href="../../../command_summary/parameter_intro/xlate">XLATE</a>  | Identifier of the translation table used for send transfers relative to this model file. |


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
| <a href="../../../command_summary/filename_conventions">FNAME = filename</a>  | Sends a file or a version of a file  |
| FNAME = {mask | dirname}  | Lists a directory  |
| <a href="">FNAME = #filename</a>  | Sends a group of files, the list of which is located in the specified file  |
| FNAME = {#mask | #dirname}  |  • Sends a group of files selected in line with the generic name specified (#mask)<br/> • Sends all files in the directory specified (#dirname)  |


<span id="Example"></span>

## Examples

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
