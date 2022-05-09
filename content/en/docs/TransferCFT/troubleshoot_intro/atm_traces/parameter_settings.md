{
    "title": "Defining the trace in CFTPARM",
    "linkTitle": "Define the trace in general parameters (CFTPARM)",
    "weight": "300"
}When you define the TRACE parameter, you prompt the loading of the
trace items defined in the CFTTRACE command when Transfer CFT
is started. See also [Defining
general parameters.](../../../admin_intro/admin_config_commands/cftparm_general_parameters)

<span id="Syntax"></span>

### Syntax

`CFTPARM  [TRACE = identifier]`

<span id="Parameters"></span>

### Parameters

`[TRACE = identifier]`

Indicates the presence and the identifier of an initial trace description,
to be taken into account when Transfer CFT starts.

Character string, maximum length: 8; default value: " ".
