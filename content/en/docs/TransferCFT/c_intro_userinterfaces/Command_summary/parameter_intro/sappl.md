---

    title: sappl
    linkTitle: sappl
    weight: 3050

---
<span id="sappl"></span>

### sappl

#### CFTSEND, SEND, RECV

****\[SAPPL     =    
*string*\]****

*****string8*
   **PeSIT
D CFT profile, PeSIT SIT profile, PeSIT E******

****string48 PeSIT
E CFT/CFT****

The identifier of the application sending the. Depending on the protocol
profile, it is:

- an
    eight-character string for PeSIT D CFT, PeSIT E, PeSIT SIT
- a 48-character
    string for PeSIT E CFT/CFT

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

{{< TransferCFT/axwayvariablesComponentShortName  >}} does not check:

- The relevance as
    regards the protocol to be used for the transfer. If the protocol used
    is PeSIT D Extern profile, or ODETTE,
    this parameter is not sent.
- The maximum size
    permitted by the protocol. Only a check relative to the maximum size of
    48 characters is performed.

****PeSIT****

In standard PeSIT E or PeSIT E CFT/CFT, the responder/sender
partner can send and control this field.

In standard PeSIT E, this value is transported in the PI 03. Its maximum
length is limited by the eight-character standard. The PI 03 contains
this value concatenated with the value of the <span style="font-weight: bold;">****suser****</span>
field.

In PeSIT E between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s, the value of this SUSER parameter is transported in the PI 99, the value defined in the PI 03 being truncated to 8 characters.

[Return to Command index](../../)

 
