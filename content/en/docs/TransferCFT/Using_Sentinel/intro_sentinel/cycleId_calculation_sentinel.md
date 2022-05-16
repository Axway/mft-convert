---

    title: CycleId calculation
    linkTitle: CycleId calculation
    weight: 250

---
The internal CycleId is an XFBTransfer Tracked Object attribute. This field has the structure described in the following table.

<span class="autonumber"></span>Internal CycleID structure PeSIT

QQQ\_QQQ\_D


| Offset | Length | PI | Description |
| --- | --- | --- | --- |
| 1 | 4 | “SUIV” | Eye catcher |
| 5 | 24 | PI3 CONNECT | For transmission |
| 5  | 24  | PI4 CONNECT | For reception |
| 29 | 24 | PI4 CONNECT | For transmission |
| 29  | 24  | PI3 CONNECT | For reception |
| 53 | 5 | "0"<br/> "65535"<br/> "REPLY" | "0" |
| 58 | 76 | PI12 | Virtual filename |
| 134 | 8 | PI13 | Sequence number  |
| 142 | 12 | PI51 | Date YYMMDD padded to 12 with spaces on the right |
| 154 | 1 | E | For transmission |
| 154 | 1 | R | For reception |


 

<span class="autonumber"></span>Internal CycleID structure SFTP

QQQ\_QQQ\_D


| Offset | Length | Value  | Description |
| --- | --- | --- | --- |
| 1 | 4 | “SUIV”<br/> "TEMP" | Eye catcher |
| 5 | 24 | Sender identifier  | Sender account login name.<br/> • SEND: System login of the process that runs the SFTP client<br/> • RECV: SFTP login name sends by the client to connect to the SFTP server |
| 29 | 24 | Receiver identifier  | Receiver login name<br/> • SEND: SFTP login name sends by the client to connect to the SFTP server<br/> • RECV: System login of the process that runs the SFTP client |
| 53 | 5 | "0"  | For file transfer |
| 58 | 76 | Virtual Filename  | Logical file name, NIDF for Transfer CFT |
| 134 | 8 | Sequence number  | Unique number identifying the transfer sent, NIDT for Transfer CFT |
| 142 | 12 | Date YYMMDD padded to 12 with spaces on the right  | Only the date is used (YYMMDD), and the time is filled in 6 spaces |
| 154 | 1 | E  | For transmission |
| 154 | 1 | R  | For reception |
