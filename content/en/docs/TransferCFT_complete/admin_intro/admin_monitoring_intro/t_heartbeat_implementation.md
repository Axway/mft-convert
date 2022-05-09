{
    "title": "Implementing Heartbeats for Dashboards",
    "linkTitle": "Implementing Heartbeats for Dashboards",
    "weight": "280"
}When the Transfer CFT heartbeat function is activated, it sends the attributes, or indicators, to the Axway Sentinel server via TRKUTIL. The Transfer CFT heartbeat combined with a status Dashboard allows you to monitor Transfer CFT, having information on the Transfer CFT status from indicators such as the Transfer CFT state, Transfer CFT catalog file space, process activity, version, memory and CPU usage, and so on.

Configuring Transfer CFT
------------------------

To enable the heartbeat feature, check that the following [unified configuration](../../uconf/uconf_parameters) parameters are set to:

- **sentinel.heartbeat.enable = YES**
- sentinel.heartbeat.periodicity = 300
- sentinel.heartbeat.script = the installed value points to the script by default
- **sentinel.xfb.enable = YES**
- sentinel.trkipaddr = sentinel.server.address
- sentinel.trkipport = Sentinel.qlt/auto.port value (default = 1305)  

********Example********

In Windows the following would enable the parameter to activate Heartbeat functioning.

```
cftutil uconfset id=sentinel.xfb.enable, value=yes
cftutil uconfset id=sentinel.heartbeat.enable, value=yes
cftutil uconfset id=sentinel.heartbeat.periodicity, value=300
cftutil uconfset id=sentinel.heartbeat.script, value= %cftdirinstall%\\extras\\sentinel\\MFTheartbeat.bat
cftutil uconfset id=sentinel.trkipaddr, value=server.sentinel.address
cftutil uconfset id=sentinel.trkipport, value=11277
```
