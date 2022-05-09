{
    "title": "Transfer  scheduling",
    "linkTitle": "Transfer scheduling",
    "weight": "240"
}This topic describes transfer requests and transfer scheduling.

Transfer requests
-----------------

Transfer requests can accumulate in the Transfer CFT catalog. Sometimes
the transfers are not activated immediately.

A transfer is not activated
under the following circumstances:

If you are in the preprocessing phase and a script exists:

- It is in the **H** state or **K** state as a result of a preprocessing error
- If there is no END command with ISTATE=N
- If it is in the **D** state and is waiting for the scheduled time

Transfer phase:

- If it is in the
    ****H**** HOLD state or ****K****
    KEEP state as a result of a transfer error, or an operator command (HALT,
    KEEP, or SEND STATE = HOLD, and so on).
- If it is in the
    ****D**** DISP state, but Transfer CFT
    does not have the resources required to activate it.
    -   Or if the transfer
        cannot be activated at that time because there is a MINTIME or MINDATE requirement.
    -   Or if Transfer CFT is already processing
        MAXTRANS active transfers.

The sequencing of Transfer CFT transfers in REQUESTER mode, in which
Transfer CFT establishes the connection with a remote partner, is governed
by **activation priority**. When
scanning a list of pending transfers, the transfer having the highest
priority is activated first. If all transfers have the same priority,
the first transfer placed in the catalog is the first one activated.

Transfer scheduling
-------------------

Once a transfer request is available for scheduling, it moves to either an inactive or active transfer state.

### Transfer states

#### Inactive transfers

A transfer is set to inactive
if its diagnostic status is 0, and its mintime/mindate is not reached. In that
case, the transfer is placed into a mintime ordered queue and waits to
be activated.

#### Active transfers

If a transfer does not fit the inactive state criteria, the transfer
is set to active. Active transfers are executed if resources are available, otherwise they are placed in a queue for execution. Parameters that affect scheduling are, in order:

- Priority (PRI)
- The scheduled time and date (MINTIME/MINDATE)
- Request time

### Partner states

A partner can have one of the following three states:

- Ready
- State-locked
- Time-locked

#### Ready partners

A **ready** partner is one that
is placed into a queue organized by priority of the first active transfer.
Transfers are taken from this partner queue. As soon as a transfer completes,
the partner is re-organized or removed. By default, a partner is set to
the ready state.

#### Time-locked partners

A time-locked partner is put
into a queue that is organized by the mintime for that partner. When that
mintime expires, the partner is removed from the time-locked
queue and is put in the ready queue.

When the scheduler schedules a transfer with a non-null diagnostic,
it puts the related partner into time-locked state (except in the case
of an INACT 430 diagnostic).

Each partner updates its mintime to the transfer time to which is added
the +/-50% of the waiting delay. The transfer mintime is set to its reqtime,
to keep the correct order.

#### State-locked partners

A state-locked partner is put aside to wait for an event to reactivate
it. Currently, only the diagnostic INACT (430) causes this state. The
partner is reactivated by the CFTUTIL ACT command.

### Scheduler states

#### Going from inactive to active

Every time a transfer is requested, the scheduler moves all inactive
transfers that are ready to go in to an active state, that is, into an
ordered queue for this partner.

#### Unlocking time-locked partners

The scheduler puts every partner that became ready from the time-locked
queue into the ready queue only when no more partners remain in the ready
queue.

<span id="CFT_resource_limitations"></span>

Transfer CFT resource limitations
---------------------------------

The MAXTRANS parameter, in the CFTPARM object, sets the Transfer CFT
’s maximum number of simultaneous transfers.

For each network resource known to Transfer CFT, in the CFTNET object,
the CALL parameter defines whether it can be taken for an incoming or
outgoing connection or both. The MAXCNX parameter also sets the maximum
number of simultaneous connections for this resource.

When Transfer CFT saturates in terms of available resources, the transfers
which can be activated remain in the D state, pending the freeing of a
resource, for as long as the MAXTIME time is not reached.

When Transfer CFT saturates in terms of available resources, meaning that the maximum number of simultaneous transfer or the maximum number of connection has been reached, all additional transfers remain in the D state pending the freeing of a
resource until the timeout is reached.

<span id="Session_timers_and_holdover"></span>

### Session timers and holdover

