{
    "title": "Transfer control commands",
    "linkTitle": "Transfer control commands",
    "weight": "160"
}This topic describes the transfer control commands and provides a link
to more information on these commands. The transfer control commands can
be used to:

- Interrupt a transfer
    in process
- Activate or reactivate
    a suspended or held transfer
- Delete a transfer
    request
- Submit, or resubmit,
    an end-of-transfer procedure where the transfer in the T state
- Declare that the
    processing following a transfer is completed

The following table describes the commands associated with these actions.


| Command  | Action  |
| --- | --- |
| [DELETE](../../../admin_intro/admin_commands_intro/delete_command) | Deletes a catalog entry  |
| [HALT](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/halt_command) | Stops a transfer and sets it to the HOLD state  |
| [KEEP](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/keep_command) | Stops a transfer and sets it to the KEEP state  |
| [START](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/start_command) | Reactivates a transfer  |
| [SUBMIT](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/submit_command) | Runs a preprocessing, a post-processing or an acknowledgment processing procedure according to the current phase of the transfer request.  |
| [END](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/end_command) | Declares the processing subsequent to the transfer terminated  |
| [RESUME](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/resume_command) | Retrieves, in the server mode, a blocked send request having the hold status |


The parameters relative to these commands determine the transfer selection
criteria, catalog entries, these commands relate to.

The transfer control commands modify the state of the transfers in the
Transfer CFT catalog. This state can be queried by the [LISTCAT](../../../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/listcat_command) command,
both before and after the control command is executed. The meaning of
these states is indicated in the next topic, Transfer
states.
