---
    "title": "Transfer  services in COBOL",
    "linkTitle": "Transfer services in COBOL",
    "weight": "360"
---
{{% TransferCFT/snippets/transferservices_api%}}
<span id="Call Syntax"></span>

Call syntax
-----------


| CALL &quot;CFTU&quot; USING &lt;verb&gt; &lt;param&gt; &lt;rc&gt;<br /> CALL &quot;CFTC&quot; USING &lt;verb&gt; &lt;param&gt; &lt;rc&gt;  |
| --- |


Where:

- CFTU indicates
    that syntax analysis is requested  
    CFTC indicates that syntax analysis is not requested
- &lt;verb&gt; is
    the command that you want to process
- &lt;param&gt; is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value

<!-- -->

- &lt;rc&gt; is the
    return code

The available &lt;verbs&gt; are listed in the following table.


| &lt;verb&gt; | Value | Service |
| --- | --- | --- |
| F-SEND | SEND | Send |
| F-RECV | RECV | Receive |
| F-HALT | HALT | Interrupt |
| F-KEEP | KEEP | Suspend |
| F-START | START | Retry |
| F-DELETE | DELETE | Delete |
| F-END | END | Proceed to &quot;X&quot; state |
| F-SUBMIT | SUBMIT | Re-submit end-of-transfer procedure |
| F-SHUT | SHUT | Stop monitor |
| F-SWITCH | SWITCH | Switching monitoring files<br /> (log, statistics file) |
| F-CLOSEAPI | CLOSEAPI | Freeing resources allocated at the opening of the communication medium |


For more details on the parameter syntax for each command, refer to
the [Command index](../../../../c_intro_userinterfaces/command_summary).

If &lt;param&gt; is not defined, CFTU
takes the default name.

As these media are not available on all systems, an availability check
is performed by the function.

Return codes
------------


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


Error messages
--------------

The FIELD and MSG fields of the CFTAPI COPY CLAUSE contain:

- FIELD: name of
    the incorrect parameter detected by the {{< TransferCFT/axwayvariablesComponentShortName  >}} syntax analyzer
- MSG:
- Either a message
    relative to the error recognized by the syntax analyzer
- Or an error
    message describing an incident when the command is taken into account

See [Messages
and error codes](../../../../troubleshoot_intro/messages_and_error_codes_start_here).

If no error is detected, the FIELD and MSG fields are blank.
