{
    "title": "maxtime",
    "linkTitle": "maxtime",
    "weight": "1950"
}<span id="maxtime"></span>

### maxtime

<span id="maxtime_CFTRECV"></span>

#### CFTRECV, CFTSEND

****Only used in requester
mode****

**[MAXTIME = {<span class="underline">23595999</span> &#124; *time*}]**

Final validity time limit for the transfer on the final date (MAXDATE).

Define the time as:

- Explicit: Enter an absolute time for the transfer command using the format HHMMSSCC (hours, minutes, seconds, and hundredths of a second).
- Relative: Enter a value, preceded by the plus sign (`+`), that is relative to the time the transfer command is taken into
    account. The value is in minutes with a maximum increment of `+1439` minutes (23 hours and 59 minutes).

> **Note**
>
> Note: MAXTIME cannot be equal to MINTIME except if MAXDATE is defined and
> is not the current date.

#### START

**[MAXTIME = {+n}]**

`Maxtime `is used in the `START `command to indicate a relative transfer validity time limit. This means the time that the command is taken into account plus n minutes. The `maxtime` value is between 1 and the number of minutes in a day minus one, (60 x 24) – 1, and must be less than 24 hours.

****Example****

```
start part=part1,maxtime=+10
```

****Example****

`MAXTIME = +180` means that the maximum time limit for a transfer to be valid is the time from which the command is taken into account plus 180 minutes. The acceptance time for the transfer command must be less than 24 hours.

[Return to Command index](../../)

####
