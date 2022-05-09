{
    "title": "Transfer records in the catalog",
    "linkTitle": "Transfer records in the catalog",
    "weight": "150"
}Transfer commands are used to initiate the sending or receiving of files,
or the sending of messages, with a partners. The
records of these transfer commands specify the:

- Filename or message
    text
- Transfer characteristics
- Partner

All transfer requests, either SEND or RECV, are recorded and saved in
the Transfer CFT transfer CATALOG.

A catalog record, known as a **catalog
entry**, is identified by the:

- Transfer direction:
    Send or Receive
- Type of object
    transferred: File, Message, or Reply
- Partner name: PART
- Transfer identifier:
    IDT

All the information required for the correct execution of the transfer
is indicated in the catalog.
