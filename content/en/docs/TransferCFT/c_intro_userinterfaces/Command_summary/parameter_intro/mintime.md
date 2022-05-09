{
    "title": "mintime",
    "linkTitle": "mintime",
    "weight": "2000"
}<span id="mintime"></span>

#### mintime

<span id="mintime_CFTSEND"></span>

#### CFTSEND, CFTRECV

****Only used in requester
mode****

******[ MINTIME =
{ <span class="underline">00000000</span> &#124; time } ]******

Initial validity time for the transfer on
the first day (MINDATE).

Define the time as:

- Explicit: Enter an absolute time for the transfer command using the format HHMMSSCC (hours, minutes, seconds, and hundredths of a second).
- Relative: Enter a value, preceded by the plus sign (`+`), that is relative to the time the transfer command is taken into
    account. The value is in minutes with a maximum increment of `+1439` minutes (23 hours and 59 minutes).

****Example****

`MINTIME = +30` means that the transfer’s initial validity time is the time
the command is taken into account plus 30 minutes.

> **Note**
>
> Note: MINTIME cannot be equal to MAXTIME unless MAXDATE is defined and is
> not the current date.

[Return to Command index](../../)
