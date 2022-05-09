{
    "title": "PURGE  - Purging the catalog",
    "linkTitle": "PURGE - Purge the catalog",
    "weight": "300"
}This topic describes how to delete records which have exceeded the retention
time in the catalog. Transfer CFT provides the following purge options:

- [Startup configuration purge](#Startup)
- [Periodic purge](#Periodic)
- [Dynamic catalog purge](#Dynamic)
- [Compatibility](#Compatib)

********Syntax********


| Command or parameter  | Description  |
| --- | --- |
| PURGE  | Use this command to delete records which have exceeded the retention time indicated in the Transfer CFT parameter setting. You can set the purge time to meet your requirements and Transfer CFT activity.  |
|  [TIMEP](../../../c_intro_userinterfaces/command_summary/parameter_intro/timep) | Purge time selected by the user.<br/> You can deactivate the next purge function by setting TIMEP = 00000000. Use care with this operation due to a risk of catalog overloading with a loss of performance, or overflow.<br/> If the next purge is part of a cycle, see the CFTCAT TIMEP parameter, the entire cycle is deleted, and not just the next occurrence of this cycle. |


<span id="Startup"></span>

Start-up configuration purge
----------------------------

You can configure a catalog purge to occur on Transfer CFT start up. The following [unified configuration](../../uconf/uconf_parameters) options are available:

- Purge before starting transfers uconf
- Start purging when Transfer CFT is running in the background
- Disable purge at startup

<span id="Periodic"></span>

Periodic purge
--------------

You can use the [unified configuration](../../uconf/uconf_parameters) parameter cft.purge.periodicity to schedule periodic catalog purges. To define this parameter enter an integer, optionally followed by the letter D, H, or M, to indicate day, hour, or minute, respectively. If you enter zero as the value, the default, there is no periodic purge.

When scheduling periodic purges, be sure to consider the amount of time that you want to keep transfers. This is especially important for transfers that are in the HOLD state, as they have not yet been executed. Set the parameters cft.purge.sh and cft.purge.rh to values that guarantee that these transfer are not purged before they are completed regardless of purge scheduling.

********Examples********

The examples provide as a basis for scheduling catalog purges, where the time  is from the start of the transfer. See the section [Compatibility](#Compatib) for information on how to use the former purge scheduling (exclusively by days).

To disable the periodic purge:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=0
```

To set a purge period of one day:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=1
```

To set a purge period of 2 days:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=2D
```

To set a purge period of 3 hours:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=3H
```

To set a purge period of 120 minutes:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=120M
```

To define a periodic purge every 30 minutes, where transfers in Hold are kept for two days, configure as follows.

- To keep X and T states transfers for 30 minutes:

```
CFTUTIL uconfset id=cft.purge.rx,value=30M
CFTUTIL uconfset id=cft.purge.sx,value=30M
CFTUTIL uconfset id=cft.purge.rt,value=30M
CFTUTIL uconfset id=cft.purge.st,value=30M
```

- To keep H state transfers for 2 days:

```
CFTUTIL uconfset id=cft.purge.rh,value=2D
CFTUTIL uconfset id=cft.purge.sh,value=2D
```

- To schedule a periodic purge every 30 minutes:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=30M
```

- To apply the dynamic configuration parameters change:

```
CFTUTIL reconfig type=UCONF
```
<span id="Dynamic"></span>

Dynamic catalog purge
---------------------

You can use the [unified configuration](../../uconf/uconf_parameters) parameters to modify the amount of time to keep transfers in catalog before purging without modifying the CFTCAT object.

For each of the following, enter an integer value for the amount of time. If you enter the default value of -1, the CFTCAT configuration value is used.

- cft.purge.rx = requester state X
- cft.purge.sx = server state X
- cft.purge.rt = requester sate T
- cft.purge.st = server state T
- cft.purge.rh = requester sate H
- cft.purge.sh = server state H

Where the state is:

- H: Hold
- T: Terminated
- X: Executed

<span id="Compatib"></span>

Compatibility
-------------

The parameters listed above have a different meaning according to their value, either:

- Setting the parameter value to -1 indicates that the value is ignored, and the CFTCAT setting is used.
- Setting the parameter value to a lone integer, or an integer followed by the letter “D” specifies an amount of time in days. Setting the value to an integer followed by the letter “H” or “M” specifies an amount of time in hours or minutes. In each case, the value is converted to the corresponding amount of minutes, and the purge is calculated to within a minute.

****Example****

For a transfer created at 10:30 on Monday February 13, for example, if the retention period is '1D', or '24H', or '1440M', it cannot be purged before 10:30 on February 14.

See also [CFTCAT - Catalog attributes.](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftcat)
