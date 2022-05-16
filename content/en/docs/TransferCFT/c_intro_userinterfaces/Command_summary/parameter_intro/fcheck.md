---

    title: fcheck
    linkTitle: fcheck
    weight: 1090

---
<span id="fcheck"></span>

### fcheck

#### CFTRECV

****FCHECK = { NO | YES }****

This parameter enables you to reject an incoming transfer if local file
attributes don't match the virtual file attributes.

- <span style="font-weight: bold;">****NO****</span>
    (default value) - {{< TransferCFT/axwayvariablesComponentShortName >}} behavior is unchanged, and no check is
    performed. If the virtual file record length and format do not match the
    FLRECL and FRECFM attributes, the record is truncated or padded.
- <span style="font-weight: bold;">****YES****</span> - A record check is performed. FLRECL and FRECFM attributes are compared
    with the virtual file record length and format. If attributes do not match
    the transfer is rejected.

****Rejected
transfer on the receiver:****

- The local error code 139 “Invalid file
    attributes” is set (receiver side)
- PeSIT reason 200 or OFTP 99 is returned
    to the sender
- The message “CFTF27E PART=&part IDF=&idf
    IDT=&idt local/virtual file attribute mismatch” is logged

****Rejected transfer on the sender:****

- The local error code 639 “file attribute
    mismatch” is set (sender side)
- The transfer is canceled (state H)

[Return to Command index](../../)
