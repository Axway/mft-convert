{
    "title": "RECOV: General  network error recovery codes",
    "linkTitle": "General network error recovery codes",
    "weight": "350"
}<span id="RECOV___General_Network_Error_Recovery_Codes"></span>RECOV corresponds to the code common to all network access methods,
providing a general indication about the cause of the error.

One value can correspond to two causes, depending on whether the source
of the error is at the local or remote end; "L" is indicated
in case of ambiguity:

- L=1:
    local
- L=0:
    remote

![](/Images/TransferCFT/temp_network.png)

The codes are expressed in decimal form.

RECOV - General Network Error Recovery Codes


| Code &lt;/th&gt;  | Meaning &lt;/th&gt;  |
| --- | --- |
| 1 | Normal remote end disconnection |
| 2 | L=1 Local time-out<br/> L=0 Network time-out |
| 3 | L=1 Insufficient local resources<br/> L=0 Protocol procedure error |
| 4 | No more contexts available |
| 5 | Incoming connection request while the maximum number of sessions (MAXCNX) for this resource has been reached |
| 9 | Other non-fatal problems |
| 64 | Invalid call syntax |
| 67 | Incorrect remote address |
| 68 | Incorrect local address |
| 99 | The resources are temporarily unavailable |
| 128 | Malfunctions on the network |
|   | Undefined refusal reason |

