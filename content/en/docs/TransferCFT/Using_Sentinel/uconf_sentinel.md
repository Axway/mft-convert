---
    title: "UCONF: Sentinel options"
linkTitle: "UCONF: Sentinel options"
weight: 240
---This section describes how to configure the Sentinel monitoring agent, which is part of Transfer CFT, using the following groups of UCONF parameters:

- sentinel.trkxxxxx: These parameters correspond to the trkxxxxx parameter described in the Sentinel Universal Agent User Guide .
- sentinel.xfb.xxxx: These parameters indicate how the Sentinel monitoring agent is used in Transfer CFT.
- Additional uconf values are related to the communication between the Sentinel monitoring agent and Sentinel
  - ssl.ciphersuites
  - ssl.version_min
  - ssl.extension.enable_sni
  - cft.ipv6.disable_connect
  - See [UCONF parameters](../../admin_intro/uconf/uconf_directory) for parameter details.
- sentinel.trktname: The path to the overflow file, where the maximum number of messages that the overflow file can store is equal to sentinel.xfb.buffer_size (not sentinel.trktmaxmsg).
- `sentinel.trktmaxmsg`: *Obsolete*. The maximum number of messages in the `sentinel.trktname` overflow file is defined by the sentinel.xfb.buffer_size.

## Overflow file

You can define an overflow file to store Sentinel events if the Sentinel server is not available, for example due to network issues or because the server is stopped. If Sentinel tracking is enabled but the server is not available, the message "CFTS30I XTRK Information Sentinel Connection failed" displays in the log. When the Sentinel connection is available again, the next event triggers the sending of previously-stored events.

Use the following uconf parameters to configure the name and size of the overflow file:

- sentinel.trktname: The path to the overflow file.
- sentinel.xfb.buffer_size: The maximum number of messages that the overflow file can store. Once the maximum value is reached, messages are no longer sent to Sentinel and:
  - If `sentinel.xfb.shut` is set to any value other than 0, Transfer CFT stops.
  - If `sentinel.xfb.shut` is set to 0, Transfer CFT continues to run.

> **Note**
>
> The sentinel.trktmaxmsg parameter is obsolete. Sentinel.xfb.buffer_size defines the maximum number of messages in the sentinel.trktname overflow file.

## Sentinel configuration parameters

The following table lists the Sentinel parameters in the unified configuration and the corresponding former Sentinel parameter.

