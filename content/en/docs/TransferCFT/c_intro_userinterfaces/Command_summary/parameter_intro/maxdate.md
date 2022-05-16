---

    title: maxdate
    linkTitle: maxdate
    weight: 1900

---
<span id="maxdate"></span>

### maxdate

<span id="maxdate_CFTRECV"></span>

#### CFTRECV, RECV

****\[MAXDATE = {see below| date}\]****

****Only
in requester mode****

Final validity date of the transfer.

The value dat’ may be expressed:

- Explicitly (absolute
    date)
- Or, in RECV commands,
    relative to the date the command is taken into account. This value is
    then expressed as a number of days.

****Example****

MAXDATE = +4 means that the transfer validity final date is 4 days after
the date the command is taken into account.

The default value is assigned by the monitor according to the transfer
context.

<span id="maxdate_CFTSEND"></span>

#### CFTSEND, SEND

\[MAXDATE = {see comment | date}\]

#### In requester mode only

Final validity date of the transfer.

#### In initiator mode only

Specifies the last day of the time slot for activating
delayed transfers.

It can be expressed:

- explicitly (absolute
    date)
- relative to the
    date the command is taken into account. This value is then expressed as
    a number of days.

The default value is assigned by the monitor depending on the transfer
context:

- If MAXTIME is greater
    than MINTIME,  
    MAXDATE = MINDATE.
- If MAXTIME is less
    than MINTIME,  
    MAXDATE = MINDATE+1.
- If MAXTIME is omitted,  
    MAXDATE = 99991231.

****Example****

MAXDATE = +4 means that the transfer validity final date is 4 days after
the date the command is taken into account.

[Return to Command index](../../)
