---
    title: "Transfer  services in COBOL"
    linkTitle: "Transfer services in COBOL"
    weight: 360
---Use the transfer services to send transfer control commands to Transfer
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


| CALL "CFTU" USING &lt;verb&gt; &lt;param&gt; &lt;rc&gt;<br /> CALL "CFTC" USING &lt;verb&gt; &lt;param&gt; &lt;rc&gt;  |
| --- |


Where:

- CFTU indicates
    that syntax analysis is requested  
    CFTC indicates that syntax analysis is not requested
- &lt;verb> is
    the command that you want to process
- &lt;param> is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value

<!-- -->

- &lt;rc> is the
    return code

The available &lt;verbs> are listed in the following table.


| &lt;verb&gt; | Value | Service |
| --- | --- | --- |
| F-SEND | SEND | Send |
| F-RECV | RECV | Receive |
| F-HALT | HALT | Interrupt |
| F-KEEP | KEEP | Suspend |
| F-START | START | Retry |
| F-DELETE | DELETE | Delete |
| F-END | END | Proceed to "X" state |
| F-SUBMIT | SUBMIT | Re-submit end-of-transfer procedure |
| F-SHUT | SHUT | Stop monitor |
| F-SWITCH | SWITCH | Switching monitoring files<br /> (log, statistics file) |
| F-CLOSEAPI | CLOSEAPI | Freeing resources allocated at the opening of the communication medium |


For more details on the parameter syntax for each command, refer to
the [Command index](../../../../c_intro_userinterfaces/command_summary).

If &lt;param> is not defined, CFTU
takes the default name.

As these media are not available on all systems, an availability check
is performed by the function.

## Return codes


| Mnemonic | Description |
| --- | --- |
| CAPI-NOERR | No error |
| CAPI-FUNC-UNDEF | Command not valid |
| CAPI-CMD-LENGTH | {{< TransferCFT/axwayvariablesComponentShortName  >}} command string invalid, does not exist, or greater than 1024 characters long  |
| CAPI-KEY-NAME | Command syntax incorrect: keyword name incorrect |
| CAPI-KEY-VALUE | Command syntax incorrect: keyword value incorrect |
| CAPI-MEM-GET | Memory allocation error |
| CAPI-MEM-FREE | Memory de-allocation error |
| CAPI-INT-ERR1 | Internal error 1 |
| CAPI-INT-ERR2 | Internal error 2 |
| CAPI-INT-ERR3 | Internal error 3 |


## Error messages

The FIELD and MSG fields of the CFTAPI COPY CLAUSE contain:

- FIELD: name of
    the incorrect parameter detected by the {{< TransferCFT/axwayvariablesComponentShortName >}} syntax analyzer
- MSG:
- Either a message
    relative to the error recognized by the syntax analyzer
- Or an error
    message describing an incident when the command is taken into account

See [Messages
and error codes](../../../../troubleshoot_intro/messages_and_error_codes_start_here).

If no error is detected, the FIELD and MSG fields are blank.
