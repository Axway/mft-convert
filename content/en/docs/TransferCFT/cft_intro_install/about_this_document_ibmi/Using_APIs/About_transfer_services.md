{
    "title": "Transfer  services",
    "linkTitle": "Transfer services",
    "weight": "250"
}Use the transfer services to send transfer control commands to Transfer
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
    in the {{< TransferCFT/axwayvariablesComponentShortName  >}} communication medium

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
| HALT | Interrupt one or more send or receive transfers with a given partner.<br/> The interrupted transfers are set to the &quot;H&quot; state and can be restarted at the partner's request. |
| KEEP | Suspend one or more send or receive transfers with a given partner.<br/> The interrupted transfers are set to the &quot;K&quot; state and can only be restarted by a START command. |
| START | Start one or more send or receive transfers |
| DELETE | Delete a catalog entry and any transfer in process associated with it |
| END | Set a transfer status to executed<br/> The transfer is set to the &quot;X&quot; state. This indicates that end-of-transfer procedure has been correctly executed. |
| SUBMIT | Submit the end-of-transfer procedure |
| SHUT | Shut down {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| SWITCH | Switch monitoring files, LOG, STATS... |
| CLOSEAPI | Free resources allocated at opening of communication medium: memory, network, file |
| COM | Define communication medium |
| GETXINFO | Retrieve information concerning the last transfer made from a synchronous request |