The expected network events are timed by the RTO and DISCT\* parameters
of the CFTPROT objects. These parameters are used to set the wait time-out
before disconnection and during connection. Of particular note are DISCTS
and DISCTD which are used to maintain a connection with a partner, on
completion of a transfer, for the next transfer, if any, with the same
partner.

The **Disconnect timeout** parameter is used to set the wait time-out
before disconnection and during connection. The **Keep alive** parameter is used to maintain a connection with a partner upon
completion of a transfer, for the next transfer with the same
partner.

<span id="Delayed_transfers"></span>

Delayed transfers
-----------------

In addition to the call
limitation, which can be imposed on partners, the user can also define time slots for activating a given transfer.
This section describes delayed transfers and how to define these:

- In the description
    of the model type to be sent or received: CFTSEND and CFTRECV objects
- In the transfer
    request: SEND and RECV

A request made before the time slot for activating the transfer will
wait to be activated for the specified date and time remaining in the
D, at disposal, state. If the request cannot be processed by the Transfer CFT
before the end of the set time slot, it can no longer be activated. A
request made after the time slot for activating the transfer time out
can no longer be activated and changes to the ****K****
Keep state.

This transfer activation time slot is defined using the MINTIME/MINDATE
and MAXTIME/MAXDATE parameters of the CFTSEND/SEND, CFTRECV/RECV commands.

The values of these parameters can be defined either:

- Explicitly
- Relatively
    -   Relatively indicates that the calculation is performed at the time the request is entered in the
        catalog.
    -   A relative value for each of these parameters is defined using the following
        syntax:  
                  parameter = + &lt;relative
        value&gt;
    -   The ****+**** sign indicates that the
        value is a relative value. If the parameter is the "time"
        type, the value is expressed in minutes, and must not exceed 24 (hours)
        less one minute, in other words, the value 1439. If the parameter is
        the "date" type, the value is expressed as a number of days,
        and must not exceed 365.

### Example 1

```
SEND PART=PART,IDF=IDFA,FNAME=FILEA, MAXDATE=+10
```

The request is valid for up to 10 days, beginning at the time of the catalog entry.

```
SEND PART=PART,IDF=IDFA,FNAME=FILEA, MAXTIME=+30
```

The request is valid today, for half an hour, beginning at the time of the catalog entry.

When the time slot exceeds a day, the value of MAXDATE is always greater
than the value of MINDATE.

### Example 2

```
MINDATE = 20151224
MINTIME = 1800
MAXDATE = 20160101
MAXTIME = 0900
```

The transfer can be activated between December 24, 2015 at 6 pm and January
1, 2016 at 9 am.

When this slot is less than 24 hours, it can be included within one
day or over two days.

To specify a time slot within one day, the values of the MINDATE and
MAXDATE parameters must be equal and the value of MINTIME must be less
than the value of MAXTIME.

### Example 3

```
MINDATE = 20151224
MINTIME = 0900
MAXDATE = 20151224
MAXTIME = 1800
```

The transfer can be activated on December 24, 2015 between 9 am and 6 pm.

If you want to specify a time slot that spans into the next day, set the value of MINTIME to later
than the value of MAXTIME. The MINDATE value specifies
the first day. It is not necessary to define the MAXDATE value, as Transfer CFT automatically assigns it to the day following the MINDATE value.

### Example 4

```
MINDATE = 20151224
MINTIME = 2200
MAXDATE = 20151225
MAXTIME = 0500
```

is equivalent to:

```
MINDATE = 20151224
MINTIME = 2200
MAXTIME = 0500
```

The transfer can be activated between December 24, 2015 at 10 pm and December
25, 2015 at 5 am.

Default value
of:

- MINDATE: current
    date
- MAXDATE:
- -   If MAXTIME
        is greater than MINTIME : MINDATE
    -   If MAXTIME
        is less than MINTIME : MINDATE + 1
    -   If MAXTIME
        is not defined: 99991231

### Example 5

```
MINTIME = 2200
MAXTIME = 2300 (MAXDATE and MINDATE not defined)
```

The transfer can be activated today between 10 pm and 11 pm.

```
MINTIME = 2200
MAXTIME = 0500 (MAXDATE and MINDATE not defined)
```

The transfer can be activated between today at 10 pm and tomorrow at 5
am.
