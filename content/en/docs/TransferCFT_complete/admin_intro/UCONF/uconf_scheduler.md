{
    "title": "UCONF: Configuration scheduling",
    "linkTitle": "Configuration scheduling",
    "weight": "320"
}In {{< TransferCFT/axwayvariablesComponentShortName  >}} you can define daily periods where dynamic configuration variables can be changed. For example, you might want to schedule that the value for cft.purge.sx can be modified during the period between 15:30 and 19:30.

****Steps overview****

1. Access the Unified Configuration using either command line or the UI.
1. Create the configuration alias by adding a new alias name to the `cft.scheduled_values` list. Do not use spaces or periods (.) in the alias name.
1. Configure the remaining parameters as described in the following table to define the new alias.


| Parameters  | Description  |
| --- | --- |
| cft.scheduled_values  | List of scheduled aliases. Use a space to separate alias names.  |
| cft.scheduled_values.(alias-id).start_time  | Start time using the format MM:HH:DAYS_OF_THE_WEEK. This is the begin time for when a value switches from its existing value to the temporary value. See [Details](#Details,%20days) below.  |
| cft.scheduled_values.(alias-id).delay  | Delay using the format MM:HH.<br/> This is the length of time during which the value can be changed. |
| cft.scheduled_values.(alias-id).id  | The configuration entity id (uconf parameter) that you want to provide scheduling for.  |
| cft.scheduled_values.(alias-id).value  | Temporary value. This value replaces the existing configuration value for the defined uconf parameter.<br/> To find the existing value, in command line enter:<br/> <code>CFTUTIL uconfget id=&lt;uconf_parameter&gt;</code> |


****<span id="Details, days"></span>Details****

DAYS_OF_WEEK:

- A star ‘\*’ indicates every day of the week.
- Indicate one or several days between Monday (0) and Sunday (6) using the digits separated by a comma (,).
- Indicate a range of days using the minus (-) sign.
- Examples of start_time using different of DAY_OF_WEEK formats:
    -   10:11:\* : 11h10 indicates every day of the week
    -   10:11:1 : 11h10 indicates every Tuesday
    -   10:11:0,2,4-6 : 11h10 indicates every Monday, Wednesday, Friday, Saturday and Sunday

****Example****

This example defines a schedule where the value of cft.purge.sx can be changed during the period that begins at 15:30 and has a duration of 4 hours on Saturday and Sunday.

1. Add the new alias name to the `scheduled_values` list. This example creates a new alias called alias04.
    ```
    CFTUTIL uconfset id=cft.scheduled_values,value='"alias01 alias02 alias03 alias04"'
    ```
1. Configure the new alias, alias04.
    ```
    CFTUTIL uconfset id=cft.scheduled_values.alias04.start_time,value='”30:15:5,6”'
    CFTUTIL uconfset id=cft.scheduled_values.alias04.delay,value=00:04
    CFTUTIL uconfset id=cft.scheduled_values.alias04.id,value= cft.purge.sx
    CFTUTIL uconfset id=cft.scheduled_values.alias04.value,value=2
    ```
1. Activate the configuration modification. Enter:
    ```
    CFTUTIL reconfig type=UCONF
    ```

> **Note**

- The same configuration value can be scheduled several times in a day.
- Scheduled periods for the same value can be contiguous, but they should not overlap.

****Related topics****

- [About the Unified Configuration interface](../)
- [Using Unified configuration (UCONF) in the GUI](../uconf_userinterface)
- [Using Unified configuration (UCONF) in CFTUTIL](../uconf_w_cftutil)
