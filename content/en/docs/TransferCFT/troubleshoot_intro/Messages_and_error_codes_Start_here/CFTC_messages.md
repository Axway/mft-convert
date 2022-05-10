---
    "title": "Transfer CFT messages: CFTC",
    "linkTitle": "CFTC messages",
    "weight": "280"
---
This topic lists the CFTC (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

{{% TransferCFT/snippets/message_format%}}


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC01W"></span>CFTC01W CFT catalog storage is short n record(s) free <br/> CFTC01W CFT catalog storage is short n record(s) free  |
| --- | --- |
| Explanation | Catalog storage is short n record(s) free. |
| Consequence | The catalog is nearly full - there are only n records free. Consider modifications to free up space. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTC01E"></span>CFTC01E CFT catalog storage is full <br/> CFTC01E CFT catalog storage is full |
| --- | --- |
| Explanation | The catalog storage is full. A command SHUT FAST=KILL is executed. |
| Consequence | The stored commands can only be retrieved by restarting {{< TransferCFT/axwayvariablesComponentShortName  >}}. First purge the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog (and modify the retention dates, for example). |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC03W"></span>CFTC03W PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Running out of time (HHMMSSCC)<br/> CFTC03W Transfer Running out of time &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt [sgt=HHMMSSCC] |
| --- | --- |
| Explanation | The transfer start time is outside the interval authorized by the &amp;part partner. |
| Consequence | The next transfer retry will take place at the specified time [HHMMSSCC]. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC04W"></span>CFTC04W PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ State C delete forbidden<br/> CFTC04W _ State C delete forbidden &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt |
| --- | --- |
| Explanation | A DELETE command was executed on a catalog request (in the C state), but this operation is not allowed. |
| Consequence | The catalog entry could not be deleted. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC05W"></span>CFTC05W PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt _ Delete failed<br/> CFTC05W _ Delete failed &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt |
| --- | --- |
| Explanation | A DELETE command was executed on a catalog request (in a state other than C or D), but it failed as a result of a catalog access error. |
| Consequence | The catalog entry could not be deleted.<br/> The CFTT21E message may be displayed before this message. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTC06E"></span>CFTC06E PART=&amp;part IDF=&amp;idf IDT=&amp;idt _ Update failed<br/> CFTC06E _ Update failed &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt&gt; |
| --- | --- |
| Explanation | A Transfer CFT catalog access error was detected when executing commands, such as END, START, ACT and INACT. |
| Consequence | The catalog entry was not updated and the command was ignored.<br/> The CFTT21E message is displayed prior to this message. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC07I"></span>CFTC07I PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm]IDT=&amp;idt STATE=&amp;state - Deleted<br/> CFTC07I Transfer Deleted &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt STATE=&amp;state DIRECT=&amp;direct |
| --- | --- |
| Explanation | A Transfer CFT catalog entry for partner &amp;part, with identifier &amp;idf, idt &amp;idt and state &amp;state, has been deleted. |


 


| V23 format<br/> V24 format<br/> Information  | <span id="CFTC08I"></span>CFTC08I &amp;str<br/> CFTC08I &amp;str |
| --- | --- |
| Explanation  | Possible values for &amp;str are described here. The following messages are displayed when the catalog is purged on {{< TransferCFT/axwayvariablesComponentShortName  >}} startup, or at the time set for the daily purge. For example:<br/> ****When there are no transfers to delete:****<br/> <div > <code>Purge StartedPurge catalog-size=1000 in-use=0 pre-filtered=0(0%)Purge Treated: catalog emptyPurge deleted= n treated=n(d%) match=d%.Purge TreatedPurge Treated: no record found to delete</code><br/> </div> ****When there are transfers to delete:****<br/> <div > <code>Catalog: Loading...Catalog: Load DoneCatalog: Size=100, Used=8(8%)Purge Started.Purge catalog-size=100 in-use=8 pre-filtered=8(100)Purge deleted=1 treated=1(12) match=100Purge deleted=2 treated=2(25) match=100….Purge deleted=8 treated=8(100) match=100Purge Treated.</code><br/> </div> ****When {{< TransferCFT/axwayvariablesComponentShortName  >}} starts:****<br/> <div > If there are no transfers to delete:<br/> <div > <code>Catalog: Loading...Catalog: Load DoneCatalog: Size= &amp;00, Used=0(0%)</code><br/> </div> If there are transfers to delete:<br/> <div > <code>Catalog: Loading...Catalog: Load DoneCatalog: Size=100, Used=8(8%)</code><br/> </div> </div> ****If there is a problem with the catalog INIT:****<br/> <div > <code>Catalog: RecoveringCatalog: Recovery Done: n errorsCatalog Recovery: n transfers from C to D state</code><br/> </div>  |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC09I"></span>CFTC09I PART=&amp;part IDF=&amp;idf IDT=&amp;idt STATE=&amp;state DIRECT=&amp;direct : &amp;cmd not executed<br/> CFTC09I Command not executed &lt;IDTU=&amp;idtu PART=&amp;part IDF=&amp;idf IDT=&amp;idt STATE=&amp;state DIRECT=&amp;direct : Cmd=&amp;cmd |
| --- | --- |
| Explanation | The security system does not allow the user to execute this command on the catalog. |
| Consequence | The command is ignored.<br/> This message is followed by the CFTX02W and CFTX04W messages. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC10I"></span>CFTC10I PART=&amp;part IDF or IDM=&amp;idf STATE=&amp;state MODE=&amp;mode : &amp;cmd not executed<br/> CFTC10I PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] STATE=&amp;state MODE=&amp;mode : &amp;cmd not executed |
| --- | --- |
| Explanation | The security system does not allow this user to execute this command on the catalog. |
| Consequence | The command is ignored. One of the following messages displays after CFTC10I to provide additional information: CFTX01W , CFTX03W , or CFTX04W. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC11I"></span>CFTC11I PART=&amp;part IDM=&amp;idf IDT=&amp;idt : SEND REPLY not executed<br/> CFTC11I Command not executed &lt;IDTU=&amp;idtu PART=&amp;part [IDF=&amp;idf &#124; IDM=&amp;idm] IDT=&amp;idt : Cmd=&amp;cmd |
| --- | --- |
| Explanation | The security system does not allow the user to execute this command on the catalog. |
| Consequence | The command is ignored.<br/> <blockquote> **Note**<br/> This message is followed by the CFTX01W message.<br/> </blockquote>  |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC12I"></span>CFTC12I PART=&amp;part STATE=&amp;state DIRECT=&amp;direct TYPE=&amp;type SENTINEL_STATE=&amp;trkstate Deleted<br/> CFTC12I IDTU=&amp;idtu PART=&amp;part STATE=&amp;state PHASE=&amp;phase PHASESTEP=&amp;phasestep DIRECT=&amp;direct TYPE=&amp;type SENTINEL_STATE=&amp;trkstate Deleted |
| --- | --- |
| Explanation | This {{< TransferCFT/axwayvariablesComponentShortName  >}} message is displayed for each transfer that is deleted when the catalog is purged. Where:<br/> • &amp;state = transfer status (C/D/H/K/T/X)<br/> • &amp;direct = S (send) / R (recv)<br/> • &amp;type = F (file) / M (message)<br/> • &amp;trkstate = Sentinel state<br/> Possible values are:<br/> • TO_EXECUTE<br/> • SUSPENDED<br/> • RECEIVING<br/> • SENDING<br/> • CANCELED<br/> • RECEIVED<br/> • SENT<br/> • CREATED<br/> • INTERRUPTED<br/> • ACKED<br/> • NACKED |
| Consequence | The command is ignored.<br />  |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC13I"></span>CFTC13I Catalog resize (xxxx --&gt; yyyy) done<br/> CFTC13I Catalog resize (xxxx --&gt; yyyy) done |
| --- | --- |
| Explanation | A dynamic command to resize the catalog was executed. The first message displays the new size, and the second message indicates that the resizing (from the original size xxxx to the new size yyyy) is complete. |
| Consequence | The catalog automatically expands to the new size (the &lt;yyyy&gt; value) when possible, up to the maximum defined limit. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTC13E"></span>CFTC13E {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog resize (xxxx --&gt; yyyy) reached max before expansion<br/> CFTC13I Catalog resize (xxxx --&gt; yyyy) done |
| --- | --- |
| Explanation | A dynamic command to automatically expand the catalog failed, as the maximum number of records has already been reached. The catalog size remains unchanged (the &lt;xxxx&gt; value). |
| Consequence | Normal functioning with existing catalog size, and no catalog expansion occurs. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTC13E"></span><span id="CFTC15I"></span>CFTC15I Deprecated command not executed BLKNUM=&amp;blknum PART=&amp;part IDT=&amp;idt : Cmd=&amp;cmd&gt;<br/> CFTC15I Deprecated command not executed BLKNUM=&amp;blknum PART=&amp;part IDT=&amp;idt : Cmd=&amp;cmd |
| --- | --- |
| Explanation | Set the uconf parameter <code>cft.cftcat.enable_deprecated_blknum=Yes</code> to enable BLKNUM.<br/> <blockquote> **Note**<br/> Regardless of the cft.cftcat.enable_deprecated_blknum parameter setting, BLKNUM is disabled in a multi-node configuration (uconf:cft.multi_node.enable=Yes), and this message is displayed.<br/> </blockquote>  |
| Consequence | The command is ignored. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC29W"></span>CFTC29W Catalog Alert fill threshold reached: level=&amp;level , id=CAT0<br/> CFTC29W Catalog Alert fill threshold reached: level=&amp;level ID=&amp;id |
| --- | --- |
| Explanation | &amp;level of the catalog space has been used. &amp;level is the amount set by the CFTCAT TLVWARN parameter.<br/> When the critical fill threshold is reached, a message is recorded in the {{< TransferCFT/axwayvariablesComponentShortName  >}} log.<br/> A batch in response to the alert, the CFTCAT TLVWEXEC parameter, is submitted. |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTC30W"></span>CFTC30W Catalog Alert cleared: level=&amp;level, id=CAT0<br/> CFTC30W Catalog Alert cleared : level=&amp;level ID=&amp;id |
| --- | --- |
| Explanation | This alert stops when the fill level drops below the TLVCLEAR level.<br/> When the alert stops, the message is recorded in the Transfer CFT log, and a batch, the CFTCAT TLVCEXEC parameter, is submitted. |

