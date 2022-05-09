{
    "title": "Implement the Edge Agent",
    "linkTitle": "Implement the Edge Agent",
    "weight": "200"
}This page describes how to configure the Edge Agent for AMPLIFY MFT implementations and configure {{< TransferCFT/headerfootervariableshflongproductname  >}} for the following use cases:

- Usage tracking only
- Usage tracking and Sentinel monitoring

********Usage tracking only********

![](/Images/TransferCFT/edge_direct.png)

********Usage tracking and Sentinel monitoring********

![](/Images/TransferCFT/edge_indirect.png)

Report contents are computed once daily and reflect the completed transfers from the preceding day. Depending on your start and end dates, which must be at the very least the previous day, the report will contain the completed transfers for that time period.

For more information on reporting usage, Edge Agent setup and architecture, and other usage tracking details, please refer to the [AMPLIFY Usage Metering and Reporting Guide](https://docs.axway.com/bundle/subusage_en).

Configure the Edge Agent
------------------------

Perform the following steps on the Edge Agent for MFT implementations that use {{< TransferCFT/headerfootervariableshflongproductname  >}} or {{< TransferCFT/suitevariablesSecureTransportName  >}}.

1. Download the `AMPLIFY_Edge_Agent_MFT_<version>_configuration_<BNxxx>.zip `package from the [Axway Support Site](https://support.axway.com/).
1. Extract the zip locally.
1. Upload the `MFT-usage.json` file from the package to the `<Edge_Agent_install_dir>/aggregator/usage_tracking/``conf/agent/aggregation` directory.
1. Upload the `MFT.json` file from the package to the `<Edge_Agent_install_dir>/conf/agent/report` directory.
1. Restart the Edge Agent. Refer to the [AMPLIFY Usage Metering and Reporting Guide](https://docs.axway.com/bundle/subusage_en).

> **Note**
>
> Note: The MFT.json file name is the configurationName that you use when querying the Edge Agent.

Configure Transfer CFT
----------------------

There are two methods of configuration depending on the use case you implement.

### Usage tracking only

In this use case, Transfer CFT sends the usage report directly to the Edge Agent.

Set the following `uconf `parameters to the Edge Agent values:

- sentinel.trkipaddr: Edge Agent IP address
- sentinel.trkipport: 8002 (by default, the non-SSL port for the Edge Agent)
- sentinel.xfb.use_ssl: No

### Usage tracking with the Edge Agent and monitoring with Sentinel

In this use case, Transfer CFT uses the Event Router to send notifications to both Sentinel and the Edge Agent.

You require an installed Sentinel and Event Router to implement this method. You may want to also refer to the [Sentinel Installation Guide](https://docs.axway.com/bundle/Sentinel_420_InstallationGuide_allOS_en_HTML5/page/Content/AxwayStartPage.htm).

#### On Transfer CFT 

Set the following uconf parameters to the Edge Agent values:

- sentinel.trkipaddr: Event Router IP address
- sentinel.trkipport: Event Router listening port

#### On the Sentinel server

1. On Sentinel, copy the `XFBCFTInfo `and `XFBTransfer` Tracked Object files from the` <Transfer_CFT_install_dir>/home/extra/sentinel` to `<Sentinel_install_dir>/broadcast/commit/trackingobject `folder.  
    If the Tracked Objects folder does not exist, you must create it.
1. Restart Sentinel.

#### On the Event Router

When you are using the Event Router to send both usage tracking to the Edge Agent and monitoring to Sentinel, you must customize the Event Router. In the following configuration steps, `XFBTransfer `and `CycleLink `are sent to both the Edge Agent and Sentinel. However, `XFBCFTInfo `and `STXFBINFO `are only sent to the Edge Agent.

> **Note**
>
> Note: The default target is called SENTINEL in the steps below.

1. Access the `<install_dir>/SentinelEventRouter/conf `directory.
1. Edit the `target.xml` file to route the usage information to Sentinel and the Edge Agent (`EDGEAGENT`).
    1.  Add the Edge Agent as a new target.
    2.  ```
        <Target name="EDGEAGENT" defaultXntf="no" defaultXml="no">
        <Access mode="QLT" addr="<Edge_Agent_IP_address>" port="8002" />
        </Target>
        ```
    3.  Define a route to send the `XFBTransfer `Tracked Object to the Edge Agent.
    4.  ```
        <Route object="XFBTransfer" default_Notify="NotifyIf">
        <Condition notify="NotifyIf" target="EDGEAGENT" if="
        [PRODUCTIPADDR] NOT _"/>
        </Route>
        ```
    5.  Define a route to send the `CYCLELINK `to the Edge Agent.
    6.  ```
        <Route object="CYCLELINK" default_Notify="NotifyIf">
        <Condition notify="NotifyIf" target="EDGEAGENT" if=" [PRODUCTIPADDR] NOT _"/>
        </Route>
        ```
    7.  Define a route to send the `XFBCFTInfo `only to the Edge Agent.
    8.  ```
        <Route object="XFBCFTInfo" default_Notify="NotifyIf">
        <Condition notify="NotNotifyIf" target="SENTINEL" if="[PRODUCTIPADDR] NOT _"/>
        <Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
        </Route>
        ```
    9.  If you are also implementing SecureTransport, define a route to send the `STXFBINFO `only to the Edge Agent.
    10. ```
        <Route object="STXFBINFO" default_Notify="NotifyIf">
        <Condition notify="NotNotifyIf" target="SENTINEL" if="[PRODUCTIPADDR] NOT _"/>
        <Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
        </Route>
        ```
1. Save the file.
1. Restart the Event Router.

****Example****

```
<TrkEventRouterCfg>
<TrkXml version="x.x" />
<EventRouter name="DEFAULT">
</EventRouter>
<Target name="SENTINEL" defaultXntf="yes" defaultXml="yes">
</Target>
<Target name="EDGEAGENT" defaultXntf="no" defaultXml="no">
<Access mode="QLT" addr="<Edge_Agent_IP_address>" port="8002" />
</Target>
<Route object="XFBTransfer" default_Notify="NotifyIf">
<Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
</Route>
  <Route object="CYCLELINK" default_Notify="NotifyIf">
    <Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
   </Route>
<Route object="XFBCFTInfo" default_Notify="NotifyIf">
<Condition notify="NotNotifyIf" target="SENTINEL" if="[PRODUCTIPADDR] NOT _"/>
<Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
</Route>
  <Route object="STXFBINFO" default_Notify="NotifyIf">
<Condition notify="NotNotifyIf" target="SENTINEL" if="[PRODUCTIPADDR] NOT _"/>
<Condition notify="NotifyIf" target="EDGEAGENT" if="[PRODUCTIPADDR] NOT _"/>
</Route>
</TrkEventRouterCfg>
```

> **Note**
>
> Note: Do not modify the [PRODUCTIPADDR].
