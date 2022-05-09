{
    "title": "CFTUTIL  command line interface",
    "linkTitle": "Manage the server ",
    "weight": "220"
}{{% TransferCFT/snippets/cftutil_description%}}
<span id="About_the_Command_line_interface_CFTUTIL"></span>

Overview
--------

All Transfer CFT actions are controlled
by a series of Transfer CFT commands. CFTUTIL makes it possible to create the working environment and configure
the Transfer CFT application, performing the following operations:

- Transfer CFT configuration,
    which is the customization for a given operating environment
- Transfer CFT use,
    which includes the initiation and monitoring of transfers

More precisely, with CFTUTIL you can:

- Create and delete
    parameter, partner, catalog, log, and account files. Such operations can
    be performed only when the Transfer CFT is stopped.
- Change and add
    to certain parameters.
- View parameter,
    partner, catalog, log, and account files.
- Send commands to
    Transfer CFT.

For a complete listing of all Transfer
CFT commands, a description of their function, and a link to the specific
command topic, refer to the [Command index](../../c_intro_userinterfaces/command_summary).

<span id="Command_syntax"></span>

### Command syntax

{{% TransferCFT/snippets/cftutil_syntax%}}
<span id="CFTUTIL_commands"></span>

### CFTUTIL command categories

The CFTUTIL commands can be divided into three categories:

- Transfer CFT services
- Querying and extracting
- File management

When querying or extracting, the files read do not necessarily correspond
to the ones which Transfer CFT is currently using. The files
accessed can be selected for a CFTUTIL execution, using the CONFIG command.

The CONFIG command redefines the data media that the CFTUTIL utility
uses. A medium, in the Transfer CFT meaning of the term, refers to any data
medium or local communication means.

### CFTUTIL modes

{{% TransferCFT/snippets/cftutil_modes%}}
