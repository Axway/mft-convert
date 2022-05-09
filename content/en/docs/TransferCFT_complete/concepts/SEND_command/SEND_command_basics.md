{
    "title": "SEND - Send files",
    "linkTitle": "Sending files",
    "weight": "180"
}The SEND command initiates a send transfer. You can use it to send files, messages, or replies (acknowledgments). You can define the characteristics in the SEND command itself, or in a Central {{< TransferCFT/suitevariablesGovernance  >}} flow definition in a template command. Additionally you can use the SEND command for a single partner or a list of partners.

About sending files
-------------------

When sending files:

Free parameters
for the {{< TransferCFT/axwayvariablesComponentShortName  >}} user, such as adding comments to the transfer:

- Sent to the
    receiver
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
with the file sent, for example the file name:

- File management
- Physical name
- Physical characteristics
    (whole file)
- Physical characteristics
    (records)

File parameters
for the partner, for example the remote file name:

- Physical name
- Physical characteristics
    (whole file)
- Physical characteristics
    (records)

Sending messages
----------------

The SEND TYPE = MESSAGE command is used to send a message to the designated
partner. You can, as with files, define local parameters and execution control
parameters such as scheduling.

The maximum length for a message is 512 characters.

Sending replies
---------------

When sending a reply it is similar to sending a message, but there are two types of replies - acknowledgments and negative acknowledgement. This type of SEND is always linked to a transfer.
