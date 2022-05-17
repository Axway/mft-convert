---
    title: "Transfer CFT messages: CFTH"
    linkTitle: "CFTH messages"
    weight: 310
---This topic lists the CFTHxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`


| V23 format<br/> V24 format<br/> Error | <span id="CFTH01E"></span>CFTH01E PART=&amp;part Context area allocation failure CS=&amp;scs<br/> CFTH01E Context area allocation failure &lt;PART=&amp;part CS=&amp;cs&gt; |
| --- | --- |
| Explanation | Cannot allocate the working area necessary for the transfer. |
| Consequence | In REQUESTER mode, the transfer is refused with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 122 diagnostic code and a MALLOC protocol diagnostic message.<br/> In SERVER mode, the incoming call is rejected. In this case, as the partner's name is not known, the value UNKNOWN is displayed. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH02E"></span>CFTH02E PART=&amp;part TFIL Exchange buffer allocation failure CS=&amp;scs<br/> CFTH02E TFIL Exchange buffer allocation failure &lt;PART=&amp;part CS=&amp;cs&gt; |
| --- | --- |
| Explanation | Cannot allocate the working area required to exchange information between the PROTOCOL task and the FILE task. |
| Consequence | In REQUESTER mode, the transfer is refused with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 122 code and a MALLOC protocol diagnostic message.<br/> In SERVER mode, the incoming call is rejected. In this case, as the partner's name is not known, the value UNKNOWN is displayed. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH03E"></span>CFTH03E PART=&amp;part Error sending data on network NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTH03E Error sending data on network &lt;PART=&amp;part NCR=&amp;ncr NCS=&amp;cs NET=&amp;net&gt; |
| --- | --- |
| Explanation | Cannot send a message on the network. |
| Consequence | The transfer is interrupted with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 302 code and a protocol diagnostic message indicating the specific error code of the error that occurred during the send request. This code is expressed in hexadecimal. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH04E"></span>CFTH04E PART=&amp;part Mismatch between header and FPDU size<br/> CFTH04E Mismatch between header and FPDU size &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | The transfer is aborted. A 318 protocol code, transported by an ABORT FPDU, is reported to the remote partner. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH05E"></span>CFTH05E: PART=&amp;part ,&amp;message<br/> CFTH05E &lt;PART=&amp;part &amp;message&gt; |
| --- | --- |
| Explanation | Inconsistent FPDU received.<br/> The end of the message is then set to one of the following values:<br/> • Unknown FPDU type=n:<br/> Reception of an FPDU, for which the type byte in the header is unknown<br/> • Missing PI number n into FPDU fpdu:<br/> Reception of an FPDU with missing mandatory PI<br/> • PGI n in PGI into FPDU fpdu:<br/> Reception of an FPDU embedding a PGI in a PGI<br/> • Invalid length n for PI n into FPDU fpdu:<br/> Reception of an FPDU with a PI of invalid length<br/> • Unknown PI number n into FPDU fpdu:<br/> Reception of an FDPU with an unwanted PI |
| Consequence | The transfer is aborted. A 318 protocol code, transported by an ABORT FPDU, is reported to the remote partner. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH06E"></span>CFTH06E PART=&amp;part Error &amp;cr while formatting FPDU<br/> CFTH06E Error &amp;cr while formatting FPDU &lt;PART=&amp;part &gt; |
| --- | --- |
| Explanation | Problem encountered while formatting an FPDU.<br/> • -1 PGI embedded in a PGI<br/> • -2 Output buffer overflow<br/> • -3 End of PGI without PGI<br/> • -4 External/internal type error<br/> • -5 End of FPDU with PGI not closed<br/> • -8 Invalid PI length<br/> • -11 FPDU description pointer null |
| Consequence | The transfer is aborted. A 220 protocol code, transported by an ABORT FPDU, is reported to the remote partner. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH07E"></span>CFTH07E PART=&amp;part TFIL task Synchronization error CR=&amp;cr CS=&amp;scs<br/> CFTH07E TFIL task Synchronization error &lt;PART=&amp;part CR= &amp;cr CS=&amp;cs&gt; |
| --- | --- |
| Explanation | Problem encountered when sending a {{< TransferCFT/axwayvariablesComponentShortName  >}} internal message to the FILE task. |
| Consequence | The transfer is aborted by the protocol task (network disconnection). However, as the FILE task is not protected by a time-out, the request remains in the C status in the catalog. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH08E"></span>CFTH08E PART=&amp;part Network release response error NCR=&amp;ncr NCS=&amp;ncs<br/> CFTH08E Network release response error &lt;PART=&amp;part NCR=&amp;ncr NCS=&amp;cs&gt; |
| --- | --- |
| Explanation | Cannot respond to a network connection failure indication. |
| Consequence | This incident has no impact on the previous transfer (whether terminated or interrupted). |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH09E"></span>CFTH09E PART=&amp;part Network connect request local error NCR=&amp;ncr NCS=&amp;ncs<br/> CFTH09E Network connect request local error &lt;PART=&amp;part NCR=&amp;ncr NCS=&amp;cs NET=&amp;net&gt; |
| --- | --- |
| Explanation | Cannot make an outgoing connection request on the network. For a general -6 code (maximum number of connections reached on the resource), the transfer is refused with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 416 diagnostic code and a MAXCNX protocol diagnostic message.<br/> The transfer will be retried (minimum time-out equal to the WSCAN parameter of the CFTCAT command), without incrementing the retry counter. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH10E"></span>CFTH10E PART=&amp;part Network connect reject RECOV=&amp;recov L= &amp;local R=&amp;reason D=&amp;ncs<br/> CFTH10E Network connect reject &lt;PART=&amp;part RECOV=&amp;recov L=&amp;local R=&amp;reason D=&amp;ncs NET=&amp;net&gt; |
| --- | --- |
| Explanation | The physical connection has been refused. |
| Consequence | Depending on the origin of the refusal and the RECOV code, the transfer is set to the K state (diagnostics code 303) or remains set to the D state (diagnostics code 302) and will be retried. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH11E"></span>CFTH11E PART=&amp;part Error Opening session EV=&amp;pevent ST=&amp;pstate<br/> CFTH11E Error Opening session &lt;PART=&amp;part EV=&amp;pevent ST=&amp;pstate&gt; |
| --- | --- |
| Explanation | Problem opening a PeSIT session with a remote partner after establishing the network session. |
| Consequence | The transfer is aborted with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 451 diagnostic code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH12E"></span>CFTH12E PART=&amp;part Logon reject logon<br/> CFTH12E Logon reject &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The PESIT pre-connection phase, logon, is refused by the SERVER partner (the correct response is ACK0 in EBCDIC). The string received is included in the message. |
| Consequence | The transfer is aborted with one of the following possible diagnostic codes:<br/> • 903: invalid password<br/> • 970: password expired, or<br/> • 963: unknown acknowledgment of a pre-connection request |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH13E"></span>CFTH13E PART=&amp;part FPDU Remote reject DIAGI=&amp;diagi DIAGP=&amp;diagp<br/> CFTH13E FPDU Remote reject &lt;PART=&amp;part DIAGI=&amp;diagi DIAGP=&amp;diagp&gt; |
| --- | --- |
| Explanation | FPDU including a diagnostic code has been received. The DIAGP field is of the XXX NNN type. |
| Consequence | The transfer is aborted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH14E"></span>CFTH14E PART=&amp;part Invalid AckCONNECT FPDU &amp;str<br/> CFTH14E Invalid AckCONNECT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckCONNECT FPDU sent by the SERVER partner is invalid.<br/> The field "&amp;str" is an explicit character string:<br/> • Version = n: The protocol version negotiated by the SERVER partner is invalid Window without Pacing: A synchronization window is specified even though the interval is null,<br/> • Window = n too large: The synchronization window negotiated by the SERVER partner is too large The SIT profile does not allow a value greater than 16<br/> • Pacing = n not authorized: The synchronization interval negotiated by the SERVER partner does not comply with the specifications of the SIT profile The values 1, 2 and 3 are not allowed<br/> • Pacing = n greater than initial value = n: The synchronization interval negotiated by the SERVER partner is larger than that proposed<br/> • Window = n greater than initial value = n: The synchronization window negotiated by the SERVER partner is larger than that proposed<br/> • Resynchronization = n: The value of the resynchronization option negotiated by the SERVER partner does not comply with the specifications of the protocol<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | The transfer is aborted with DIAGI=220, DIAGP=ACO + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH15E"></span>CFTH15E PART=&amp;part Invalid AckCREATE FPDU &amp;str<br/> CFTH15E Invalid AckCREATE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckCREATE FPDU sent by the SERVER partner does not conform.<br/> The "&amp;str" string is an explicit character string:<br/> • NSDU size = n greater than initial value = n: The maximum NSDU size negotiated by the partner is greater than that proposed<br/> • NSDU size = n too lower for LRECL = n: In the SIT profile, as segmentation is not authorized, a record must fit in an FPDU. The negotiated NSDU size (RUSIZE) must therefore be greater than the record length of the file (plus 6 for the FPDU header)<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ACR + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH16E"></span>CFTH16E PART=&amp;part Invalid AckWRITE FPDU &amp;str<br/> CFTH16E Invalid AckWRITE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckWRITE FPDU sent by the SERVER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Restart point without restart option: The remote partner proposes a restart point for the transfer, even though it is a new transfer<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=AWR + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH17E"></span>CFTH17E PART=&amp;part Invalid Check Point acknowledge &amp;n<br/> CFTH17E Invalid Check Point acknowledge &lt;PART=&amp;part &amp;n&gt; |
| --- | --- |
| Explanation | The synchronization check point number is not correct. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH18E"></span>CFTH18E Incoming call reject error NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTH18E Incoming call reject error &lt;NCR=&amp;ncr NCS=&amp;cs NET=&amp;net&gt; |
| --- | --- |
| Explanation | Cannot refuse an incoming call. |
| Consequence | The transfer is aborted by the protocol task (it is not registered in the catalog) |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH19E"></span>CFTH19E Incoming call accept error NCR=&amp;ncr NCS=&amp;ncs NET=&amp;net<br/> CFTH19E Incoming call accept error &lt;NCR=&amp;ncr NCS=&amp;cs NET=&amp;net&gt; |
| --- | --- |
| Explanation | Cannot accept an incoming call. |
| Consequence | The transfer is aborted by the protocol task (it is not registered in the catalog). |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH20E"></span>CFTH20E Invalid CONNECT FPDU &amp;str<br/> CFTH20E Invalid CONNECT FPDU &lt;&amp;str&gt; |
| --- | --- |
| Explanation | The CONNECT FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • CRC option = n: The remote partner proposes a value for the CRC option which does not comply with protocol specifications.<br/> • This value is displayed: Only the values 0 (no CRC) and 1 (application of a CRC) are correct<br/> • Version = n: The protocol version proposed by the remote partner does not comply with the specifications of the PeSIT protocol.Only the values 1 (version D) and 2 (version E) are allowed. The incorrect value is displayed in the message<br/> • Window without Pacing: A synchronization window is specified even though the interval is null<br/> • Window = n too large: The synchronization window negotiated by the REQUESTER partner is too large. The SIT profile does not allow a value greater than 16<br/> • Access = n: The correct access types are 0 for write mode, 1 for read mode and 2 for read/write mode. The other values represent a violation of the protocol. The incorrect value received is displayed in the message.<br/> • Resynchronization = n: The functional resynchronization unit is negotiated by the value 0 (no) or 1 (yes). Any other value is not allowed<br/> • Pacing = n not authorized: The synchronization interval negotiated by the SERVER partner does not comply with the specifications of the SIT profile. The values 1, 2 and 3 are not allowed<br/> • Application type relation R=sapp S=rapp: The SIT profile imposes a correlation between the sender and receiver applications. This correlation is not respected. The message displays the first byte of PI 3 and 4 of the CONNECT FPDU in numeric form<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | The transfer is aborted without trace in the catalog. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH21E"></span>CFTH21E PART=&amp;part MAIN task Synchronization error CR=&amp;cr CS=&amp;scs<br/> CFTH21E MAIN task Synchronization error &lt;PART=&amp;part CR= &amp;cr CS=&amp;cs&gt; |
| --- | --- |
| Explanation | {{< TransferCFT/axwayvariablesComponentShortName  >}} internal synchronization error. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH22E"></span>CFTH22E PART=&amp;part rejected DIAGI=&amp;diagi &lt;HOST=&amp;addr&gt;<br/> CFTH22E NPART=&amp;part rejected DIAGI=&amp;diagi &lt;HOST=&amp;pstate&gt; |
| --- | --- |
| Explanation | Where:<br/> • addr: The caller's IP address<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} refuses to open a protocol session following a request to do so from a partner.<br/> The message displays the {{< TransferCFT/axwayvariablesComponentShortName  >}} diagnostic code. |
| Consequence | The transfer is aborted. No trace of this attempt appears in the catalog. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH23E"></span>CFTH23E NPART=&amp;part rejected EVENT=&amp;pevent<br/> CFTH23E PART=&amp;part rejected EVENT=&amp;pevent |
| --- | --- |
| Explanation | {{< TransferCFT/axwayvariablesComponentShortName  >}} refuses to open a protocol session for internal reasons, following a request to do so from a partner. The event which caused this rejection is displayed in the message. |
| Consequence | The transfer is aborted. No trace of this attempt appears in the catalog. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH24E"></span>CFTH24E PART=&amp;part Invalid CREATE FPDU &amp;str<br/> CFTH24E Invalid CREATE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The CREATE FPDU sent by the REQUESTER partner does not conform. The field "&amp;str" is an explicit character string:<br/> • IDT is null: Reception of a CREATE FPDU with a null Transfer Identifier (PI 13)<br/> • Restart = n: Invalid value for the restart option of a transfer<br/> • Data Code = n: Unknown code for the data to be transported<br/> • Priority = n: Invalid priority assigned to the transfer<br/> • Record Format = n: Unknown record format<br/> • Record size = n greater than Pacing: The record size is greater than the synchronization interval<br/> • NSDU size = n too lower for LRECL = n: In the SIT profile, as segmentation is not authorized, a record must fit in the FPDU. The negotiated NSDU size (RUSIZE) must therefore be greater than the record length of the file (plus 6 for the FPDU header).<br/> • File Organization = n: Unknown file organization<br/> • Key length without indexed organization: A key length is specified for a file that is not indexed<br/> • Key Position without indexed organization: A key position is specified for a file that is not indexed<br/> • Space Unit in record without fixed format: A file must be in fixed format for its size to be expressed as a number of records<br/> • Space Unit = n: Space reservation unit unknown<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=CRE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH25I"></span>CFTH25I PART=&amp;part Concatenation area allocation failure CS=&amp;scs<br/> CFTH25I Concatenation area allocation failure &lt;PART=&amp;part CS=&amp;cs&gt; |
| --- | --- |
| Explanation | Cannot allocate a working area to execute the concatenation option. |
| Consequence | The transfer continues but the concatenation option remains inhibited for the rest of the session. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH26E"></span>CFTH26E PART=&amp;part Too many data without synchronization<br/> CFTH26E Too many data without synchronization &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | Detection of a synchronization error. |
| Consequence | The transfer is aborted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH27E"></span>CFTH27E PART=&amp;part SYNC FPDU without synchronization<br/> CFTH27E SYNC FPDU without synchronization &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | A synchronization FPDU was received unexpectedly as the Functional Synchronization Unit was not negotiated at the beginning of the session. |
| Consequence | The transfer is aborted with a {{< TransferCFT/axwayvariablesComponentShortName  >}} 730 diagnostic code, a protocol violation. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH28E"></span>CFTH28E PART=&amp;part Invalid Checkpoint n<br/> CFTH28E Invalid Chekpoint &lt;PART=&amp;part &amp;n&gt; |
| --- | --- |
| Explanation | Reception of an invalid synchronization point, which does not follow the sequence. |
| Consequence | The transfer is aborted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH29E"></span>CFTH29E PART=&amp;part Invalid FPDU RC=&amp;n<br/> CFTH29E Invalid FPDU RC=&amp;rc Incoming call address= &amp;str &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | An inconsistent FPDU has been received. The RC code enables the error found to be defined more specifically: this code is identical to the one included in the PDU_iNN protocol diagnostic message. |
| Consequence | The transfer is aborted. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH30E"></span>CFTH30E PART=&amp;part Invalid AckORF FPDU &amp;str<br/> CFTH30E Invalid AckORF FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckORF FPDU sent by the SERVER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Compression Indicator = n: The compression indicator has a value which does not comply with the specifications of the PeSIT protocol (0 no compression, 1 compression)<br/> • Compression Value without Indicator: A compression value is negotiated even though the indicator inhibits the compression option<br/> • Compression Indicator without Value: The compression indicator is set even though the negotiated value is null<br/> • Compression Value = n: The negotiated compression value does not comply with to the specifications of the PeSIT protocol<br/> • Compression Negotiation: n for n: The negotiated compression is greater than the proposed compression<br/> • Extended LRECL greater than PACING: n for n:Compression may cause a record to be extended. This risk, which is measurable (1 byte for 32 bytes), means that the record size becomes greater than the synchronization interval<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=AOF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH31E"></span>CFTH31E PART=&amp;part Invalid AckTRANS.END FPDU &amp;str<br/> CFTH31E Invalid AckTRANS.END FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckTRANSFER.END FPDU sent by the SERVER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Byte count mismatch n for n: The number of bytes transferred does not correspond to the {{< TransferCFT/axwayvariablesComponentShortName  >}}-maintained counter<br/> • Record count mismatch n for n: The number of records transferred does not correspond to the {{< TransferCFT/axwayvariablesComponentShortName  >}}-maintained counter<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ATE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH32E"></span>CFTH32E PART=&amp;part Invalid AckMESSAGE FPDU &amp;str<br/> CFTH32E Invalid AckMESSAGE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | This message is only displayed in security-enabled mode and corresponds to a security problem.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=AMG + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH33E"></span>CFTH33E PART=&amp;part Invalid AckSELECT FPDU &amp;str<br/> CFTH33E Invalid AckSELECT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckSELECT FPDU sent by the SERVER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • File type value not authorized: Reception of an AckSELECT FPDU with an invalid file type (PI 11) The values between 0xFFFC and 0xFFFF are invalid<br/> • IDT is null: Reception of an AckSELECT FPDU with a null Transfer Identifier (PI 13)<br/> • Data Code = n: Unknown code for the data to be transported<br/> • Priority = n: Invalid priority assigned to the transfer<br/> • Record Format = n: Unknown record format<br/> • Record size = n greater than Pacing: The record size is greater then the synchronization interval<br/> • NSDU size negotiation n for n: The negotiated NSDU size is greater than that proposed<br/> • NSDU too small n: The negotiated NSDU size is smaller than the minimum authorized value (128)<br/> • File Organization = n: Unknown file organization<br/> • Key length without indexed organization: A key length is specified for a file that is not indexed<br/> • Key Position without indexed organization: A key position is specified for a file that is not indexed<br/> • Space Unit in record without fixed format: A file must be in fixed format for its size to be expressed as a number of records<br/> • Space Unit = n: Space reservation unit unknown<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ASE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH34E"></span>CFTH34E PART=&amp;part Invalid ORF FPDU &amp;str<br/> CFTH34E Invalid ORF FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The ORF FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Compression Indicator = n: The compression indicator has a value that does not comply with the specifications of the PeSIT protocol (0 no compression, 1 compression)<br/> • Compression Value without Indicator: A compression value is negotiated even though the indicator inhibits the compression option<br/> • Compression Indicator without Value: The compression indicator is set even though the negotiated value is null<br/> • Compression Value = n: The negotiated compression value does not comply with the specifications of the PeSIT protocol<br/> • Extended Record size greater than pacing: n for n: Compression may cause a record to be extended. This risk, which is measurable (1 byte for 32 bytes), means that the record size becomes greater than the synchronization interval<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ORF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH35E"></span>CFTH35E PART=&amp;part Invalid TRANS.END FPDU &amp;str<br/> CFTH35E Invalid TRANS.END FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | This message is only displayed in security-enabled mode and corresponds to a security problem.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=TFE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH36E"></span>CFTH36E PART=&amp;part Invalid MESSAGE FPDU &amp;str<br/> CFTH36E Invalid MESSAGE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The MESSAGE FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • IDT is null: Reception of a MESSAGE FPDU with a null Transfer Identifier (PI 13)<br/> • Attribute = n: PI 14 in the MESSAGE FPDU is set to an invalid value (attribute request)<br/> • Data Code = n: Unknown code for the data to be transported<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=MSG + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH37E"></span>CFTH37E PART=&amp;part Invalid D.MESSAGE FPDU &amp;str<br/> CFTH37E Invalid D.MESSAGE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The START of MESSAGE FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • IDT is null: Reception of a D.MESSAGE FPDU with a null Transfer Identifier (PI 13)<br/> • Attribute = n: PI 14 of the D.MESSAGE FPDU is set to an invalid value (attribute request)<br/> • Data Code = n: Unknown code for the data to be transported<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=DMG + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH38E"></span>CFTH38E PART=&amp;part Invalid READ FPDU &amp;str<br/> CFTH38E Invalid READ FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The READ FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Restart point must be null for new transfer: Reception of a READ FPDU with a restart point other than 0 for a new transfer<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=RDF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH39E"></span>CFTH39E PART=&amp;part Invalid SELECT FPDU &amp;str<br/> CFTH39E Invalid SELECT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The SELECT FPDU sent by the REQUESTER partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • IDT not null: Reception of a SELECT FPDU with a non-null Transfer Identifier (PI 13) for a new transfer<br/> • IDT null with restart: Reception of a SELECT FPDU with a null Transfer Identifier (PI 13) for a transfer to be restarted<br/> • Attribute = n: PI 15 of the SELECT FPDU is set to an invalid value (file attribute request)<br/> • Restart = n: PI 15 of the SELECT FPDU is set to an invalid value (restart or new transfer)<br/> • Priority = n: PI 17 of the SELECT FPDU is set to an invalid value (transfer priority)<br/> • NSDU too small n: The negotiated NSDU size is smaller than the minimum allowed value (128)<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=SEL + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH40E"></span>CFTH40E PART=&amp;part Invalid DTF FPDU (MULTART)<br/> CFTH40E Invalid DTF FPDU (MULTART) &lt;PART=&amp;part&gt; |
| --- | --- |
| Explanation | The DTF FPDU received is multi-record but it is not valid (the sum of the record lengths is not equal to the total length of the received FPDU). |
| Consequence | The transfer is aborted. The protocol code transported to the remote partner is 220. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH41E"></span>CFTH41E PART=&amp;part Invalid DTF.END FPDU &amp;str<br/> CFTH41E Invalid DTF.END FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The DTF END (end of data) FPDU sent by the sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=DTE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH42E"></span>CFTH42E PART=&amp;part Invalid SYNC FPDU &amp;str<br/> CFTH42E Invalid SYNC FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The SYNC (set synchronization point) FPDU sent by the sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=SYN + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH43E"></span>CFTH43E PART=&amp;part Invalid AckSYNC FPDU &amp;str<br/> CFTH43E Invalid AckSYNC FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckSYNC (acknowledge synchronization point) FPDU sent by the receiver partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ASY + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH44E"></span>CFTH44E PART=&amp;part Invalid IDT FPDU &amp;str<br/> CFTH44E Invalid IDT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The IDT (transfer interrupt) FPDU sent by the partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received,<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=IDT + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH45E"></span>CFTH45E PART=&amp;part Invalid AckIDT FPDU &amp;str<br/> CFTH45E Invalid AckIDT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckIDT (acknowledge transfer interrupt) FPDU sent by the partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=AID + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH46E"></span>CFTH46E PART=&amp;part Invalid RESYNC FPDU &amp;str<br/> CFTH46E Invalid RESYNC FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The RESYNC (dynamic transfer resynchronization) FPDU sent by the sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Resynchronization is not authorized:<br/> Dynamic resynchronization is not authorized (CFTPROT RESYNC parameter) or the maximum number of resynchronizations is exceeded (CFTPROT RESTART parameter),<br/> • Mismatch between header and FPDU size:<br/> The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:<br/> The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:<br/> The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:<br/> The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU:<br/> The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:<br/> The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=RST + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH47E"></span>CFTH47E PART=&amp;part Invalid DESELECT FPDU &amp;str<br/> CFTH47E Invalid DESELECT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The DESELECT FPDU sent by the requester partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=DSE + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH48E"></span>CFTH48E PART=&amp;part Invalid DESELECT FPDU &amp;str<br/> CFTH48E Invalid AckREAD FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckRead FPDU sent by the receiver/sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ARD + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH49E"></span>CFTH49E PART=&amp;part Invalid WRITE FPDU &amp;str<br/> CFTH49E Invalid WRITE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The WRITE FPDU sent by the requester/sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=WRI + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH50E"></span>CFTH50E PART=&amp;part Invalid M.MESSAGE FPDU &amp;str<br/> CFTH50E Invalid M.MESSAGE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The Middle of Message FPDU sent by the requester partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=MMG + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH51E"></span>CFTH51E PART=&amp;part Invalid F.MESSAGE FPDU &amp;str<br/> CFTH51E Invalid F.MESSAGE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The End of Message FPDU sent by the requester partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU:The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU:The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU:The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU:The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=FMG + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH52E"></span>CFTH52E PART=&amp;part Invalid AckCLOSE FPDU &amp;str<br/> CFTH52E Invalid AckCLOSE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckCLOSE FPDU sent by the server partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ACF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH53E"></span>CFTH53E PART=&amp;part Invalid AckDESELECT FPDU &amp;str<br/> CFTH53E Invalid AckDESELECT FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The AckDESELECT FPDU sent by the server partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n into FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n into FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum le |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=ADS + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH54E"></span>CFTH54E PART=&amp;part Invalid CLOSE FPDU &amp;str<br/> CFTH54E Invalid CLOSE FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The CLOSE (file close) FPDU sent by the requester partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Mismatch between header and FPDU size: The FPDU length indicated in the header is not equal to the length of the FPDU received<br/> • Unknown FPDU: The number identifying the received FPDU is not referenced<br/> • Missing PI number n in FPDU: The PI is mandatory for this type of FPDU<br/> • Unknown PI number n in FPDU: The PI is unknown for this type of FPDU<br/> • PGI n in PGI into FPDU: The presence of a PGI embedded in another PGI is invalid<br/> • Invalid length n for PI n into FPDU: The length of the PI is invalid (less than minimum length or greater than maximum length) |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=CRF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH55E"></span>CFTH55E PART=&amp;part Invalid DTF FPDU &amp;str<br/> CFTH55E Invalid DTF FPDU &lt;PART=&amp;part &amp;str&gt; |
| --- | --- |
| Explanation | The DTF (data send) FPDU sent by the sender partner does not conform.<br/> The field "&amp;str" is an explicit character string:<br/> • Too much data without synchro<br/> A synchronization interval, CFTPROT SPACING or RPACING parameter, has been negotiated and the amount of data received since the start of the transfer (or since the last synchronization FPDU) is greater than this interval. |
| Consequence | Transfer aborted with DIAGI=220, DIAGP=DTF + PeSIT code. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH56I"></span>CFTH56I PART=&amp;part IDS=&amp;ids &amp;prot &amp;str session opened pi7=&amp;n:&amp;n HOST=&amp;pstate &amp;prot<br/> CFTH56I &amp;prot &amp;str session opened &lt;PART=&amp;part IDS=&amp;ids pi7=&amp;n:&amp;n HOST=&amp;pstate&gt; &amp;prot |
| --- | --- |
| Explanation | An &amp; prot session in either Requester or Server mode was opened, where &amp;prot = PeSIT, ODETTE, or SFTP. And where:<br/> • PART: partner<br/> • PROT: local protocol definition (CFTPROT)<br/> • IDS: reference for this session<br/> • pi7: the window and the interval of the negotiated synchronization<br/> • pi2 and pi24: the window and the interval of the negotiated synchronization<br/> • HOST: • Requester: The host address configured through CFTTCP for the related partner (either an IP or a logical hostname).<br/><br/> • Server: The IP address of the incoming connection.<br/><br/>  |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH57I"></span>CFTH57I PART=&amp;part IDS=&amp;ids IDF=&amp;idf IDT=&amp;idt transfer selected pi25=&amp;n<br/> CFTH57I transfer selected PART=&amp;part IDS=&amp;ids IDF=&amp;idf IDT=&amp;idt pi25=&amp;n |
| --- | --- |
| Explanation | A transfer passed the selection phase in the PeSIT session that was referenced by the IDS field.<br/> The field pi25 indicate the maximum size of the negotiated message. The displayed reference in the second message is the public transfer reference. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH58I"></span>CFTH58I PART=&amp;part IDS=&amp;ids IDF=&amp;idf NIDT=&amp;idt transfer deselected T=&amp;n<br/> CFTH58I transfer deselected &lt;PART=&amp;part IDS=&amp;ids IDF=&amp;idf NIDT=&amp;idt T=&amp;n&gt; |
| --- | --- |
| Explanation | A transfer passed the deselection phase in the PeSIT session referred to by the IDS. The IDS is the reference for this particular session context.<br/> The T field indicates the armed time-out for the CFTPROT parameter:<br/> • disctd – requester mode, or<br/> • discts – server mode |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH59I"></span>CFTH59I PART=&amp;part IDS=&amp;ids IDM=&amp;idm NIDT=&amp;idt message transferred<br/> CFTH59I message transferred PART=&amp;part IDS=&amp;ids IDM=&amp;idm NIDT=&amp;idt |
| --- | --- |
| Explanation | A message transfer was carried out in the PeSIT as referenced by the IDS field, where the IDS refers to this specific session context.<br/> The displayed reference in the second message is the public transfer reference. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH60I"></span>CFTH60I PART=&amp;part IDS=&amp;ids IDM=&amp;idm NIDT=&amp;idt [Reply &#124; Nack] transferred<br/> CFTH60I [Reply &#124; Nack] transferred PART=&amp;part IDS=&amp;ids IDM=&amp;idm NIDT=&amp;idt |
| --- | --- |
| Explanation | An acknowledgement type transfer message was carried out in the PeSIT session, where the IDS references the session context.<br/> The reference in the second message is the public transfer reference. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH61I"></span>CFTH61I PART=&amp;part IDS=&amp;ids ["Requester"&#124;"Server"] &amp;ref session closed &amp;prot<br/> CFTH61I &amp;prot ["Requester"&#124;"Server"] session closed &lt;PART=&amp;part IDS=&amp;ids&gt; &amp;prot |
| --- | --- |
| Explanation | An &amp; prot session in either Requester or Server mode was closed, where &amp;prot = PeSIT, ODETTE, or SFTP.<br/> And:<br/> • &amp;part: the partner identifier involved in the session<br/> • &amp;ids: the reference for this session context |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH62I"></span>CFTH62I REF=&amp;ref<br/> CFTH62I REF=&amp;ref |
| --- | --- |
| Explanation<br/>  |  • The transfer has passed the selection phase in the PeSIT session referenced by the IDS field. <br/> • An acknowledgement-type message was performed in the PeSIT session referenced by the IDS, the session reference.<br/> • The pi25 field indicates the maximum size of the negotiated message.<br/> • The reference displayed in the second message is the public reference of the transfer (pi13.pi3.pi4.pi11.pi12.pi61.pi62). |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH63I"></span>CFTH63I PART=&amp;part IDS=&amp;ids PESIT DMZ session for messages only<br/> CFTH63I PESIT DMZ session for messages only &lt;PART=&amp;part IDS=&amp;ids&gt; |
| --- | --- |
| Explanation | A PeSIT DMZ session was open but only for mailing messages. This message follows message CFTH56I, where the IDS is the session call id. |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH64I"></span>CFTH64I PESIT session rejected L=&amp;reason R=&amp;diag<br/> CFTH64I PESIT session rejected L=&amp;local R=&amp;reason |
| --- | --- |
| Explanation | Protocol connection refused at network level.<br/> • &amp;reason: Network reason<br/> • &amp;diag: Network diagnostic |


 


| V23 format<br/> V24 format<br/> Information | <span id="CFTH65I"></span>CFTH65I PART=&amp;part IDS=&amp;ids PESIT DMZ permanent session control call=&amp;n<br/> CFTH65I PESIT DMZ permanent session control call=&amp;n &lt;PART=&amp;part IDS=&amp;ids &gt; |
| --- | --- |
| Explanation | Support for permanent links in DMZ.<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} in DMZ does not give the TURN when there are no more files to send, but sends an FPDU Control Call to the initiator {{< TransferCFT/axwayvariablesComponentShortName  >}} at regular negotiated intervals to prevent the temporization from expiring.<br/> • &amp;ids = Session call id<br/> • &amp;call = interval for the DMZ control call |


 


| V23 format<br/> V24 format<br/> Error | <span id="CFTH66E"></span>CFTH66E Incoming calls (&amp;count) rejected, ERROR=&amp;error (&amp;info1&#124;&amp;info2), PROTOCOL=&amp;protocol<br/> CFTH66E Incoming calls (&amp;count) rejected, ERROR=&amp;error (&amp;info1&#124;&amp;info2), PROTOCOL=&amp;protocol |
| --- | --- |
| Explanation | Incoming calls are rejected:<br/> • &amp;count = Number of rejected calls<br/> • &amp;error = Error message<br/> • &amp;info1 = Additional information<br/> • &amp;info2 = Additional information<br/> • &amp;protocol = Protocol type when available |



| V23 format<br/> V24 format<br/> Information | <span id="CFTH67I"></span>PeSIT Server Application Transparent TLS SSL mode &lt;PART=xxxxxx IDS=00005 CIPHER=49199&gt;<br/> PeSIT Server Application Transparent TLS SSL mode &lt;PART=xxxxxx IDS=00005 CIPHER=49199&gt; |
| --- | --- |
| Explanation | *z/OS environments*<br/> When using PeSIT mode server with AT-TLS, a message displays in the log indicating a secure connection if the socket is secure. Details include the partner, session ID, and cipher used in the exchange. |

