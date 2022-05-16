---

    title: FOR
    linkTitle: for
    weight: 1250

---
<span id="for"></span>

### FOR

#### CFTDEST

****\[ FOR
= { <span style="text-decoration: underline;">BOTH</span>
| COMMUT |
LOCAL } \]****

Option include:

- BOTH
    (default value): On a local {{< TransferCFT/headerfootervariableshflongproductname >}} with both LOCAL and COMMUT facilities.
- COMMUT:
    On a local {{< TransferCFT/headerfootervariableshflongproductname >}} that serves as an intermediate site. {{< TransferCFT/headerfootervariableshflongproductname >}} receives
    a file or a message whose identifier in the CFTDEST object is set to the
    recipient’s network name. It then broadcasts the file or message to all
    partners in the list.
- LOCAL:
    On a local {{< TransferCFT/headerfootervariableshflongproductname >}} to send (broadcast) or receive (collect) a file  

[Return to Command index](../../)
