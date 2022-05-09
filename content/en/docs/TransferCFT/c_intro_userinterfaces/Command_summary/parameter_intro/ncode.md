{
    "title": "ncode",
    "linkTitle": "ncode",
    "weight": "2140"
}<span id="ncode"></span>

### ncode

<span id="ncode_CFTSEND"></span>

#### CFTSEND, SEND

****[NCODE = { '_' &#124; ASCII
&#124; BINARY &#124; EBCDIC}]****

****PeSIT****

The network data code to use for sending transfers.

Although there is no default value for this field, for each transfer
it can implicitly take one of the following values:

- BINARY
    if FCODE=BINARY
- ASCII
    or EBCDIC, depending on the partner
    computer code deduced from the SYST
    field

Additionally, with the parameters FCODE and XLATE,  NCODE
determines the translation performed during a send transfer.

The following values explicitly or implicitly determine the action:

- If FCODE or NCODE
    is equal to BINARY, no translation is performed
- If not (the local
    data and the on-line data being assumed to be coded in ASCII or EBCDIC):
- If NCODE is
    equal to FCODE, ASCII/ASCII or EBCDIC/EBCDIC translation can only be performed
    with an external translation table (see the use of the XLATE parameter)
- If NCODE is
    not FCODE, ASCII/EBCDIC or EBCDIC/ASCII translation is always performed,
    whether by means of an external translation table or the {{< TransferCFT/axwayvariablesComponentShortName  >}}
    internal translation table

In the PeSIT protocol Transfer
CFT can send a "data code" protocol parameter (an indicator);
this parameter then corresponds to NCODE.

****SFTP****

The network data code for sending transfers. The default ' ' indicates BINARY (no transcoding).

The following values explicitly or implicitly determine the action:

- If FCODE or NCODE
    is equal to BINARY, no translation is performed
- If NCODE is equal to FCODE, no ASCII/ASCII or EBCDIC/EBCDIC translation is performed
- If NCODE is not equal FCODE **and FTYPE = T, X, O or J**, ASCII/EBCDIC or EBCDIC/ASCII translation is performed using either an external translation table or the internal Transfer CFT translation table
    -   FTYPE = T (End of Line CRLF on Windows, LF on Unix)
    -   FTYPE = X (End of line = LF)
    -   FTYPE = O (End of line = CRLF)
    -   FTYPE = J (Stream Text)

Regardless of the FTYPE, when using SFTP, the end-of-line in the received file is Text type (CRLF on Windows, and LF on Unix).

For further information, refer to *[Protocols](../../../../protocols_start_here)*.

#### CFTRECV

****[NCODE = { '_' &#124; ASCII
&#124; BINARY &#124; EBCDIC}]****

****SFTP only****

The network data code when receiving transfers. The default ' ' indicates BINARY (no transcoding).

The following values explicitly or implicitly determine the action:

- If FCODE or NCODE
    is equal to BINARY, no translation is performed
- If NCODE is equal to FCODE, no ASCII/ASCII or EBCDIC/EBCDIC translation is performed
- If NCODE is not equal FCODE **and FTYPE = T, X, O or J**, ASCII/EBCDIC or EBCDIC/ASCII translation is performed using either an external translation table or the internal Transfer CFT translation table
    -   FTYPE = T (End of Line CRLF on Windows, LF on Unix)
    -   FTYPE = X (End of line = LF)
    -   FTYPE = O (End of line = CRLF)
    -   FTYPE = J (Stream Text)

Regardless of the FTYPE, when using SFTP, the end-of-line in the received file is Text type (CRLF on Windows, and LF on Unix).

<span id="ncode_CFTXLATE"></span>

#### CFTXLATE

****[NCODE = {ASCII &#124; EBCDIC}]****

Code of data sent over the network.

About translation during a transfer:

- If a data translation must be performed, the translation table defined
    by CFTXLATE will be used. For the sender, the translation is done from
    FCODE to NCODE, and for the receiver from NCODE to FCODE.

[Return to Command index](../../)
