---

    title: Transfer control commands
    linkTitle: Transfer control commands
    weight: 170

---
This topic describes the transfer control commands and provides a link
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
| <a href="../../../admin_intro/admin_commands_intro/delete_command">DELETE</a> | Deletes a catalog entry  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/halt_command">HALT</a> | Stops a transfer and sets it to the HOLD state  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/keep_command">KEEP</a> | Stops a transfer and sets it to the KEEP state  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/start_command">START</a> | Reactivates a transfer  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/submit_command">SUBMIT</a> | Runs a preprocessing, a post-processing or an acknowledgment processing procedure according to the current phase of the transfer request.  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/end_command">END</a> | Declares the processing subsequent to the transfer terminated  |
| <a href="../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/resume_command">RESUME</a> | Retrieves, in the server mode, a blocked send request having the hold status |


The parameters relative to these commands determine the transfer selection
criteria, catalog entries, these commands relate to.

The transfer control commands modify the state of the transfers in the
Transfer CFT catalog. This state can be queried by the [LISTCAT](../../../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/listcat_command) command,
both before and after the control command is executed. The meaning of
these states is indicated in the next topic, Transfer
states.
