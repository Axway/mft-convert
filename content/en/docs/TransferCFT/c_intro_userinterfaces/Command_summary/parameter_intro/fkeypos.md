{
    "title": "fkeypos",
    "linkTitle": "fkeypos",
    "weight": "1180"
}<span id="fkeypos"></span>

### fkeypos

#### CFTSEND, CFTRECV, SEND, RECV

****[FKEYPOS = {0 &#124; n}]    ****

****{0...32767}****

Key position in bytes, relative to 0, in the records of an indexed
file.

******For receiver:******

The default value is the value received from the protocol (PI 39) which
is present if it has been defined by the requester. If this value is absent,
the default value is then equal to 0.

{{< TransferCFT/suitevariablesTransferCFTName  >}} receives the file in the form of sequential records. The
client can develop a file type EXIT or write an end-of-transfer procedure
to use this information which can be recovered by the symbolic variable
&FKEYPOS.

******For sender:******

{{< TransferCFT/suitevariablesTransferCFTName  >}} sends this information, transported by the protocol in the
PI 39, but does not use it. The indexed file is transferred in the form
of sequential records.

[Return to Command index](../../)
