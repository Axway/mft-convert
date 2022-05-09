{
    "title": "Implementing the heartbeat functionality",
    "linkTitle": "Implement the heartbeat functionality",
    "weight": "230"
}When the Transfer CFT heartbeat function is activated, it sends the attributes to the Sentinel server via TRKUTIL. The Transfer CFT heartbeat combined with a Status Dashboard allows you to monitor Transfer CFT, providing information on the Transfer CFT status from indicators such as the Transfer CFT state, Transfer CFT catalog file space, process activity, version, memory and CPU usage, and so on.

For more information on Dashboards and tracked objects, refer to the Sentinel User's Guide.

> **Note**
>
> Note: To enhance performance, you can install the Event Router on the same machine as the Transfer CFT.

Check that the unified configuration values are set as follows for heartbeat functionality:

- sentinel.heartbeat.enable = YES

<!-- -->

- sentinel.heartbeat.periodicity = 300: This recommended value has a direct correlation with the value defined in the Monitoring requests.
- sentinel.heartbeat.script = D$CFT_INST:[extras.sentinel]MFTheartbeat.com: The script name and path according to your system environment.
- sentinel.trkipaddr = sentinel.server.address
- sentinel.trkipport = Sentinel.qlt/auto.port value (default = 1305)

<!-- -->

- sentinel.xfb.enable = YES
