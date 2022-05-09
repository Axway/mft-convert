{
    "title": "Monitoring",
    "linkTitle": "Monitoring",
    "weight": "170"
}Monitoring Transfer CFT with Sentinel
-------------------------------------

You can use Sentinel to collect information about targeted Transfer CFT processing events. Sentinel enables you to generate graphic representations of these events (Dashboards) that you can then statistically and visually analyze, and respond to.

Sentinel provides you with tools that help you create pre-programmed automatic responses to specified network conditions. Using these tools, you can configure Sentinel to act immediately to handle exceptional and/or abnormal transfer events that occur in Transfer CFT. You can also configure Sentinel to respond to a programmed event that fails to occur.

Before you use the Sentinel tracking tools with Transfer CFT, you should become familiar with the Sentinel concepts and
user interface.
For Sentinel documentation, go to{{< TransferCFT/axwayvariablesCompanyName  >}} Support at [https://support.axway.com](https://support.axway.com/).

Required components
-------------------

To use Sentinel to monitor Transfer CFT you require the following components:

- A database
- Transfer CFT
- Sentinel and, optionally, Sentinel MonitoringPlus
- Universal Agent used for end-to-end application monitoring and Heartbeat functionality (*optional*)
- Event Router (*optional but recommended*)

You can install all components on the same machine or on different machines, and use the same or different platforms. When installed on the same machine, you can configure either a single or multiple users to access the components. Additionally when installed on the same machine, you can use the same database for the Composer and Sentinel server.

Transfer CFT processing
-----------------------

A Sentinel monitoring agent resides natively on Transfer CFT. This agent generates Tracked Event Messages that contain data about Transfer CFT processing events. The agent then sends the Tracked-Event messages to the Sentinel server environment.

Sentinel processing
-------------------

In the Server environment, the Acquisition Server contains Tracked Objects. A Tracked Object is a model containing a set of attributes that describe an application event. Each incoming Tracked-Event Message contains a name field that indicates the name of the Tracked Object that is to be used with that message. Using the specified Tracked Object, the Acquisition Server extracts the data from the fields of the Tracked-Event Message, and writes the data to a specific table in the Tracking Database.

For Transfer CFT monitoring, Sentinel uses the following Tracked Objects (TO):

- [XFBTransfer](intro_sentinel)
- [XFBLog](xfblog)

Requests
--------

Once data is recorded in the Tracking database, you can then use the full array of Sentinel functionalities to track the flow of messages in Transfer CFT as well as in other applications across your network.

Sentinel uses sets of executable SQL instructions known as ****Requests**** to retrieve data from tables in the Tracking Database. Transfer CFT provides a set of predefined Requests that you can use in Sentinel to facilitate retrieval of data specific to Transfer CFT events.

You can use a set of predefined [Requests](xfbtransfer_request)
to:

- Globally monitor
    all transfers
- Monitor specific
    transfers
- Monitor specific
    steps in the transfer process
- Identify the sources
    of transfer errors

You can set Sentinel to repeatedly execute requests. This
means that you can retrieve and display transfer details as soon as
Sentinel receives them from Transfer CFT.

For more information, refer to the Axway *Sentinel* documentation available on the support site, {{< TransferCFT/axwayvariablesCompanyName  >}} Support at [https://support.axway.com](https://support.axway.com/)..
