{
    "title": "Define the source",
    "linkTitle": "Define the source",
    "weight": "420"
}The following tables describe the parameters available for the source side of a flow definition.

Transfer properties
-------------------


| CG parameter  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Transfer priority  | LOW,<br /> MEDIUM: default,<br /> HIGH,<br /> CUSTOM  | CFTSEND, pri<br /> <br /> LOW -&gt;0<br /> MEDIUM-&gt;128<br /> HIGH-&gt; 255<br /> CUSTOM-&gt; integer between 0...255  | Transfer priorities are equivalent to integer values ranging from 0 (low) to 255 (high).<br /> When {{< TransferCFT/axwayvariablesComponentShortName  >}} reaches the maximum number of transfers allowed, it queues transfers. When an ongoing transfer is finished and a slot is available for a new transfer, the system selects the one with the highest priority.<br /> The same priority is used for the transfer in target side  |
| Bandwidth allocation  | LOW ,<br /> MEDIUM : default, HIGH  | CFTSEND, cos<br /> LOW -&gt;0<br /> MEDIUM-&gt;1<br /> HIGH-&gt; 2  | The amount of bandwidth allocated to this flow. The value you select determines the data transfer rate for this flow.<br /> The same Bandwidth allocation is used for the transfer in target side  |
| Transfer state  | Ready: default ,<br /> On hold,<br /> Kept  | CFTSEND, state<br /> <br /> Ready (D) -&gt; DISP<br /> On hold (H) -&gt;HOLD<br /> Kept (K) -&gt;KEEP  | Indicates the state of the transfer request.: Ready, On Hold, Kept.<br /> Field is available in UI only if the Initiator is the source.<br /> If Target is the initiator, in source side the transfer state is ready and the field cannot be configured in CG UI.<br />  |
| User id  | string, max 32, empty by default  | CFTSEND, userid | Identifier of the transfer owner. If this parameter is not defined, its default value is the system &quot;userid&quot; of the {{< TransferCFT/axwayvariablesComponentShortName  >}}.  |
| Detect duplicate transfers  | string max 512, empty by default  | CFTSEND, duplicat  | This field is used in detecting duplicate transfers and may contain a list of symbolic variables separated by a period &quot;.&quot;.  |
| Compress file  | Yes: default,<br /> No  | CFTSEND, comp<br /> <br /> Yes → 15<br /> No → 0  | Indicates whether files are compressed before they are transferred.<br /> Same value will be used for the compression on target side  |
| On file modification  | Continue transfer: default,<br /> Stop transfer  | CFTSEND , fdisp<br /> Continue transfer -&gt; SHR<br /> Stop transfer-&gt; CHECK  | Available only in source side.<br /> Specify what happens if files are modified during the transfer.  |
| Action after transfer  | Delete file ,<br /> Delete file content,<br /> None: default  | CFTSEND, faction<br /> <br /> Delete file -&gt; Delete<br /> Delete file content -&gt; Erase<br /> None-&gt; None: default  | Specifies what happens to the file in source side when the transfer is complete.<br /> Delete file – Deletes the file.<br /> Delete file content – Removes the contents of the file but leaves the &quot;end of file&quot; mark at the beginning of the file.<br /> None – No action is performed on the file.  |
| Delete file on purge  | Ready (D) ,<br /> Transferring (C), On Hold (H),<br /> Kept (K), Transferred (T), Executed (X) default: no selection  | CFTSEND, fdelete<br /> <br /> Ready (D) -&gt;D<br /> Transferring (C) -&gt; C<br /> On Hold (H) -&gt; H<br /> Kept (K) -&gt; K<br /> Transferred (T) -&gt; T<br /> Executed (X) -&gt; X  | Indicates the transfer states of files that will be deleted when you remove the associated transfers from the transfer list or when you purge the transfer list. You can select any combination of statuses. If you do not select anything, files are not deleted even when the associated transfers are removed from the transfer list.<br /> Ready – The transfer is available and can start immediately.<br /> Transferring – The transfer is being executed.<br /> On hold – The transfer was interrupted due to an error, such as a network failure, or by a user.<br /> Kept – The transfer was suspended by {{< TransferCFT/axwayvariablesComponentShortName  >}} or by a user.<br /> Transferred – The transfer was successfully completed.<br /> Executed – The transfer was ended by an application or user.  |
| Additional information  | string max 512, empty by default  | CFTSEND, parm  | Use this field for any information you want to provide.  |


File properties &gt; files
--------------------------

Indicates whether you are sending a single file or multiple files.


