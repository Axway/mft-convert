---
title: "Housekeeping for  catalog and output files"
linkTitle: "Housekeeping for catalog and output files"
weight: 250
---{{< TransferCFT/axwayvariablesComponentShortName  >}} provides a set of services to track activity related to transfers and their status as well as evaluating the system health.

This section describes how to manage the log, catalog, and output files to keep your system running smoothly.

## Catalog management

Transfer CFT records all file transfers in its local database, the catalog. The size of the Catalog is defined to store a maximum number of file transfer records.

### Automatic catalog expansion

The auto-expand catalog option lets you enlarge the catalog by a preset percentage when an alert is sent that the catalog is reaching its threshold. Additionally you can indicate a script to execute if this expanded limit is exceeded.

#### Overview

Once the catalog exceeds the minimum alert level, it can be expanded to the amount defined in the auto-expand option. For example, if you set the auto-expand to 100%, then the catalog doubles. If you define it as 50%, the catalog resizes to 1.5 times the original number of records. After the auto-expand limit is reached, a predefined script can be executed.

If you defined a limit for catalog alerts, TLVCLEAR, once the usage surpasses the allotted value one of the following occurs:

* The catalog is expanded and a message sent to the log, but no script is executed
* The catalog is expanded, a message is sent to the log, and the TLVCEXEC script executes.

#### Steps

To enable the auto-expand option, with {{< TransferCFT/axwayvariablesComponentShortName  >}} running:

1. Set the uconf values for:
    *   `cft.cftcat.auto_expand_percent `
    *   `cft.cftcat.auto_expand_max_size`
1. To activate the new values, run the command: CFTUTIL reconfig type = uconf
    *   If {{< TransferCFT/axwayvariablesComponentShortName >}} is stopped when setting uconf values, you do not need to execute the reconfig command.


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.cftcat.auto_expand_percent  | 0  | This value indicates the factor increase, as a percentage, that the catalog will automatically expand.<br/> The value 0 disables the automatic expansion feature.<br/> <blockquote> **Note**<br/> Tip We recommend that you set this to a relatively high value, at least 50. When repeatedly expanded, the catalog's internal structure may become fragmented and, consequently, catalog access less efficient.<br/> </blockquote>  |
| cft.cftcat.auto_expand_max_size  | 1M  | The maximum number of records for the automatic catalog expansion option. |


Related parameters:

* TLVCLEAR: Level below which the alert stops.
* TLVCEXEC: Batch to execute when the alert ends.
* TLVWRATE: The minimum amount of time, in seconds, to wait before resending an alert.
* TLVWEXEC: Batch to execute when CFTCAT/TLVWARN is reached.
* TLVWARN: Catalog usage limit before issuing an alert. When this limit is reached, the CFTCAT/TLVWEXEC is executed.

****Example****

The example is based on the following settings:

* catalog size = 100
* cft.cftcat.auto_expand_percent = 20
* cft.cftcat.auto_expand_max_size = 140
* TLVCLEAR = 70
* TLVWARN = 80

When you reach the TLVWARN (level=80%), the following messages are sent to the log:

```
12/10/17 17:53:30  CFTC29W Catalog Alert fill threshold reached: level=80%
ID=CAT0
12/10/17 17:53:30  CFTC13I Catalog resize (100 --> 120) done
12/10/17 17:54:16  CFTT17I _ STATE=HOLD <IDTU=A0000029 PART=PARIS IDF=TXT IDT=J1718064>
12/10/17 17:54:16  CFTR12I SEND Treated for USER Nougat  <IDTU=A0000029 PART=PARIS IDF=TXT>
12/10/17 17:54:16  CFTS20I Communication file row number deleted: 00000252   
```

The new fill rate is now 80/120 = ~67%, which is below TLVCLEAR (70), so the alerts stop at the next update.

```
   12/10/17 17:54:16  CFTC30W Catalog Alert cleared : level=67% ID=CAT0
```

The message CFTC30W Catalog Alert cleared : **level=67%** indicates that the catalog is sufficient. If it were not, the catalog would be extended again at next alert in TLVWRATE seconds.

The catalog continues to fill until it reaches 80%. Expanding 20% more would resize the catalog to 144 records, which exceeds the limit (140). If you exceed the limit the following log messages display:

```
12/10/17 18:06:57  CFTC29W Catalog Alert fill threshold reached: level=80% ID=CAT0
12/10/17 18:06:57  CFTC13I Catalog resize (120 --> 144) too much
12/10/17 18:06:57  CFTC13I Catalog resize (120 --> 140) done
12/10/17 18:06:57  CFTT17I _ STATE=HOLD <IDTU=A000002P PART=PARIS IDF=TXT IDT=J1718092>
12/10/17 18:06:57  CFTR12I SEND Treated for USER Nougat <IDTU=A000002P PART=PARIS IDF=TXT>
12/10/17 18:06:57  CFTS20I Communication file row number deleted: 00000269                               
12/10/17 18:06:57  CFTC30W Catalog Alert cleared : level=69% ID=CAT0
```

The next time the catalog limit is reached, it can no longer expand. Here, the log displays 210, which is the theoretical number computed by the auto-expand feature, but in reality it is an error (CFTC13E):

