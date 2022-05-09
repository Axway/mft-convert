{
    "title": "Miscellaneous  codes",
    "linkTitle": "Miscellaneous codes",
    "weight": "370"
}The codes in this topic are listed by protocol.

<span id="FPDU_Build_Error_Codes__PeSIT_"></span>

### FPDU Build error codes (PeSIT)

This code specifies a build error in the received PeSIT FPDU. It forms the "PDU iNN"-type protocol diagnostic code.


| Error code  | Description  |
| --- | --- |
| 1 | Received AckCONNECT FPDU header does not conform: an error was detected in the source or target identifier content. The source identifier must be null. The target identifier must be the same as the source identifier sent in the CONNECT FPDU |
| 2 | Reception of two or more FPDUs concatenated in one NSDU. According to protocol specifications, an FPDU cannot be followed or preceded by another FPDU |
| 3 | Reception of a CONNECT FPDU followed by two bytes that do not belong to it, but the CRC option has not been implemented |
| 4 | Reception of a CONNECT FPDU with an incorrect CRC |
| 5 | Received CONNECT FPDU header does not conform: an error has been detected in the source or target identifier content. The source identifier must not be null. The target identifier must be null |
| 6 | Reception of an FPDU other than CONNECT with an incorrect CRC |
| 7 | Reception of two or more FPDUs concatenated in one NSDU, while the CRC option is active. According to the protocol specifications, FPDU concatenation is inhibited with the CRC option enabled |
| 8 | Header of a received FPDU does not conform: the size indicated is smaller than the minimum size of an FPDU. The minimum size of an FPDU in the PeSIT protocol is six bytes (length of the header). If a CRC is applied, the minimum size of an FPDU becomes eight bytes (length of a header with its CRC) |
| 9 | Header of a received CONNECT-phase FPDU does not conform: an error has been detected in the target identifier in the header |
| 10 | Header of a received CONNECT-phase FPDU does not conform: an error has been detected in the source identifier in the header |
| 11 | Reception of a network message that is smaller than the minimum size of an FPDU. The minimum size of an FPDU in the PeSIT protocol is six bytes (length of the header). If a CRC is applied, the minimum size of an FPDU becomes eight bytes (length of a header with its CRC) |
| 12 | Concatenated FPDU with invalid header |
| 13 | Reception of an NSDU that is larger than that negotiated |
| 14 | Header of a received FPDU does not conform: an error has been detected in the phase byte |
| 15 | Header of the received RelCONNECT FPDU does not conform: an error has been detected in the source or target identifier content. The source identifier must be null. The target identifier must be the source identifier of the CONNECT FPDU |
| 16 | Header of a received FPDU does not conform: the size indicated is greater than the length of the received network message |
| 17 | Header of a received SERVICE phase FPDU does not conform: an error has been detected in the source or target identifier in the header |
| 18 | Received FPDU of unknown type |
| 19 | Received FPDU of a phase inconsistent with its type |


<span id="CFT_Numeric_codes___OFTP__ODETTE__protocol"></span>

### {{< TransferCFT/axwayvariablesComponentShortName  >}} Numeric codes - OFTP (ODETTE) protocol

These codes, specific to the ODETTE protocol and internal to the {{< TransferCFT/axwayvariablesComponentShortName  >}},
indicate the source of the failure. This code forms the DIAGP protocol diagnostic
code. Values are expressed in hexadecimal.


