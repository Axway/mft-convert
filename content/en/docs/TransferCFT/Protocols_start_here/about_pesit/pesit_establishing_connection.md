{
    "title": "Transfer concatenation ",
    "linkTitle": "Establishing a connection",
    "weight": "220"
}The figure below shows a concatenation of one-way send transfers. If
the time interval between two requests remains below the values of the
time-out parameters DISCTD (at the requester end) and DISCTS
(at the server end), the transfers are able to be concatenated over the
same protocol connection.

Once these time-outs have expired, the protocol connection is broken
by the first partner whose time-out limit is reached. Another session
therefore has to be established to respond to a further transfer request.
It is consequently best to correctly set up the wait time-outs between
transfers to optimize the use of network resources and avoid performance
being degraded by the systematic establishing of a protocol connection.

Time-out role

![Protocol session time out role betweeen a requester and server ](/Images/TransferCFT/Timeout_role3.gif)

#### PeSIT CFT/CFT

The following two diagrams show how the REVERSE command is used to concatenate
transfers in different directions over the same protocol connection. This
parameter is only taken into account in requester mode. If REVERSE = YES
is specified, concatenation is possible provided that the time interval
between two successive transfer requests does not exceed the configured
wait time-outs. If REVERSE = NO is specified, a protocol connection is established
for each transfer.

It should be noted that a transfer only uses an established connection
for the same pair of requester and server identifiers. In particular,
a requester cannot request a transfer with its partner over a connection
for which it was initially server.

One-way protocol connection

![One way protocol session connection between requester and server](/Images/TransferCFT/One_way_protocol_connection.gif)

Two-way protocol connection

![Two way protocol session connection between requester and server](/Images/TransferCFT/Two_way_protocol_connection.gif)
