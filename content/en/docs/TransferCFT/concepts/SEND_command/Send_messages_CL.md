{
    "title": "Send  a message",
    "linkTitle": "Sending a MESSAGE",
    "weight": "230"
}You can use the SEND MESSAGE command to send a message to a designated partner, for example small amounts of content that are not considered sensitive content.

- The message length in the PeSIT protocol has a maximum value of 4096 bytes
- The message can be extracted from the Catalog file, and redirected to a specified file (fout parameter), using the DISPLAY command:

**Example**

```
CFTUTIL SEND part=PART, idm=MSG, type=message, msg=’hello’CFTUTIL DISPLAY content=msg, fout=msgfile, direct=recv, idt=IDT
```

**Result**

The resulting msgfile will contain the message 'hello'.

> **Note**
>
> Note: The &lt;TransferCFTinstallation&gt;\\runtime\\conf folder contains the dspcnf.xml file, a DISPLAY command template, which includes a specific filter templatel id='msg' that enables message extraction.

See the [xlate](../../../c_intro_userinterfaces/command_summary/parameter_intro/xlate) parameter for transcoding details.

### Sending messages


| Parameters | Description |
| --- | --- |
| [IDA](../../../c_intro_userinterfaces/command_summary/parameter_intro/ida)  | Local transfer identifier assigned by the user or user application. |
| [IDM](../../../c_intro_userinterfaces/command_summary/parameter_intro/idm)  | Message identifier. |
| [PART](../../../c_intro_userinterfaces/command_summary/parameter_intro/part)  | Transfer partner identifier. |
| [TYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/type) = MESSAGE | Characterizes a message send request. |
| OPTIONAL PARAMETERS for [SEND](../send_command_basics) | The optional common parameters form the SEND subset, used for sending a message (but not applicable to files). |


#### Example

A message using the identifier ANDREW.

```
SEND
IDM = ANDREW,
MSG = ‘ANDREW: call PETER ’,
PART = HQ,
TYPE = MESSAGE,
```
