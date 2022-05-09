{
    "title": "Client and server recommendations",
    "linkTitle": "Client and server recommendations",
    "weight": "230"
}This section provides recommendations and examples based on:

- [Client mode outgoing transfers](#Client)
- [Server mode incoming transfers](#Server)
- [Impact on scheduling (reschedule a transfer)](#Impact)

<span id="Client"></span>

Client mode
-----------

If the Transfer CFT instance is mainly used for outgoing transfers it is recommended to set the MAXTRANS to an equal or lower value than the MAXCNX. In fact, Transfer CFT is quite efficient if its transfer scheduler is rejected at the transfer activation level rather than at the connection level.

Rejected transfers are scheduled immediately after completed transfers to provide more consistent performance.

**Example**

If your MAXCNX is lower than MAXTRANS and you have more transfers than MAXCNX, any excess transfers are rescheduled in WSCAN minutes. Depending on the number of sessions available (closed) that same number of transfers are processed and any excess transfers wait again for WSCAN minutes.

Transfers having a MAXCNX DIAG are rescheduled in WSCAN minutes (CFTCAT object). In WSCAN minutes, if the session is active, the transfer is not activated. In this case, the transfer waits another WSCAN minutes until the session is inactive, at which point it is activated. The session still remains active during DISCTD or DISCTS (on the remote partner) seconds after a transfer is completed.

```
Partner    DTSAPP  File                Diags
-------- ------ -------- --------
DESTSUN SFH TH TEST2 F1517083 0 0 0 DIFFUS
SUN35-1 SFT XX TEST2 F1517084 14 14 0 CP NONE
SUN35-2 SFT XX TEST2 F1517085 14 14 0 CP NONE
SUN35-3 SFT XX TEST2 F1517090 14 14 0 CP NONE
SUN35-4 SFT XX TEST2 F1517091 14 14 0 CP NONE
SUN35-5 SFD TD TEST2 F1517092 0 0 416 MAXCNX
```
<span id="Session"></span>

### Session persistence

This section describes the effect of setting either a high or especially low value for session persistence.

#### Setting to low value

You may want to use a low DISCTD value when you have a lot of partners and you reach or exceed the MAXTRANS number of transfers. For example, if there is a central hub diffusing to a large distribution list, this allows a session to close so that you can quickly establish a new session for another partner.

> **Note**
>
> Caution  
>  Setting DISCTD to 0 (zero) creates an infinite timeout, and does not indicate zero seconds.

#### Setting to a higher value

You may want to use a high DISCTD to keep the session active when you have a lot of small transfers with the same partners, in order to keep session open with these partners.

### Determine the number of parallel transfers

Determine the typical daily peak number of incoming and outgoing transfers in parallel, where we assume that most transfers in client mode are outgoing.

- If the value is less than 999 (the MAXTRANS max value), for example 600, then set the MAXTRANS and MAXCNX to the value you determined as the peak; the same value for each (you would set the value to 600).
- If the value is between 999 and 2000, for example 1200, then you should set MAXTRANS to 999 and MAXCNX to 1200.
- If the value exceeds 2000, for example 3000, set MAXTRANS to 999 and MAXCNX to 2000.

<span id="Server"></span>

Server mode
-----------

If you use the Transfer CFT mainly for incoming transfers, it is recommended that you set the MAXTRANS to a greater value than (or equal to) the sum of all configured MAXCNX (in the case where you have multiple CFTNET objects).

When an incoming session is established, an incoming transfer can be executed immediately because it is not waiting on a session.

If MAXCNX is greater than MAXTRANS, and you already have MAXTRANS active session, then the next transfer is rejected with a MAXTRANS 416 DIAG. The transfer is then rescheduled by the requester, depending on requester WSCAN value (CFTCAT object).

### Session persistence

#### Setting to low value

When you have a lot of different partners, in general you may want to set DISCTS to a low value, for example 10 seconds, in order to free up connections more quickly. Remember though that normally the client side DISCTD should be a lower value.

In a situation where all sessions are active, and no additional sessions are available:

- All additional incoming connection are rejected
- A new connection can be established when at least one transfer completes and the session has closed
- In this particular scenario where all sessions are active, setting DISTCS to a high value negatively impacts performance due to the effect on latency

> **Note**
>
> Caution  
>  Setting DISCTD to 0 (zero) creates an infinite timeout (and does not indicate zero seconds).

#### Setting to a higher value

If you are receiving a lot of small files from the same partners, you may want to set DISCTS to a higher value (for example the default value of 165 seconds). To optimize performance, this setting must be defined in conjunction with the remote partner's DISCTD.

For each small transfer the time to establish a session may be equivalent to the time of the transfer itself. In this case, keeping a session open with a partner for an extended period allows you to benefit from an open session and not lose time on reestablishing partner sessions.

### Determine the peak number of parallel transfers

Determine the typical daily peak incoming and outgoing rate where we assume that most connections are incoming connections.

- If the value is less than 999, for example 500, (the MAXTRANS max value) then set the MAXTRANS and MAXCNX to the value you determined as the peak (in this case 500).
- If the value is between 999 and 2000, for example 1200, then you should set MAXTRANS to 999 and MAXCNX to 999.
- If the value exceeds 2000, for example. 3000, set MAXTRANS to 999 and MAXCNX to 999.

<span id="Impact"></span>

Impact on scheduling
--------------------

In addition to simply looking at the maximum number of sessions, connections, and transfers, it is important to take into consideration how excess transfers are treated.

The following table shows the impact on scheduling once Transfer CFT reaches a file transfer parameter limit.


| Reached limit for the ...  | DIAGI - DIAGP  | Reschedules...  | Conditions to be executed..  |
| --- | --- | --- | --- |
| Local MAXTRANS  | 0  | Immediately after a transfer is completed  | A transfer slot is available on the local site  |
| Remote MAXTRANS  | 916-RCO 201  | In WSCAN minutes(CFTCAT)  | When a session is available on the remote site  |
| Local MAXCNX  | 416-MAXCNX  | In WSCAN minutes (CFTCAT)  | When a session is available on the local site  |
| Remote MAXCNX  | 302-R 0 2F2  | In RETRYW minutes (CFTTCP)  | When a session is available on the remote site  |
| Local CNXINOUT or CNXOUT | 418-MAXCV  | Immediately after a transfer with the partner is completed  | When a transfer slot for this partner is available locally  |
| Remote CNXIN<br/> or CNXINOUT | 916-RCO 309  | Immediately after a transfer with the partner is completed  | When a transfer slot for this partner is available on the remote site  |


### Scheduling issues

In some cases you may find that your Transfer CFT instance is not configured using the recommended settings. While these configurations are supported, they may provide less than top performance. The following section describes typical "unconventional" configurations and the possible consequence.

#### WSCAN is set to too high of a value

Transfers are only scheduled after WSCAN minutes, so this value can negatively affect either a remote MAXTRANS or a local MAXCNX.

> **Note**
>
> Note: The default value is 5, and should in almost all cases be modified. We recommend that you modify the value to 1. Not changing the value could lead to latency issues for either remote MAXTRANS local MAXCNX. See table above.

#### DISCTS/DISCTD value is set to zero

You should not set this parameter to 0 (zero), as that indicates an infinite session.

#### Local DISCTD/remote DISCTS and remote DISCTD / local DISCTS value is too high

A potential issue when set to too high of a value is that incoming/outgoing sessions are still active so new sessions might be rejected due to session persistence. Additionally this could contribute to latency issues, as the transfers are rescheduled after WSCAN minutes (meaning that they are not immediately scheduled).

#### RETRYW

If RETRYW minutes are set to a high value, this has an impact on when the remote MAXCNX is reached. This can affect performance as transfers are rejected and then rescheduled in RETRYW minutes (for example, the default value is 7 minutes).

> **Note**
>
> Note: A good rule of thumb is to set this value to 1.

### Negative impact on transfer times

#### Using low value for MAXTRANS

If you have a low value, take into consideration that there is always a transfer slot reserved for an incoming session. This means that if the parameter is set to 3, you will only have 2 available "slots" for incoming transfers.

#### Using the default CNXOUT value

In most setups we recommend that you modify the CNXOUT default setting. Keeping the default value could have a negative impact on latency.

****Related topics****

- [About parallel transfers](../)
- [FAQ and troubleshooting](../faq)