| Unified configuration parameter  | Default value  | Former Sentinel parameter name<br/> trkapi.cfg |
| --- | --- | --- |
| sentinel.xfb.enable  | NO  | XFB.Sentinel (trkapi.cfg)  |
| sentinel.xfb.transfer | ALL | XFB.Transfer (trkapi.cfg) &lt;/p&gt; |
| sentinel.xfb.shut | 0 &lt;/p&gt; | XFB.Shut (trkapi.cfg) &lt;/p&gt; |
| sentinel.xfb.log | IEWF<br/> <blockquote> **Note**<br/> To disable, set to ' '.<br/> </blockquote>  | XFB.Log (trkapi.cfg) &lt;/p&gt; |
| sentinel.trktname | $(cft.runtime_dir)/data/trkapi.buf  | TRKTNAME (trkapi.cfg)  |
| sentinel.trksharedfile  | No  | TRKSHAREDFILE  |
| sentinel.trklenmsg  |   | TRKLENMSG  |
| sentinel.trklocmaxtime  | 300  | TRKLOCMAXTIME  |
| sentinel.trktmode (*[see below](#*sentinel.TRKTMODE)) | RETRY | TRKTMODE  |
| sentinel.trktconnretry  | 60 | TRKTCONNRETRY  |
| sentinel.trkretrydelay  | 10 | TRKRETRYDELAY  |
| sentinel.trkretrynb  | 6 | TRKRETRYNB  |
| sentinel.trkdelay  | 10 | TRKDELAY  |
| sentinel.trktimeout  | 60 | TRKTIMEOUT  |
| sentinel.trkproductname  | CFT  | TRKPRODUCTNAME  |
| sentinel.trkipaddr  | sentinel-server-hostname  | TRKIPADDR  |
| sentinel.trkipport  | 1761  | TRKIPPORT  |
| sentinel.trk_min_port  | 5000  | TRK_MIN_PORT  |
| sentinel.trk_max_port  | 32000 | TRK_MAX_PORT  |
| sentinel.trkipaddr_bkup |   | TRKIPADDR_BKUP  |
| sentinel.trkipport_bkup  | 1761  | TRKIPPORT_BKUP  |
| sentinel.trk_min_port_bkup  | 5000  | TRK_MIN_PORT_BKUP  |
| sentinel.trk_max_port_bkup  | 32000  | TRK_MAX_PORT_BKUP  |
| sentinel.trktype  | TCP  | TRKTYPE  |
| sentinel.trkgmtdiff  | 60  | TRKGMTDIFF  |
| sentinel.trktrcfile  | $(cft.runtime_dir)/run/sentinel.trc  | TRKTRCFILE  |
| sentinel.trktrace  | 0  | TRKTRACE  |
| sentinel.trktmaxmsg  | 100000 (0 indicates no limit)  | TRKTMAXMSG  |

For more information on event messages, refer to the Axway Sentinel documentation.

## Sentinel Heartbeat implementation parameters

The following table lists the Heartbeat parameters that you can set in the unified configuration.

| Unified configuration parameter  | Default value  | Description  |
| --- | --- | --- |
| sentinel.heartbeat.enable  | NO  | Enables sending Heartbeats to the Sentinel Server. |
| sentinel.heartbeat.periodicity  | 300  | The delay in seconds between sending Heartbeats.  |
| sentinel.heartbeat.script  | $(cft.install_dir)<br/> /extras/sentinel/MFTheartbeat.sh<br/> or<br/> $(cft.install_dir)<br/> \extras\sentinel\MFTheartbeat.bat | Script for executing Heartbeats.  |

## Sentinel parameters

The following table lists the parameters that you can set in the unified configuration.

| Unified configuration parameter  | Default value  | Description  |
| --- | --- | --- |
| sentinel.xfb.transfer_progress_period  | 60  | The frequency in seconds in which Transfer CFT notifies Sentinel (for both SENDING and RECEIVING states) that a transfer is running.<br/> 0 = no notification<br/>  |
| sentinel.xfb.transfer.send_relay_site_nidf  | No  | Enables an NIDF on the relay site. This uses an NIDF instead of COMMUT when sending an event to Sentinel using the XFBTransfer object.  |

<span id="sentinel.TRKTMODE"></span>\*sentinel.TRKTMODE

The mode that Transfer CFT uses to send messages to the Sentinel Server. The possible values of this parameter include:

- Immediat: When a connection to the Sentinel Server is established, Transfer CFT sends the messages that are saved in the overload file; it then sends new messages arriving from the application. If a connection does not exist, it attempts a connection to the Sentinel Server upon reception of each new message from the application.  
    For MVS, the messages in the overflow file are not automatically sent on connection.

<!-- -->

- Differ: Transfer CFT saves incoming messages in the overflow file.
- Retry: (default mode) Behaves as in the immediate mode (above), except that if the connection to the Sentinel Server fails, the application repeats connection attempts every `n` minutes. To configure the retry period set the UCONF sentinel.TRKTCONNRETRY parameter.

## Sentinel EventTime

By default, the data sent to Sentinel as the EventTime has the format HH:MM:SS. To add milliseconds to the format, HH:MM:SS.sss, set the Transfer CFT UCONF `cft.cftlog.time_precision` parameter, where:

- 1 (default): the time in CFTLOG displays in seconds
- 10: the time in CFTLOG displays in tenths of seconds
- 100: the time in CFTLOG displays in hundredths of seconds

If the` cft.cftlog.time_precision` value is greater than 1, the Transfer CFT EventTime message sent to Sentinel has the HH:MM:SS.dh0 format.

**Example**

```
uconfset id=cft.cftlog.time_precision, value=10
```

- 1 (default): the time in CFTLOG displays in seconds
- 10: the time in CFTLOG displays in tenths of seconds
- 100: the time in CFTLOG displays in hundredths of seconds

If the` cft.cftlog.time_precision` value is greater than 9, the Transfer CFT EventTime message sent to Sentinel has the HH:MM:SS.dh0 format.

**Example**

```
uconfset id=cft.cftlog.time_precision, value=10
```

## Sentinel cycle link metadata

The UCONF `sentinel.xfb.cyclelink.metadata` model defines the metadata attribute that Transfer CFT adds when it generates a Sentinel cycle link message. Please see [Symbolic variables](../../c_intro_userinterfaces/command_summary/symbolic_variables) for available symbolic variables.
