---
    title: "LISTLOG - List the log content "
    linkTitle: "LISTLOG - List log content"
    weight: 320
---This topic describes how to use the LISTLOG command to display the log content according to certain defined criteria, such as date or node. Additionally, you can filter the log depending on multiple criteria, or view a merged log for several nodes in cluster mode.

****Command syntax: `listlog <filter list>`****


| Parameter  | Default  | Description  |
| --- | --- | --- |
| loglevel  | I  | Minimum severity level for the log lines to display.<br/> • I: INFORMATION<br/> • E: ERROR<br/> • W: WARNING<br/> • F: FATAL |
| lines  | -100  | Defines the number of lines to display from all log files, both current and backup.<br/> • A positive value displays the *x* oldest lines.<br/> • The value zero (0) displays all of the lines.<br/> • A negative value displays the **x** most recent lines.<br/> Enter zero (0), or a positive or negative value between 1 and 10,000. |
| datemin  | 0  | There are multiple formats to use to define the minimum date for log display.<br/> • Use the format YYMMDD to display logs that happened on or after this date.<br/> • Use a partial date for a more generic display. For example, <code>1604 </code>displays the log since April 2016.<br/> • Use a negative value to display logs for x number of days prior to today. For example, entering -2 displays the log since the day before yesterday, and -0 displays today’s log. |
| datetimemin | 0  | Use to display logs that happened on or after this start date and time.  |
| datetimemax | 99123123595999  | Use to display logs that happened on or before this end date and time.  |
| datemax  | 991231  | There are multiple formats to use to define the maximum date for log display.<br/> • Use to display logs that happened on or before this date. Use the format: YYMMDD<br/> • You can also enter a partial date. For example, <code>16 </code>displays the log prior to 2016.<br/> • A negative value is interpreted as a number of days before today. For example, -1 displays the log till yesterday (included). |
| timemin  | 0  |  • Logs only happened at this time or after this time during the day. Use the format HHMMSSss.<br/> • A negative value is interpreted as a number of minutes before the current time. For example, -20 displays the log for the last 20 minutes.<br/> See also [Time log precision](#Time). |
| timemax  | 23595999  |  • Logs only happened at this time or before this time during the day. Use the format HHMMSSss.<br/> • A negative value is interpreted as a number of minutes before current time. For example , -20 displays the log with the last 20 minutes filtered out.<br/> See also [Time log precision](#Time). |
| pattern  |   | Only displays the log lines that match this specific pattern. Enter any pattern with a maximum of 63 characters.  |
| displaynodeid  | YES  | Defines if the node ID displays at the beginning of each line in the log. Enter:<br/> • YES: display<br/> • NO: does not display |
| node  | -  | Defines which nodes you want to display the log for. Specify the node by entering the node number :<br/> • Empty: if you do not define this parameter LISTLOG displays all nodes<br/> • Enter the node number(s) and separate with a comma. Node numbers cannot be more than two digits. For example: 00, 01, 02 |


****Example****

To display the 10 most recent lines in the log:

```
CFTUTIL listlog lines=-10
```

To list the log for nodes 0 through 3, use any one of the following formats:

```
CFTUTIL listlog node='(0,1,2,3)'
CFTUTIL listlog node='(00,01,02,03)'
CFTUTIL listlog node='('0','1','2','3')'
CFTUTIL listlog node='('00','01','02','03')'
```

To display the fatal errors since yesterday:

```
CFTUTIL listlog loglevel=F, datemin=110807 or CFTUTIL listlog loglevel=F, datemin=-1
```

To display the errors that match the pattern \*TEST\*:

```
CFTUTIL listlog loglevel=E, pattern="\*TEST\*"
```

****Command parameter types****

```
COMMAND LISTLOG USAGE
LOGLEVEL D S 12 <I> 'FATAL','ERROR','WARNING','INFORMATION','F','E','W','I'
LINES N X 4 Number <-100> min=-10000 max=10000
DATETIMEMIN N N 14 Number <0> min=0 max=99123123595999
DATETIMEMAX N N 14 Number <99123123595999> min=0 max=99123123595999
DATEMIN N N 6 Number <0> min=-3660 max=991231
DATEMAX N N 6 Number <991231> min=-3660 max=991231
TIMEMIN N N 8 Number <0> min=-1440 max=23595999
TIMEMAX N N 8 Number <23595999> min=-1440 max=23595999
PATTERN s S 64 string max_length=63
DISPLAYNODEID S N 1 <YES> 'YES','NO'
NODE S L 2 \*99 STRING LIST max_length=1 max_entries=99
```
<span id="Time"></span>

### Time log precision

By default, the data sent to Sentinel as the EventTime has the format HH:MM:SS. To add milliseconds to the format, HH:MM:SS.sss, set the Transfer CFT UCONF `cft.cftlog.time_precision` parameter, where:

- 1 (default): the time in CFTLOG displays in seconds
- 10: the time in CFTLOG displays in tenths of seconds
- 100: the time in CFTLOG displays in hundredths of seconds

If the` cft.cftlog.time_precision` value is greater than 1, the Transfer CFT EventTime message sent to Sentinel has the HH:MM:SS.dh0 format.

**Example**

```
uconfset id=cft.cftlog.time_precision, value=10
```
