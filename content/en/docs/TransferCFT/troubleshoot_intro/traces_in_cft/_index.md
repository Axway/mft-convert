---

    title: How to use ATM traces
    linkTitle: Using ATM traces
    weight: 270

---
This section describes ATM traces and how to implement them.

> **Note**
>
> ATM traces are only available when using the Local Administration version of Transfer CFT. We recommend using Central Governance to manage Transfer CFT.


| Topic  | Description  |
| --- | --- |
| <a href="trace_management">Trace management concepts</a> | Describes the concepts behind performing an ATM trace in Transfer CFT. |
| <a href="parameter_settings">Defining trace in {{< TransferCFT/axwayvariablesComponentShortName  >}} parameters</a> | Describes the Transfer CFT general parameter for a trace. |
| <a href="defining_a_trace_file_externally">Defining a trace file externally (CFTTRACE utility)</a> | Describes how to create or remove a trace file, or to reinitialize it with an empty useable content. |
| <a href="defining_the_internal_trace_file">Defining the internal trace file (CFTUTIL command)</a> | Describes how to create a trace when Transfer CFT starts, with the possibility of tracing an initialization sequence, or during Transfer CFT operations. |
| <a href="">Using the start trace command</a> | Describes the start trace command, which is associated with a unique identifier, defines and describes the conditions for starting and selecting traced data, and associates a file identifier. |


## About Transfer CFT traces

Transfer CFT traces are managed by the <span style="font-weight: bold;">****A****</span>dvanced
<span style="font-weight: bold;">****T****</span>race <span style="font-weight: bold;">****M****</span>anager
(ATM) component.

ATM is a problem resolution assistance tool that is used:

- To save the information
    exchanged at the {{< TransferCFT/axwayvariablesComponentLongName >}} level, in one or more dedicated files

The information traced relates to protocol information
(exchanges between Transfer CFT and its remote partners) and/or Transfer
CFT internal information (exchanges between Transfer CFT internal components
or between Transfer CFT tasks).

- To retrieve and
    interpret previously saved information, through an off-line analysis program

Users are only concerned by ATM in that they may need to initiate tracing,
at the request of the Transfer CFT Support service. The Transfer CFT Support
service is then responsible for analyzing traces.

The next section introduces the concepts
surrounding the trace mechanism and how this works in Transfer CFT.
