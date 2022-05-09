{
    "title": "CycleId calculation",
    "linkTitle": "CycleId calculation",
    "weight": "240"
}The internal CycleId is an XFBTransfer Tracked Object attribute. This field has the structure described in the following table.

Internal CycleID structure PeSIT


| Offset | Length | PI | Description |
| --- | --- | --- | --- |
| 1 | 4 | “SUIV” | Eye catcher |
| 5 | 24 | PI3 CONNECT | For transmission |
| 5  | 24  | PI4 CONNECT | For reception |
| 29 | 24 | PI4 CONNECT | For transmission |
| 29  | 24  | PI3 CONNECT | For reception |
| 53 | 5 | &quot;0&quot; | &quot;0&quot; |
| 53  | 5  | &quot;65535&quot; | &quot;0&quot;  |
| 53  | 5  | &quot;REPLY&quot; | &quot;0&quot;  |
| 58 | 76 | PI12 | Virtual filename |
| 134 | 8 | PI13 | Sequence number  |
| 142 | 12 | PI51 | Date YYMMDD padded to 12 with spaces on the right |
| 154 | 1 | E | For transmission |
| 154  | 1  | R | For reception |


 

Internal CycleID structure SFTP


| Offset | Length | Value  | Description |
| --- | --- | --- | --- |
| 1 | 4 | “SUIV”<br/> &quot;TEMP&quot; | Eye catcher |
| 5 | 24 | Sender identifier  | Sender account login name.<br/> • SEND: System login of the process that runs the SFTP client<br/> • RECV: SFTP login name sends by the client to connect to the SFTP server |
| 29 | 24 | Receiver identifier  | Receiver login name<br/> • SEND: SFTP login name sends by the client to connect to the SFTP server<br/> • RECV: System login of the process that runs the SFTP client |
| 53 | 5 | &quot;0&quot;  | For file transfer |
| 58 | 76 | Virtual Filename  | Logical file name, NIDF for Transfer CFT |
| 134 | 8 | Sequence number  | Unique number identifying the transfer sent, NIDT for Transfer CFT |
| 142 | 12 | Date YYMMDD padded to 12 with spaces on the right  | Only the date is used (YYMMDD), and the time is filled in 6 spaces |
| 154 | 1 | E  | For transmission |
| 154  | 1  | R  | For reception |

