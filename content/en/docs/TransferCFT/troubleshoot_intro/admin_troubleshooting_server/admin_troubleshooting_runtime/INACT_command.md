{
    "title": "           INACT  - Deactivate an object",
    "linkTitle": "Deactivate an object",
    "weight": "320"
}<span id="kanchor67"></span>

This page describes the INACT command and its parameters. You can use the INACT command to deactivate:

- One or more partners (CFTPART)
- Sentinel notifications
- CRON object (CFTCRON)
- Folder object (CFTFOLDER)

> **Note**
>
> Note: This command cannot be applied to partners whose definition was provided
> or modified by a directory EXIT.

Command guide: [INACT](../../../../c_intro_userinterfaces/command_summary#INACT)


| Parameter  | Description  |
| --- | --- |
| [ID](../../../../c_intro_userinterfaces/command_summary/parameter_intro/id)  | Identifier for the object to be deactivated. To deactivate several objects with a single command, use wildcard characters or meta characters. |
| [TYPE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/type)  | Object to be deactivated:<br/> • PART<br/> • TRK<br/> • CRON<br/> • FOLDER |
| [MODE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/mode)  | Mode to be deactivated on partners (only when TYPE=PART object):<br/> • BOTH (default)<br/> • REQUESTER<br/> • SERVER<br/> You can use the shortcuts B, R and S in place of the keywords. |
| [FORCE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/force) |  • YES: Stops any transfers in progress involving the deactivated partners.<br/> • NO: Default value, transfers progress normally. |


Using the INACT command
-----------------------

Using CFTUTIL you can perform the following commands.

### Deactivate a partner

****Syntax****

```
INACT TYPE=PART,ID=<CFTPART_ID>,MODE=<mode>,FORCE=<NO &#124; YES>
```

Where:

- `CFTPART_ID` is the identifier of the partner to deactivate. To deactivate several partners with a single command, use wildcard characters or meta characters.
- Mode is the mode to be deactivated, with values: "BOTH" , "B", "REQUESTER", "R" , "SERVER", "S"
- When `FORCE `is set to `YES`, stops any transfers in progress involving the deactivated partners.

****Example****

to deactivate the partner called PARIS in requester mode, enter:

```
INACT TYPE=PART, ID=PARIS, MODE=REQUESTER
```

Returning the following output:

```
CFTU20I Part=PARIS : ACTIVEBOTH -> ACTIVESERV
CFTU00I INACT _ Correct (TYPE=PART,ID=PARIS,MODE=REQUESTER)
```

When a partner is deactivated, transfers awaiting processing are:

- Suspended in requester mode
- Refused in server mode

The state of a transfer request awaiting execution in requester mode
for a deactivated partner remains ****D****,
with a diagnostic code 430 and a protocol diagnostic INACT.

The state of a transfer request awaiting execution in server mode for
a deactivated partner remains ****D****,
with a diagnostic code 930 and a protocol diagnostic RCO 312, or ABO 312
if the session is already open.

The state of transfers that are interrupted by an INACT command when
FORCE=YES is ****H****, with a diagnostic
code 121 and a protocol diagnostic OPER.

### Deactivate Sentinel notifications

****Syntax****

```
INACT TYPE=TRK
```

All notifications to Sentinel are suspended.

### Deactivate cron object

****Syntax****

```
INACT TYPE=CRON,ID=<CFTCRON_ID>
```

Where `CFTCRON_ID` is the identifier of the CRON object to deactivate. To deactivate several CRON objects with a single command, use wildcard characters or meta characters.

****Example****

To deactivate the CRON referenced by CRON1, enter:

```
INACT TYPE=CRON,ID=CRON1
```

Returning the following output:

```
CFTU20I Cronjob=CRON1 : ACTIVE -> NOACTIVE
CFTU00I INACT _ Correct (id=cron1,type=cron)
```

### Deactivate folder object

****Syntax****

```
INACT TYPE=FOLDER,ID=<CFTFOLDER_ID>
```

Where `CFTFOLDER_ID` is the identifier of the folder object to deactivate. To deactivate several folder objects with a single command, use wildcard characters or meta characters.

****Example****

To deactivate the folder referenced by USER1, enter:

```
INACT TYPE=FOLDER,ID=USER1
```

Returning the following output:

```
CFTU20I Folder=USER1 : ACTIVE -> NOACTIVE
CFTU00I INACT _ Correct (TYPE=FOLDER,ID=USER1)
```
