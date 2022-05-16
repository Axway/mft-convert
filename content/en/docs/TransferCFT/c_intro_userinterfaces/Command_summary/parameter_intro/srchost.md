---

    title: srchost
    linkTitle: srchost
    weight: 3270

---
<span id="srchost"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTNET

****\[ SRCHOST = { hostname1 | 64 } \]****

This parameter is used only for outgoing calls on a resource. If
the value, either the local hostname or IP address, is assigned then that value is used
to select the interface on which the outgoing call will occur.

<span style="font-weight: bold;">****Example****</span>:

CFTPROT ID = PANY, SAP
= 1761....NET=ANY

<span style="font-family: 'Courier New', monospace;">CFTNET ID=ANY,
HOST = INADDR\_ANY, SRCHOST= </span><span style="font-family: 'Courier New', monospace;font-weight: bold;text-decoration: underline;">****my.address.net****</span>

Where <span style="font-family: 'Courier New', monospace;font-weight: bold;">****my.address.net****</span>
is used for the outgoing call.

 

[Return to Command index](../../)