| Error code  | Description  |
| --- | --- |
| 0101 | Application area allocation error |
| 0102 | Unknown event during network connection |
| 0150 | Protocol release error |
| 0151 | Invalid restart value |
| 0152 | CREDIT value error:<br/> 1. Reception of a CDT FPDU but the &quot;credit&quot; has not been used up<br/> 2. Reception of a CDT FPDU but the negotiated &quot;credit&quot; value is 0 |
| 0202 | Restart position negotiation. The position returned by the partner is higher than the proposed position |
| 0203 | Restart option proposed by the requester and that required by the server (RESYNC parameter) are incompatible |
| 0204 | Compression negotiation. The compression value returned by the partner is greater than the proposed value |
| 0205 | Network buffer length negotiation. The buffer size requested by the partner is less than 128 or greater than that proposed |
| 0206 | CREDIT negotiation. The &quot;credit&quot; value requested by the partner is out of bounds |
| 0207 | Transfer DIRECTION negotiation. The value requested by the partner and the SRIN (or SROUT) parameter value are incompatible |
| 0208 | PAD option negotiation (special logic). The partner requests &quot;special logic&quot; whilst the PAD parameter is set to &quot;NO&quot; (default value) |
| 0250 | Restart position error. Reception of a restart request (SFID FPDU) but the &quot;no restart&quot; option has been previously negotiated |
| 0301 | During the protocol recognition phase the {{< TransferCFT/axwayvariablesComponentShortName  >}} does not receive the expected string: &quot;ODETTE FTP READY&quot; |
| 0350 | The total length of &quot;subrecords&quot; forming the FPDU is different from the FPDU size specified in the SDATAIN field |
| 0351 | Invalid size for subrecord sent |
| 0401 | A restart is requested by the partner but the restart option is not set (RESYNC parameter) |
| 0402 | Reception of a &quot;CREATE CONF NEG&quot; FPDU: creation of the receive file refused by the partner |
| 0501 | Reception of an &quot;ABORT&quot; FPDU: transfer interrupted by the partner |
| 0550 | The SRUSIZE parameter value is less than 128, which is forbidden by the protocol |
| 0551 | Invalid restart parameter value |
| 0601 | IDF incompatibility. The received NIDF value does not correspond to the IDF requested (RECV IDF=xxxx command).<br/> Note: the only valid value for the IDF parameter of the RECV request is &quot;*&quot; |
| 0650 | Reception of a negative A_SELECT |
| 0701 | Error during the file de-selection phase at the partner end |
| 0750 | Internal monitor error: attempt to send a DATA FPDU but the &quot;credit&quot; has been completely spent and the {{< TransferCFT/axwayvariablesComponentShortName  >}} is waiting for a CDT FPDU |
| 0751 | Record size is greater than the size of the exchange buffer |
| 0A00 | Local SSRM FPDU formatting error |
| 0A02 | Local SSID FPDU formatting error |
| 0A03 | Local ASSID FPDU formatting error |
| 0A04 | Local SFID FPDU formatting error |
| 0A05 | Local EFID FPDU formatting error |
| 0A06 | Local ESID FPDU formatting error |
| 0A07 | Local CDT FPDU formatting error |
| 0A08 | Local CD FPDU formatting error |
| 0A09 | ocal EERP FPDU formatting error |
| 0A0A | Local DTF FPDU formatting error |
| 0B00 | Formatting error in connection acknowledge FPDU |
| 0B01 | Local SFPA FPDU formatting error |
| 0B02 | Local SFNA FPDU formatting error |
| 0B03 | Local EFPA FPDU formatting error |
| 0B04 | Local EFNA FPDU formatting error |
| 0B05 | Local RTR FPDU formatting error |


<span id="CFT_Mnemonic_codes___Odette_protocol"></span>

### {{< TransferCFT/axwayvariablesComponentShortName  >}} Mnemonic codes - ODETTE protocol

These codes, specific to the ODETTE protocol and internal to the {{< TransferCFT/suitevariablesTransferCFTName  >}},
indicate the source of the fault.

This code forms the "XXX HHHH"-type DIAGP protocol diagnostic
code. Values are expressed in mnemonic form.


| Error code  | Description  |
| --- | --- |
| CDT | Error during &quot;credit&quot; negotiation |
| DAT | Synchronization problem in &quot;credit&quot; and &quot;data&quot; exchanges |
| FMT | Internal FPDU formatting error |
| IDF | Received NIDF incompatible with sent IDF<br/> Note: only the RECV IDF=* command is valid in ODETTE |
| LDT | Error in the network buffer size negotiation phase |
| MSG | Error when acknowledging the EERP message |
| PAD | Special logic negotiation error |
| POS | Restart point negotiation error |
| RST | Restart option negotiation error |
| SFI | Error during negotiation of a send file parameter (SFID FPDU) |
| SSI | Error during negotiation of a session parameter (SSID FPDU) |
| VER | Error in the protocol software release number (at present this number is set to 1) |


<span id="FPDU_Mnemonic_codes___PeSIT_protocol"></span>

### FPDU Mnemonic codes - PeSIT protocol

This code forms the "XXX NNN" or "XXX iNNN" DIAGP
protocol diagnostic code in the PeSIT protocol; it represents the XXX
part. Values are expressed in mnemonic form.


| Code  | FPDU  |
| --- | --- |
| ABO | ABORT |
| ACF | Ack CLOSE REMOTE FILE |
| ACO | Ack CONNECT |
| ACR | Ack CREATE |
| AID | Ack IDT |
| ADS | Ack DESELECT |
| AMG | Ack MESSAGE |
| AOF | Ack OPEN REMOTE FILE |
| ARD | Ack READ |
| ASE | Ack SELECT |
| ASY | Ack SYNC |
| ATE | Ack TRANSFER END |
| AWR | Ack WRITE |
| CON | CONNECT |
| CRE | Ack CREATE |
| CRF | CLOSE REMOTE FILE |
| DMG | Start of MESSAGE |
| DSE | DESELECT |
| DTE | DATA TRANSFER END |
| DTF | DATA |
| FMG | End of MESSAGE |
| IDT | TRANSFER INTERRUPT |
| MMG | Middle of MESSAGE |
| MSG | MESSAGE |
| ORF | OPEN REMOTE FILE |
| RCO | Release CONNECT |
| RDF | READ |
| RST | RESTART |
| SEL | SELECT |
| SYN | CHECK |
| TFE | TRANSFER END |
| WRI | WRITE |

