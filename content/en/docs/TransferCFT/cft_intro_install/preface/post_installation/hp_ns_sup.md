---

    title: Use the NonStop mode 
    linkTitle: Use the NonStop mode
    weight: 200

---
Transfer CFT HP NonStop provides a start-process supervisor (CFTSUP) that can restart the Transfer CFT server or UI server in case of an unexpected stop. This section presents the following NonStop information:

- [Overview](#Overview): describes the non-stop feature and compares it with the NonStop functionality delivered in previous versions
- [Enable](#Enable): how to activate the non-stop mode
- [Configure](#Configur): how to set additional non-stop mode parameters
- [Syntax](#Syntax): the commands and their syntax
    -   [Messages](#Conventi): provides examples of using CFTSUP commands and returned messages
- [Help](#Help): information on using command line help

<span id="Overview"></span>

## Overview

The CFTSUP interface consist of:

- A component watchdog called the supervisor
- A utility that allows you to:
    -   Start and stop the supervisor
    -   Start and stop the Transfer CFT server and Transfer CFT Copilot server
    -   Display the status of these components

The differences between the CFTSUP interface and the NonStop mode delivered in Transfer CFT 2.3 are:

- The Transfer CFT server and Transfer CFT Copilot server are both managed in NonStop mode.
- The CFTSUP interface replaces the former CFTNSMON and CFTNSHUT commands.
- Transfer CFT stops and is not restarted unless the reset command is set to<span class="code">` RESTART=YES`</span> when you execute the Transfer CFT command <span class="code">`CFTUTIL SHUT <...`</span>&gt;.

<span id="Enable"></span>

## Enable NonStop mode

This section describes how to enable the NonStop mode and start Transfer CFT.

1. Activate the option:
1. Start the Transfer CFT server, for example, using the supervisor:
1. Optionally, check the Transfer CFT server status. For example:

More information and additional commands are described in the following sections.

### Recommendations

- You can use the <span class="code">`cftsup cft start `</span>command to start Transfer CFT, which also starts the supervisor. However, this does not mean the NonStop mode is running. To implement the NonStop mode, you must activate <span class="code">`cft.guardian.nonstop`</span> in the UCONF configuration.
- If you need to kill the Transfer CFT server, for example, use the <span class="code">`cftsup cft kill`</span> command to keep the component from restarting.

<span id="Configur"></span>

## Configure

The following table lists the UCONF parameters related to the NonStop option configuration. See the <a href="../../intro_os_features/hp_ns_batch#UCONF" class="MCXref xref">UCONF parameters</a> descriptions for more detailed information.


| Parameter  | Default value  | Description  |
| --- | --- | --- |
| cft.guardian.nonstop  | No  | Enable the nonstop mode.<br/> • Yes: Activate<br/> • No: Deactivate |
| cft.guardian.collector  | &lt;no value&gt;  | Name of the EMS collector where the supervisor sends messages. See <a href="#Customiz">Customize the EMS collector</a> for details.  |
| cft.guardian.processor  | -1  | Processor on which Transfer CFT is started.  |
| cft.guardian.backup_processor  | -1  | Backup processor on which Transfer CFT is started.  |
| cft.guardian.process_name_prefix  | LA  | The first two letters of the Guardian process names.  |


<span id="Customiz"></span>

### Customize the EMS collector

To use the same collector for the supervisor as for {{< TransferCFT/suitevariablesTransferCFTName  >}} log messages, perform the following steps:

1. Set the uconf <span class="code">`cft.guardian.collector`</span> value to the name of the collector.  
    ```
    CFTUTIL uconfset id=cft.guardian.collector,value='$QACOL'
    ```

1. In the Transfer CFT configuration, modify the CFTLOG definition to: <span class="code">`NOTIFY=’%uconf:cft.guardian.collector%’. F`</span>or example<span class="code">`:`</span>  
    ```
    CFTLOG ID = 'LOG0',

    > FNAME = '_CFTLOG',
    > AFNAME = '_CFTLOGA',
    > ...
    > **NOTIFY = ’%uconf:cft.guardian.collector%’,**
    > CONTENT = 'FULL',
    > ...

    ```

1. Interpret the modified CFTLOG object.

<span id="Syntax"></span>

## Syntax

****Format****

`cftsup [component] Actions [Options]`

Where:

`component [ALL | SUPV | CFT | COPILOT]`

- ALL (default): Action applies to all components
- SUPV: Watchdog utility
    -   Process is started with the name: prefix added to SUP
        -   For example: <span class="code">`CFTL50I Started the supervisor with process id $`</span>**`LASUP`**
        -   Prefix = cft.guardian.process\_name\_prefix (the prefix in the example is <span class="code">`LA`</span>)
    -   Stops when all components are terminated except if it was started explicitly as standalone process:
    -   <span style="font-family: 'Courier New';">cftsup SUPV START</span>
    -   In this case, it only stops with an explicit stop:
    -   <span style="font-family: 'Courier New';">cftsup SUPV STOP</span>
- CFT: Action applies to the Transfer CFT server
- COPILOT: Action applies to Transfer CFT Copilot server

`Actions [ START | STOP | STATUS | KILL | SHUT (for Transfer CFT server only)]`

- KILL is only valid for the Transfer CFT and Transfer CFT Copilot servers.

> **Note**
>
> The SHUT option only apply to the Transfer CFT sever.

- CFTUTIL SHUT FAST=YES the equivalent is cftsup CFT SHUT FAST=YES

<span id="Conventi"></span>

### Messages

<span class="autonumber"></span>Message when starting the supervisor and all components

```
cftsup start
 
CFTL50I Started supervisor with process id $MDSUP
CFTL50I Started Transfer CFT with process id $MDAIN
CFTL50I Started COPILOT with process id $MDCOP
```

<span class="autonumber"></span>Message when checking the status

```
cftsup status
 
CFTL50I SUPV ($MDSUP) status Running
CFTL50I CFT ($MDAIN) status Running
CFTL50I COPILOT ($MDCOP) status Running
```

<span class="autonumber"></span>Message when performing a stop

```
cftsup stop
 
CFTL50I Processing command
CFTL50I Started COPSTOP with process id $MDCST
CFTL57E Error: Transfer CFT is still active (status=TERMINATING)
```

<span class="autonumber"></span>Message when the supervisor is not started

```
cftsup supv status
 
CFTL59E Supervisor $MDSUP is not started
```
<span id="Help"></span>

## Help

From the home directory, enter the <span class="code">`help `</span>command. For example:

```
/home/axway/<user>: cftsup help
 
Syntax: cftsup [ALL|CFT|COPILOT|SUPV] Actions [Options]
: cftsup ? (or HELP) for a list of the component and actions
: cftsup Component ? (HELP) for actions to perform on components
```

Use <span class="code">`command ?`</span> to display the parameter list:

```
/home/axway/<user>: cftsup "action" ?
```
