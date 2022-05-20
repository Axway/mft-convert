---
title: "Manage  transfers"
linkTitle: "Transfers"
weight: 160
---This section describes transfers, and how to create and
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


| UI  | More information  | Description  |
| --- | --- | --- |
| New  | Create a transfer request | Create a new transfer request  |
| Clone  | No equivalent parameter  | Copy an existing transfer request  |
| Restart  | [Restarting transfers](../../../about_cftutil/managing_transfer_states/start_command) | Restart transfers in the H or K state in the catalog |
| Delete  | [Deleting catalog entries](../../../../admin_intro/admin_commands_intro/delete_command) | Delete one or more catalog entries |
|   | [Suspending transfers](../../../about_cftutil/managing_transfer_states/keep_command) | Suspend one or all of the send and/or receive transfers with selected partners |
|   | [Submitting an end-of-tranfser](../../../about_cftutil/managing_transfer_states/submit_command) | Submit an end-of-transfer procedure for each selected transfer |
| Halt  | [Halting a transfer](../../../about_cftutil/managing_transfer_states/halt_command) | Suspend one or all the send and/or receive transfers, with the partners selected |
| End  | [Declaring executed transfers](../../../about_cftutil/managing_transfer_states/end_command) | Declare that all the operations related to the end-of-transfer, send and receive, have been executed correctly |
|   | [Retrieving a blocked request](../../../about_cftutil/managing_transfer_states/resume_command) | Retrieves, in server mode, a blocked send request that has the *hold* status, if the diagnostic codes are not null |
|   | [Suspending a catalog request](../../../about_cftutil/managing_transfer_states/kstate_command) | Suspend a transfer in the catalog |
|   | [Deleting a transfer request](../../../about_cftutil/managing_transfer_states/clearcmd_command) | Delete a transfer request from the communication file |
| Ack  | [Use the SEND acknowledgement commands](../../../../concepts/send_command/send_replies)  | Send a transfer acknowledgement  |
| Nack  | [Sending a negative acknowledgement](../../../../concepts/send_command/transfers_neg_ack_pesit)  | Send a notification indicating an error occurred  |


## Create transfer requests filters

To create a new transfer request filter or modify an existing filter:

1. Click an exiting filter to use as the basis for the filter or click the filter icon ![](/Images/TransferCFT/filter_create.png) .
1. Customize the filter.
1. Click **Save** if you are modifying and existing filter or **Save as...** if this is a new filter.

## Display transfer request details

To display transfer request details:

* Click any line to expand details that include the severity, timing, code, and message.
* Click the IDTU to display the request details.

For details on the transfer states in Transfer CFT,
refer to Transfer states
topic.

## Create or modify the page layout

Optionally you can select a **Layout** in the drop-down menu to use a customized column or filter display. To create a new layout:

* Click the settings icon ![](/Images/TransferCFT/settings_icon.png)to open the column options. You can use the filter field to help you find fields more quickly.
* Add or remove the filters you want to display in your page layout.
* Click **Save as** and name the layout.

> **Note**
>
> These page customizations are defined in the CFTUIPREF object.

## Customize a CSV export file

The {{< TransferCFT/suitevariablesTransferCFTName  >}} UI allows you to export transfer details in a CSV file. If you have modified the default column names in the user interface as describe above in *Create or modify the page layout*, these custom names are used in the CSV file export.

## Troubleshooting transfer filters

****Issue****: I cannot create filters

****Solution****: Check that you have the MANAGE CFTUIPREF or VIEW CFTUIPREF privilege. This issue may have occurred due to an upgrade.
