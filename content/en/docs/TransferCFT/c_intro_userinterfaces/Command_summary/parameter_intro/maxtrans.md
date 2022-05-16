---

    title: maxtrans
    linkTitle: maxtrans
    weight: 1960

---
<span id="maxtrans"></span>

### maxtrans

#### CFTPARM

****\[MAXTRANS = { <u>256</u> | n} \]****

The maximum authorized number of transfers in parallel. When using multi node, this is the number of transfers per node.

- 1 to **1000** for all operating systems (256 default)

This value sets the physical limit for the product independently of
the limit set by the software key.

The UCONF<span class="code">` cft.server.maxtrans`</span> parameter overrides MAXTRANS as long as it is not set to 0. If `cft.server.maxtrans `is set to 0, the MAXTRANS value is used.

[Return to Command index](../../)
