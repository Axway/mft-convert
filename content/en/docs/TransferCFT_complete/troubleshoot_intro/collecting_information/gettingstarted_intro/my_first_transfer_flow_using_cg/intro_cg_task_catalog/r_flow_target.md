{
    "title": "Define the target",
    "linkTitle": "Define the target",
    "weight": "430"
}The following tables describe the parameters available in flow definition, target side.

Transfer properties
-------------------


| CG parameter  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Transfer state  | Ready: default ,<br /> On hold,<br /> Kept  | CFTRECV, state<br /> <br /> Ready (D) -&gt; DISP<br /> On hold (H) -&gt;HOLD<br /> Kept (K) -&gt;KEEP  | Indicates the state of the transfer request.: Ready, On Hold, Kept<br /> Field is available in UI only if the Initiator is the target.<br /> If Source is the initiator, in Target side the transfer state is ready and the field cannot be configured in CG UI.  |
| User id  | string, max 32, empty by default  | CFTRECV, userid  | Identifier of the transfer owner. If this parameter is not defined, its default value is the system &quot;userid&quot; of the {{< TransferCFT/axwayvariablesComponentShortName  >}}.  |
| Detect duplicate transfers  | string max 512, empty by default  | CFTRECV, duplicat  | This field is used in detecting duplicate transfers and may contain a list of symbolic variables separated by a period &quot;.&quot;.  |
| No file exists  | Create: default,<br /> Cancel  | CFTRECV | Specifies the action taken if the received file does not already exist.<br /> Create – The file is created.<br /> Cancel – The transfer is refused.  |
| File exists  | Delete: default,<br /> Cancel,<br /> Overwrite, Overwrite only if empty  | CFTRECV | Specifies the action taken if the received file exists.<br /> Delete – The existing file is deleted.<br /> Cancel – The transfer is refused.<br /> Overwrite – The existing file is overwritten.<br /> Overwrite only if empty – The existing file is overwritten only if it contains no data.  |
| Aborted transfer  | Keep: Default,<br /> Delete  | CFTRECV, rkerror  | Specifies the action taken if a transfer is terminated due to a file creation error on the target.<br /> Keep – The transfer remains in the transfer list.<br /> Delete – The transfer is removed from the transfer list.  |
| Delete file on purge  | Ready (D) ,<br /> Transferring (C), On Hold (H),<br /> Kept (K), Transferred (T), Executed (X) default: no selection  | CFTRECV, fdelete<br /> <br /> Ready (D) -&gt;D<br /> Transferring (C) -&gt; C<br /> On Hold (H) -&gt; H<br /> Kept (K) -&gt; K<br /> Transferred (T) -&gt; T<br /> Executed (X) -&gt; X  | Indicates the transfer states of files that will be deleted when you remove the associated transfers from the transfer list or when you purge the transfer list. You can select any combination of statuses. If you do not select anything, files are not deleted even when the associated transfers are removed from the transfer list.<br /> Ready – The transfer is available and can start immediately.<br /> Transferring – The transfer is being executed.<br /> On hold – The transfer was interrupted due to an error, such as a network failure, or by a user.<br /> Kept – The transfer was suspended by {{< TransferCFT/axwayvariablesComponentShortName  >}} or by a user.<br /> Transferred – The transfer was successfully completed.<br /> Executed – The transfer was ended by an application or user.  |


File properties &gt; files
--------------------------

Indicates whether you are sending a single file or multiple files.


| CG parameter  | CG values  | CFTUTIL‑parameter  | Description  |
| --- | --- | --- | --- |
| Filename  | string max 64,<br /> Default value: pub\&amp;IDF.&amp;IDTU.&amp;FROOT.RCV  | CFTRECV, fname  | Specify the file name or full path name for the received file or files. This field is required if the initiator of the flow is the source. Default value: pub\&amp;IDF.&amp;IDTU.&amp;FROOT.RCV  |
| Temporary file  | string max 64  | CFTRECV, wfname  | Specify the name of the temporary file used during the transfer. When the transfer is complete, the temporary file is renamed using the name defined in the Filename field. If you do not specify a value, {{< TransferCFT/axwayvariablesComponentShortName  >}} directly creates the file with the name specified in the Filename field.  |


File properties &gt; file type
------------------------------


| CG parameter  | CFTUTIL parameter  | Description  |
| --- | --- | --- |
| Binary  | CFTRECV, fcode =B  | Specify whether the file is a binary file.  |
| Text  | CFTRECV, see configuration for FTYPE=TEXT | Specify whether the file is a text file.  |
| Record format  | CFTRECV, see configuration for record format | Indicate whether the records in the file are fixed or variable length.  |


Processing scripts &gt; post-processing
---------------------------------------


| CG parameter  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTRECV, exec  | Specify the script to be executed after the file is received.  |
| Apply to group of files  | On main request: Default,<br /> For each file in group,<br /> Both  | CFTRECV, execsub<br /> On main request -&gt; LIST<br /> For each file in group -&gt; FILE<br /> Both -&gt; SUBF  | This field is displayed if you enabled a broadcast list in source transfer properties.<br /> Values – On main request &#124; For each target in the list &#124; Both<br /> On main request – Executes the script only on the main request.<br /> For each target in the list – Executes the script only for each target in the list.<br /> Both – Executes the script both for the main request and for each target in the list.  |
| Apply to collect list  | On main request: default,<br /> For each source in the list,<br /> Both  | CFTDEST, exec<br /> <br /> On main request -&gt; DEST For each source in the list -&gt; CHILDREN<br /> Both -&gt; PART  | This field is displayed if you enabled a collect list in target transfer properties.<br /> On main request – Executes the script only on the main request.<br /> For each source in the list – Executes the script only for each source in the list.<br /> Both – Executes the script both for the main request and for each source in the list.  |


Processing scripts &gt; acknowledgment
--------------------------------------


| CG parameter  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTRECV, ackexec  | Specify the script to be executed after the file is received and post-processing is complete.  |
| State  | Require,<br /> Ignore: default  | CFTRECV, ackstate<br /> Require -&gt; REQUIRE<br /> Ignore -&gt; IGNORE  | Indicate if the transfer must wait for an acknowledgement.<br /> Require – The transfer must wait for an acknowledgement before it can be considered complete.<br /> Ignore – The transfer can be considered complete, even if an acknowledgement is not received.  |
| Apply to collect list  | On main request: default,<br /> For each source in the list,<br /> Both  | CFTDEST, execa<br /> <br /> On main request -&gt; DEST For each source in the list -&gt; CHILDREN<br /> Both -&gt; PART  | This field is displayed if you enabled a collect list in target transfer properties.<br /> On main request – Executes the script only on the main request.<br /> For each source in the list – Executes the script only for each source in the list.<br /> Both – Executes the script both for the main request and for each source in the list.  |


Processing scripts &gt; error
-----------------------------


| CG parameter  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| Script -&gt; Filename  | if Custom, Filename field: string of max 512c  | CFTRECV, exece  | Specify the script to be executed if an error occurs when a file is received.  |


 

****&lt;&lt;**** [](../../)
