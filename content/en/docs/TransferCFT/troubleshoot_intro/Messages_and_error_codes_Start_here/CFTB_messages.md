{
    "title": "Transfer CFT messages: CFTB",
    "linkTitle": "CFTAB messages",
    "weight": "270"
}This topic lists the CFTBxx messages and provides a type, a description, and when applicable a consequence and corrective action.

{{% TransferCFT/snippets/message_format%}}

### Auto documented messages

Certain auto-documented messages, for example CFTA01I, CFTA02W, CFTA03E, CFTA04F, are not fully detailed as they are considered self-explanatory. The CFTAnnx messages refer to the acceleration feature on all platforms that support acceleration.

- CFTA01, CFTLOG_SYS_INFOR "CFTA01I &str"
- CFTA02, CFTLOG_SYS_WARNI "CFTA02W &str"
- CFTA03, CFTLOG_SYS_ERROR "CFTA03E &str"
- CFTA04, CFTLOG_SYS_FATAL "CFTA04F &str"

 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB01E"></span>CFTB01E PART=&amp;part Context area allocation failure CS=&amp;scs<br/> CFTB01E PART=&amp;part Context area allocation failure CS=&amp;cs |
| --- | --- |
| Explanation | Cannot allocate the working area necessary for the transfer. |
| Consequence | In REQUESTER mode, the transfer is refused with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 122 diagnostic code and a MALLOC protocol diagnostic message.<br /> In SERVER mode, the incoming call is rejected.<br/> In this case, as the partner's name is not known, the value UNKNOWN is displayed. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB02E"></span>CFTB02E PART=&amp;part TFIL Exchange buffer allocation failure CS=&amp;scs<br/> CFTB02E PART=&amp;part TFIL Exchange buffer allocation failure CS=&amp;cs |
| --- | --- |
| Explanation | Cannot allocate the working area required to exchange information between the PROTOCOL task and the FILE task. |
| Consequence | In REQUESTER mode, the transfer is refused with a Transfer CFT 122 code and a MALLOC protocol diagnostic message.<br/> In SERVER mode, the incoming call is rejected. In this case, as the partner's name is not known, the value UNKNOWN is displayed. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB03E"></span>CFTB03E PART=&amp;part Error sending data on network NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB03E PART=&amp;part Error sending data on network NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Cannot send a message on the network. |
| Consequence | The transfer is interrupted with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 302 code and a protocol diagnostic message indicating the specific error code of the error occurring during the send request. This code is expressed in hexadecimal form. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB06E"></span>CFTB06E Flow control error NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB06E PART=&amp;part Flow control error NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Network flow control error. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB07E"></span>CFTB07E PART=&amp;part TFIL task Synchronization error CR=&amp;cr CS=&amp;cs<br/> CFTB07E PART=&amp;part TFIL task Synchronization error CR= &amp;cr CS=&amp;cs |
| --- | --- |
| Explanation | Problem encountered when sending an internal {{< TransferCFT/axwayvariablesComponentShortName  >}} message to the FILE task. |
| Consequence | The transfer is aborted by the protocol task (network disconnection). However, as the FILE task is not protected by a time-out, the request remains in the C status in the catalog |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB08E"></span>CFTB08E PART=&amp;part Network release resp err NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB08E PART=&amp;part Network release resp err NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Cannot respond to a network disconnection indication. |
| Consequence | This incident has no effect on the previous transfer (whether completed or interrupted). |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB09E"></span>CFTB09E PART=&amp;part Network connect req local err NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB09E PART=&amp;part Network connect req local err NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Cannot make an outgoing connection request on the network. |
| Consequence | For a general -6 code (maximum number of connections reached on the resource), the transfer is refused with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 416 diagnostic code and a MAXCNX protocol diagnostic message. The transfer will be retried (minimum time-out equal to the WSCAN parameter of the CFTCAT command), without incrementing the retry counter. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB10E"></span>CFTB10E PART=&amp;part RECOV=&amp;recov L=&amp;local R=&amp;reason D=&amp;diag NET=&amp;net<br/> CFTB10E PART=&amp;part RECOV=&amp;recov L=&amp;local R=&amp;reason ld D=&amp;diag ld NET=&amp;net |
| --- | --- |
| Explanation | Physical connection refused. |
| Consequence | Depending on the source of the refusal and the RECOV code, the transfer is set to the K status (diagnostic code 303) or remains in the D status (diagnostic code 302) and will be retried. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB18E"></span>CFTB18E Incoming call reject error NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB18E Incoming call reject error NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Cannot refuse an incoming call. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB19E"></span>CFTB19E Incoming call accept error NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTB19E Incoming call accept error NCR=&amp;ncr NCS=&amp;cs NET=&amp;net |
| --- | --- |
| Explanation | Cannot accept an incoming call. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB19E"></span>CFTB20E Invalid ETEBAC3 Card &amp;id NET=&amp;net<br/> CFTB20E Invalid ETEBAC3 Card &amp;id NET=&amp;net |
| --- | --- |
| Explanation |   |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTB21E"></span>CFTB21E PART=&amp;part MAIN task Synchronization error CR=&amp;cr CS=&amp;scs<br/> CFTB21E PART=&amp;part MAIN task Synchronization error CR= &amp;cr CS=&amp;cs |
| --- | --- |
| Explanation | Transfer CFT internal synchronization error. |

