{
"title": "Schedule policy execution",
"linkTitle": "Schedule policy execution",
"weight": 5,
"date": "2019-10-17",
"description": "Schedule the execution of any policy on a specified date and time."
}

You can configure a policy execution scheduler at the level of the API Gateway instance. This enables you to schedule the execution of any policy on a specified date and time in a recurring manner. The API Gateway provides a preconfigured library of schedules to select from when creating a policy execution scheduler. You can also add your own schedules to the globally available library in Policy Studio.

You can use policy execution scheduling in any policy (for example, to perform a message health check). This feature is also useful when polling a service to enforce a Service Level Agreement (for example, to ensure the response time is less than 1000 ms, and if not, to send an alert).

## Add a global schedule

To add a schedule to the globally available library in the Policy Studio, perform the following steps:

1. Select the **Environment Configuration** > **Libraries** > **Schedules**
    node in the tree.
2. Click the **Add**
    button at the bottom of the **Schedules**
    window.
3. In the **Schedules**
    dialog, enter a **Name**
    (for example, `Run every 30 seconds`).
4. Enter a **Cron expression** (for example, `0/30 * * * * ?`). Alternatively, click the browse button to select an expression in **Cron dialog**.
5. Click **OK**.

You can also edit or delete a selected schedule using the appropriate button.

## Add a policy execution scheduler

To add a policy execution scheduler in the Policy Studio, perform the following steps:

1. Select the **Environment Configuration** > **Listeners**
    node on the left.
2. Right-click the instance node (for example, **API Gateway**), and select **Add policy execution scheduler**.
3. Click the button next to the **Schedule**
    field, select a cron expression in the dialog, and click **OK**.
4. Click the button next to the **Policy**
    field, select a policy in the dialog, and click **OK**. You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically.
5. Click **OK**.

## Configure cron expressions

Cron expressions are used in policy execution scheduling, and within several filters (for example, the **Time** utility filter).

The **Cron Dialog**
enables you to create a cron expression used to trigger regularly occurring events (for example, generate a report, or block or allow messages at specified times). You can use the time tabs in this dialog to guide you through the configuration steps.

Alternatively, enter the cron expression value directly in the text boxes. When you have created the cron expression, you can click the **Test Cron**
button to test the value of the cron expression and see when it is next due to fire.

### Create a cron expression using the time tabs

Using the time tabs in the dialog to guide you through the configuration steps is the default option. You can create a cron expression to trigger at the specified times using the following settings:

#### Seconds

Select one of the following options:

* **Every Second of the Minute**: Fires every second of the minute. This is the default setting.
* **Just on Second**: Fires only on the specified second of the minute.
* **Range from Second**: Fires over a range of seconds. For example, if the first value is 10 and the second value is 25, the trigger starts firing on second 10 of the minute and continues to fire for 15 seconds.
* **Start on Second**: Fires on the specified second of the minute and repeats every specified number of seconds. For example, if the first number is 15 and the second number is 30, the trigger fires at 15 seconds and repeats every 30 seconds until stopped.
* **On Multiple Seconds**: Fires on the specified seconds of each minute. Enter a comma separated list of seconds (values of `0-59` inclusive). For example, `10,20,30`.

#### Minutes

Select one of the following options:

* **Every Minute of the Hour**: Fires every minute of the hour. This is the default setting.
* **Just on Minute**: Fires only on the specified minute of the hour.
* **Range from Minute**: Fires over a range of minutes. For example, if the first value is 5 and the second value is 15, the trigger starts firing on minute 5 of the hour and continues to fire for 10 minutes.
* **Start on Minute**: Fires on the specified minute of the hour and repeats every specified number of minutes. For example, if the first number is 10 and the second number is 20, the trigger fires at 10 minutes and repeats every 20 minutes until stopped.
* **On Multiple Minutes**: Fires on the specified minute of each hour. Enter a comma-separated list of minutes (values of `0-59` inclusive). For example, `5,15,30`.

#### Hours

Select one of the following options:

* **Every Hour of the Day**: Fires every hour of the day. This is the default setting.
* **Just on Hour**: Fires only on the specified hour of the day.
* **Range from Hour**: Fires over a range of hours. For example, if the first value is 9 and the second value is 17, the trigger starts firing on hour 9 of the day and continues to fire for 8 hours.
* **Start on Hour**: Fires on the specified hour of the day and repeats every specified number of hours. For example, if the first number is 6 and the second number is 2, the trigger fires at hour 6 and repeats every 2 hours until stopped.
* **On Multiple Hours**: Fires on the specified hours of each day. Enter a comma-separated list of hours (values of `0-23`
    inclusive). For example, `6,12,18`.
