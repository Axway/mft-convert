{
    "title": "Transfer CFT to Sentinel communication parameters",
    "linkTitle": "Sentinel communication parameters",
    "weight": "240"
}To define to enable Sentinel to Transfer CFT interoperability, refer to:

- [](#Communication%20with%20the%20Event%20Router)[Communication with an Event via TCP all platforms](#Communication%20with%20an%20Event%20via%20TCP) 
- [UCONF parameters for Sentinel interoperability all platforms](#UCONF%20parameters%20for%20Sentinel%20interoperability)
- [Communication with the Event Router](#Communication%20with%20the%20Event%20Router) [for z/OS](#Communication%20with%20the%20Event%20Router)   
- [Overflow file definition for z/OS](#Overflow%20file%20definition) 

<span id="Communication with an Event via TCP"></span>

Communication with an Event via TCP
-----------------------------------

**All platforms**

This table lists the main parameters that you must define in order to communicate with an Event via TCP, and uses TRKTYPE=TCP as the default value.


|   | Event Router  | TRKUTIL  | Transfer CFT  |
| --- | --- | --- | --- |
| Configuration file  | USEPARIN  | TRKCONF  | UCONF  |
| TCP definition  | (AGENT)<br/> queue=”NOQ” | TRKTYPE=TCP  | UCONFSET ID=sentinel.TRKTYPE,VALUE=TCP  |
| TCP definition  | (TCPSOURCE) sap=nnnn  | TRKIPPORT=nnnn  | UCONFSET ID=sentinel.TRKIPPORT,VALUE=nnnn  |
| TCP definition  | (TCPSOURCE)<br/> local_address= | TRKIPADDR=  | UCONFSET ID=sentinel.TRKIPADDR,VALUE=  |


<span id="UCONF parameters for Sentinel interoperability"></span>

UCONF parameters for Sentinel interoperability
----------------------------------------------

**All platforms**

The table below lists the UCONF values that are used in Transfer CFT to define Sentinel interoperability. You can set these UCONF values using either the Copilot UI or command line.


| Unified configuration parameter  | Default value  | Former Sentinel parameter name<br/> trkapi.cfg |
| --- | --- | --- |
| sentinel.xfb.enable  | NO  | XFB.Sentinel (trkapi.cfg)  |
| sentinel.xfb.transfer | ALL | XFB.Transfer (trkapi.cfg) &lt;/p&gt; |
| sentinel.xfb.shut | 0 &lt;/p&gt; | XFB.Shut (trkapi.cfg) &lt;/p&gt; |
| sentinel.xfb.log | IEWF | XFB.Log (trkapi.cfg) &lt;/p&gt; |
| sentinel.trktname | $(cft.runtime_dir)/data/trkapi.buf  | TRKTNAME (trkapi.cfg)  |
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
| sentinel.trktrcfile  | $(cft.runtime_dir)/run/sentinel.trc  | TRKTRCFILE  |
| sentinel.trktrace  | 0  | TRKTRACE  |


<span id="Communication with the Event Router"></span>

 Communication with the Event Router
------------------------------------

**Z/OS environment**

The following parameters define communication with the Event Router via XCF. In this setup:

- The XCF definition (queue=xxxx) is the XCF member name representing the ER server
- The XCF group is PELISCOP by default. You can modify this default by setting queue = “member group”


|   | ER  | TRKUTIL  | Transfer CFT  |
| --- | --- | --- | --- |
| Configuration file  | USEPARIN  | TRKCONF  | UCONF  |
| SVC  | (SYSTEM)<br/> svc_nb=nnn | TRKSVC=nnn  | UCONFSET ID=sentinel.TRKSVC,VALUE=nnn  |
| XCF definition  | (AGENT)queue=  | TRKQUEUE=  | UCONFSET ID=sentinel.TRKQUEUE,VALUE=xxxx  |
| XCF definition  | (AGENT)queue=  | TRKTYPE=XCF  | UCONFSET ID=sentinel.TRKTYPE,VALUE=XCF  |


<span id="Overflow file definition"></span>

Overflow file definition
------------------------

**Z/OS environment**

The following table describes the overflow file definition for the Logger file. In this setup:

- TRKSHAREDFILE=YES is MANDATORY when the logger file is shared between the {{< TransferCFT/suitevariablesEventRouterName  >}} and other applications. Set this to NO if the applications are sending messages directly to the Sentinel server without going through the {{< TransferCFT/suitevariablesEventRouterName  >}}
- The log structure is ONLY used to define a logger file shared between the partitions of the SYSPLEX, and is NOT referenced in any parameters


|   | Event Router  | TRKUTIL  | Transfer CFT  |
| --- | --- | --- | --- |
| Configuration file  | USEPARIN  | TRKCONF  | UCONF  |
| Logger file  | (AGENT)<br/> api_file= | TRKTNAME=  | UCONFSET ID=sentinel.TRKTNAME, VALUE=xxxx.xxxx.xxx  |
| Logger file  | (AGENT)<br/> api_file= | TRKSHAREDFILE=YES  | UCONFSET ID=sentinel.TRKSHAREDFILE,VALUE=YES  |


****Related topics****

****[About Axway Sentinel](../../../../../using_sentinel)****

****[Using the unified configuration GUI](../../../../../admin_intro/uconf/uconf_userinterface)****

****[Using the unified configuration CFTUTIL](../../../../../admin_intro/uconf/uconf_w_cftutil)****

****[Installing Sentinel for Transfer CFT z/OS](../../../overview_install_zos/manual_installation_steps/t_install_sentinel_zos)****
