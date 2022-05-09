{
    "title": "Transfer CFT messages:  CFTF",
    "linkTitle": "CFTF messages",
    "weight": "300"
}This topic lists the CFTFxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`


| V23 format<br/> V24 format<br/> Error | <span id="CFTF01E"></span>CFTF01E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file [&amp;fname] creation error &amp;diagi<br/> CFTF01E local file [&amp;fname] creation error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer request a local error was detected when creating a file. |
| Consequence | The transfer is not executed. The corresponding entry in the catalog is set to the K state if RKERROR=KEEP and the catalog entry is deleted by the {{< TransferCFT/axwayvariablesComponentShortName  >}} if RKERROR=DELETE. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF02E"></span>CFTF02E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file selection error &amp;scs<br/> CFTF02E local file selection error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer request a local error was detected when selecting a file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Warning | CFTF02W PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file selection error (file not found ignored) &amp;scs<br/> CFTF02W local file selection error (file not found ignored) &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer request, a local error was detected when selecting a file.  |
| Consequence | When you set filenotfound to ignore in the transfer request, the transfer is executed, the file is ignored, and the corresponding catalog entry is terminated (completed).  |
| Action | You can ignore the message.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF03E"></span>CFTF03E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file deselect error &amp;scs<br/> CFTF03E local file deselect error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when releasing the transferred file. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF04E"></span>CFTF04E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file open error &amp;scs<br/> CFTF04E local file open error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer request a local error was detected when opening a file. |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF05E"></span>CFTF05E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file close error &amp;scs<br/> CFTF05E local file deselect error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when closing the file. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF06E"></span>CFTF06E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file note error &amp;scs<br/> CFTF06E local file note error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&lt;/p&gt; |
| --- | --- |
| Explanation | During a transfer a local error was detected when recording the current position in the file. |
| Consequence | The transfer is interrupted and the corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF07E"></span>CFTF07E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file point error &amp;scs<br/> CFTF07E local file point error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer restart a local error was detected when repositioning the pointer in the file. |
| Consequence | The transfer is not restarted and the corresponding catalog entry remains on HOLD. |
| Action | Analyze the file access system code, correct the error and restart the transfer. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF08E"></span>CFTF08E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file read error &amp;scs<br/> CFTF08E local file read error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when reading the file. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF09E"></span>CFTF09E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file write error &amp;scs<br/> CFTF09E local file write error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when writing the file. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the file access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF13E"></span>CFTF13E PART=&amp;part IDF=&amp;idf IDT=&amp;idt remote file deselect error &amp;diagp<br/> CFTF13E remote file deselect error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | The file deselect operation was refused by the partner. |
| Consequence | The transfer is not executed. The corresponding catalog entry is put on HOLD. |
| Action | Correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF21E"></span>CFTF21E PART=&amp;part IDF=&amp;idf IDT=&amp;idt storage allocation error &amp;scs<br/> CFTF21E storage allocation error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when allocating memory. |
| Consequence | The transfer is interrupted. The corresponding catalog entry is put on HOLD. |
| Action | Analyze the memory access system code, correct the error and try again. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF22E"></span>CFTF22E PART=&amp;part IDF=&amp;idf IDT=&amp;idt duplicate file &amp;diagp<br/> CFTF22E duplicate file &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | A transfer requests the creation of a file but the file already exists - the FDISP parameter prevents the existing file from being erased. |
| Consequence | The transfer is not executed and the corresponding catalog entry is set to the KEEP status. |
| Action | Delete the existing file, revise the parameter settings, or receive the data in another file. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF23E"></span>CFTF23E PART=&amp;part IDF=&amp;idf IDT=&amp;idt file space allocation exhausted &amp;scs<br/> CFTF23E file space allocation exhausted &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | During a transfer a local error was detected when writing the file (insufficient space reserved for the file). |
| Consequence | The transfer is interrupted and the corresponding catalog entry is set to the KEEP status. |
| Action | Allocate more available space for this file. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF24E"></span>CFTF24E PART=&amp;part IDF=&amp;idf IDT=&amp;idt duplicate working file<br/> CFTF24E duplicate working file &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | During a transfer using an intermediate file (CFTRECV FNAME =) an error was detected: the intermediate file already exists and consequently cannot be created. |
| Consequence | The transfer is interrupted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF25E"></span>CFTF25E PART=&amp;part IDF=&amp;idf IDT=&amp;idt working file rename error &amp;scs<br/> CFTF25E working file rename error &amp;scs &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &gt; |
| --- | --- |
| Explanation | At the end of a transfer using an intermediate file (CFTRECV WFNAME =) an error was detected: the intermediate file could not be renamed. |
| Consequence | The file is correctly transferred but an error is reported to the {{< TransferCFT/axwayvariablesComponentShortName  >}} user. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF26E"></span>CFTF26E PART=&amp;part IDF=&amp;idf IDT=&amp;idt will be unable to rename working file<br/> CFTF26E will be unable to rename working file &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | During a transfer using an intermediate file (CFTRECV FNAME = ) an error was detected: the intermediate file could not be renamed at the end of the transfer (as the real file already exists). |
| Consequence | The transfer is interrupted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF27E"></span>CFTF27E PART=part IDF=idf IDT=idt local /virtual file attribute mismatch<br/> CFTF27E local/virtual file attribute mismatch &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | There are invalid file attributes in CFTRECV command. The local file attributes, FLRECL or FRECFM, do not match with virtual file attributes.<br/> (This is only applicable if FCHECK parameter is set to YES in the CFTRECV command.) |
| Consequence | The transfer is cancelled with DIAGP 139. DIAGP = ERR LRECL or ERR RECFM. |


 


| V23 format<br/> V24 format<br/> V23 format<br/> V24 format<br/> Warning | <span id="CFTF30W"></span>CFTF30W PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagc<br/> CFTF30W +PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;str<br/> CFTF30W &amp;diagc &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &gt;<br/> CFTF30W+&amp;str &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt &gt; |
| --- | --- |
| Explanation | After an error, an additional message or two containing the DIAGC zones are displayed:<br/> • CFTF02E PART=&amp;part IDF=&amp;idf IDT=&amp;idt local file selection error xxxx<br/> • CFTF30W PART=&amp;part IDF=&amp;idf IDT=&amp;idt &amp;diagc<br/> • CFTF30W+U=&lt;user&gt; F=&lt;file&gt; PART=&amp;part IDF=&amp;idf IDT=&amp;idt<br/> Where &amp;diagc could be:<br/> • SFM_ALLOC: file not found<br/><br/> • SFM_ALLOC: CFTSU socket: connection refused (this occurs when USERCTRL is set to yes, but the user does not have read-file privileges - Unix only)<br/> • HTTP 403: when using AWS the DNS connection was refused |
| Action  | If the connection was refused for AWS, check that the server DNS is correctly configured. See the [Amazon 3 (ASW) troubleshooting](../../../app_integration_intro/amazon_s3) section. |


 


| V23 format<br/> V24 format<br/> Information | CFTF31I PART=&amp;part IDF=&amp;idf IDT=&amp;idt EXIT=&amp;id &amp;user1 changed to &amp;user2<br/> <span id="CFTF31I"></span>CFTF31I PART=&amp;idf IDF=&amp;idf IDT=&amp;idt EXIT=&amp;id &amp;user1 User id changed to &amp;user2 the user id is modified by the exit |
| --- | --- |
| Explanation | Possibility for a file type exit to change the transfer owner (userid) in the Before File Allocation phase for a new transfer. This possibility is offered regardless of the transfer direction.<br/> • When sending, to open the file for the 'userid' account furnished by the exit.<br/> • When receiving, to create the file for the 'userid' provided by the exit. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF32E"></span>CFTF32E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Maximum number of rename retries reached<br/> CFTF32E Maximum number of rename retries reached &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Maximum number of rename retries reached &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;. |
| Action  | Check why the filename exists (when it should not).<br/> Correct filename issue, for example manually rename the filename in question.<br/> Restart the transfer.<br/> See also [Post-transfer file renaming](../../../app_integration_intro/spoolout). |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTF33I"></span>CFTF33I PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Rename to FNAME=&amp;FNAME done<br/> CFTF33I Rename to FNAME=&amp;fname done &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The file was renamed, when using RETRYRENAME, and any post-processing or exits are executed.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF34E"></span>CFTF34E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ WFNAME=&amp;wfname not found<br/> CFTF34E WFNAME=&amp;fname not found &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | The rename operation failed because the file referenced by &amp;wfname cannot be accessed (or does not exist).  |
| Action  | Check that the wfname exists on the receiver side.<br/> • If not, recreate the transfer on the sender side.<br/> • If so, check that the &amp;userid can access the file, fix the user rights if necessary, and restart the transfer on the receiver side. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTF35W"></span>CFTF35W PART=&amp;part IDF=&amp;idf IDT=&amp;idt Rename to FNAME=&amp;fname failed, will be retried at &amp;datetime<br/> CFTF35W Rename to FNAME=&amp;fname failed, will be retried at &amp;datetime &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | Could not rename the temporary file to &amp;fname because the file referenced by &amp;fname already exists. Transfer CFT will try again at &amp;datetime.  |


 


| V23 format<br/> V24 format<br/> Error  | <span id="CFTF38E"></span>CFTF38E The file or directory &amp;fname can't be renamed into &amp;newfname for reason code &amp;reason<br/> CFTF38E The file or directory &amp;fname can't be renamed into &amp;newfname for reason code &amp;reason |
| --- | --- |
| Explanation  | Cannot rename the file or directory.  |
| Action  | Check the reason code and make the appropriate action if possible.  |


 


| V23 format<br/> V24 format<br/> Error  | <span id="CFTF39E"></span>CFTF39E Missing NFNAME when executing RECV &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;<br/> CFTF39E Missing NFNAME when executing RECV &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation  | The NFNAME parameter was empty or missing when executing a RECV command.  |
| Action  | Add the NFNAME parameter in your RECV command.  |



| V23 format<br/> V24 format<br/> Information | <span id="CFTF40I"></span>CFTF40I The file &amp;fname has been removed &lt;PART=&amp;part IDS=&amp;ids&gt;<br/> CFTF40I The file &amp;fname has been removed &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | A file was removed per an SFTP client request. |



| V23 format<br/> V24 format<br/> Error | <span id="CFTF41E"></span>CFTF41E The file &amp;fname can't be removed for reason code &amp;reason &lt;PART=&amp;part IDS=&amp;ids&gt;<br/> CFTF41E The file &amp;fname can't be removed for reason code &amp;reason &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | Could not remove the file. |
| Action  | Check the reason to code and take the appropriate action if possible. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTF42I"></span>CFTF42I The directory &amp;fname has been removed &lt;PART=&amp;part IDS=&amp;ids&gt;<br/> CFTF42I The directory &amp;fname has been removed &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | A directory has been removed as requested by a SFTP client. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF43E"></span>CFTF43E The directory &amp;fname can't be removed for reason code &amp;reason &lt;PART=&amp;part IDS=&amp;ids&gt;<br/> CFTF43E The directory &amp;fname can't be removed for reason code &amp;reason &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | Cannot remove the directory. |
| Action  | Check the reason code and take the appropriate action if possible. |


 


| V23 format<br/> V24 format<br/> Information  | <span id="CFTF44I"></span>CFTF44I The directory &amp;fname has been created &lt;PART=&amp;part IDS=&amp;ids&gt;<br/> CFTF44I The directory &amp;fname has been created &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation  | A directory was created per an SFTP client request.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTF45E"></span>CFTF45E The directory %s can't be created for reason code &amp;reason %d &lt;PART=%s IDS=&amp;ids&gt;<br/> CFTF45E The directory %s can't be created for reason code &amp;reason %d &lt;PART=%s IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | Cannot create the directory.  |
| Action  | Check the reason code, take appropriate steps, and retry as needed.  |


 


| V23 format<br/> V24 format<br/> V23 format<br/> V24 format<br/> Error  | <span id="CFTF46E"></span>CFTF46E Defined filename not inside WORKINGDIR &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;<br/> CFTF46E Defined filename not inside WORKINGDIR &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;<br/> CFTF46E+&amp;fname &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt;<br/> CFTF46E+&amp;fname &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation  | The selected file must be in the configured WORKINDIR.  |
| Action  | Select another file or change or remove the WORKINGDIR.  |


 


| V23 format<br/> V24 format<br/> Information  | <span id="CFTF47I"></span>CFTF47I The file rights of &amp;fname have been set to &amp;frights<br/> CFTF47I The file rights of &amp;fname have been set to &amp;frights |
| --- | --- |
| Explanation  | File rights were changed as requested by an SFTP client. The format of rights is the UNIX format, for example 666.  |


 


| V23 format<br/> V24 format<br/> Information  | <span id="CFTF48I"></span>CFTF48I The file time of &amp;fname has been set to &amp;time<br/> CFTF48I The file time of &amp;fname has been set to &amp;time |
| --- | --- |
| Explanation  | The file time was changed as requested by an SFTP client.  |


 


| V23 format<br/> V24 format<br/> Error  | <span id="CFTF49E"></span>CFTF49E The file properties of &amp;fname can't be changed for reason code &amp;reason<br/> CFTF49E The file properties of &amp;fname can't be changed for reason code &amp;reason |
| --- | --- |
| Explanation  | Cannot change the rights or time of a file.  |
| Action  | Check the reason code and make the appropriate action if possible.  |


 


| V23 format<br/> V24 format<br/> Information  | <span id="CFTF50I"></span>CFTF50I The file or directory &amp;fname has been renamed into &amp;newfname<br/> CFTF50I The file or directory &amp;fname has been renamed into &amp;newfname |
| --- | --- |
| Explanation  | A file or directory has been renamed as requested by a SFTP client.  |


 


| V23 format<br/> V24 format<br/> Warning  | <span id="CFTF51W"></span>CFTF51W [Faction or Naction]: The &amp;fname file can't be archived as &amp;archivefname due to reason code &amp;reason<br/> CFTF51W [Faction or Naction]: The &amp;fname file can't be archived as &amp;archivefname due to reason code &amp;reason |
| --- | --- |
| Explanation  | Could not move the file at the end of the transfer.<br/> • FACTION: See [NARCHIVEFNAME]() for details. |



| V23 format<br/> V24 format<br/> Error  | <span id="CFTF51W"></span><span id="CFTF52E"></span>CFTF52E The IDF &amp;idf is not allowed<br/> CFTF52E The IDF &amp;idf is not allowed |
| --- | --- |
| Explanation  | You cannot transfer a file using that IDF.  |
| Action  | Use another IDF on the client, or update the SAUTH/RAUTH parameters on the server.  |

