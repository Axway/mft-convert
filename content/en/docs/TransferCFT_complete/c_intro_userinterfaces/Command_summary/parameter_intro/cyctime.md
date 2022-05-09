{
    "title": "cyctime",
    "linkTitle": "cyctime",
    "weight": "590"
}<span id="cyctime"></span>

### cyctime

#### CFTSEND, SEND, CFTRECV, RECV

**[CYCTIME = {<span class="underline">see comment</span> &#124; *time*}]**

The cyctime parameter forms
part of the CFTSEND and CFTRECV definition. Use the cyctime parameter
to define a cyclic transfer request. A cyclic transfer request is a recurring, periodic
transfer request.

The period (time between two transfer activations) is defined by the
cycle and tcycle parameters in the CFTSEND object. This period can be
expressed in minutes, days, or months.

The validity time slot for this request is defined by the mindate/mintime
and maxdate/maxtime parameters and corresponds to the global time during
which the transfers are periodically repeated.

To complete this parameter, enter the initial activation time for the
cycle of delayed transfers.

If the interval is expressed in:

- days or months
    (tcycle = day or tycycle
    = month), the default value is cyctime
    = mintime.
- minutes (tycycle
    = min), the default value is cyctime
    = mintime + tcycle \* cycle.

Upper limit time for activating the first transfer of a cycle.

For the subsequent transfers, the final date/time is calculated as follows:
CYCDATE.CYCTIME + CYCLE \* TCYCLE (period value).

If the interval is expressed in minutes (TCYCLE = MIN), the default
value of CYCTIME is then: MINTIME + TCYCLE\*CYCLE; if not, CYCTIME = MINTIME.

Example:

if TCYCLE = MIN and CYCLE = 60, the default value of CYCTIME is MINTIME
+ 1 hour.

[Return to Command index](../../)