| CG parameter &lt;/th&gt;  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Single - &gt; Path  | string max 64  | CFTSEND, fname  | Indicate the single file to be sent.  |
| Multiple -&gt; Path  | string max 64  | CFTSEND, fname  | If you selected Multiple, the value you enter can be:<br /> A directory name – All the files in this directory will be transferred.<br /> A generic file name, including wildcard characters – Only files that match are transferred. For example, mydirectory/toto*.  |
| Multiple -&gt; File list  | string max 64  | CFTSEND, selfname  | This field is displayed if you selected Multiple in the Files field.<br /> Specify the name of the file that contains the list of files to be transferred. This file is also referred to as an indirection file. It must contain one file name per record, and that name must start in the first column of the file. The file names contained in the file must not contain an asterisk (*).<br /> When specifying a file here, the Path field is also required.  |
| Multiple -&gt; Archive name  | string max 64,<br /> &amp;IDF.&amp;idtu.rcv  | CFTSEND, wfname  | Name of the file that contains the set of files to be transmitted. Archive files are sent between systems that have the same operating system (grouped mode). The archive file is created automatically by {{< TransferCFT/axwayvariablesComponentShortName  >}} at the time of the transfer. The file created will be a zip file on Windows systems and a tar file on Linux/UNIX systems. Because Windows systems do not have default compression utilities, {{< TransferCFT/axwayvariablesComponentShortName  >}} for Windows includes zip and unzip utilities.  |
| File name sent  | string max 64  | CFTSEND, nfname  | Specify the name of the physical file that is to be used during transmission over the network.  |
| Signature file  | string max 64  | CFTSEND, sigfname  | Specify a file name that contains multiple signatures. The signatures in this file must conform to the format &lt;signature_path&gt; where path is the file path specified in the Path field.  |


File properties &gt; file type
------------------------------


| CG parameter  | CFTUTIL parameter  | Description  |
| --- | --- | --- |
| Binary  | CFTSEND, fcode =B  | Specify whether the file is a binary file.  |
| Text  | CFTSEND, see configuration for FTYPE=TEXT | Specify whether the file is a text file.  |
| Record format  | CFTSEND, see configuration for record format | Indicate whether the records in the file are fixed or variable length.  |


Processing scripts &gt; pre-processing
--------------------------------------


| CG parameter &lt;/th&gt;  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTSEND, preexec  | Specify the script to be executed before the file is transferred.  |
| State  | Ready: default,<br /> On hold  | CFTSEND, prestate<br /> Ready -&gt;DISP : default<br /> On hold -&gt; HOLD  | Indicate the status of the transfer on the source. The script is run only if the transfer is in the specified state.<br /> Ready – Indicates that the transfer is available and can start immediately.<br /> On hold – Indicates that the transfer is deferred until a remote receive request is accepted, or until a local START command changes this transfer to the ready state.  |
| Apply to broadcast list  | On main request: default,<br /> For each target in the list,<br /> Both  | CFTDEST, execpre<br /> <br /> On main request -&gt; DEST For each target in the list -&gt; CHILDREN<br /> Both -&gt; PART  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |


Processing scripts &gt; post-processing
---------------------------------------


| CG parameter &lt;/th&gt;  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTSEND, exec  | Specify the script to be executed after the file is transferred.  |
| Apply to group of files  | On main request: Default,<br /> For each file in group,<br /> Both  | CFTSEND, execsub<br /> On main request -&gt; LIST<br /> For each file in group -&gt; FILE<br /> Both -&gt; SUBF  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> Values – On main request &#124; For each target in the list &#124; Both<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |
| Apply to broadcast list  | On main request: default,<br /> For each target in the list,<br /> Both  | CFTDEST, exec<br /> <br /> On main request -&gt; DEST For each target in the list -&gt; CHILDREN<br /> Both -&gt; PART  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |


Processing scripts &gt; acknowledgment
--------------------------------------


| CG parameter &lt;/th&gt;  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTSEND, ackexec  | Specify the script to be executed after an acknowledgement is received for a sent file.  |
| State  | Require,<br /> Ignore: default  | CFTSEND, ackstate<br /> Require -&gt; REQUIRE<br /> Ignore -&gt; IGNORE  | Indicate if the transfer must wait for an acknowledgement.<br /> Require – The transfer must wait for an acknowledgement before it can be considered complete.<br /> Ignore – The transfer can be considered complete, even if an acknowledgement is not received.  |
| Apply to group of files  | On main request,<br /> For each file in group,<br /> Both: default  | CFTSEND, execsub<br /> On main request -&gt; LIST<br /> For each file in group -&gt; FILE<br /> Both -&gt; SUBF  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> Values – On main request &#124; For each target in the list &#124; Both<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |
| Apply to broadcast list  | On main request: default,<br /> For each target in the list,<br /> Both  | CFTDEST, execa<br /> <br /> On main request -&gt; DEST For each target in the list -&gt; CHILDREN<br /> Both -&gt; PART  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |


Processing scripts &gt; error
-----------------------------


| CG parameter &lt;/th&gt;  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTSEND, exece  | Specify the script to be executed after an error occurs during a transfer.  |


 

****&lt;&lt;**** [](../../)
