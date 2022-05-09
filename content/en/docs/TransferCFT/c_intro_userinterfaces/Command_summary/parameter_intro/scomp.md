{
    "title": "scomp",
    "linkTitle": "scomp",
    "weight": "3100"
}<span id="scomp"></span>

### scomp

#### CFTPROT

**[SCOMP = { <span class="underline">0</span> &#124; 15 } ]** 

The maximum authorized compression for sending a file.

- rcomp//scomp
    = ****0**** (no compression)
- rcomp//scomp
    = ****1**** (compression of a string of
    blanks)
- rcomp//scomp
    = ****2**** (compression of a string of
    identical characters)
- rcomp//scomp
    = ****4**** (character compression)
- rcomp//scomp
    = ****8**** (vertical compression)
- rcomp//scomp
    = ****15**** (all compressions)

This compression is negotiated between the sender and the receiver.

Default values are:


| Protocol  | Default value |
| --- | --- |
| PeSIT ANY profile | 0 |
| ODETTE  | 1  |


[Return to Command index](../../)
