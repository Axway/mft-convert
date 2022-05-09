{
    "title": "XFBTransfer predefined requests",
    "linkTitle": "XFBTransfer predefined requests",
    "weight": "230"
}XFBTransfer requests
--------------------

When you configure Sentinel, you can import a set of predefined XFBTransfer
requests into the Monitoring interface. The following table describes
these Requests. All of these Requests retrieve Tracked Instances
from XFBTransfer.


| Request  | Retrieves...  |
| --- | --- |
| CurrentTransfers | All Tracked Instances from the Current Table of XFBTransfer. |
| CurrentTransfersToday | Tracked Instances from the Current Table of XFBTransfer that correspond to the current date. |
| CurrentTransfersAlert | Tracked Instances from the Current Table of XFBTransfer that correspond to an Alert. |


XFBLog request
--------------

When you configure Sentinel with Transfer CFT, you can import
a predefined request for XFBTransfer **TransferLog**.
This Request retrieves Tracked Instances that describe the contents of
a {{< TransferCFT/axwayvariablesComponentShortName  >}} log.

****Related topics****

- [XFBLog Tracked Object](../xfblog)
