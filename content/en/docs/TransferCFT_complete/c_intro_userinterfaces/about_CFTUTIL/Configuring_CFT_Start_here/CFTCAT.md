{
    "title": "Catalog - CFTCAT ",
    "linkTitle": "CFTCAT - Catalog attributes",
    "weight": "290"
}The catalog logs all information relative to transfer requests. Use this command to describe the way the catalog is
managed: file, scanning mode, retention time before purge, and so on.

<span id="About_the_CFTCAT_Command"></span>The Catalog (CFTCAT) object is used to describe
the Transfer CFT transfer catalog access for:

- File
- Scanning mode
- Retention time,
    before the purge, of associated information

The catalog purge can be initiated at midnight (default value) or at
a specific time indicated in the TIMEP parameter.

The catalog is purged in batches of 10 entries. Between each batch, Transfer CFT handles all pending transfer events. As a result, if the transfer
activity is intense, the catalog purge action may be extended but does
not significantly disrupt transfers.

User interface options
----------------------

In the user interface, you can customize the catalog columns and filters. From the Catalog - CFTCAT page in the user interface, you can create, clone, modify, or delete catalog objects.

Parameter descriptions
----------------------

****Related
topics****

- Command syntax
    [CFTCAT](../../../command_summary#CFTCAT)
- Object concepts
    [Catalog attributes](../../../../admin_intro/admin_config_commands/catalog_parameter_concepts)


| Parameter  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id)  | Name identifying the CFTCAT command. |
| [FNAME](../../../command_summary/parameter_intro/fname) | Catalog file name. |
| [CACHE](../../../command_summary/parameter_intro/cache)  | Size parameter for the monitor cache memory buffer size, containing the transfers waiting for resources. |
| [RH](../../../command_summary/parameter_intro/rh) | Number of days (where a day equals a 24-hour interval) after which the catalog entries of &quot;unterminated&quot; receive requests ( H or K state) are automatically purged. |
| [RKERROR](../../../command_summary/parameter_intro/rkerror) | Action to be taken if a transfer aborts during the selection phase in server mode. |
| [RT](../../../command_summary/parameter_intro/rt)  | Number of days (where a day equals a 24-hour interval) after which the catalog entries of terminated receive transfers (RT state) are automatically purged. |
| [RX](../../../command_summary/parameter_intro/rx)  | Number of days after which the catalog entries of receive transfers for which the end of reception procedure is correctly executed (RX state) are automatically purged. |
| [RY]()  | Number of days after which the catalog entries of receive transfers that are the in post-processing phase (RY state) are automatically purged.  |
| [SH](../../../command_summary/parameter_intro/sh)  | Number of days (where a day equals a 24-hour interval) after which the catalog entries for &quot;unterminated&quot; send requests (H or K state) are automatically purged. |
| [ST](../../../command_summary/parameter_intro/st)  | Number of days (where a day equals a 24-hour interval) after which the catalog entries of terminated send transfers (ST state) are automatically purged. |
| [SX](../../../command_summary/parameter_intro/sx) | Number of days (where a day equals a 24-hour interval) after which the catalog entries of terminated send transfers, for which the end-of-send transfer procedure was correctly executed (SX state), are automatically purged. |
| [SY]()  | Number of days (where a day equals a 24-hour interval) after which the catalog entries of send transfers that are in the post-processing phase (SY state), are automatically purged.  |
| [TLVCLEAR](../../../command_summary/parameter_intro/tlvclear) | Level below which the alert ceases, as a percentage of filling, where 0% indicates the file is empty and 100% that it is full. |
| [TLVCEXEC](../../../command_summary/parameter_intro/tlvcexec) | Batch to execute when the alert ends. |
| [TLVWRATE](../../../command_summary/parameter_intro/tlvwrate) | The minimum amount of time, in seconds, to wait before resending an alert. |
| [TLVWEXEC](../../../command_summary/parameter_intro/tlvwexec) | Batch to execute when CFTCAT/TLVWARN is reached. |
| [TLVWARN](../../../command_summary/parameter_intro/tlvwarn) | Catalog usage limit before issuing an alert, as is a percentage of filling, where 0% indicates the file is empty, and 100% that it is full.<br/> When this limit is reached, the CFTCAT/TLVWEXEC is executed. |
| [TIMEP](../../../command_summary/parameter_intro/timep)  | Daily purge time chosen by the user.<br/> The user can program an automatic, cyclic catalog purge. The default purge time is midnight.<br/> <blockquote> **Note**<br/> Note: To completely deactivate purging, set TIMEP = 00000000. Use this option with caution as no automatic purging is performed (at a selected time or at midnight).<br/> </blockquote>  |
| [UPDAT](../../../command_summary/parameter_intro/updat)  | Number of synchronization points between two consecutive updates of the catalog file during a transfer. |
| [WSCAN](../../../command_summary/parameter_intro/wscan)  | Enter the frequency (in minutes) with which {{< TransferCFT/axwayvariablesComponentShortName  >}} scans the catalog file when restarting a transfer: • 5 (default value)<br/> • 1 to 60 |


****Example****

```
CFTCAT ID = IDCAT,
FNAME = filename,
RH = 7,
RT  = 3,
RX = 3,
SH = 7,
ST = 3,
SX = 3
```

- Non-terminated send requests (SH state) and interrupted receive
    transfers are automatically purged after seven days (RH state).
- Terminated send transfer entries (ST state) and terminated receive
    transfer entries (RT state) are automatically purged after three days.
    The entries of terminated send transfers and receive transfers for which
    the end procedure was correctly executed, SX and RX state, are automatically
    purged after three days.
- The UPDAT parameter is not mentioned; it takes the default value
    1, which results in the catalog file being updated at each synchronization
    point of each transfer.

****Related topics****

- [PURGE - Purging the catalog](../../../../admin_intro/admin_commands_intro/purge_catalog)
