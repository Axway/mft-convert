{
    "title": "Trace  functions in CFT Windows",
    "linkTitle": "Trace functions",
    "weight": "270"
}This topic introduces trace functions in Transfer CFT Windows. Trace
functions are processes that concern the system and network sections of
Transfer CFT. These functions do not concern the application traces implemented
in common by all the existing Transfer CFT products and described in the
Trace function topics.

### Automatic incident tracing

Transfer CFT automatically generates a trace when it detects a system
or network error that is fatal to its operation. Some of
these traces are informational. These traces are specific to the
Transfer CFT/Windows products.

Depending on the origin of the problem it has detected, these traces
are recorded in one of the following files:


| Name of the Trace file  | Source of the problem  |
| --- | --- |
| CFTSYSP.TRC  | Intermediate system section  |
| CFTSYSS.TRC  | Specific Windows system section  |
| CFTNET.TRC  | Network section  |
| TCPCLI.TRC and TCPSRV.TRC  | TCP/IP network section  |


By default Transfer CFT automatically generates these trace files in the folder:`runtime\run folder`

You can use UCONF to change the folder.
For example:

`CFTUTIL uconfset id=copilot.trace.trcfilename,value=c:\trc\copilot.trc`

Only Axway Technical Support is able
to interpret these traces. In a certain cases, Axway Technical Support may
request users to send the content of these files.

The maximum size and the location of these specific
trace files can be defined.

Defining the size of specific trace files

****Environment variable****

`CFTTRCSIZE`

CFTTRCSIZE defines the maximum size of each of the trace files in megabytes.  
If this value is set to 0, no trace file will be generated. If the environment
variable has not been defined, the maximum size of these files is 100
Mb. Once the maximum file size has been reached, Transfer CFT/Windows
saves the file as \*b.trc and sets the file to 0.

Defining the location of specific trace
files

****Environment variable****

`CFTTRCPATH`

CFTTRCPATH indicates the Transfer CFT/Windows the name of the folder
where trace files discussed in this section are generated.

For more information, read the section Specific
Network Functions

### Network traces

Refer to [Specific Network Functions](../../running_cft_for_the_first_time_windows/specific_network_functions).
