---

    title: Transfer command basics
    linkTitle: Transfer concepts
    weight: 140

---
The transfer concept topics introduce the various {{< TransferCFT/axwayvariablesComponentShortName  >}} transfer
processes. For general transfer principles, refer to the introductory
overview section.

The transfer commands are
divided in categories depending on their function.

## Entering a command

There are multiple interfaces that you can use to enter Transfer CFT commands, such as the Transfer CFT command utility CFTUTIL or client user interface.

You can enter a command using CFTUTIL in a command window from the Transfer CFT runtime directory. For example in a Windows environment:

```
C:\\Axwaycft313sp1\\Transfer_CFT\\runtime> profile
C:\\Axwaycft313sp1\\Transfer_CFT\\runtime> CFTUTIL  
1:[CFU]
```

## Entering parameters

There are two ways to supply parameters for a command:

- You can define all information for a command, both mandatory and optional parameters

<!-- -->

- You can rely on default values or template values that are common for a given command

<span id="Transfer_associated_commands"></span>

## Using basic transfer associated commands

QQQ\_QQQ\_CHECK I put the "For each command" part before the 2 tables. They were after a single table.

For each command, the {{< TransferCFT/axwayvariablesComponentShortName  >}} command interface performs the following
actions:

- Checks the syntax
    of the command
- Puts the command
    in the Transfer CFT communication medium

### Send/Receive Transfer commands


| Command | Description |
| --- | --- |
| <a href="">SEND</a>  | Send files, messages, or replies (acknowledgments)  |
| <a href="">RECV</a>  | Receive files  |
| <a href="../../admin_intro/admin_commands_intro/delete_command">DELETE</a> | Delete catalog entries  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/halt_command">HALT</a> | Stop transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/keep_command">KEEP</a> | Suspend transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/start_command">START</a> | Restart transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/submit_command">SUBMIT</a> | Submit an end-of-transfer procedure |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/end_command">END</a> | Declare that flow is finished |


### Actions on transfers 


| Command | Description |
| --- | --- |
| <a href="">SEND</a>  | Send files, messages, or replies (acknowledgments)  |
| <a href="">RECV</a>  | Receive files  |
| <a href="../../admin_intro/admin_commands_intro/delete_command">DELETE</a> | Delete catalog entries  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/halt_command">HALT</a> | Stop transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/keep_command">KEEP</a> | Suspend transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/start_command">START</a> | Restart transfers  |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/submit_command">SUBMIT</a> | Submit an end-of-transfer procedure |
| <a href="../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/end_command">END</a> | Declare that flow is finished |

