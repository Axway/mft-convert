---

    title: mintime
    linkTitle: mintime
    weight: 2000

---
<span id="mintime"></span>

#### mintime

<span id="mintime_CFTSEND"></span>

#### CFTSEND, CFTRECV, RECV, SEND

#### In requester mode

******\[MINTIME =
{<u>00000000</u> | time}\]******

Initial validity time of the transfer on
the first day (MINDATE).

The value ‘time’ may be expressed:

- Explicitly
    (absolute time)
- In
    SEND or RECV commands, relative to the time the command is taken into
    account

In this case, the corresponding absolute time must be less than 24 hours.
The indicated value is expressed in minutes.

****Example****

MINTIME = +30 means that the transfer initial validity time is the time
the command is taken into account plus 30 minutes. This acceptance time
must be less than 23 h 30.

MINTIME must not be equal to MAXTIME unless MAXDATE is defined and is
not the current date.

[Return to Command index](../../)
