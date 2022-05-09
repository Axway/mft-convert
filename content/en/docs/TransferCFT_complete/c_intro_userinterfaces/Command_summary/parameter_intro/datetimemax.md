{
    "title": "datetimemax",
    "linkTitle": "datetimemax",
    "weight": "630"
}### datetimemax

****[ DATETIMEMAX = { 0 &#124; 99123123595999 } ]****

Use to display logs that happened on or before this end date and time. Entering 99123123595999 indicates the year 2099 December 31st at 23:59:59.99.

#### LISTCAT, DISPLAY

****[ DATETIMEMAX { <span class="underline">0</span> &#124; 99991231235959 } ]****

Use to display catalog transfers that happened on or before this end date and time according to the transfer record creation (DATEK, TIMEK). The default value unit is DAY. This parameter accepts either absolute or relative values. Use the syntactical format where:

- Relative values are: -1, -1M, -1H, -1D
- Absolute values are: YYYYMMDDhhmmss with a maximum of 14 characters

You can use this parameter in combination with [datetimemin](../datetimemin).

****Relative example****

```
CFTUTIL display datetimemax=-2H
```

****Absolute example****

```
CFTUTIL listcat datetimemax=2021030323
```

[Return to Command index](../../)
