---

    title: About the CFTUTIL  interface
    linkTitle: CFTUTIL interface
    weight: 120

---
CFTUTIL is the command line interface for Transfer CFT. The Transfer
CFT utility is a program which can be activated in batch or interactive
mode. The CFTUTIL interface is able to translate Transfer CFT parameter
setting commands and operating commands.

<span id="About_the_Command_line_interface_CFTUTIL"></span>

## Overview

All Transfer CFT actions are controlled
by a series of Transfer CFT commands. For a complete listing of all Transfer
CFT commands, a description of their function, and a link to the specific
command topic, refer to the [Command index](../command_summary).

The commands which can be interpreted by CFTUTIL are associated with:

- Transfer CFT configuration,
    which is the customization for a given operating environment
- Transfer CFT use,
    which includes the initiation and monitoring of transfers

The following topics contain additional information on how to set up
Transfer CFT and incorporate file transfers into application processing
systems: use of end-of-transfer procedures, symbolic variables, and so
on.

The typographical conventions used in the Transfer CFT documentation
are described the next topic, *[Typographical
conventions](../../gettingstarted_intro/my_first_transfer_flow_using_cg/typographical_conventions).* These conventions specify the syntax used in the
description of Transfer CFT commands.

> **Note**
>
> When logging on the interface, the username is limited to 32 characters.

<span id="Using_interactive_mode"></span>

## Use interactive mode

You can use CFTUTIL in interactive mode, which consists of several
operating modes that use the same CFTUTIL operations regardless of the operating system.

<span id="Data_entry_in_command_line"></span>

### Data entry in command line

Enter the CFTUTIL commands via the keyboard. The output is sent to the
screen by default, or to a file as shown below:

`CFTUTIL ‘file_symb’file_in file_out`

In this syntax:

- file\_in is a file
    containing the Transfer CFT commands
- file\_out is a file
    into which the results of the commands are written

<!-- -->

- ‘file\_symb’ designates a character specific to
    each environment. Refer to the Transfer CFT Installation Guide that corresponds
    to your OS.


| OS |  file_symb |
| --- | --- |
| Windows | # |
| UNIX |  @ |


Specifying parameters is optional, the default values are the
standard input and output respectively.

The CFTUTIL module is activated in the Transfer CFT executable file
directory. The file\_in and file\_out designate the complete path or path
relative to the current directory.

On all systems:

- CFTUTIL standard
    input and standard output are equivalent to:
    -   CFTUTIL ‘file\_symb’
        CFTIN CFTOUT
- CFTIN and CFTOUT
    are reserved words corresponding to the standard input and output
- Standard input
    and output to a file: CFTUTIL ‘file\_symb’ CFTIN file\_out
- File input and
    standard output: CFTUTIL ‘file\_symb’ file\_in CFTOUT
- File input, file
    output: CFTUTIL ‘file\_symb’ file\_in file\_out

<span id="CONFIG_type"></span>

#### CONFIG TYPE=...,FNAME=...

Another way to change the input/output file, similar to the *Data entry in command line* instructions, is to use the CONFIG command with a syntax as displayed
here in the following two steps:

`CFTUTIL> CONFIG TYPE=OUTPUT, FNAME=file_out`

`CFTUTIL> CONFIG   TYPE=INPUT, FNAME=file_in`

See the [CONFIG
command](../../admin_intro/admin_config_commands/communication_media_concepts).

<span id="Command_syntax"></span>

### Command syntax

For each command, CFTUTIL checks the command syntax and enters the command.
See also [Typographical conventions](../../gettingstarted_intro/my_first_transfer_flow_using_cg/typographical_conventions).

CFTUTIL makes it possible to create the working environment and configure
the Transfer CFT application, performing the following operations:

- Create and delete
    parameter, partner, catalog, log, and account files. Such operations can
    be performed only when the Transfer CFT is stopped
- Change and add
    to certain parameters
- View parameter,
    partner, catalog, log, and account files
- Send commands to
    Transfer CFT

<span id="CFTUTIL_commands"></span>

### CFTUTIL command categories

The CFTUTIL commands can be divided into three categories:

- Operations
- General configuration
- Flow management
- Miscellaneous actions

### CFTUTIL modes

CFTUTIL can be used in three different modes:

- Command mode, for
    example:

```
> CFTUTIL SEND PART=PART1, IDF=IDF1, etc.
```

- Batch mode, for
    example:

```
> CFTUTIL #file.smp
```

- Interactive line
    mode, for example:

```
> CFTUTIL [to start CFTUTIL]> /END [to quit CFTUTIL]
```

> **Note**
>
> In all modes you can use the command EXIT to quit CFTUTIL.

## Use the help command

The <span class="code">`help `</span>command enables you to get more information on a command and its parameters when using CFTUTIL. To use, enter <span class="code">`help `</span> to list available commands.

```
> CFTUTIL [to start CFTUTIL]
**help**

HELP SEND LISTCAT DISPLAY DELETE END HALT
KEEP START RECV SUBMIT ABOUT LISTCOM SHUT
PURGE SWITCH MQUERY RECONFIG SWAITCAT STEST SGET
SDISPLAY ACT INACT CFTACCNT CFTAUTH CFTCAT CFTCOM
CFTDEST CFTETB CFTEXIT CFTEXT GETDICT CFTFILE CFTIDF
....

```

To get help for a specific command, use <span class="code">`cmd=<command>`</span>:

```
> **help cmd=CFTSEND** [name of the command]
COMMAND CFTSEND USAGE
MODE S S 8 <REPLACE>
'CREATE'

<LOCMBX > S S 65 STRING max_length=64
<SYNC > S C 1 <NO> 'YES','NO'
<TODATE > J N 8 Day
<TOTIME > H N 8 Hour
CYCDATE N N 8 Number <0> min=0 max=99991231
CYCTIME N N 8 Number <0> min=0 max=23595999
MAXDATE N N 8 Number <99991231> min=0 max=99991231

...
```

To get a parameter description as well as defaults and type of input, add <span class="code">`content=detail`</span>:

```
> **help cmd=CFTSEND, content=detail**
COMMAND CFTSEND USAGE
MODE STRING max_length=7 Action to do in the parameter or partner database <REPLACE>
'CREATE'

<LOCMBX > STRING max_length=64 Mailbox name
<SYNC > STRING max_length=0 Synchronous API <NO> ('YES','NO')
<TODATE > Date (YYYYMMDD) Date limit for request catch
<TOTIME > Hour (HH:mm:ss) Time limit for request catch
CYCDATE Number <0> min=0 max=99991231 Upper final date for activating the first transfer of a cycle
CYCTIME Number <0> min=0 max=23595999 Cyclic transfer time
MAXDATE Number <99991231> min=0 max=99991231 Final date for transfer validity

...
```
