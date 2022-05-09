{
    "title": "cycdate",
    "linkTitle": "cycdate",
    "weight": "570"
}<span id="cycdate"></span>

### cycdate

#### CFTRECV, RECV, CFTSEND, SEND

****[CYCDATE =
{see comment &#124; date}]****

Upper final date for activating
the first transfer in a cycle.

If the interval is daily or greater (TCYCLE = DAY or TCYCLE = MONTH),
the default value of CYCDATE is then MINDATE + TCYCLE\*CYCLE; if not, this
value is CYCDATE = MINDATE.

****Example****

if TCYCLE = DAY and CYCLE = 2, the default value of CYCDATE is MINDATE
+ 2 days.

The cycdate parameter forms
part of the CFTSEND and CFTRECV definitions. Use the cycdate parameter
to define a cyclic transfer request.

Specify the last day to activate the cycle of delayed transfers.

If the interval is expressed in:

- Days or months
    (TCYCLE = DAY or TCYCLE = MONTH), the default value is: CYCDATE = MINDATE + TCYCLE \* CYCLE
- Minutes (TCYCLE
    = MIN), the default value is: CYCDATE = MINDATE

 

[Return to Command index](../../)
