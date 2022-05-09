{
    "title": "Network resource depletion prevention (NRDP)",
    "linkTitle": "Network resource depletion prevention (NRDP)",
    "weight": "270"
}Configuring more network connection than simultaneous file transfers is not always enough to ensure that the MAXTRANS transfers can proceed simultaneously at any given time.

The principal reasons for this are:

1. File transfers are performed over protocol sessions.
1. Each protocol session uses a network connection.
1. Session termination requires exchanges, equaling time, over the associated network connection.
1. Establishing protocol sessions can be very costly (TLS for example).
1. The Transfer CFT requester side tries to start all active transfers up to the MAXTRANS value regardless of the actual number of network connections used.

****Consequently****

- Due to reasons 1 and 2, the number of sessions is at least equal to the number of simultaneous transfers.
- Due to reason 3, there can be intervals of time where sessions with no active transfer exists. Let's call such sessions "idle" sessions. This sometimes leads to a number of sessions greater than the number of transfers.
- Due to reason 4, it is advisable to use sessions for more that a single transfer when possible. To enable this sort of reuse, sessions are not terminated immediately after a transfer ends but instead after a configurable delay. These idle sessions can then be reused for subsequent transfers. This can lead to a number of established sessions that is significantly larger than the number of active transfers when the number of idle sessions is high.

When the network resource depletion prevention (NRDP) feature is enabled ([below](#How)), Transfer CFT detects the situations where existing idle sessions may prevent new transfers that require new sessions from being started. In such situations, as a precaution Transfer CFT gently closes these idle sessions in order to allow the new transfers to be started quickly.

This feature is only effective when you frequently reach the number of simultaneous connections (MAXCNX) while the actual number of transfers is less than the MAXTRANS. In this case, CFTN09E log messages can flood the log and the Transfer CFT behavior may be adversely affected.

****Example error message****

`CFTH09E Network connect request local error <PART=PARIS0457 NCR=416 NCS=MAXCNX NET=TCP>`

And the corresponding catalog entry  would include the DIAGI=416 and DIAGP=MAXCNX.

<span id="How"></span>

How to configure
----------------


| UCONF value  | Type  | Default  | Description  |
| --- | --- | --- | --- |
| cft.server.nrdp.enable  | boolean  | No  | Enable the prevention of network resource depletion.  |
| cft.server.nrdp.conn_retry_delay_min  | int  | 1  | Connection retry delay minimum in seconds when a network resource depletion is detected.  |
| cft.server.nrdp.conn_retry_delay_max | int  | 5  | Connection retry delay maximum in seconds when a network resource depletion is detected.  |


### Parameter usage

The NRDP feature only applies to the TCP/IP network, and requires the configuration of a CFTNET object for a given class that allows OUTGOING, or both OUTGOING and INCOMING, connections where MAXCNX &gt;= MAXTRANS.

- A MAXCNX value significantly larger than MAXTRANS can be advantageous, especially if the number of INCOMING connections is high.
- A MAXCNX value 10% to 20% larger than MAXTRANS should be adequate in most situations.

### Log messages

The [related messages](../../../troubleshoot_intro/messages_and_error_codes_start_here/cftn_messages) only display in the log when the `cft.server.nrdp_enable=yes`.

- `CFTN05I Network resource depletion prevention enabled for class %d`
- `CFTN06I No network class suitable for resource depletion prevention activation`

### Calculating the threshold

Given the value of MAXTRANS and the MAXCNX for a selected CFTNET object, the threshold for network capacity is calculated as follows:

`threshold = (MAXCNX+MAXTRANS)/2`

****Example****

This example uses the following values and shows the different outcomes with and without NRDP enabled. In this example, we have 10 new transfers to perform that require new sessions to be opened (for example, with partners that do not already have an established session).

- `MAXCNX `= 450 and `MAXTRANS `= 400, therefore `THRESHOLD`=425
- `NBCNX `is the current number of open sessions = 450
- `NBTRANS `is the number of active transfers = 300

****NRDP not enabled****

Because the number of sessions would exceed the `MAXCNX `if a new session is established, the 10 new transfers must wait for a network resource to become available (a `416 MAXCNX`  message displays).

****NRDP enabled****

With NRDP enabled, the 10 new transfers are processed (idle sessions are reused). As a precaution, Transfer CFT closes idle sessions in order to reduce `NBCNX `and defer the network connection request for new sessions until` NBCNX <= THRESHOLD`.
