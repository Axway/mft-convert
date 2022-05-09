{
    "title": "Use an internal trace (CFTUTIL command)",
    "linkTitle": "Use an internal trace (CFTUTIL command)",
    "weight": "320"
}Depending on how the trace is started, the file defined is available:

- When Transfer CFT
    starts up, with the possibility of tracing an initialization sequence
- During Transfer
    CFT operations

<span id="Trace_commands"></span><span id="Trace_parameter_setting_commands"></span>Trace parameter setting
commands

CFTUTIL parameter setting commands, grouped by function, are presented in the
following table.


| Action &lt;/th&gt;  | Command &lt;/th&gt;  |
| --- | --- |
| Update the general parameters before Transfer CFT start-up  | CFTPARM |
| Define the trace file or files:<br/> • Before Transfer CFT starts <br/> • During Transfer CFT operations  | CFTTRACE<br />  |
| Start information collection:<br/> • Transfer CFT start<br/> • During Transfer CFT operations  |  <br/> CFTTRACE<br />  |


<span id="Trace_command_overview"></span>

### Trace command overview

The following tables are an example of the commands and parameters to
be used for the various trace processes.

#### Defining trace files


| Trace file definition | Command &lt;/th&gt;  | Parameter &lt;/th&gt;  | Description &lt;/th&gt;  |
| --- | --- | --- | --- |
| Before starting<br /> Transfer CFT  | CFTPARM  | TRACE=identifier  | CFTTRACE command identifier  |
|   | CFTTRACE  |   |   |


(1): TRCFILE is used in environments
that do not allow dynamic file definition.

#### Start collecting information


| Starting information collection &lt;/th&gt;  | Command used to define the file &lt;/th&gt;  | Parameter &lt;/th&gt;  | Command to enter &lt;/th&gt;  |
| --- | --- | --- | --- |
| When starting up Transfer CFT  | CFTTRACE  | START=CFT |   |
|   | TRCFILE (1)  | START=CFT  | CFTTRACE  |
| During Transfer CFT<br /> operations  | CFTTRACE  | START=DELAYED  | STARTTRC  |
|   | SETTRC  |   | STARTTRC  |


(1): TRCFILE is used in environments
that do not allow dynamic file definition.

#### Stop collecting - close the file and shutdown the process


| Action &lt;/th&gt;  | Define the file with &lt;/th&gt;  | Enter the command &lt;/th&gt;  |
| --- | --- | --- |
| Stop information collection  | CFTTRACE  | STOPTRC |
| Stop collection, close the files and shutdown the process  | CFTTRACE  | STOPTRC<br />  |


Trace commands with CFTUTIL
---------------------------

### Syntax

```
CFTTRACE 
[XTRACE = {<span class="underline">0</span> &#124; 0..7},]
ID = *identifier*,
[TRCFNAM = <span class="underline">" "</span> &#124; *filename*,]
[TRCFTYP = {<span class="underline">STANDARD</span> &#124; CIRCULAR},]
[TRCLREC = n,]
[TRCNREC = n,]
[MODE = {<span class="underline">CREATE</span> &#124; REPLACE &#124; DELETE},]
START = {<span class="underline">CFT</span> &#124; DELAYED}
```

### Parameters

`[FTRACE = {0   &#124; 0..15}]  `

**ID = identifier**

Character string, maximum length: 8; uniquely identifies the trace file
defined by the set of parameters **TRCFNAM, TRCFTYP, TRCLREC, TRCNREC**.

`[MODE = {CREATE   &#124; REPLACE &#124; DELETE},]`

Operation to be performed on the ‘‘trace file" entry designated
by the **ID** parameter:

- CREATE: Create
    an entry
- REPLACE: Replace
    an entry
- DELETE: Delete
    an entry

Where **MODE=DELETE**, only the **ID** parameter is useful.

`[MTRACE = {0   &#124; 0..31}]`

**`[PTRACE = {0   &#124; 0..31}]`**

`START = {CFT   &#124; DELAYED}`

Starting the trace:

- CFT: at Transfer
    CFT start-up
- DELAYED: during
    Transfer CFT operations

If **START = CFT**, a trace vector is created with the identifier
defined in the **ID** parameter. This identifier is used in the **STOPTRC**
command to stop collection.

`[TRCFNAM = {"   " &#124; filename}]`

Name of trace file to be fed by traces.

Character string maximum length: 64 characters.

`[TRCFTYP = {STANDARD   &#124; CIRCULAR}]`

Trace file type:

- STANDARD: sequential
    file written in extend. The new records are written after the old ones.
- CIRCULAR: direct
    access file, with a set number of fixed-length records. This file is accessed
    through a circular up-date, the new records over-writing the old ones

`[TRCLREC = n]`

Trace file physical records (fixed) length.

This parameter is:

- Mandatory if TRCFTYP
    = CIRCULAR
- Optional if TRCFTYP
    = STANDARD

`[TRCNREC = n]`

Number of trace file records.

This parameter is mandatory if **TRCFTYP = CIRCULAR**.

`[XTRACE = {0   &#124; 0..7}]`

Checks the level 1 traces for Transfer CFT EXIT type operations.

This parameter is only relevant if the parameter **START = CFT**.

The chosen value is a mask (logical OR) combination of the desired values.
These values are:

- 1: Trace of the
    request field sent by Transfer CFT to the "EXIT" executive
- 2: Trace of the
    user work field
- 4: Trace of the
    data field

[Back to top](#)
