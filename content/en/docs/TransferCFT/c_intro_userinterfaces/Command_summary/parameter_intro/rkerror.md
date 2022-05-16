---

    title: rkerror
    linkTitle: rkerror
    weight: 2930

---
<span id="rkerror"></span>

### rkerror

#### CFTCAT, CFTRECV

**\[RKERROR = {<u>KEEP</u>** | **DELETE}\]**

**A**ction to be taken if a transfer
aborts due to the receiving file creation error (server mode):

- <span style="font-weight: bold;">****KEEP****</span>:
    the transfer remains in the catalog
- <span style="font-weight: bold;">****DELETE****</span>:
    the transfer is removed from the catalog

If the RKERROR parameter is also set in the CFTRECV command, the CFTRECV
command takes precedence.

 

[Return to Command index](../../)
