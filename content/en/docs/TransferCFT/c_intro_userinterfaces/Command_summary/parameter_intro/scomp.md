---

    title: scomp
    linkTitle: scomp
    weight: 3080

---
<span id="scomp"></span>

### scomp

#### CFTPROT

**\[SCOMP = { <u>0</u> | 15 } \]** 

The maximum authorized compression for sending a file.

- rcomp//scomp
    = <span style="font-weight: bold;">****0****</span> (no compression)
- rcomp//scomp
    = <span style="font-weight: bold;">****1****</span> (compression of a string of
    blanks)
- rcomp//scomp
    = <span style="font-weight: bold;">****2****</span> (compression of a string of
    identical characters)
- rcomp//scomp
    = <span style="font-weight: bold;">****4****</span> (character compression)
- rcomp//scomp
    = <span style="font-weight: bold;">****8****</span> (vertical compression)
- rcomp//scomp
    = <span style="font-weight: bold;">****15****</span> (all compressions)

This compression is negotiated between the sender and the receiver.

Default values are:


| Protocol  | Default value |
| --- | --- |
| PeSIT ANY profile | 0 |
| ODETTE  | 1  |


[Return to Command index](../../)
