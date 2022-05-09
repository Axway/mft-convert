{
    "title": "RECV - Receive files",
    "linkTitle": "Using the RECV command",
    "weight": "170"
}This command is used to request the reception of files from a designated partner.

> **Note**
>
> Note: When using the PeSIT protocol, you cannot create a request for messages or replies.

The RECV command is executed at the level of a receiver/requester (requester because it initiated the connection). The partner is therefore the sender/server.

The characteristics of a receive transfer request can be specified in the RECV command itself or in the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} flow definition, where the IDF is equal to the flow IDENTIFIER itself, or in the CFTRECV command, the ID of which is equal to the value of the IDF parameter.

When receiving files you can define parameters related to the following categories.

Free parameters
for the {{< TransferCFT/axwayvariablesComponentShortName  >}} user, such as adding comments to the transfer:

- Sent to the
    sender
- Local parameters

Execution control
parameters, such as specify the scheduling time for a transfer:

- General
- User
- Schedule management

<!-- -->

- Data processing
    parameters

Parameters associated
with the file received, for example the file name:

- File management
- Physical name
- Physical characteristics
    (whole file)
- Physical characteristics
    (records)