```
12/10/19 15:53:21  CFTC29W Catalog Alert fill threshold reached: level=80% ID=CAT0
12/10/19 15:53:21  CFTC13E
Catalog resize (140 --> 210) reached max before expansion
12/10/19 15:53:21  CFTC08I Purge Treated : no record found to delete.
```

#### Auto-expand option on z/OS

If you are using the `cft.cftcat.auto_expand` parameter in a z/OS environment, refer to the `SHARECAT `parameter in the *Installation and Operation Guide* for OS specific details.

### Catalog purge policies

#### Purge using the CFTCAT command

The local file transfer internal datafile rules include standard purge rules that you can modify using the catalog command CFTCAT.

There are 6 parameters that manage the purge, depending on the transfer status and direction. For each of the following you can set the number, in days, for the purge to occur. In our example, the purge is set for 10 days.

```
CFTCAT ID = 'CAT0',

FNAME = '$CFTCATA',
WSCAN = '1',
TIMEP = '23595999',
UPDAT = '1',
SH = '10',
ST = '10',
SX = '10',
RH = '10',
RT = '10',
RX = '10',

```

The first letter indicates the transfer direction (S for SEND or R for receive ).

The second letter refers to the state (CFTSTATE).

**Normal mode**

* \(H\) Transfer phase and hold phasestep, or Transfer phase and kill phasestep
* \(T\) Ack phase and all phasesteps
* \(X\) Done phase and Done phasestep

> **Note**
>
> Transfers in phase (Y) and phasestep (D) are not purged when purging the catalog. To purge these records, you must execute an END command that modifies the state to (X).

**Compatibility mode**

* \(H\) Hold, keep, or preprocessing status
* \(T\) Completed status
* \(X\) Executed status

#### Purge using UCONF settings

You can use the unified configuration to perform the same sort of purges as with the CFTCAT command, except that UCONF offers a more granular scheduling, i.e. hours, minutes, or seconds.

For example, set the following where the sx represents 10 days (where a day equals a 24 hour interval) for an executed SEND transfer:

```
UCONFSET id=cft.purge.sx, value=10D
```

> **Note**
>
> The amount of time is entered in days (x or xD), in hours (xH) or in minutes (xM). If set to -1, the CFTCAT value is used.

To schedule a periodic purge every 30 minutes:

```
CFTUTIL uconfset id=cft.purge.periodicity,value=30M
```

To apply the dynamic configuration parameters change:

```
CFTUTIL reconfig type=UCONF
```

For information on the RECONFIG command, please see [Manage configuration updates - RECONFIG](../../admin_commands_intro/reconfig).

#### Purge per template definitions

When defining CFTSEND or CFTRECV templates you can set a parameter to purge the records after the transfer completes.

```
CFTSEND DELETE=YES
```

#### Purge records with the keep status

Delete all file transfer records in keep status (K) due to a file creation error.

```
CFTRECV RKERROR=YES
```

#### Manually delete catalog records

The DELETE command is used to <span id="delete_command"></span>delete one
or more catalog entries. A transfer in process is interrupted and its
transfer request disappears from the catalog.

This file is automatically deleted for an R, Receive, type request in
non terminated state, a state other than T and X, if the receive file
is a temporary file.

For any selected set of transfers, transfers are processed in batches
of 20 transfers every 5 seconds.

For example, delete all executed transfers from the catalog.

```
DELETE STATE=X
```

#### Manage transfer events sent to Sentinel

A file transfer event is sent as an XFB.Transfer event to the Sentinel server each time the file transfer status is updated.

You can set filters to reduce the number of messages (more than a 50% reduction) sent to Sentinel, for example:

```
UCONFSET id=sentinel.xfb.transfer,value=SUMMARY
```

## Archive Transfer CFT standard output files

All output files are stored in the &lt;runtime/run> directory. Among these are the standard output files for Transfer CFT and the Copilot processes. The copui.trc is the standard output for the Transfer CFT Copilot server, which continues to grow in size but cannot be rotated.

However, Transfer CFT processes use a standard output file &lt;runtime/run>/cft.out to log internal system messages. You can define the number of archive files you want to rotate using the command:

```
UCONFSET id=cft.output.backup_count,value=n
```

## Delete completed transfer files

There are multiple ways to clear out completed transfer files. Let's begin with a simple example of deleting files that have successfully been sent.

```
CFTSEND ID=CLEANUP,FNAME=<FILENAME>,FACTION=DELETE
```

If you would additionally like to delete the catalog records, as well as the file after it's transfer (defined according to the transfer state).

* The FDELETE option removes the file once the catalog record is deleted.
* You can use the DELETE=YES option in conjunction with FDELETE to remove both the file and the record.

For example, to remove both the file and the record when sending a file.

```
CFTSEND ID=CLEANUP,FNAME=<FILENAME>,DELETE=YES,FDELETE=CDKHTX
```

> **Note**
>
> The DELETE/FDELETE options are also valid for a CFTRECV, but note that setting FDELETE=CDKHTX deletes the file regardless of the state at the end of the transfer.
