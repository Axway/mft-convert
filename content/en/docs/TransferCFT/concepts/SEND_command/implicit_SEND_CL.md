{
    "title": "Defining an implicit SEND ",
    "linkTitle": "Defining an implicit SEND",
    "weight": "200"
}Files can be made available for a transfer request either implicitly
or explicitly. The explicit mode provides more security because the server
must have authorization to make a file available. In the implicit mode,
IMPL parameter is set to yes, which enables a file to be available for
transfer without a specific request for that file.

Description:

Use this command to make a file available for transfer
without it being explicitly requested.


| Parameter  | Description  |
| --- | --- |
| [IMPL](../../../c_intro_userinterfaces/command_summary/parameter_intro/impl)  | The implicit send makes a file available. Select from:<br/> • YES<br/> • NO<br/> This parameter must be set to NO for the default model file description. |
| Other parameters | For other SEND parameters refer to [Sending files](../send_command_basics).  |


For SEND command details refer to the following syntax:

{{% TransferCFT/snippets/SEND_command%}}
