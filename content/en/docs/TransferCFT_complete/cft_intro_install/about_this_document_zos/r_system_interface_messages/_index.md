{
    "title": "System interface messages",
    "linkTitle": "Troubleshooting",
    "weight": "200"
}The Transfer CFT system interfaces generate z/OS operator messages that display on the system console, and appear when:

- A severe error is detected by a system function

<!-- -->

- Or the corresponding SGTRACE trace is active

> **Note**
>
> Note: Certain OS specific messages that are auto-documented, CFDMnnx, CARMnnx, CCFTnnx, CFRNnnx, CFCAnnx, and CFTInnx, may not be detailed in this document. These messages are considered self-explanatory.

System interface message definitions
------------------------------------

The messages have the following format:

- 4 letters corresponding to the system interface

<!-- -->

- 2 digits to number the messages

<!-- -->

- 1 classification letter

<!-- -->

- The message text

****Transfer CFT z/OS system interface messages****


| Message | Definition |
| --- | --- |
| CFINnnI:text | CFTINT interface information messages under VTAM. |
| SGEXnnL:text | Task manager information message that displays with information ‘SGTRACE 2’. |
| SGABnnL:text | Message consecutive to a Transfer CFT abnormal end. These messages are also recorded in the Transfer CFT diagnostics file. |
| SYNA01E:text | The text is returned by the MACRO SYNADAF following an input/output error. |


**Transfer CFT z/OS messages**


| Message | Definition |
| --- | --- |
| CFST00E | Transfer CFT region is limited to 1024 Mb. It is recommended that you not start Transfer CFT with REGION=0M. |
| CHSM01I | HSM Recall of: filename HSM recall in synchronous mode for a file. |
| CHSM02I | Recall completed: filename HSM recall in synchronous mode terminated normally. |
| CHSM03E | Recall FAILED: filename A problem occurred with the HSM recall, the volume is still MIGRAT. |
| CHSM04I | HSM Recall (ASY) of: filename. HSM recall in asynchronous mode. |
| CHSM05I | Recall completed: filename HSM recall in asynchronous mode terminated normally. |
| SITM02E | LOAD FAILED FOR: module_name Error loading a module because module was not found in STEPLIB, JOBLIB, etc. |
| CTCP01F  | Unexpected TCP error caused a forced dump. A Transfer CFT restart is required. |
| CTCP02F  | Unexpected TCP error. You must restart the Copilot server.<br/> Configure job scheduling to restart the Copilot server to manage this type of error (it is not automatically restarted by the ARM service). |


For a complete list of possible error messages and their meaning, refer to the Troubleshooting topics in the *[*Transfer CFT* {{< TransferCFT/axwayvariablesComponentVersion  >}} *Documentation*](http://docs-dev.ecd.axway.int/u/documentation/transfer_cft/3.2.4/webhelp_portal/content/troubleshooting/messages_and_codes/messages_and_error_codes_start_here.htm)*.
