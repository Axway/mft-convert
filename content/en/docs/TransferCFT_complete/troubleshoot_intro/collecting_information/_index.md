{
    "title": "Collect information",
    "linkTitle": "Collect information",
    "weight": "210"
}Transfer CFT generates messages that provide information about the processes
and events that occur on the Transfer CFT. These messages are collected and
stored in a log file. If you are using {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, in the **Products List** page select the Transfer CFT in question. In the Transfer CFT page, select **Logs** in the right pane to view the Transfer CFT logs.

Transfer CFT also generates messages that provide information about
the state of a transfer. These messages are collected and stored in a
catalog file. From the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} main menu, select **Flow** &gt; **Monitoring** &gt; and then the type of View, for example **View all flows in error for a given application**.

<span id="How_to_get_information_when_an_error_occurs"></span>

Getting information when an error occurs
----------------------------------------

All Transfer CFT error messages and diagnostics are listed in the section
[Messages
and error codes](../messages_and_error_codes_start_here). If an error occurs in Transfer CFT that you cannot
correct using the diagnostic information provided there, see
**Performing checks** below.

As you go through the questions in **Performing checks**, note your responses. If you need to submit
a Support request this information can help you to save time.

[Documentation](#Finding_OS_specific_relevant_information) lists related documentation that you may want to consult.

Performing checks
-----------------

To better help you resolve an incident, you can print and use
the following table. It can serve as a guideline for gathering information, which can help you if you need to file an incident report.


| Step  | Your remarks  |
| --- | --- |
| 1. Identify the symptoms  |   |
| Did the system crash? |   |
| Is the error message due to a transfer request, a transfer in server mode, or other? |   |
| Is Transfer CFT functioning abnormally? |   |
| Is the symptom persistent? |   |
| Can you reproduce the symptom? |   |
| ****2. Locate the problem**** |   |
| Which component and function are concerned? |   |
| Can you identify the source of the problem? |   |
| Can you bypass the problem? |   |
| 3. Find the cause |   |
| Has your system or Transfer CFT recently been re-configured? |   |
| Have you installed a new component, either hardware or software? |   |
| Has the problem only recently occurred, or has it existed for some time? |   |


<span id="Finding_OS_specific_relevant_information"></span>

Documentation
-------------

If an error occurred while running Transfer CFT in Central Governance, you may also want to consult the {{< TransferCFT/PrimaryCGorUM  >}} documentation.

Some corrective actions may be operating system specific, so you may also want to refer to:

- Windows
    operations in the {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}
- UNIX
    operations in the {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}
- {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} z/OS Installation and Operation Guide
- {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} OpenVMS Installation and Operation Guide
- {{< TransferCFT/suitevariablesTransferCFTName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}} IBM i Installation and Operation Guide
