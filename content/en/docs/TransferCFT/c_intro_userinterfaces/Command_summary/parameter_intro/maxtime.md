---

    title: maxtime
    linkTitle: maxtime
    weight: 1950

---
<span id="maxtime"></span>

### maxtime

<span id="maxtime_CFTRECV"></span>

#### CFTRECV, CFTSEND, SEND, RECV

**\[MAXTIME = {<u>23595999</u> | *time*}\]**

****Only taken into account in requester
mode****

Transfer validity limit time for the final date (MAXDATE).

The value time may be expressed:

- Explicitly
    (absolute time)
- Or,
    in RECV or SEND commands, relative to the time the command is taken into
    account

In this case, the corresponding absolute time must
be less than 24 hours. The indicated value is expressed in minutes.

MAXTIME must not be equal to MINTIME except if MAXDATE is defined and
is not the current date.

#### START

**\[MAXTIME = {+<span class="italic_in_para">n</span>}\]**

<span class="code">`Maxtime `</span>is used in the <span class="code">`START `</span>command to indicate a relative transfer validity time limit. This means the time that the command is taken into account plus <span class="italic_in_para">n</span> minutes. The <span class="code">`maxtime`</span> value is between 1 and the number of minutes in a day minus one, (60 x 24) – 1, and must be less than 24 hours.

****Example****

```
start part=part1,maxtime=+10
```

****Example****

<span class="code">`MAXTIME = +180`</span> means that the maximum time limit for a transfer to be valid is the time from which the command is taken into account plus 180 minutes. The acceptance time for the transfer command must be less than 24 hours.

[Return to Command index](../../)

####  
