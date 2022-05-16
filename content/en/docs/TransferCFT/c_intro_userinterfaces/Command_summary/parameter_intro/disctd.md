---

    title: disctd
    linkTitle: disctd
    weight: 730

---
<span id="disctd"></span>

### disctd

#### **CFTPROT**

****\[DISCTD = { <u>see table below</u> | n} {0..3600}
\]****

Wait timeout in seconds before disconnection, in the absence of a new
transfer request to the partner, in requester mode.

The session established for a transfer remains active for DISCTD seconds
after the completion of this transfer.

If the value is 0, the wait timeout is infinite.

The default value, in seconds, depends on the protocol and is
indicated in the table below.

Default values are:


| Protocol  | Default value |
| --- | --- |
| PeSIT ANY profile | 10  |
| ODETTE  | 20  |


[Return to Command index](../../)