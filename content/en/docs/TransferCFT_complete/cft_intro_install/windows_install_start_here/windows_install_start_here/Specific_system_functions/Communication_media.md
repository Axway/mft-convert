{
    "title": "Communication  media",
    "linkTitle": "Communication media",
    "weight": "250"
}-   [About
    the communication media](#About_the_communication_media)
- [Defining
    the CFT user name](#Defining_the_CFT_user_name)
- [Adjusting
    the time and changing the date](#Adjusting_the_time_and_changing_the_date)
- [CFTUTIL
    shutdown timeout](#CFTUTIL_shutdown_timeout)

<span id="About_the_communication_media"></span>

### About the communication media

Functionally, Transfer CFT is made up of two parts:

- The transfer monitor
- The user interface(s)

Transfer CFT uses a communication medium so that requests coming from
the user interface can be communicated to the monitor section.

Transfer CFT can use a file as the communication media, and by default, the communication medium used is the file with the logical
name of $CFTCOM.

<span id="Defining_the_CFT_user_name"></span>

### Defining the CFT user name

The Transfer CFT user name is the name under which the Windows user
is logged. Under certain systems you can open a session without giving
a user name. In this case, the Transfer CFT user name is USERCFT.

<span id="Adjusting_the_time_and_changing_the_date"></span>

### Adjusting the time and changing the date

The time and date used or shown by Transfer CFT are those stated by
the operating system.

By default the date changes at midnight, GMT. But as a general rule,
if the country of use has been indicated, the date changes at midnight
local time. However in some infrequent and rather specialized cases, the
date can change at a different time (GM ± n).

Environment variable

`TZ or TZSET`

In general and to solve this problem, the operating system provides a
solution whereby users can indicate the time zone in which they are located.
This solution is supplied by the environment and consists in general in
setting an environment variable or a system parameter called TZ
or TZSET. For additional information, consult the operating
system documentation.

Transfer CFT takes this parameter into account, if it has been set.

<span id="CFTUTIL_shutdown_timeout"></span>

### CFTUTIL shutdown timeout

****Environment variable****

#### CFTEXITTIME

In general, this problem occurs when a procedure
executes automatically (end of transfer, …) using CFTUTIL, and if the
machine processor is loaded.

When this occurs, the Transfer CFT command that CFTUTIL uses
to address to Transfer CFT is not received, as if CFTUTIL
had not been called. In fact CFTUTIL has been called and the command
has been posted, but since the machine is loaded, Transfer CFT
cannot read the command posted by CFTUTIL until it disappears. The disappearance
of CFTUTIL causes the command that has been posted, but not
yet received by the Transfer CFT, also to disappear.

By default, CFTUTIL waits for a maximum of 60 seconds before disappearing. To modify this time, you define the CFTEXITTIME environment variable
in seconds of wait time.

****Example****

`SET CFTEXITTIME=120`

This example causes CFTUTIL to wait a maximum of 120 seconds before disappearing.

> **Note**
>
> Note: This environment variable
> can be used by all types of Transfer CFT interface: CFTUTIL, Copilot and
> the Transfer CFT API application.
