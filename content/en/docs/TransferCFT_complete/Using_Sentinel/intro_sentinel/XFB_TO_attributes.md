{
    "title": "XFBTransfer Tracked Object attributes",
    "linkTitle": "XFBTransfer Tracked Object attribute",
    "weight": "230"
}Roles
-----


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| Direction | Integer | - |  • S: The file is sent (Sender).<br/> • R: The file is received (Receiver). | DIRECT |
| IsServer | Integer | - |  • 1: The Sender or the Receiver is a Server.<br/> • 0: The Sender the Receiver is a Requester. | FLAG |


Senders and receivers
---------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| Site | String | 25 | Partner alias of the remote partner. | PART |
| ReceiverId | String | 80 | Name of the Receiver. | NRPART |
| SenderId | String | 80 | Name of the Sender. | NSPART |
| RAppl | String | 80 | Optional identifier of the Receiving application. | RAPPL |
| SAppl | String | 80 | Optional identifier of the Sending application. | SAPPL |
| SourceApplication | String | 100 | The source application for a Central Governance flow, for monitoring purposes. The source system is the system that initiates a flow. | SOURCEAPPL |
| TargetApplication | String | 100 | The target application in a Central Governance flow, for monitoring purposes. The target system is the system making a request to initiate the flow. | TARGETAPPL |
| FinalReceiverId | String | 80 | Name of the final Receiver when using Store and Forward, otherwise the same as the ReceiverId. | NDEST |
| OriginalSenderId | String | 80 | Name of the original Sender when using Store and Forward, otherwise the same as the SenderId. | NORIG |


Product identification
----------------------

The product that sends the events is identified with the following:


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| Location | String | 31 | Unique and logical identifier for the product. | PART parameter of the CFTPARM object. |
| Monitor | String | 4 | Product name. | “CFT” |
| MonitorVersion | String | 25 | Product version. | Product version (see CFTUTIL about). |


Transfer users
--------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| UserId | String | 25 | Local identifier of the user who owns the transferred file. | USERID |
| GroupId | String | 25 | Local identifier of the group to which the transfer owner belongs. | GROUPID |
| RequestUserId | String | 25 | Local identifier of the user who requested the transfer. | REQUSER |
| RequestGroupId | String | 25 | Local identifier of the group to which the requesting user belongs. | REQGROUP |
| Trustee | String | 25 | If User Access Control is:<br/> • Activated for the transfer, the value of this attribute is the local identifier of the transfer owner (UserId).<br/> • Not activated for the transfer, the value of this attribute is the network identifier of the user who requested the transfer (RequestUserId). | IDAPPL |
| RUser | String | 30 | Optional local identifier of the user who received the transfer. | RUSER |
| SUser | String | 30 | Optional local identifier of the user who sent the transfer. | SUSER |


Transfer identification
-----------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| IdAppl | String | 25 | If the value of the IsServer attribute is:<br/> • 0: The value of this attribute is the local identifier of the Requester.<br/> • 1: The value of this attribute is empty. | ****IDA**** |
| Application | String | 80 | Local application/Transfer profile (ST). | IDF |
| FileName | String | 512 | If the value of the CommandType attribute is:<br/> • File and the value of the Direction attribute is E (Sender): This attribute identifies the file from which the Sender retrieved the transfer data (full path).<br/> • File and the value of the Direction attribute is R (Receiver): This attribute identifies the file in which the Receiver recorded the transfer data (full path). | FNAME |
| LocalId | String | 25 | If the value of the Direction attribute is:<br/> • E (Sender): The value of this attribute is the local transfer identifier for the Sender.<br/> • R (Receiver): The value of this attribute is the local transfer identifier for the Receiver. | IDTU |
| ProtocolFileName | String | 512 | Network transfer identifier. The Sender sent this identifier to the Receiver. | NIDF |
| ProtocolFileLabel | String | 80 | The Sender sent this name to the Receiver. The Receiver can use this name to locally name the transfer. | NFNAME |
| ProtocolId | String | 80 | Network transfer identifier. The Sender generated this identifier. The Receiver acknowledged this identifier. | NIDT |
| ProtocolMessage | String | 4000 | If the value of the CommandType attribute is:<br/> • Message: the value of this attribute is the content of the transferred message.<br/> • File: this attribute is empty. | SMSG |
| PositionNumber | String | 80 | Local transfer identifier. If the value of the Direction attribute is:<br/> • E (Sender): The value of this attribute identifies the transfer in the Sender's transfer catalog or DB<br/> • R (Receiver): The value of this attribute identifies the transfer in the Receiver's transfer catalog or DB | IDT |
| ProtocolParameter | String | 512 | Network transfer description. The Sender sent this description to the Receiver. | PARM |
| UserParameter1 | String | 255 | Local transfer description. This description is recorded at the transfer request time in the local DB and remains local. | COMMENT |
| Flowname | String | 100 | Parameter to define a flow name in Central Governance for monitoring purposes. | Flowname (v3.1.x) |


