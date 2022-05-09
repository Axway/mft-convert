{
    "title": "Configuring  the environment",
    "linkTitle": "Configuring the environment",
    "weight": "350"
}This topic describes how to configure the environment for a directory
type exit. Before you submit a directory type EXIT, you must customize
the following {{< TransferCFT/axwayvariablesComponentShortName  >}} objects:

- [CFTPROT](#Defining_the_CFTPROT_object)
    defines both the application protocol type and profile
- [CFTEXIT](#Defining_the_CFTEXIT_object)
    describes the EXIT environment and how this EXIT is activated

Each CFTEXIT object corresponds to an EXIT task. The number of EXIT
tasks of all types simultaneously active is limited to a number depending
on the operating system.

EXIT type directory tasks are activated in memory when {{< TransferCFT/axwayvariablesComponentShortName  >}}
is started and de-activated when the monitor is shut down.

<span id="Defining_the_CFTPROT_object"></span>

Defining the CFTPROT object
---------------------------

The parameters mentioned here are the ones that are specific to this
EXIT.

### Syntax

`CFTPROTID = identifier,...[EXITA = identifier,..,]....[DYNAM = identifier,]     ...`

### Parameters

******[ID](../../../../c_intro_userinterfaces/command_summary/parameter_intro/id) =
identifier******

Protocol identifier.

******[[EXITA](../../../../c_intro_userinterfaces/command_summary/parameter_intro/exita) =
identifier]     ******

Directory EXIT identifier.

To activate a directory type EXIT for the protocol considered, an EXIT
identifier of this type should be indicated. There can only be one directory
type EXIT task per protocol command.

The directory type EXIT identifier can include the symbolic variable
&NPART:(EXITA = (&NPART, ...) where NPART designates the remote
partner network name.

******[DYNAM](../../../../c_intro_userinterfaces/command_summary/parameter_intro/dynam)  =
identifier******

Identifier of the dynamic partner in server mode.

The value of this identifier corresponds to that of the model [CFTPART](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftpart)
object ID parameter.

<span id="Defining_the_CFTEXIT_object"></span>

Defining the CFTEXIT object
---------------------------

### Syntax

`CFTEXITID = identifier,TYPE = ACCESS,[FORMAT = { V23   &#124; V24 }][LANGUAGE = {COBOL &#124; C},][MODE = MODE,][PARM = string,][PROG = {CFTEXIT &#124; string},][RESERV = {1024 &#124; n}]`

### Parameters

******[ID](../../../../c_intro_userinterfaces/command_summary/parameter_intro/id) =
identifier******

Command identifier.

The value of this identifier corresponds to the identifier defined in
the EXITA parameter of the related CFTPROT object.

****[[FORMAT](../../../../c_intro_userinterfaces/command_summary/parameter_intro/format)
= V23 &#124; V24 ]****

Optional parameter. Indicates the format
for the communication area.

- ****V23**** (Default value)
- ****V24****

******[[LANGUAGE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/language)
= {COBOL &#124; C}]******

Language in which the user program is written.

The possible values are COBOL and C language.

{{< TransferCFT/axwayvariablesComponentShortName  >}} uses this attribute to exchange data with the program using
the EXIT via the structure best suited to the language in which it is
implemented.

******[[PARM](../../../../c_intro_userinterfaces/command_summary/parameter_intro/parm) =
string64]******

Free user field.

******[[PROG](../../../../c_intro_userinterfaces/command_summary/parameter_intro/prog)  =
{CFTEXIT &#124; string512}]******

Name of the executable module associated with the EXIT task.

This module is built from the interface provided with {{< TransferCFT/axwayvariablesComponentShortName  >}} linked
to the program written by the user. In order to facilitate identification
of the associated module, it is advised to name it CFTEXIA.

****[[RESERV](../../../../c_intro_userinterfaces/command_summary/parameter_intro/reserv)  =
{<span class="underline">1024</span> &#124; n}]     ****{0 ...1024}    ********

Size of the working area reserved for the user.

This area is not used by the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface. You can use it
to save data required for the processing of the program that you have
written. This area is de-allocated when the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface de-selects
the file.

******[TYPE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/type) =
ACCESS******

EXIT type.
