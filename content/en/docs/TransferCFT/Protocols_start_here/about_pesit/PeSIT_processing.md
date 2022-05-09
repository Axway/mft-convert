{
    "title": "PeSIT partner interactions",
    "linkTitle": "PeSIT processing",
    "weight": "140"
}This topic describes the interactions and processes that occur between
partners when using the PeSIT protocol.

PeSIT provides a set of user services and a protocol
dialog used by two partners to exchange files and messages. A service
defines the interaction between two PeSIT end users.

Four types of service primitives implement this interaction:


| Primitive  | Notation  | Description  |
| --- | --- | --- |
| Request | REQ | Request a given service |
| Indication | IND | Notify the arrival of a service request |
| Response | RSP | Answer a request |
| Confirmation | CNF | Confirm the arrival of a response to a previous request |


At the very minimum, a service involves a request submitted by a user
and its corresponding indication to the partner. Some services also require
a response and a confirmation to be complete.

Send-file flows
---------------

The following table illustrates the communication flows
between Initiator and Responder during file transmission.

> **Note**
>
> Note: A file exchange involves more than just sending a file's data. Control information also has to be conveyed before, during and after the data transfer. This extra information is contained in protocol messages called FPDU (File Protocol Data Unit).


| SEND FILE<br /> PeSIT INITIATOR  | FPDU  |   | PeSIT<br /> RESPONDER  |
| --- | --- | --- | --- |
|   |   | idle |   |
| F.CONNECT, REQ ------&gt; |   |   | ------&gt;IND, F.CONNECT |
| F.CONNECT, CNF &lt;------ |   |   | &lt;------RSP, F.CONNECT |
|   |   | connected |   |
| F.CREATE, REQ ------&gt; |   |   | ------&gt;IND, F.CREATE |
| F.CREATE, CNF &lt;------ |   |   | &lt;------RSP, F.CREATE |
|   |   | file selected |   |
| F.OPEN, REQ ------&gt; |   |   | ------&gt;IND, F.OPEN |
| F.OPEN, CNF &lt;------ |   |   | &lt;------RSP, F.OPEN |
|   |   | file opened |   |
| F.WRITE, REQ ------&gt; |   |   | ------&gt;IND, F.WRITE |
| F.WRITE, CNF &lt;------ |   |   | &lt;------RSP, F.WRITE |
|   |   | transfer started |   |
| F.DATA, REQ -------&gt; |   | data transfer | ------&gt;IND, F.DATA |
| F.DATA, REQ -------&gt; |   |   | ------&gt;IND, F.DATA |
| F.CHECK, REQ -------&gt; |   | synchronization point | ------&gt;IND, F.CHECK |
| F.DATA, REQ -------&gt; |   |   | ------&gt;IND, F.DATA |
| F.DATA, REQ -------&gt; |   |   | ------&gt;IND, F.DATA |
| F.CHECK, REQ -------&gt; |   |   | ------&gt;IND, F.CHECK |
| F.DATA, REQ -------&gt; |   |   | ------&gt;IND, F.DATA |
| F.DATA-END, REQ -----&gt; |   |   | ------&gt;IND, F.DATA-END |
|   |   |   |   |
| F.TRANSFER-END, REQ -----&gt; |   |   | ------&gt;IND,F.TRANSFER-END |
| F.TRANSFER-END, CNF &lt;----- |   |   | &lt;------RSP,F.TRANSFER-END |
|   |   | transfer ended |   |
| F.CLOSE, REQ ------&gt; |   |   | ------&gt;IND, F.CLOSE |
| F.CLOSE, CNF &lt;------ |   |   | &lt;------RSP, F.CLOSE |
|   |   | file closed |   |
| F.DESELECT, REQ ------&gt; |   |   | ------&gt;IND, F.DESELECT |
| F.DESELECT, CNF &lt;------ |   |   | &lt;------RSP, F.DESELECT |
|   |   | file deselected |   |
| F.RELEASED, REQ ------&gt; |   |   | ------&gt;IND, F.RELEASED |
| F.RELEASED, CNF &lt;------ |   |   | &lt;------RSP, F.RELEASED |
|   |   | disconnected |   |


Receive-file flows
------------------

The following diagram illustrates the communication flows
between Initiator and Responder during file reception.


| RECEIVE FILE<br /> PeSIT INITIATOR |   | PeSIT<br /> RESPONDER |
| --- | --- | --- |
|   | connected |   |
| F.SELECT, REQ ------&gt; |   | ------&gt;IND, F.SELECT |
| F.SELECT, CNF &lt;------ |   | &lt;------RSP, F.SELECT |
|   | file selected |   |
| F.OPEN, REQ ------&gt; |   | ------&gt;IND, F.OPEN |
| F.OPEN, CNF &lt;------ |   | &lt;------RSP, F.OPEN |
|   | file opened |   |
| F.READ, REQ ------&gt; |   | ------&gt;IND, F.READ |
| F.READ, CNF &lt;------ |   | &lt;------RSP, F.READ |
|   | transfer started |   |
| F.DATA, IND &lt;------ |   | &lt;------REQ, F.DATA |
| F.DATA, IND &lt;------ |   | &lt;------REQ, F.DATA |
| F.CHECK, IND &lt;------ |   | &lt;------REQ, F.CHECK |
| F.DATA, IND &lt;------ |   | &lt;------REQ, F.DATA |
| F.DATA, IND &lt;------ |   | &lt;------REQ, F.DATA |
| F.CHECK, IND &lt;------ |   | &lt;------REQ, F.CHECK |
| F.DATA, IND &lt;------ |   | &lt;------REQ, F.DATA |
| F.DATA-END, IND |   | &lt;------REQ, F.DATA-END |
|   |   |   |
| F.TRANSFER-END, REQ -----&gt; |   | ------&gt;IND,F.TRANSFER-END |
| F.TRANSFER-END, CNF &lt;----- |   | &lt;------RSP,F.TRANSFER-END |
|   | transfer ended |   |
| F.CLOSE, REQ ------&gt; |   | ------&gt;IND, F.CLOSE |
| F.CLOSE, CNF &lt;------ |   | &lt;------RSP, F.CLOSE |
|   | file closed  |   |
| F.DESELECT, REQ ------&gt; |   | ------&gt;IND, F.DESELECT |
| F.DESELECT, CNF &lt;------ |   | &lt;------RSP, F.DESELECT |
|   | file deselected |   |


Send-message flows
------------------

The following interaction illustrates the communication flows
between Initiator and Responder during message transmission.


| SEND MESSAGE<br /> PeSIT INITIATOR<br />  |   | PeSIT<br /> RESPONDER<br />  |
| --- | --- | --- |
|   | idle |   |
| F.CONNECT, REQ ------&gt; |   | ------&gt;IND, F.CONNECT |
| F.CONNECT, CNF &lt;------ |   | &lt;------RSP, F.CONNECT |
|   | connected  |   |
| F.MESSAGE, REQ ------&gt; |   | ------&gt;IND, F.MESSAGE |
| F.MESSAGE, CNF &lt;------ |   | &lt;------RSP, F.MESSAGE |
|   |   |   |
| F.RELEASED, REQ ------&gt; |   | ------&gt;IND, F.RELEASED |
| F.RELEASED, CNF &lt;------ |   | &lt;------RSP, F.RELEASED |
|   | disconnected |   |

