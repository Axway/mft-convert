{
    "title": "Transfer  services in C",
    "linkTitle": "Transfer services",
    "weight": "360"
}Transfer services allow actions to be taken
on transfers with the security system performing an authorization check
when the request is submitted, and not when the request is processed by
the monitor, the behavior of unprotected IPC with an error message in
the log file.

The application can detect commands that do
not have permission for access. This lightens the workload, improving {{< TransferCFT/axwayvariablesComponentShortName  >}} performance, and reducing the cluttering
of the communication medium by invalid requests.

{{% TransferCFT/snippets/transferservices_api%}}
<span id="Call Syntax"></span>

Call syntax
-----------

`rc =      cftau (verb,param)`

`rc =      cftac (verb,param)`

Where:

- cftau indicates
    that syntax analysis is requested
- cftac indicates that syntax analysis is not requested
- &lt;verb&gt; is
    the command that you want to process
- &lt;param&gt; is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc&gt; is the
    return code

The available &lt;verbs&gt; are listed in the following table.


| &lt;verb&gt; | Service |
| --- | --- |
| SEND | Send |
| RECV | Receive |
| HALT | Interrupt |
| KEEP | Suspend |
| START | Retry |
| DELETE | Delete |
| END | Proceed to &quot;X&quot; state |
| SUBMIT | Re-submit end-of-transfer procedure |
| SHUT | Stop monitor |
| SWITCH | Switching monitoring files<br /> (log, statistics file) |
| COM |   |


For more information on the parameter syntax for each command, refer to
the [Command index](../../../../c_intro_userinterfaces/command_summary).

If &lt;param&gt; is not defined, CFTU will
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

Return codes
------------


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


Error messages
--------------

The FIELD and MSG fields of the CFTAPI COPY CLAUSE contain:

- FIELD: name of
    the incorrect parameter detected by the {{< TransferCFT/axwayvariablesComponentShortName  >}} syntax analyzer
- MSG:
    -   Either a message
        relative to the error recognized by the syntax analyzer
    -   Or an error
        message describing an incident when the command is taken into account

See [Messages
and error codes](../../../../troubleshoot_intro/collecting_information).

If no error is detected, the FIELD and MSG fields are blank.
