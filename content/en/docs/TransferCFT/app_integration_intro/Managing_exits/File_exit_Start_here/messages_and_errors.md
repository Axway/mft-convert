{
    "title": "Messages  and errors",
    "linkTitle": "Messages and errors",
    "weight": "360"
}When serious faults arise, such as parameter setting errors, overwriting
data, time-outs, and so on, {{< TransferCFT/axwayvariablesComponentShortName  >}} and the communication
interface generate messages, which are sent to the standard output for your use. For a list of the possible
errors that may occur with a File exit, see [Messages
and error codes](../../../../troubleshoot_intro/messages_and_error_codes_start_here).

You can also generate your own messages and error messages via the fields
indicated in the table below.


| Field  | Description  |
| --- | --- |
| ret2 | Error message (up to eight characters)<br /> To be defined in case of transfer refusal (ret1 = 9)<br /> This message appears in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog in the DIAGP (protocol diagnosis) field  |
| msg  | Error message (up to 512 characters)<br /> The user function can define this field at each stage<br /> This message is redirected to the standard output  |

