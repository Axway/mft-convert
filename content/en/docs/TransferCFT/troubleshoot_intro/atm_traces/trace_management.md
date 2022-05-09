{
    "title": "Trace  management concepts",
    "linkTitle": "Trace management concepts",
    "weight": "290"
}ATM trace management is comprised of two stages:

- Collect information,
    or trace acquisition, in Transfer CFT
- Examine the information, outside Transfer CFT

Only the collection stage is covered in this section. The Transfer CFT support staff is responsible for interpreting the information.

The following sections describe:

- How to define a trace
    , either before starting up Transfer CFT or during operations
- How to start/stop
    data collection with application events linked to transfers

<span id="Information_Collection"></span>

How to collect information
--------------------------

Trace implementation involves these
operations:

- Defining
    the trace files
- Controlling the traces, for example:
    activate, close files, or shutdown of the process

<span id="Defining_trace_files"></span>

### Defining trace files

You can activate one or more trace acquisition processes to supply one or more trace files. Additionally, you can define the trace file using one of two methods:

- CFTUTIL - use the CFTTRACE command, which is taken into account
    when Transfer CFT is started
- CFTTRACE utility - use the SETTRC command, dynamically sending to the Transfer CFT communication medium. You can perform the SETTRC command before starting Transfer
    CFT.

The word *process* is not used here as a synonym for task. There is at
most one task dedicated to trace acquisition in Transfer CFT. In general, when a CFTTRACE or SETTRC command is taken into account, this
is accompanied by the physical creation and initialization of the corresponding
trace file. In some environments, especially on mainframe platforms, this
operation must be carried out in advance, using a TRCFILE TYPE=TRACE command.

This command is also used to define a direct file.

<span id="Managing_information_collection"></span><span id="How_to_start_and_stop_the_information_collection_process"></span>

Start /stop the information collection
--------------------------------------

This section describes how to begin and end the information collecting
process.

<span id="Starting_information_collection"></span>

### Start information collecting

To start information collection:

- When Transfer CFT starts up,
    enter the CFTTRACE START parameter (START=CFT).
- During Transfer CFT operations,
    enter a STARTTRC command.

Information collection is managed by a trace server
task, which makes it possible to resolve trace file access conflict problems.
Serialization is ensured by an internal flag setting mechanism, which
enables message exchanges and synchronization of Transfer CFT tasks.

The collection operation does not pre-empt or significantly
disturb other Transfer CFT mechanisms.

Consequently, if there is a bottleneck in the flag
mechanism (if the volume of traces requested exceeds flagging capabilities
or system resources), messages are purely and simply lost.

Trace mechanisms include the following features:

- You can make traces
    started at the same time as Transfer CFT co-exist with other traces,
    triggered during Transfer CFT operations, for the same trace file or for
    different trace files.
- You can create
    a trace file with the CFTTRACE command, without having to synchronize
    collection with Transfer CFT start-up. To do this, set the START
    parameter to DELAYED.
- If a STARTTRC
    command is entered before the corresponding file has been defined, it
    is simply rejected and is without effect.
- Whether there are
    one or more CFTTRACE commands, a single task ensures that all the
    trace files are filled.

### Stop the collection process

To stop
collecting, enter the command: `STOPTRC`  
The command is transmitted to Transfer CFT, which immediately shuts
down information collection defined for this trace and destroys the corresponding
vector.

Note:

- You can restart the process with STARTTRC; it is possible to use new initialization parameters.
- If the same information
    is requested in several different traces and only one of these traces
    is shutdown, then the information is still traced (according to the
    definitions of the other traces still active).

<span id="Stopping"></span>

### Close a trace and shut down the process

To close a trace file and shutdown the process, enter the `SETTRC   MODE=DELETE` command.

Transfer CFT processes the command as follows:

1. All traces with output to the
    file in question are shutdown immediately.
1. The trace file entry is deleted
    by the ATM manager.

The ATM component’s server task shutdowns as soon
as there are no more trace files to manage, that is, that there are no
more active trace acquisition processes.

When Transfer CFT shuts down
(SHUT), all the trace files are closed and the server task shuts down.

<span id="Filtering_Traces_by_partner"></span>

Filtering traces by partner
---------------------------

This function enables you to filter FPDU protocol traces
by partner.

****Example
&lt;/b&gt;&lt;/p&gt;****

`CFTTRACE STARTTRC ID=ID,TID=TID,PTRACE=16,FILTER=Part`

The command to start a trace FPDU (PTRACE=16) only applies to the `Part `partner. When you start a Trace, a new message is written
in the LOG File with a CFTT57 message:

`CFTT92I IDTU=&idtu CTX=&ctx IDT=&idt  `
