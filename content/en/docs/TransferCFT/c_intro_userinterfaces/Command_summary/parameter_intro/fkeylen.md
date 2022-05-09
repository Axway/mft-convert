{
    "title": "fkeylen",
    "linkTitle": "fkeylen",
    "weight": "1170"
}<span id="fkeylen"></span>

### fkeylen

#### CFTSEND, CFTRECV, SEND, RECV

****[FKEYLEN = {0 &#124; n}]     ****

****{
0...32767}  PeSIT
E****

Key length (in bytes) of an indexed file.

******For a receiver:******

The default value is the value received from the protocol (PI 38) which
is present if it has been defined by the requester. If this value is absent,
the default value is then equal to 0.

The monitor receives the file in the form of sequential records. The
client can develop a file type EXIT or write an end-of-transfer procedure
to use this information which can be recovered by the symbolic variable
&FKEYLEN.

******For a sender:******

{{< TransferCFT/suitevariablesTransferCFTName  >}} sends this information, transported by the protocol in the
PI 38, but does not use it. The indexed file is transferred in the form
of sequential records.

[Return to Command index](../../)
