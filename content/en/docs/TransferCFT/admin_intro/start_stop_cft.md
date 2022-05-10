---
    "title": "Manage the Transfer CFT server",
    "linkTitle": "Start, stop, and administration scripts",
    "weight": "190"
---
Server command overview
-----------------------

****This section refers only to non multi-node architecture.****

You can use Central Governance to stop, start, check status, and restart a Transfer CFT, or alternatively use the administration commands and scripts provided in this section to manage the application.

When opening a new session to manage your {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must first set the environmental parameters. See [Set the Transfer CFT profile](#Set).

- [Start {{< TransferCFT/axwayvariablesComponentShortName  >}} server](#Start)
    -   Standard start
    -   Force a start after an abnormal stop
    -   Start and suspend interactive mode
- [Stop {{< TransferCFT/axwayvariablesComponentShortName  >}} server](#Stop__server)
    -   Standard shutdown
    -   Quick stop
    -   Forced shutdown
- [Restart {{< TransferCFT/axwayvariablesComponentShortName  >}}](#Restart_server)
- [Check Transfer CFT status](#Check)
- [Purge on Transfer CFT start](#Purge%20on%20Transfer%20CFT%C2%A0start)

### OS specific tasks and menus

- Windows
    -   [Service mode](#Service)
    -   [Windows menus](#Windows)
- z/OS
    -   [Start/stop](#Start/st)
    -   [Check status](#Perform)

> **Note**
>
> Note: The commands listed in this section apply generally to all supported platforms for this version, except for Transfer CFT z/OS. For more information, please refer to the OS-specific Installation Guide.

### Autocomplete command

****UNIX only****

To simplify the use of `cft `commands, you can use the autocomplete feature when working in interactive mode. See [Autocomplete](../../c_intro_userinterfaces/about_cftutil/autocomplete).

<span id="Start"></span>

Start the {{< TransferCFT/axwayvariablesComponentShortName  >}} server
---------------------------------------------------------------------------

<span id="Set"></span>

### Set the {{< TransferCFT/axwayvariablesComponentShortName  >}} profile

From your runtime directory prompt type:

******UNIX******

```
 . ./profile
```

******Windows******

```
profile.bat
```

**IBM i**

To change the user profile:

{{% TransferCFT/snippets/ibm_i_profile%}}

### Start {{< TransferCFT/axwayvariablesComponentShortName  >}}

#### Standard start

Use the following command to start {{< TransferCFT/axwayvariablesComponentShortName  >}}after installation or stopping the server.

{{% TransferCFT/snippets/start_transfer_cft%}}

In Windows only you can also use the Start menu or automatically start the server in Service Mode. See [Windows tasks](#Windows2).

#### Start and suspend interactive mode

This mode launches the server, but freezes the session where you executed this start command. Closing this session automatically stops the {{< TransferCFT/axwayvariablesComponentShortName  >}}, and if the server is stopped the initiating session is unfrozen.

```
cft start-and-wait
```

#### Force a start after an abnormal stop

If {{< TransferCFT/axwayvariablesComponentShortName  >}} was stopped abnormally, you can force a start using the following command. Notice though that this kills any {{< TransferCFT/axwayvariablesComponentShortName  >}} processes that were not previously stopped.

```
cft force-start
```
<span id="Stop__server"></span>

Stop the {{< TransferCFT/axwayvariablesComponentShortName  >}} server
--------------------------------------------------------------------------

This program shuts down
Transfer CFT, using either an immediate or delayed shutdown.

#### Standard shutdown

{{< TransferCFT/axwayvariablesComponentShortName  >}} completes all the transfers
in process and shuts down. No new transfer is initialized.

```
CFTUTIL shut fast=no
```

#### Quick stop

This method of stopping interrupts transfers in progress and changes
them to D state (available). No pending transfers are activated.

```
cft stop
```

-or-

```
CFTUTIL shut fast=yes
```

#### Forced  shutdown

Immediate {{< TransferCFT/axwayvariablesComponentShortName  >}} shutdown occurs,
but without updating the transfer states. No pending transfers are activated.

```
CFTUTIL shut fast=kill
```
<span id="Restart_server"></span>

Restart the server
------------------

To restart the {{< TransferCFT/axwayvariablesComponentShortName  >}} server use the following command. The behavior for in progress transfers depends on the setting defined for the FAST parameter (default=NO).

```
CFTUTIL shut restart=yes
```
<span id="Check"></span>

Check the system
----------------

#### Check the {{< TransferCFT/axwayvariablesComponentShortName  >}} status

To check the state of the {{< TransferCFT/axwayvariablesComponentShortName  >}} sever, enter:

```
cft status
```
<span id="Purge on Transfer CFT start"></span>

Purge on Transfer CFT start
---------------------------

To configure the Transfer CFT start-up PURGE option, set the uconf values for:

- cft.purge.enable_on_start: Defines if purge should run when starting Transfer CFT
- cft.purge.background_on_start: Defines if purging on start-up occurs in the background
- cft.purge.quantity: Defines the number of transfers to delete in a step (only applicable for background purging)

<span id="Windows2"></span>

Windows tasks
-------------

<span id="Windows"></span>

#### Windows menus

{{% TransferCFT/snippets/stop_windows_start_menu%}}
<span id="Service"></span>

#### Service mode

{{% TransferCFT/snippets/using_service_mode%}}
<span id="Multi-node_specific"></span>

#### Multi-node specific commands

To view all commands for multi-node architecture, go to the topic [Multi-node commands](../../about_multinode/multi_node_commands).

z/OS tasks
----------

<span id="Start/st"></span>

#### Start

{{% TransferCFT/snippets/zos_start%}}

#### Stop

{{% TransferCFT/snippets/zos_stop%}}

#### Restart

{{% TransferCFT/snippets/zos_restart%}}
<span id="Perform"></span>

#### Perform a ping

{{% TransferCFT/snippets/zos_status%}}

#### Check the Transfer CFT Copilot server status

You can use COPSTATU, for example, as the JCL statement to display the Copilot server status in the current LPAR.
