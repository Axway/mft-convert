---

    title: Manage the Transfer CFT server
    linkTitle: Start, stop, and administration scripts
    weight: 200

---
## Server command overview

****This section refers only to non multi-node architecture.****

You can use Central Governance to stop, start, check status, and restart a Transfer CFT, or alternatively use the administration commands and scripts provided in this section to manage the application.

When opening a new session to manage your {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must first set the environmental parameters. See <a href="#Set" class="MCXref xref">Set the Transfer CFT profile</a>.

- [Start {{< TransferCFT/axwayvariablesComponentShortName >}} server](#Start)
    -   Standard start
    -   Force a start after an abnormal stop
    -   Start and suspend interactive mode
- [Stop {{< TransferCFT/axwayvariablesComponentShortName >}} server](#Stop__server)
    -   Standard shutdown
    -   Quick stop
    -   Forced shutdown
- [Restart {{< TransferCFT/axwayvariablesComponentShortName >}}](#Restart_server)
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
> The commands listed in this section apply generally to all supported platforms for this version, except for Transfer CFT z/OS. For more information, please refer to the OS-specific Installation Guide.

### Autocomplete command

****UNIX only****

To simplify the use of <span class="code">`cft `</span>commands, you can use the autocomplete feature when working in interactive mode. See [Autocomplete](../../c_intro_userinterfaces/about_cftutil/autocomplete).

<span id="Start"></span>

## Start the {{< TransferCFT/axwayvariablesComponentShortName  >}} server

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

1. In the system Objects menu, type 8 and press Enter to validate.
1. Modify the user profile to take into account the selections that you made in previous customization steps.
1. Indicate the name of the job description that you created for the

Transfer CFT working environment.

****Change profile (CHGPRF)****

```
Type choices, press Enter.
 
Assistance level . . . . . . . . \*SYSVAL \*SAME, \*SYSVAL, \*BASIC...
Current library . . . . . . . . \*CRTDFT Name, \*SAME, \*CRTDFT
Initial program to call . . . . CFGINLPGM Name, \*SAME, \*NONE
Library . . . . . . . . . . . DOICONFIG Name, \*LIBL, \*CURLIB
Initial menu . . . . . . . . . . MAIN Name, \*SAME, \*SIGNOFF
Library . . . . . . . . . . . QSYS Name, \*LIBL, \*CURLIB
Text 'description' . . . . . . . > '<TRANSFER CFT USER PROFILE>'
 
Additional Parameters
 
Job description . . . . . . . . > CFTJOBD Name, \*SAME
Library . . . . . . . . . . . > CFTPROD Name, \*LIBL, \*CURLIB
 
Additional Parameters
 
Job description . . . . . . . . > CFTJOBD Name, \*SAME
Library . . . . . . . . . . . > CFTPROD Name, \*LIBL, \*CURLIB
```

### Start {{< TransferCFT/axwayvariablesComponentShortName  >}}

#### Standard start

Use the following command to start {{< TransferCFT/axwayvariablesComponentShortName  >}}after installation or stopping the server.

```
<span class="code">` cft start`</span>
```

In Windows only you can also use the Start menu or automatically start the server in Service Mode. See <a href="#Windows2" class="MCXref xref">Windows tasks</a>.

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

## Stop the {{< TransferCFT/axwayvariablesComponentShortName  >}} server

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

## Restart the server

To restart the {{< TransferCFT/axwayvariablesComponentShortName  >}} server use the following command. The behavior for in progress transfers depends on the setting defined for the FAST parameter (default=NO).

```
CFTUTIL shut restart=yes
```
<span id="Check"></span>

## Check the system

#### Check the {{< TransferCFT/axwayvariablesComponentShortName  >}} status

To check the state of the {{< TransferCFT/axwayvariablesComponentShortName  >}} sever, enter:

```
cft status
```
<span id="Purge on Transfer CFT start"></span>

## Purge on Transfer CFT start

To configure the Transfer CFT start-up PURGE option, set the uconf values for:

- cft.purge.enable\_on\_start: Defines if purge should run when starting Transfer CFT
- cft.purge.background\_on\_start: Defines if purging on start-up occurs in the background
- cft.purge.quantity: Defines the number of transfers to delete in a step (only applicable for background purging)

<span id="Windows2"></span>

## Windows tasks

<span id="Windows"></span>

#### Windows menus

From the desktop <span class="bold_in_para">****Start****</span> menu, select<span class="bold_in_para"> ****All Programs &gt; Axway Software &gt;Axway &gt; &lt;product> &gt; Stop &lt;product>****</span>

<span id="Service"></span>

#### Service mode

During installation you can elect to run most {{< TransferCFT/axwayvariablesCompanyName  >}} products as a Windows service, which causes the server to start and stop with the computer. When running a product as a Windows service, the only visual clues of the server status outside of the user interface are the application log files and the Windows computer management window.

If you installed the products in Windows service mode, you can start and stop most products with Windows already running as follows:

1. From the desktop, select <span class="bold_in_para">****Start > Settings > Control Panel > Administrative Tools > Services****</span>.
1. Scroll down the Services list and right-click the product.
1. From the menu, select <span class="bold_in_para">****Start****</span>.

<span id="Multi-node_specific"></span>

#### Multi-node specific commands

To view all commands for multi-node architecture, go to the topic [Multi-node commands](../../about_multinode/multi_node_commands).

## z/OS tasks

<span id="Start/st"></span>

#### Start

Start the CFTMAIN JCL in the target.INSTALL library.

#### Stop

The following are commands that you can use to stop Transfer CFT outside of a customized JCL (such as the delivered sample JCL, ******CFTSTOP******, in the target.INSTALL library).

**Normal stop**

```
/F <Jobname>,SHUT FAST=NO *or* SHUT FAST=YES
```

**Quick stop**

Enter the operator command:

```
/P <{{< TransferCFT/axwayvariablesComponentShortName >}} Jobname>
```

\- or -

```
/F <{{< TransferCFT/axwayvariablesComponentShortName >}} Jobname>,SHUT FAST=YES
```

**Force** {{< TransferCFT/axwayvariablesComponentShortName  >}} **shut down**

```
/F <{{< TransferCFT/axwayvariablesComponentShortName >}} Jobname>,SHUT FAST=KILL
```

#### Restart

The following command restarts Transfer CFT outside of a customized JCL. Enter the operator command:

```
/F Jobname,SHUT RESTART=YES
```
<span id="Perform"></span>

#### Perform a ping

Use the CFTPING in the target.INSTALL library to ping your {{< TransferCFT/axwayvariablesComponentShortName  >}}.

#### Check the Transfer CFT Copilot server status

You can use COPSTATU, for example, as the JCL statement to display the Copilot server status in the current LPAR.

 
