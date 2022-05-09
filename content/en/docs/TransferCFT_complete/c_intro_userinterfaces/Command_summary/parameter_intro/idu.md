{
    "title": "idt",
    "linkTitle": "idt",
    "weight": "1560"
}<span id="idt"></span>

### idt

#### HALT, END, KEEP, SUBMIT, START, DELETE, LISTCAT, DISPLAY, RESUME

******[IDT =
{ <span class="underline">\*</span> &#124; *transid*}]******

Transfer identifier. Identifies a transfer for a given partner and transfer
direction. The value '\*' means that no selection is required on the IDT
parameter (default value).

#### SEND TYPE = REPLY

******[IDT =
*transid* ]******

When sending replies, this corresponds to the original transfer IDT,
and hence to the corresponding catalog entry (in the RT or RX state, in
the catalog).

Its value is an 8-character string, defined as follows:

- one letter indicating
    the MONTH (A to L, A for January, etc.),
- two digits for
    the DAY (01 to 31),
- two digits for
    the HOUR (00 to 23),
- two digits for
    the MINUTES (00 to 59),
- one digit for the
    TENS OF SECONDS (0 to 5

For a reply SEND command, used by a file end-of-receive procedure (most
common occurrence), the symbolic variable &IDT is used to define the
value of this IDT parameter, in order to retrieve the transfer identifier
associated with the file received.

 

[Return to Command index](../../)
