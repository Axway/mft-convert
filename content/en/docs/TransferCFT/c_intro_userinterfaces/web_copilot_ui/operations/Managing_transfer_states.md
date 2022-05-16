---

    title: Manage  transfers
    linkTitle: Transfers
    weight: 170

---
This section describes transfers, and how to create and
manage your transfer operations using {{< TransferCFT/axwayvariablesComponentShortName  >}}.

{{< TransferCFT/axwayvariablesComponentShortName  >}} can execute both file and message transfers. A transfer
consists of a set of processes that result in the exchange of files between
computers. In a transfer, one computer is the sender, the other is the
receiver. The sender and receiver are linked together by a network. A
file transfer may consist of sending a file, group of files, or a message.

## Managing transfers in the user interface

To view the transfers log:

1. In the left pane, click **Transfer**.
1. In the main pane, select a transfer.
1. Click the transfer action.  
    ![](/Images/TransferCFT/ui_transfers.png)

Available actions to include in the Transfers page include:


| UI  | Parameter details  | Description  |
| --- | --- | --- |
| New  | <a href="../../../../concepts" >Managing transfers and partners</a>  | Create a new transfer request  |
| Clone  | No equivalent parameter  | Copy an existing transfer request  |
| Restart  | <a href="../../../about_cftutil/managing_transfer_states/start_command">Restarting transfers</a> | Restart transfers in the H or K state in the catalog |
| Delete  | <a href="../../../../admin_intro/admin_commands_intro/delete_command">Deleting catalog entries</a> | Delete one or more catalog entries |
|   | <a href="../../../about_cftutil/managing_transfer_states/keep_command">Suspending transfers</a> | Suspend one or all of the send and/or receive transfers with selected partners |
|   | <a href="../../../about_cftutil/managing_transfer_states/submit_command">Submitting an end-of-tranfser</a> | Submit an end-of-transfer procedure for each selected transfer |
| Halt  | <a href="../../../about_cftutil/managing_transfer_states/halt_command">Halting a transfer</a> | Suspend one or all the send and/or receive transfers, with the partners selected |
| End  | <a href="../../../about_cftutil/managing_transfer_states/end_command">Declaring executed transfers</a> | Declare that all the operations related to the end-of-transfer, send and receive, have been executed correctly |
|   | <a href="../../../about_cftutil/managing_transfer_states/resume_command">Retrieving a blocked request</a> | Retrieves, in server mode, a blocked send request that has the *hold* status, if the diagnostic codes are not null |
|   | <a href="../../../about_cftutil/managing_transfer_states/kstate_command">Suspending a catalog request</a> | Suspend a transfer in the catalog |
|   | <a href="../../../about_cftutil/managing_transfer_states/clearcmd_command">Deleting a transfer request</a> | Delete a transfer request from the communication file |
| Ack  | <a href="../../../../concepts/using_the_send_command/sending_replies" >Use the SEND acknowledgement commands</a>  | Send a transfer acknowledgement  |
| Nack  | <a href="../../../../concepts/using_the_send_command/transfers_neg_ack_pesit" >Sending a negative acknowledgement</a>  | Send a notification indicating an error occurred  |


## Create transfer requests filters

To create a new transfer request filter or modify an existing filter:

1. Click an exiting filter to use as the basis for the filter or click the filter icon ![](/Images/TransferCFT/filter_create.png) .
1. Customize the filter.
1. Click **Save** if you are modifying and existing filter or **Save as...** if this is a new filter.

## Display transfer request details

To display transfer request details:

- Click any line to expand details that include the severity, timing, code, and message.
- Click the IDTU to display the request details.

For details on the transfer states in Transfer CFT,
refer to Transfer states
topic.

## Create or modify the page layout

Optionally you can select a **Layout** in the drop-down menu to use a customized column or filter display. To create a new layout:

- Click the settings icon ![](/Images/TransferCFT/settings_icon.png)to open the column options. You can use the filter field to help you find fields more quickly.
- Add or remove the filters you want to display in your page layout.
- Click **Save as** and name the layout.

> **Note**
>
> These page customizations are defined in the CFTUIPREF object.

## Troubleshooting transfer filters

<span class="bold_in_para">****Issue****</span>: I cannot create filters

<span class="bold_in_para">****Solution****</span>: Check that you have the MANAGE CFTUIPREF or VIEW CFTUIPREF privilege. This issue may have occurred due to an upgrade.
