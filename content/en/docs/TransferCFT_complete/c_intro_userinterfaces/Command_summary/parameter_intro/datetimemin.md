{
    "title": "datetimemin",
    "linkTitle": "datetimemin",
    "weight": "640"
}### datetimemin

#### LISTLOG

****[ DATETIMEMIN { <span class="underline">0</span> &#124; 99123123595999 } ]****

Use to display logs that happened on or after this start date and time. The default of <span class="underline">0</span> indicates no check.

#### LISTCAT, DISPLAY

****[ DATETIMEMIN { 0 &#124; 99991231235959 } ]****

Use to display catalog transfers that happened on or after this start date and time according to the transfer record creation (DATEK, TIMEK). The default value unit is DAY. This parameter accepts either absolute or relative values. Use the syntactical format where:

- Relative values are: -1, -1M, -1H, -1D
- Absolute values are: YYYYMMDDhhmmss with a maximum of 14 characters

You can use this parameter in combination with [datetimemax](../datetimemax).

****Relative example****

```
CFTUTIL display datetimemin=-3H
```

****Absolute example****

```
CFTUTIL listcat datetimemin=2021030420
```

[Return to Command index](../../)
