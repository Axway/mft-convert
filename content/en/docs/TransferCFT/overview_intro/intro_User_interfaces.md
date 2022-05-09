{
    "title": "User  interface options",
    "linkTitle": "User interface options",
    "weight": "160"
}Select the appropriate user interface according to tasks you want to perform.

Configuration, monitoring, and flow management
----------------------------------------------

****{{< TransferCFT/suitevariablesFlowManager  >}} and {{< TransferCFT/PrimaryCGorUM  >}}****

{{< TransferCFT/suitevariablesFlowManager  >}} and Central Governance are {{< TransferCFT/axwayvariablesCompanyName  >}} user interfaces that provides a complete set of services to govern {{< TransferCFT/axwayvariablesComponentShortName  >}}. For more information, see [Governance services](../c_cg_concepts).

- For details on creating your first {{< TransferCFT/axwayvariablesComponentLongName  >}} flows using {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, see [Getting started](../../troubleshoot_intro/collecting_information/gettingstarted_intro).
- For details on integrating {{< TransferCFT/suitevariablesCentralGovernanceName  >}} with {{< TransferCFT/axwayvariablesComponentLongName  >}}, see [Integration overview](../../governance_services_intro/governance_overview).
- Refer to the OS specific installation guide for instructions on how to activate {{< TransferCFT/suitevariablesCentralGovernanceName  >}} connectivity and register your Transfer CFT.

Runtime activities
------------------

### Browser based user interface

{{< TransferCFT/suitevariablesTransferCFTName  >}} features a web browser user interface that you can use to create, track and manage transfers, manage security, and mange the Transfer CFT log.

<span id="CFTUTIL_interface"></span>

### CFTUTIL interface

The CFTUTIL interface provides a batch or interactive line-mode interface
to execute {{< TransferCFT/axwayvariablesComponentShortName  >}} operating commands.

The CFTUTIL commands that are described in this documentation are collectively listed
in the topic [Command index](../../c_intro_userinterfaces/command_summary). You can use this as a quick reference
to find command syntax and parameters.

### Application Programming Interface and REST API

An Application Programming Interface, or API, is a way to interface your application with {{< TransferCFT/axwayvariablesComponentLongName  >}}. In Transfer CFT, APIs operate as a set of functions
that use a service.

In Transfer CFT you can use:

- C, COBOL, and JPI APIs to create customized and automated transfer and monitoring services for a more tightly knit application integration.
- REST APIs to create and monitor transfers, and to configure {{< TransferCFT/suitevariablesTransferCFTName  >}}. Each supported Transfer CFT REST API command provides a description, which includes possible parameters.

### Web services interface

You can use the Web services option to open a {{< TransferCFT/axwayvariablesComponentShortName  >}} session.
Using {{< TransferCFT/axwayvariablesComponentShortName  >}} Web Services, you can access all main functions for
managing {{< TransferCFT/axwayvariablesComponentShortName  >}}. These functions include:

- Monitoring Transfer
    CFT: consult the log, the catalog, and transfer statistics
- Creating transfers:
    create a new file transfer, a message, or transfer reply

The Web Services requests sent by the client are processed on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server listening port.
