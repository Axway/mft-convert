---
title: "wscan"
linkTitle: "wscan"
weight: 3800
---<span id="wscan"></span>

### wscan

#### CFTCOM

[WSCAN = {60
&#124; n}]  {1..3600}

Enter the frequency (in seconds) with which the {{< TransferCFT/axwayvariablesComponentShortName  >}} scans the communication
file:

- 60 (default value)
- any
    other value

<span id="wscan_CFTCAT"></span>

#### CFTCAT

[WSCAN = {5 &#124; n}]  {1..60}

Enter the frequency (in minutes) with which Transfer CFT scans the catalog file when restarting a transfer:

- 5 (default value)
- 1 to 60

****Behavior specifics****

Technically, the next retry is triggered **wscan** minutes after the previous try. However, sometimes you may have a {{< TransferCFT/axwayvariablesComponentLongName  >}} log where the 416 diagnostic codes are not evenly distributed (by the same time intervals). This may occur if the scheduling task believes resources are available and schedules a retry, but in reality the resource is taken.

[Return to Command index](../../)