Transfer validity periods
-------------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> product |
| --- | --- | --- | --- | --- |
| EarliestDate | Date | - | Date on which the validity period begins. | ****DATEM**** |
| EarliestTime | Time | - | Time at which the validity period begins. | TIMEM |
| LatestDate | Date | - | Date on which the validity period ends. | DATEMAX |
| LatestTime | Time | - | Time at which the validity period ends. | TIMEMAX |


Transfer dates and times
------------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| CreationDate | Date | - | By default, the system date on which the Sender sent the transfer. The Sender can set this date. The Receiver can filter transfers based on this date. | ****FDATE**** |
| CreationTime | Time | - | By default, the system time at which the Sender sent the transfer. The Sender can set this time. The Receiver can filter transfers based on this time. | FTIME |
| SendDate | Date | - | If the value of the Direction attribute is:<br/> • E (Sender): The value of this attribute is the date on which the Sender recorded the transfer in the transfer catalog.<br/> • R (Receiver): The value of this attribute is the date on which the Receiver recorded the transfer in the transfer catalog.<br/> The Sender and the Receiver record each transfer only once. | DATEK |
| SendTime | Time | - | If the value of the Direction attribute is:<br/> • E (Sender): The value of this attribute is the local time at which the Sender recorded the transfer in the transfer catalog.<br/> • R (Receiver): The value of this attribute is the local time at which the Receiver recorded the transfer in the transfer catalog.<br/> The Sender and the Receiver record each transfer only once. | TIMEK |
| AckDate | Date | - | Date on which the Receiver acknowledged the transfer (if any). |   |
| AckTime | Time | - | Time at which the Receiver acknowledged the transfer (if any). |   |
| StartDate | Date | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is the date on which the Sender began sending the transfer.<br/> • RECEIVED: The value of this attribute is the date on which the Receiver began receiving the transfer.<br/> These dates are expressed in dd.mm.yyyy format. | DATEB |
| StartTime | Time | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is the local time at which the Sender began sending the transfer.<br/> • RECEIVED: The value of this attribute is the local time at which the Receiver began receiving the transfer.<br/> These times are expressed in hh:mn:ss format. | TIMEB |
| EndDate | Date | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is the date on which the Sender stopped sending the transfer.<br/> • RECEIVED: The value of this attribute is the date on which the Receiver stopped receiving the transfer.<br/> These dates are expressed in dd.mm.yyyy format. | DATEE |
| EndTime | Time | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is the local time at which the Sender stopped sending the transfer.<br/> • RECEIVED: The value of this attribute is the local time at which the Receiver stopped receiving the transfer.<br/> These times are expressed in hh:mn:ss format. | TIMEE |
| RequestCreationDate | Date | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is local date of the creation of the file on the Sender side.<br/> • RECEIVED: The value of this attribute is the date of the creation of the file on the Sender side. | DATEK |
| RequestCreationTime | Time | - | If the value of the State attribute is:<br/> • SENT: The value of this attribute is local time of the creation of the file on the Sender side.<br/> • RECEIVED: The value of this attribute is the time of the creation of the file on the Sender side. | TIMEK |
| TransmissionDuration | Integer | - | Transfer duration, expressed in seconds. | TIMES |
| EventTimestamp  | String  | 100  | The event time, in milliseconds, indicating the exact time the event occurred on Transfer CFT, rather than the time the event reaches Sentinel.<br/> You can use the a tool, such as [EpochConverter](https://www.epochconverter.com/), to convert the timestamp into a human-readable date.<br/> **Example**<br/> If the EventTimeStamp is 1631110828, the equivalent in GMT is Wednesday 8 September 2021 14:20:28.<br/> <blockquote> **Note**<br/> Note: Introduced in the XFBTransfer TO as of the version 5.5.<br/> </blockquote>  | N/A  |
| EventDateTime  | String  | 50  | The date and time event indicating the exact time the event occurred on Transfer CFT, rather than the date and time the event reaches Sentinel. This is a UTC standard value and follows the ISO 8601 format.<br/> **Example**<br/> 2021-07-28T08:32:30.049708Z<br/> <blockquote> **Note**<br/> Note: Introduced in the XFBTransfer TO as of the version 5.5.<br/> </blockquote>  | N/A  |


Transfer protocols
------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| Protocol | String | 25 | Name of the protocol that operates at the Protocol Layer of the transfer. Possible values:<br/> • CFT (PeSIT, version CFT)<br/> • PSIT_HS_E (PeSIT, version E)<br/> • PSIT_HS_D (PeSIT, version D)<br/> • ODT (ODETTE File Transfer Protocol)<br/> • SFTP | Protocol |
| IsSSL | String | 1 |  • 1: SSL/TLS used for the transfer.<br/> • 0: SSL/TLS not used for the transfer. | SSLMODE |
| SSLAuth | String | 1 |  • S: The Server sent X.509 certificates to the Requester.<br/> • B: Both the Server and the Requester sent X.509 certificates to each other.<br/> • N: Neither the Server nor the Requester sent X.509 certificates. | SSLAUTH |
| SSLCypher | String | 2 | The cipher suite that the Server and the Requester used during the SSL/TLS session. | SSLCIPH |


Transfer options
----------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| Compression | String | 1 | One of the following:<br/> • 0: Undefined<br/> • 1: Horizontal<br/> • 2: Vertical<br/> • 3: Both horizontal and vertical<br/> • 4: Not compressed | NCOMP |
| EOTProcedure | String | 255 | Name of the end-of-transfer procedure executed upon the completion of the transfer. | EXEC |
| Priority | Integer | - | Transfer priority. Receivers process transfers in the order of their priority. The range of possible values for this attribute is 0 to 255. The lowest priority is zero. The highest priority is 255.  | PRI |
| RetryMaxNumber | Integer | - | Maximum number of times that the Sender can attempt to send transfers. | RETRYM |
| RetryNumber | Integer | - | Number of times that the Sender attempted to send the transfer. Each time the Sender established a connection with the Receiver, the Sender counted one attempt. | RETRY |
| RequestType | String | 1 | One of the following:<br/> • S: The Sender sent a single transfer to a single Receiver.<br/> • F: The Sender sent a group of transfers to a single Receiver. For each transfer in the group, the product generated one Processing Cycle.<br/> • D: The Sender sent a single transfer to a group of Receivers (diffusion). For each Receiver in the group, the product generated one Processing Cycle.<br/> • P: Cyclic transfer. | DIFTYP |
| TransferType | String | 1 | One of the following:<br/> • S: The Sender sent a single transfer to a single Receiver.<br/> • F: The transfer belongs to a group of transfers that the Sender sent to a single Receiver. For each transfer in the group, the product generated one Processing Cycle.<br/> • D: The Receiver belongs to a group of Receivers to whom the Sender sent the transfer (diffusion). For each Receiver in the group, the product generated one Processing Cycle. | FILTYP |


Transfer size
-------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| FileSize | Integer | - | Size of the transferred file. This size is expressed in bytes.<br/> <blockquote> **Note**<br/> Note: For PeSIT, an estimation of size is given at the beginning of the transfer. This value is updated upon completion of the transfer with the real value.<br/> </blockquote>  | FSPACE |
| TransmittedBytes | Integer | - | Number of bytes transferred, after decompression, to transfer the file. This size is expressed in bytes.<br/> <blockquote> **Note**<br/> Note: For PeSIT, this value sent is crosschecked by both the sender and receiver.<br/> </blockquote>  | NCAR |


The structure and content of transfers
--------------------------------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| CommandType | String | 1 |  • F: File transfer<br/> • M: Message transfer<br/> • A: Message reply<br/> • N: Message nack | TYPE |
| FileOrganization | String | 25 |  • org_sequential: The transferred data is not indexed.<br/> • indexed: The transferred data is indexed.<br/> • direct: The transferred data is assigned relative access. | FORG |
| FileType | String | 60 |  • B: The transferred file is a binary file.<br/> • J, T, O, X: The transferred file is a text file. | FTYPE |
| RecordNumber | Integer |   | Number of record in the file. This size is expressed in bytes.<br/> <blockquote> **Note**<br/> Note: For PeSIT, this value sent is crosschecked by both the sender and receiver.<br/> </blockquote>  | FREC |
| RecordFormat | String | 64 |  • F: fixed - The transferred data contains fixed-length records.<br/> • V: variable - The transferred data contains variable-length records.<br/> • U: undefined - The structure of the transferred data is unknown. | FRECFM |
| RecordSize | Integer |   |  • If the value of RecordFormat attribute is fixed, the value of this attribute is the size of all records in the transferred file, expressed in bytes.<br/> • If the value of RecordFormat is variable or undefined, the value of this attribute is the size of the largest record in the transferred file, expressed in bytes. | FLRECL |
| Transcoding | Integer |   | Character code of the transferred data:<br/> • A: ASCII<br/> • B: Binary<br/> • E: EBCDIC | FCODE |
| TranslationTableId | String | 25 | Name of the local translation table use during the transfer (if any). | XLATE |


Other attribute
---------------


| Sentinel<br/> attribute | Data type | Length | Description | Name in<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- | --- | --- | --- |
| IsRelay | String | 1 | If transfer event comes from a relay site:<br/> • 1: Yes<br/> • 0: No | IsRelay |
| NodeId  | Integer  |   | Identifier of the node that executes the transfer  | NodeID  |
| ParentCycleid  | String  | 250  | The identifier of the event which is the parent of the current event  | ParentCycleid  |

