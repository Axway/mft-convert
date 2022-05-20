---
title: "Transfer CFT messages: CFTN"
linkTitle: "CFTN messages"
weight: 330
---This topic lists the CFTNxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`


| V23 format<br/> V24 format<br/> Information | <span id="CFTN01I"></span>CFTN01I NET=&amp;net started<br/> CFTN01I NET=&amp;net started |
| --- | --- |
| Explanation | Start of the network resource &amp;net. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTN02I"></span>CFTN02I NET=&amp;net PROTOCOL=&amp;prot started<br/> CFTN02I NET=&amp;net PROTOCOL=&amp;prot started |
| --- | --- |
| Explanation | Start of the protocol &amp;prot associated with the &amp;net network. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTN03E"></span>CFTN03E Error creating SSL task &amp;str<br/> CFTN03E Error creating SSL task &amp;str |
| --- | --- |
| Explanation | Problem with activating the CFTTSSL task. The error is specified by &amp;str. |
| Consequence | The transfer is aborted. |
| Action | Contact the support team if necessary. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTN04E"></span>CFTN04E Synchronization error (&amp;str) SSLTID=&amp;pid CR=&amp;cr CS=&amp;scs<br/> CFTN04E Synchronization error (&amp;str) SSLTID=&amp;pid _ CR= &amp;cr CS=&amp;cs |
| --- | --- |
| Explanation | Problem with sending an internal {{< TransferCFT/axwayvariablesComponentShortName  >}} message to a CFTTSSL task. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTN05I"></span>CFTN05I &amp;message<br/> CFTN05I Network resource depletion prevention enabled for class &amp;n |
| --- | --- |
| Explanation | TCP/IP and connection dispatcher related information. The message contains the explanation of the information.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTN05E"></span>CFTN05E &amp;message<br/> CFTN05E &amp;message |
| --- | --- |
| Explanation | A TCP/IP error related to file transfer operations or resource initialization was detected. The message contains the explanation of the error in plain text. |
| Consequence  | If the error occurs during the {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization phase, this phase is stopped. Otherwise, if the error is related to a file transfer, this transfer will not proceed.  |
| Action  | For an error occurring during the initialization phase, check the CFTNET definitions. For an error involving a file transfer, check the CFTPART definitions.  |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTN05W"></span>CFTN05W &amp;message<br/> CFTN05W &amp;message |
| --- | --- |
| Explanation | The same as CFTN08E, a TCP/IP related error, but the condition is considered less severe. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTN05W"></span><span id="CFTN06I"></span>CFTN06I No network class suitable for resource depletion prevention activation<br/> CFTN06I No network class suitable for resource depletion prevention activation |
| --- | --- |
| Explanation | Network resource depletion prevention (NRDP) is enabled but not activated. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTN08E"></span>CFTN08E SFTP bind() failed on address &amp;address and port &amp;port: Only one usage of each socket address (protocol/network address/port) is normally permitted<br/> CFTN08E SFTP bind() failed on address &amp;address and port &amp;port: Only one usage of each socket address (protocol/network address/port) is normally permitted |
| --- | --- |
| Explanation | There is another server connection on that port.  |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTN09E"></span>CFTN09E SFTP bind() failed on address &amp;address and port &amp;port: &amp;reason<br/> CFTN09E SFTP bind() failed on address &amp;address and port &amp;port: &amp;reason |
| --- | --- |
| Explanation | Failed to establish the server connection for a reason other than CFTN08E.  |


 


| V23 format<br/> V24 format<br/> Error  | <span id="CFTN10E"></span>CFTN10E Server connection refused, we have reached the connection limit, MAXCNX=&amp;maxcnx<br/> CFTN10E Server connection refused, we have reached the connection limit, MAXCNX=&amp;n |
| --- | --- |
| Explanation  | Reached the maximum number of supported connections. The incoming connection is rejected.  |


 


| V23 format<br/> V24 format<br/> Error  | CFTN11E Server connection refused, we have reached the limit of file descriptors in CFTSFTP process<br/> CFTN11E Server connection refused, we have reached the limit of file descriptors in CFTSFTP process |
| --- | --- |
| Explanation  | The server connection was refused because you reached the file descriptors limit in the CFTSFTP process.  |


 


| V23 format<br/> V24 format<br/> Error  | CFTN12E Connection failed, we have reached the limit of file descriptors in CFTSFTP process<br/> CFTN12E Connection failed, we have reached the limit of file descriptors in CFTSFTP process |
| --- | --- |
| Explanation  | The client connection failed because you reached the file descriptors limit in the CFTSFTP process.  |


 


| V23 format<br/> V24 format<br/> Warning | <span id="CFTN36W"></span>CFTN36W TCPMAXUSER=&amp;maxcnx reached. Network connect reject host=&amp;host port=&amp;port &amp;str<br/> CFTN36W TCPMAXUSER=&amp;maxcnx reached. Network connect reject host=&amp;host port=&amp;port &amp;str |
| --- | --- |
| Explanation | The maximum number of connections supported has been reached. An incoming connection from host &amp;host port &amp;port is rejected. |

