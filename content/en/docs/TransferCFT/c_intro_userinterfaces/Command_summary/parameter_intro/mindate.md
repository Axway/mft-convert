---

    title: mindate
    linkTitle: mindate
    weight: 1980

---
<span id="mindate"></span>

### mindate

<span id="mindate_CFTRECV"></span><span id="mindate_CFTSEND"></span>

#### CFTRECV, CFTSEND, SEND, RECV

****Only taken into account in requester
mode****

**\[MINDATE = {current system date | date}\] **

Minimum transfer validity date.

The value ‘date’ may be expressed:

- Explicitly
    (absolute date)
- Or,
    in RECV or SEND commands, relative to the date the command is taken into
    account. This value is then expressed as a number of days.

****Example****

MINDATE = +2 means that the transfer initial validity date is 2 days
after the date the command is taken into account.

[Return to Command index](../../)