* **Multiple Ranges**: Fires on the specified ranges of hours of each day. Enter comma separated ranges of hours (values of `0-23` inclusive). For example, `9-1,14-17`.

#### Day

You must first select **Day of Week**
or **Day of Month**
from the drop-down list (using both of these fields in not supported). **Day of Month**
is selected by default.

##### Day of Month

Select one of the following options:

* **Every Day of the Month**?: Fires every day of the month. This is the default setting.
* **Just on Day**: Fires only on the specified day of the month.
* **Range from Day**: Fires over a range of days. For example, if the first value is 7 and the second value is 14, the trigger starts firing on day 7 of the month and continues to fire for 9 days.
* **Start on Day**: Fires on the specified day of the month and repeats every specified number of days. For example, if the first day is 2 and the second number is 5, the trigger fires at day 2 and repeats every 5 days until stopped.
* **On Multiple Days**: Fires on the specified days of each month. Enter a comma separated list of days (values of `1-32`
    inclusive). For example, `1,14,21,28`.
* **Last Day of the Month**: Fires on the last day of each month (for example, 31 January, or 28 February in non-leap years).
* **Last Week Day of the Month**: Fires on the last week day of each month (Monday-Friday inclusive only).

##### Day of Week

Select one of the following options:

* **Every Day of the Week**: Fires every day of the week. This is the default setting.
* **Just on *Day***: Fires only on the specified day of the week. Defaults to `SUN`.
* **Range from *Day***: Fires over a range of days. For example, if the first value is `MON`
    and the second value is `FRI`, the trigger starts firing on `MON`
    and continues to fire until `FRI`.
* **Start on *Day***: Fires on the specified day of the week and repeats every specified number of days. For example, if the first day is `TUES` and the number is 3, the trigger fires on `TUES` and repeats every 3 days until stopped.
* **On Multiple *Days***: Fires on the specified days of each week. Enter a comma separated list of days. For example, `MON,WED,FRI`.
* **Last Day of the Week**: Fires on the last day of each week (`SAT`).
* **On the *Nth Day***: Fires on the Nth day of the week of each month (for example, the second `FRI`
    of each month).

#### Month

Select one of the following options:

* **Every Month of the Year**: Fires every month of the year. This is the default setting.
* **Just on *Month***: Fires only on the specified month of the year. Defaults to `JAN`.
* **Range from *Month***: Fires over a range of months. For example, if the first value is `MAY`
    and the second value is `JUL`, the trigger starts firing on `MAY`
    and continues to fire until `JUL`.
* **Start on *Month***: Fires on the specified month of the year and repeats every specified number of months. For example, if the first month is `FEB`
    and the number is 2, the trigger fires in `FEB`
    and repeats every 2 months until stopped.
* **On Multiple *Months***: Fires on the specified months of each year. Enter a comma-separated list of months (values of `JAN-DEC`
    or `1-12`
    inclusive). For example, `MAR,JUN,SEPT`.

#### Year

Select one of the following options:

* **Every Year**: Fires every year. This is the default setting.
* **No Specific Year**: Fires no specific year.
* **Just on *Year***: Fires only on the specified year. Defaults to current year.
* **Range from *Year***: Fires over a range of years. For example, if the first value is `2012`
    and the second value is `2015`, the trigger starts firing on `2012`
    and continues to fire until `2015`.
* **Start on *Year***: Fires on the specified year and repeats every specified number of years. For example, if the first year is `2012`
    and the number is 2, the trigger fires in `2012`
    and repeats every 2 years until stopped.
* **On Multiple *Years***: Fires on the specified Year. Enter a comma-separated list of years (for example, `2012,2013,2015`).

### Enter a cron expression

To enter the cron expression value directly in the dialog, click **Create cron expression using edit boxes**, and enter the values in the appropriate boxes.

A cron expression is a string that specifies a time schedule for triggering an event (for example, executing a policy). It consists of six required fields and one optional field, each separated by a space, which together specify when to trigger the event. For example, the following expression specifies to run at 10:15 am every Monday, Tuesday, Wednesday, Thursday, and Friday in 2011:

```
0 15 10 ? * MON-FRI 2011
```

#### Syntax

The following table shows the syntax used for each field:

| Field           | Values               | Special characters |
|-----------------|----------------------|--------------------|
| Seconds         | `0-59`               | `, - * /`          |
| Minutes         | `0-59`               | `, - * /`          |
| Hours           | `0-23`               | `, - * /`          |
| Day of Month    | `1-31`               | `, - * ? / L W`    |
| Month           | `1-12` or `JAN-DEC`    | `, - * /`         |
| Day of Week     | `1-7` or `SUN-SAT`     | `, - * ? / L #`    |
| Year (optional) | empty or `1970-2199` | `, - * /`          |

