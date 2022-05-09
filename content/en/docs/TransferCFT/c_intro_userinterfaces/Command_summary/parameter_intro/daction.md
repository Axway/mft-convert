{
    "title": "daction",
    "linkTitle": "daction",
    "weight": "600"
}### daction

#### SEND, RECV

****[ daction= { <span class="underline">ERROR</span> &#124; RESUME} ]****

Use this option to manage erroneous duplicate file transfers.

- ERROR: creates a new file transfer with STATE=K and DIAGI=432 and writes an error message in the log. In synchronous mode returns DIAGI=432
- RESUME: in synchronous mode returns identifiers (IDT and IDTU) of the already existing file transfer

See also [DUPLICAT](../duplicat).

[Return to Command index](../../)
