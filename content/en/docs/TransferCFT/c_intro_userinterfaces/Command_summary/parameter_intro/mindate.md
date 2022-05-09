{
    "title": "mindate",
    "linkTitle": "mindate",
    "weight": "1980"
}<span id="mindate"></span>

### mindate

<span id="mindate_CFTRECV"></span><span id="mindate_CFTSEND"></span>

#### CFTRECV, CFTSEND

****Only used in requester
mode****

**[MINDATE = { <span class="underline">10000101</span> &#124; date }] **

Minimum, or initial, transfer validity date.

Define the date as:

- Explicit: Enter the absolute
    date using the format YYYYMMDD (year, month, day).
- Relative: Enter a value, preceded by the plus sign (`+`), that is  relative to the date the command is taken into account. The value is
    expressed in days.

****Example****

`MINDATE = +2` means that the initial validity date for the transfer is 2 days
after the date the command is taken into account.

[Return to Command index](../../)
