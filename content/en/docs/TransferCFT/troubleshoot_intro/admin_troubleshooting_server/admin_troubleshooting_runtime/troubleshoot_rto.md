{
    "title": "Troubleshoot 240 or 740 RTO",
    "linkTitle": "Troubleshoot RTO 240 or 740",
    "weight": "480"
}****Issue: An error occurs with a 240 or 740 RTO****

This error may occur when a transfer CFT timeout occurs between the select and deselect file steps during the transfer phase.

A timer, which is set in the CFTPROT command RTO parameter, is triggered for each step of the transfer phase. If the timer limit is reached, the transfer is interrupted and the resulting status of the transfer is D. You can still execute the transfer, though, using the RETRY mechanism.

****Cause****

240 RTO can have one of two causes:

- Network issues (MTU, Maximum Transmission Unit, or congestion)
- Abnormal termination or high CPU usage

It is important to diagnose the root cause, as the solution may depend on the cause.

### Network issue

****MTU issue****

Various routers may be traversed when going from the local host to the remote host. If one of the routers is not configured properly, the MTU size for example, this may causes network issues.

You can use the ping command to find the appropriate MTU size. For example, on Windows:

```
ping <remote address> -l 1472 -f
```

Where:

- -l: Sets the size in bytes (1500 - 28)
- -f: Sets the *Do not fragment option*

Reducing the RUSIZE can avoid this root cause. However this should be fixed by setting properly the network environment.

****Network congestion (latency, lost packets, timeouts)****

To confirm if it is a network congestion issue, you can use the ping command or a tool such as MTR (My traceroute), which is a combination of traceroute and the ping command. Sometimes, the timer can expire locally whereas the session is still active on the remote side.

Therefore, if the transfer in 240 RTO status is restarted on a new session, the  remote side may get a `Transfer is already in progress` message and the transfer fails.

The solution:

- Set CNXINOUT=1 for this partner
- Set SCHKW/RCHKW=1 to simulate a half duplex communication

### Abnormal termination or hanging task

****{{< TransferCFT/axwayvariablesComponentLongName  >}} task has an abnormal termination****

This can cause the timer to expire and induces the 240 RTO diagnostic. Check that no core dump is detected and execute a cft_support collect before stopping Transfer CFT, or before killing the {{< TransferCFT/axwayvariablesComponentLongName  >}} processes if that's necessary.

Restart Transfer CFT. The transfer in 240 RTO status is normally restarted, but if the problem still persists cancel the transfer causing the problem and restart Transfer CFT.

****Hanging or frozen task, or high CPU usage****

These issues can also cause the timer to expire and induce the 240 RTO. You can check with various commands or system tools for frozen {{< TransferCFT/axwayvariablesComponentLongName  >}} tasks, or if there is a high CPU consumption.

Execute cft_support collect, and if necessary force a dump on a specific task in order to collect information. For example, on Unix:

```
kill -3 <process id>
```

The solution may be a mix of system tuning and Transfer CFT configuration tuning.

Transfer CFT parameters that may increase CPU usage are:

- MAXTRANS/MAXTASK (CFTPARM)
- WAITTASK (CFTPARM): recommended value 1441 on all platforms and 5 on z/OS
- WSCAN/UPDAT (CFTCAT)
- WSCAN (CFTCOM)
- Processing scripts (pre-processing, postprocessing scripts) overload
- CFTCRON
