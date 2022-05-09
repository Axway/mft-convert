{
    "title": "Requester and server modes",
    "linkTitle": "Transfer steps",
    "weight": "170"
}Transfer CFT mechanisms differ depending on if the Transfer CFT is operating in
requester mode or server mode, as mentioned in the [Introduction](). This section describes the transfer steps
in relation to each mode.

- In
    requester mode, Transfer CFT initiates the connection with the partner
    following a local request.
- In
    server mode, Transfer CFT replies to an incoming connection, re-sent
    by the system communication layers.

For details on using these modes in a Central {{< TransferCFT/suitevariablesGovernance  >}} setting, please refer to the Central {{< TransferCFT/suitevariablesGovernance  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}.

Requester mode overview
-----------------------

Transfer CFT is able to send and receive files, and send messages. Although
the receiver file can be dynamically created, directories or libraries
must already exist. The receiver file, and the directories or libraries can be created dynamically.

Steps include:

1. **Register the request**: Transfer CFT processes each transfer request in the catalog.
1. **Activate the transfer**: Transfer CFT activates all pending
    transfers, which are entries having a "ready for execution" state.
1. **Connect to the network**: When Transfer CFT has a "ready for execution" transfer,
    it attempts to make the connection to the communication system attached
    to the partner of this transfer.
1. **Exchange protocol information**: When the network connection is established, the partners participate in an exchange
    of messages in accordance with the chosen protocol.
1. **Select or create the file**: Depending on if you are the sender or the receiver, once the partner is recognized and the protocol is defined the file is selected or created.
1. **Transfer data**: The data in the file is transferred item by item.
    The exchange protocol may include compression functions.
1. **Disconnect**: On completion of data transfer, the files are closed, the network connection
    is cut after the expiration of the associated hold timeout.

Server mode overview
--------------------

Transfer CFT accepts transfer requests from the
network and then operates in server mode.

Steps include:

1. **Receive an incoming call**: {{< TransferCFT/axwayvariablesComponentLongName  >}} receives an incoming connection on the communication system, which could be rejected depending on {{< TransferCFT/axwayvariablesComponentLongName  >}} limitations and/or parameter settings.
1. **Exchange protocol information**: When the network connection is established, the partners participate in an exchange
    of messages in accordance with the chosen protocol.
1. **Create or select a
    file**: Once the recognition phase is complete, Transfer CFT receives a request
    to create a file (receive) or select a file (send).
1. **Transfer data**: The data in the file is transferred item by item.
    The exchange protocol may include compression functions.
1. **Disconnect**: On completion of data transfer, the files are closed, the network connection
    is cut after the expiration of the associated hold timeout.

********Example: Sending in requester mode********

![](/Images/TransferCFT/temp_session1.png)

******** ********

********Example: Receiving in requester mode********

![](/Images/TransferCFT/temp_session3.png)
