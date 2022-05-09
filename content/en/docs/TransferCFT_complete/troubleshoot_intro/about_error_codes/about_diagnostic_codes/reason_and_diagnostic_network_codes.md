{
    "title": "Reason  and diagnostic network codes",
    "linkTitle": "Reason and diagnostic network codes",
    "weight": "330"
}****<span id="Reason_codes"></span>Reason codes: REASON codes provide a general explanation of the error detected, or
the refusal.****

****<span id="Diagnostic_codes"></span>Diagnostic codes: DIAGNOSTIC (diag) codes provide a detailed explanation of the source
of the error detected or the refusal.****

<span id="TCP_IP_Network_codes"></span>

TCP/IP network codes
--------------------

<span id="REASON___TCP_IP_Reason_Codes"></span>

### TCP/IP reason codes

REASON corresponds to the reason code returned by the TCP/IP communication
system.

The codes are expressed in hexadecimal.

****TCP/IP Reason codes****


| Code  | Description  |
| --- | --- |
| ****00**** | Connection request rejected by the network or break caused by the remote partner |
| ****01**** | Time-out for a connection request. The called party is probably not connected to the network |
| ****02**** | Insufficient resources (other than memory) |
| ****03**** | Insufficient memory |
| ****04**** | The network access point reference passed to the connection is not valid |
| ****08**** | Invalid parameter in TCP request sent |
| ****09**** | Other cause of rejection |
| ****43**** | Invalid local or remote address |


<span id="DIAGN___TCP_IIP_Diagnostic_Codes"></span>

### TCP/IP diagnostic codes

DIAGN (TCP/IP diagnostic codes) corresponds to the diagnostic code returned
by the TCP/IP communication system.

The value of this code corresponds to the "errno" variable
provided by the TCP/IP resource. In this case, refer to the manufacturer's
documentation for the meaning of this code.
