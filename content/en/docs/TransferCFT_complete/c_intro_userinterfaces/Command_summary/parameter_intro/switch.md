{
    "title": "switch",
    "linkTitle": "switch",
    "weight": "3450"
}<span id="switch"></span>

### switch

#### CFTACCNT

**[SWITCH = {<span class="underline">00000000</span> &#124; *time*}]**

Time at which the {{< TransferCFT/axwayvariablesComponentShortName  >}}
automatically switches to the alternate statistical file.

- ****000000**00**
    (default value)
- ****any
    other value**** in the HHMMSSCC format

When this parameter is not defined, {{< TransferCFT/axwayvariablesComponentShortName  >}} switches statistical
files daily at midnight.

#### CFTLOG

**[SWITCH = {<span class="underline">00000000</span> &#124; *time*}]**

The time at which the monitor automatically switches to the alternate
log file:

- ****00000000****
    (default value)
- ****any
    other value**** in the HHMMSSCC format

When this parameter is not defined, log files are switched daily at
midnight.

If you define a value for both ****maxrec****
and switch fields, log files are automatically switched:

- Every
    day at the time indicated
- Depending
    on the number of records in the file

See also [Housekeeping for log files](../../../../admin_intro/admin_monitoring_intro/housekeeping_logs).

 

[Return to Command index](../../)
