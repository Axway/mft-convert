{
    "title": "Scheduling script execution - CFTCRON",
    "linkTitle": "Cron jobs - CFTCRON",
    "weight": "250"
}The [CRONJOB Job Scheduler]() feature enables Transfer CFT to execute scripts at predetermined
dates and times. An example script, `cron-wlog.cmd`, is delivered in the installed product
packaging ($CFTDIRRUNTIME/exec on Unix/Windows). You can adapt this example to suit your local requirements.

See [Use processing scripts](../../../../concepts/about_transfer_processing/proc_commands) for details on script processing execution methods.

CRON commands and parameters
----------------------------

This section describes the CRON related commands and parameters.

- CFTPARM: Each CRONJOB is associated with a CFTPARM via a CRONTABS parameter (environment definition):
    -   The CRONTABS parameter of the CFTPARM object refers to the CRONTAB parameter of the CFTCRON objects.
    -   You can have a CRONTAB with the same value for different CFTCRON objects (in the example below, note that CRON1 and CRON4 refer to the same CRONTAB).
    -   **Example**
- RECONFIG TYPE=CRON
    -   This sends a notification to Transfer CFT
        to reload the enabled CRONJOBs. You use this command after modifying a CFTCRON (when either inserting or deleting).
    -   The RECONFIG command does not reload CFTPARM. If
        you modify the CFTPARM CRONTABS then you must restart Transfer CFT.
- MQUERY name=CRON
    -   Displays the log, which gives the current status of
        the enabled cronjobs.
- ACT/INACT type=CRON
    -   To activate CRON4 in the previous example:
    -   To inactivate CRON1, enter:

For CFTCRON command parameter details, see the [Command reference](../../../command_summary).
&lt;/p&gt;
<span id="CFTCRON_time_syntax"></span>

CFTCRON time syntax
-------------------

This section provides CFTCRON examples.

The following command inserts the cronjob CRON1 in the crontab CRONTAB1.
The job my_exec is executed every 10 minutes once Transfer CFT is started.
This means that the job is submitted on the minute at 0, 10, 20,
30, 40, 50 minutes of every hour.

**Example using template processing**

```
CFTUTIL CFTCRON id=CRON1, crontab=CRONTAB1, EXEC=my_exec,
time='m=\*/10'
```

**Example directly processing (UNIX and Windows)**

```
CFTUTIL CFTCRON id=CRON1, crontab=CRONTAB1, EXEC='cmd:my_exec &ID',
time='m=\*/10'
```

### Time syntax

The time syntax is case sensitive. For example, a lower case m defines
minutes, while an upper case M defines months.

- Bold characters
    are terminators
- Italic characters
    are grammar rule non-terminators
- A, b, c are integers


| Rule | Syntax | Alternate syntax |
| --- | --- | --- |
| time | time_element; time<br/> time_element |   |
| time_element | seconds= content<br/> minutes= content<br/> hours= content<br/> monthdays= content<br/> weekdays= content<br/> months= content | s= content<br/> m= content<br/> h= content<br/> D= content<br/> W= content<br/> M= content |
| content | content_elt, content<br/> content_elt |   |
| content_elt | a<br/> content_set / c |   |
|   |   | z/OS (MVS) specific syntax |
| content_set | [a:b] [a:b]<br/> [a:]<br/> [:b]<br/> * | &lt;a:b&gt;<br/> &lt;a:&gt;<br/> &lt;:b&gt; |


### Time parameter descriptions


| Symbol  | Description  | Values  |
| --- | --- | --- |
| s  | Second  | 0 to 59  |
| m  | Minute  | 0 to 59  |
| h  | Hour  | 0 to 23  |
| D  | Day of the month  | 1 to 31  |
| M  | Month  | 1 to 12  |
| W  | Day of the week  | 0 to 7 (0 and 7 both represent Sunday)  |


### Time syntax examples


| If you use this syntax… | The job is executed… |
| --- | --- |
| m=30 | Once an hour at minute 30 |
| m=1,5,40 | 3 times an hour for minutes 1, 5, and 40 |
| m=[30:40]/2 | Every 2 minutes, of every hour, between the minutes 30 and 40, this means on the minute for 30, 32, 34, 36, 38, 40 |
| m=[:40] | Every minute until, and including, the minute 40 |
| m=[30:] | Every minute starting at and including the minute 30 |
| h=6;m=30 | Every day at 06:30:00 |
| D=1;h=15;m=30 | Every first day of the month, at 15:30:00 |
| D=*/2;h=6 | Every 2 days at 06:00:00, day 1, day 3, day 5...day 31. Due to the fact that all months do not have 31 days, the following month the job will be performed 2 days from the 31st, that means on day 2, then day 4, day 6, and so on. |
| D=[1:7];W=0;h=4 | Every first Sunday of the month at 04:00:00 |
| D=-1;h=6 | Every last day of the month at 06:00:00 |
| D=[-7:-1];W=3;h=6 | Every last Wednesday of the month at 06:00:00 |
| W=0 | Every Sunday at 00:00:00 |
| W=1 | Every Monday at 00:00:00 |
| M=1 | Every January, the first at 00:00:00 |


CRONJOB messages
----------------

The possible cronjob messages are:

- CFTI36I &str:
    Information Message about cronjobs load
- CFTI38E &str:
    Error Message about cronjobs load
- CFTS37I &str:
    Information Message about cronjob submit
- CFTS39E &str:
    Error Message about cronjob submit

CRONJOB symbolic variables
--------------------------

The table below lists the symbolic variables available in the CRONJOB
procedure. Define these using the EXEC parameter of the CFTCRON command.


| Symbolic variable | Corresponding substituted value |
| --- | --- |
| &amp;CFTEVENT | Fixed name=CRONJOB  |
| &amp;CFTNAME | The PART value in the command CFTPARM  |
| &amp;SYSDATE | System date |
| &amp;SYSTIME | System time |
| &amp;SYSDAY | Day of the week |
| &amp;NSUB | Counter for all the Cronjob procedures launched |
| &amp;ID | Cronjob Identifier |
| &amp;CRONTAB | Crontab value |
| &amp;USERID | User identifier indicated in CFTCRON command<br/> On all platforms except z/OS, this variable is used for information purposes only (on z/OS the USERID performs the CRONJOB job submission). |
| &amp;COMMENT | Comment value indicated in CFTCRON command |
| &amp;PARM | Parm value indicated in CFTCRON command |
| &amp;SCOUNT | Counter for the Cronjob procedures launched for a specific Identifier |


****Related
topics****

- Command syntax **** [CFTCRON](../../../command_summary#CFTCRON)
