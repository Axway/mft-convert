---
    title: "rappl"
    linkTitle: "rappl"
    weight: 2790
---<span id="rappl"></span>

### rappl

#### CFTSEND, CFTRECV, SEND, RECV

**[RAPPL = *string*]**

- string48      PeSIT
    E CFT/CFT
- string....8          PeSIT E

Identifier of the application receiving the
file.

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

{{< TransferCFT/axwayvariablesComponentShortName  >}} does not check:

- The protocol to
    be used for the transfer. If the protocol used is ODETTE, this parameter
    is not sent.
- The maximum size
    permitted by the protocol. Only a check of the maximum size of 48 characters
    is performed.

****PeSIT E or PeSIT E between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s****

In standard PeSIT E standard or PeSIT E CFT/CFT, the responder/sender
partner can send and control this field.

In standard PeSIT E, this value is transported in the PI 04. Its maximum
length is limited by the eight-character standard. The PI 04 contains
this value concatenated with the value of the ****ruser****
field.

In PeSIT E between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s, the value of the SUSER parameter is transported in the PI 99, while the value defined in the PI 03 is truncated to 8 characters.

[Return to Command index](../../)
