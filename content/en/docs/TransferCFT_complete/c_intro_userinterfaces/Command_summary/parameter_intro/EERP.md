{
    "title": "EERP",
    "linkTitle": "eerp",
    "weight": "800"
}<span id="EERP"></span>

### EERP

Used to interpret the value of the ORIGINATOR and DESTINATOR fields
contained in the EERP message, according to the protocol version.

The End to End ResPonse service generates a message called EERP. This
message informs the file sender that the data sent arrived correctly.

The first version of the protocol (1986) specifies that:

- the ORIGINATOR
    protocol field corresponds to the file sender
- the DESTINATOR
    protocol field corresponds to the file receiver

The second version (1991) specifies that:

- the ORIGINATOR
    protocol field corresponds to the EERP sender (i.e. the file receiver)
- the DESTINATOR
    protocol field corresponds to the EERP receiver (i.e. the file sender)

> **Note**
>
> Note: You must check the consistency of the customized values from one
> end to another. Indeed, if the sender and receiver have different versions,
> it is not possible to acknowledge the transfer.

[Return to Command index](../../)