#### Special characters

The special characters are explained as follows:

| Special character | Description               |
|-------------------|------------------------------------------------|
| `,`               | Separates values in a list (for example, `MON,WED,SAT` means Mondays, Wednesdays, and Saturdays only).     |
| `-`               | Specifies a range of values (for example, 2011-2015 means every year between 2011 and 2015 inclusive).   |
| `*`               | Specifies all values of the field (for example, every minute).  |
| `?`               | Specifies no value in the Day of Month and Day of Week fields. This enables you to specify a value in one field, but not in the other.   |
| `/`               | Specifies time increments (for example, in the Minutes field, `0/15` means minutes 0, 15, 30, and 45, while `5/15` means minutes `5`, `20`, `35`, and `50`). Specifying `*` before the `/` is the same as specifying `0` as the start value. The `/` character enables you to turn on every nth value in the set of values for the specified field. For example, `7/6` in the month field only turns on month `7`, and does not mean every 6th month.   |
| `L`               | Specifies the last value in the Day of Month and Day of Week fields. In the Day of Month field, this means the last day of the month (for example, January 31, or February 28 in non-leap years). In the Day of Week field, when used alone, this means `7` or `SAT`. When used after another value, it means the last `XXX` day of the month (for example, `5L` means the last Thursday of the month). When using the `L` character, do not specify lists or ranges because this can give confusing results.    |
| `W`               | Specifies the weekday (Monday-Friday) nearest the given day. For example, `15W` means the nearest weekday to the 15th of the month. If the 15th is a Saturday, the trigger fires on Friday 14th. If the 15th is a Sunday, it fires on Monday 16th. If the 15th is a Tuesday, it fires on Tuesday 15th. However, if you specify `1W`, and the 1st is a Saturday, the trigger fires on Monday 3rd to avoid crossing the month boundary. You can only specify the `W` character for a single day, and not a range or list of days.    |
| `#`               | Specifies the nth `XXX` weekday of the month in the Day of Week field. For example, a value of `FRI#2` means the second Friday of the month. However, if you specify `#5`, and there are not 5 of the specified Day of the Week in the month, no policy is run that month. When the `#` character is specified, there can only be one expression in the Day of Week field (for example, `2#1,6#4` is not valid because there are two expressions).  |

#### Examples

The following are some of the cron expressions provided in the **Schedule Library** in the Policy Studio:

| Cron expression            | Description  |
|----------------------------|---------------------------|
| `0 15 10 ? * *`            | Run at 10:15am every day. |
| `0 15 10 ? * 6L 2011-2015` | Run at 10:15am on every last Friday of every month during the years 2011, 2012, 2013, 2014, and 2015.    |
| `0 15 10 ? * 6#3`          | Run at 10:15am on the third Friday of every month.   |
| `0 0 10 1,15 * ?`          | Run at 10am on the 1st and 15th days of the month.   |
| `0 10,44 14 ? 3 WED`       | Run at 2:10pm and at 2:44pm every Wednesday in the month of March.    |
| `0,30 * * ? * SAT,SUN`     | Run every 30 seconds but only on Weekends (Saturday and Sunday).   |
| `0 0/5 14,18 * * ?`        | Run every 5 minutes starting at 2pm and ending at 2:55pm, and every 5 minutes starting at 6pm and ending at 6:55pm, every day. |
| `0 0-5 14 * * ?`           | Run every minute starting at 2pm and ending at 2:05pm, every day. |

{{< alert title="Note" color="primary" >}}
Support for specifying both a Day of Week and a Day of Month value is not complete. You must use the `?` or `*` character in one of these fields.

Overflowing ranges with a larger number on the left than the right are supported (for example, `21-2` for 9pm until 2am, or `OCT-MAR`). However, overuse may cause problems with daylight savings (for example, `0 0 14-6 ? * FRI-MON`).
{{< /alert >}}

### Test the cron expression

When you have configured the cron expression using either approach, click the **Test Cron**
button to test the syntax of the cron expression value and view when it is next due to fire. If the configured cron expression is invalid, a warning dialog is displayed.

#### Results

The test results include the following output:

**Expression**: Displays the configured cron expression. For example: `* * 9-17 * * ? *`

**Next Fire Times**: Displays when cron expression is next due to fire. For example: `Next fire event:Fri Jul 22 10:26:09 EST 2012`
