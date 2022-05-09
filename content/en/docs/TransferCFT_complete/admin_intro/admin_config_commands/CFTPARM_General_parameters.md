{
    "title": "General  parameters definition",
    "linkTitle": "CFTPARM - General parameters definition ",
    "weight": "270"
}This topic describes how to define the general {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter, which corresponds to the CFTPARM object
in the command line operations.

[What
is the CFTPARM object?](#What_is_the_CFTPARM_object_) 

[How
does the CFTPARM object work?](#How_does_the_CFTPARM_object_work_)

Related
topics

- Command syntax
    [CFTPARM](../../../c_intro_userinterfaces/command_summary#CFTPARM)
- Parameter list
    [CFTPARM](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftparm)

<span id="What_is_the_CFTPARM_object_"></span>

What is the CFTPARM object?
---------------------------

The CFTPARM object defines general parameters that:

- Specify the parameters
    that control {{< TransferCFT/axwayvariablesComponentShortName  >}} operations globally
- Select the other
    parameter setting commands that are taken into account during execution

You can create several CFTPARM objects, but you can only select a single
CFTPARM for monitor start-up.

Only the associated CFTNET, CFTCOM, and CFTPROT are listed for a given
PARM. If you modify the corresponding COM, NET, PROT, or CAT parameters
you have modified that specific PARM.

The parameters that define the {{< TransferCFT/axwayvariablesComponentShortName  >}} environment are:

- Sizing constants
- File location
- End-of-transfer
    actions
- Product activation
    key

The CFTPARM is an initial configuration object. These parameters are
set when {{< TransferCFT/axwayvariablesComponentShortName  >}} starts and cannot be modified dynamically. If you
modify CFTPARM values, the changes are not taken into account until the
{{< TransferCFT/axwayvariablesComponentShortName  >}} is stopped and restarted.

The user generally works with a single CFTPARM object in the parameter
file. There may, however, be several such commands in this file, a single
command being selected at the time {{< TransferCFT/axwayvariablesComponentShortName  >}} is activated, by specifying as
an activation parameter, the value of the identifier ID of the CFTPARM
command selected.

If this activation parameter is not defined, the {{< TransferCFT/axwayvariablesComponentShortName  >}}
looks for a command CFTPARM ID = IDPARM0 (number 0).

These parameters are defined at the time {{< TransferCFT/axwayvariablesComponentShortName  >}} is started and
cannot be modified dynamically.

The *[End
of transfer procedures](../../../concepts/about_transfer_processing/procedure_examples)* section provides a complete description
of the use of End-of-transfer procedures EXECSE, EXECSF parameters, and
so on.

<span id="How_does_the_CFTPARM_object_work_"></span>

How does the CFTPARM object work?
---------------------------------

CFTPARM is the master object and is used, in
particular, to reference the other configuration objects.

This is accomplished through the use of the
same value for the:

- Identifier
    of the referenced command, the ID parameter
- CFTPARM
    parameter referencing this command

The value of the CAT parameter, CFTPARM object, for example, designates
the identifier of the CFTCAT object which describes the transfer CATALOG
file.

**CFTPARM - defining the general environment
parameters**

Relationships also exist between the value of the NET parameter of the
CFTPARM object and the values of the NET parameter of the CFTPROT object
and ID parameter of the CFTNET object. These relationships are described
in the *[Defining
network resources and protocol](../network_resource_concepts)*.
