---
    "title": "Manually enable Sentinel ",
    "linkTitle": "Manually enable Sentinel",
    "weight": "240"
---
When using {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, the visibility features are managed by {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. Do not modify these parameters when running with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

Sentinel configuration parameters
---------------------------------

The following table lists the Sentinel parameters in the unified configuration and the corresponding former Sentinel parameter.


| UCONF parameter  | Default  | Former Sentinel parameter<br/> TRKCNF |
| --- | --- | --- |
| sentinel.xfb.enable  | NO  | XFB.Sentinel (TRKCNF)  |
| sentinel.xfb.transfer | ALL | XFB.Transfer (TRKCNF) &lt;/p&gt; |
| sentinel.xfb.shut | 0 &lt;/p&gt; | XFB.Shut (TRKCNF) &lt;/p&gt; |
| sentinel.xfb.log | IEWF | XFB.Log (TRKCNF) &lt;/p&gt; |
| sentinel.trktname | $(cft.runtime_dir)<br /> /data/trkapi.buf  | TRKTNAME (TRKCNF)  |
| sentinel.trksharedfile  | No  | TRKSHAREDFILE  |
| sentinel.trklenmsg  |  | TRKLENMSG  |
| sentinel.trklocmaxtime  | 300  | TRKLOCMAXTIME  |
| sentinel.trktmode  | DIFFER | TRKTMODE  |
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
| sentinel.trkipaddr_bkup |  | TRKIPADDR_BKUP  |
| sentinel.trkipport_bkup  | 1761  | TRKIPPORT_BKUP  |
| sentinel.trk_min_port_bkup  | 5000  | TRK_MIN_PORT_BKUP  |
| sentinel.trk_max_port_bkup  | 32000  | TRK_MAX_PORT_BKUP  |
| sentinel.trktype  | TCP  | TRKTYPE  |
| sentinel.trkgmtdiff  | 60  | TRKGMTDIFF  |
| sentinel.trktrcfile  | $(cft.runtime_dir)<br /> /run/sentinel.trc  | TRKTRCFILE  |
| sentinel.trktrace  | 0  | TRKTRACE  |
| sentinel.xfb.transfer_progress_period<br/> The frequency in seconds in which Transfer CFT notifies Sentinel (for both SENDING and RECEIVING states) that a transfer is running.<br/> 0 = no notification | 60  |   |
| sentinel.xfb.transfer.send_relay_site_nidf<br/> Enables an NIDF on the relay site. This uses an NIDF instead of COMMUT when sending an event to Sentinel using the XFBTransfer object. | No  |   |


For more information on event messages, refer to the Axway Sentinel documentation.

About Transfer CFT heartbeat functionality
------------------------------------------

{{% TransferCFT/snippets/heartbeat%}}

### Sentinel Heartbeat implementation parameters

The following table lists the Heartbeat parameters that you can set in the unified configuration.

Each Transfer CFT environment number n (from 1 to 5) has its own corresponding Heartbeat script. You should check the default names (such as in the production library, jobd, and Transfer CFT file) that are used in the script.


| Unified configuration parameter  | Default value  | Description  |
| --- | --- | --- |
| sentinel.heartbeat.enable  | NO  | Enables sending Heartbeats to the Sentinel Server. |
| sentinel.heartbeat.periodicity  | 300  | The delay in seconds between sending Heartbeats.  |
| sentinel.heartbeat.script  | CFTPROD/HEARTBEAT | Script for executing Heartbeats.  |


Example

```
uconfset id=sentinel.heartbeat.enable,value=yes
uconfset id=sentinel.heartbeat.periodicity,value=300
uconfset id=sentinel.heartbeat.script,value=CFTPROD/HEARTBEAT
uconfset id=sentinel.trkipaddr,value=serveur.sentinel.address
uconfset id=sentinel.trkipport,value=11277
uconfset id=sentinel.trklocaladdr,value=as400.local.address
```
