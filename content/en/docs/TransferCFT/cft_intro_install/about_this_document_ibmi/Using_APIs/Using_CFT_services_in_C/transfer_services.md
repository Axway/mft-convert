---
title: "Transfer  services in C"
linkTitle: "Transfer services"
weight: 340
---Transfer services allow actions to be taken
on transfers with the security system performing an authorization check
when the request is submitted, and not when the request is processed by
the monitor, the behavior of unprotected IPC with an error message in
the log file.

The application can detect commands that do
not have permission for access. This lightens the workload, improving {{< TransferCFT/axwayvariablesComponentShortName  >}} performance, and reducing the cluttering
of the communication medium by invalid requests.

Use the transfer services to send transfer control commands to Transfer
CFT, with or without a **syntax analysis**
of these commands. The programming interface proposes a function integrating
a syntax analysis of the command to detect any errors, at the source,
and a function without syntax analysis, which provides a much smaller
coding volume.

The transfer services functions:

- Check the validity
    of the command name
- Analyze the syntax
    of the command parameters, if the function using the syntax analyzer is
    used
- Place the command
    in the {{< TransferCFT/axwayvariablesComponentShortName >}} communication medium

The processing performed by {{< TransferCFT/axwayvariablesComponentShortName  >}} is totally asynchronous.

The return code only provides an indication that the function has effectively
been taken into account but does not necessarily mean that {{< TransferCFT/axwayvariablesComponentShortName  >}}
has executed the command correctly. A return code indicating the success
of the function only means that the command has been correctly placed
in the communication medium.


| Function | Use |
| --- | --- |
| SEND | Send transfer request: file, message or reply |
| RECV | Receive transfer request |
| HALT | Interrupt one or more send or receive transfers with a given partner.<br/> The interrupted transfers are set to the "H" state and can be restarted at the partner's request. |
| KEEP | Suspend one or more send or receive transfers with a given partner.<br/> The interrupted transfers are set to the "K" state and can only be restarted by a START command. |
| START | Start one or more send or receive transfers |
| DELETE | Delete a catalog entry and any transfer in process associated with it |
| END | Set a transfer status to executed<br/> The transfer is set to the "X" state. This indicates that end-of-transfer procedure has been correctly executed. |
| SUBMIT | Submit the end-of-transfer procedure |
| SHUT | Shut down {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| SWITCH | Switch monitoring files, LOG, STATS... |
| CLOSEAPI | Free resources allocated at opening of communication medium: memory, network, file |
| COM | Define communication medium |
| GETXINFO | Retrieve information concerning the last transfer made from a synchronous request |


<span id="Call Syntax"></span>

## Call syntax

`rc =      cftau (verb,param)`

`rc =      cftac (verb,param)`

Where:

- cftau indicates
    that syntax analysis is requested
- cftac indicates that syntax analysis is not requested
- &lt;verb> is
    the command that you want to process
- &lt;param> is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc> is the
    return code

The available &lt;verbs> are listed in the following table.


| &lt;verb&gt; | Service |
| --- | --- |
| SEND | Send |
| RECV | Receive |
| HALT | Interrupt |
| KEEP | Suspend |
| START | Retry |
| DELETE | Delete |
| END | Proceed to "X" state |
| SUBMIT | Re-submit end-of-transfer procedure |
| SHUT | Stop monitor |
| SWITCH | Switching monitoring files<br /> (log, statistics file) |
| COM |   |


For more information on the parameter syntax for each command, refer to
the [Command index](../../../../../c_intro_userinterfaces/command_summary).

If &lt;param> is not defined, CFTU will
take a default name.

As these media are not available on all systems, the function performs
an availability check.

The security check is performed on the user name, and the user group
if applicable, depending on the command:

- IDF if present
    in parameter field: DELETE, END, HALT, KEEP, SEND, RECV, START
- Procedure name:
    SUBMIT
- Type: SWITCH LOG
    or ACCNT

## Return codes


| Mnemonic | Description |
| --- | --- |
| CAPI_NOERR | No error |
| CAPI_FUNC_UNDEF | Command not valid |
| CAPI_CMD_LENGTH | cftau only<br/> {{< TransferCFT/axwayvariablesComponentShortName  >}} command string invalid, does not exist, or greater than 1024 characters long  |
| CAPI_KEY_NAME | cftau only<br/> Command syntax incorrect: keyword name incorrect |
| CAPI_KEY_VALUE | cftau only<br/> Command syntax incorrect: keyword value incorrect |
| CAPI_MEM_GET | Memory allocation error |
| CAPI_MEM_FREE | Memory de-allocation error |
| CAPI_INT_ERR1 | Internal error 1 |
| CAPI_INT_ERR2 | Internal error 2 |
| CAPI_INT_ERR3 | Internal error 3 |


## Error messages

The FIELD and MSG fields of the CFTAPI COPY CLAUSE contain:

- FIELD: name of
    the incorrect parameter detected by the {{< TransferCFT/axwayvariablesComponentShortName >}} syntax analyzer
- MSG:
    -   Either a message
        relative to the error recognized by the syntax analyzer
    -   Or an error
        message describing an incident when the command is taken into account

See [Messages
and error codes](../../../../../troubleshoot_intro/collecting_information).

If no error is detected, the FIELD and MSG fields are blank.
