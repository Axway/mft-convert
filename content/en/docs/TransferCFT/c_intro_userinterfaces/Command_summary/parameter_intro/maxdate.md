{
    "title": "maxdate",
    "linkTitle": "maxdate",
    "weight": "1900"
}<span id="maxdate"></span>

### maxdate

<span id="maxdate_CFTRECV"></span>

#### CFTRECV

****Only used
in requester mode****

****[MAXDATE = { <span class="underline">99991231</span> &#124; date }]****

Final validity date for the transfer.

Define the date as:

- Explicit: Enter the absolute
    date using the format YYYYMMDD (year, month, day).
- Relative: Enter a value, preceded by the plus sign (`+`), that is
    relative to the date the command is taken into account. The value is
    expressed in days.

****Example****

`MAXDATE = +4 `means that the final validity date for the transfer is 4 days after
the date the command is taken into account.

<span id="maxdate_CFTSEND"></span>

#### CFTSEND, SEND

**[MAXDATE = {see comment &#124; date}]**

#### In requester mode only

Final validity date of the transfer.

#### In initiator mode only

Specifies the last day of the time slot for activating
delayed transfers.

Define the date as:

- Explicit: Enter the absolute
    date using the format YYYYMMDD (year, month, day).
- Relative: Enter a value preceded by the plus sign (`+`), which is
    relative to the date the command is taken into account. The value is
    expressed in days.

The default value is assigned by the Transfer CFT depending on the transfer
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
