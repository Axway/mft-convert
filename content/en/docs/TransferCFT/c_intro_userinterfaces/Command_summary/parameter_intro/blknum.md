---

    title: BLKNUM
    linkTitle: blknum
    weight: 330

---
<span id="blknum"></span>

### BLKNUM

#### DELETE, HALT, START, RESUME, SUBMIT, KEEP, END

**\[BLKNUM = {<u>0</u> | n}\]    {0
... 192000}**

Block number. If the values '\*' or ' ' are used then all transfers are
selected regardless of the block that they belong to.

You should use [IDTU](../idtu) instead of the BLKNUM parameter, as BLKNUM use is:

- Deprecated in a mono-node instance
- Not supported in multi-node configurations

[Return to Command index](../../)
