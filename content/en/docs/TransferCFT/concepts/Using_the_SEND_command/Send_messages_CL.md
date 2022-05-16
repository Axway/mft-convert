---

    title: Send  a message
    linkTitle: Sending a MESSAGE
    weight: 240

---
You can use the SEND MESSAGE command to send a message to a designated partner, for example small amounts of content that are not considered sensitive content.

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
> The &lt;TransferCFTinstallation>\\runtime\\conf folder contains the dspcnf.xml file, a DISPLAY command template, which includes a specific filter templatel id='msg' that enables message extraction.

### Sending messages


| Parameters | Description |
| --- | --- |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/ida">IDA</a>  | Local transfer identifier assigned by the user or user application. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idm">IDM</a>  | Message identifier. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/part">PART</a>  | Transfer partner identifier. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/type">TYPE</a> = MESSAGE | Characterizes a message send request. |
| OPTIONAL PARAMETERS for <a href="../send_command_basics">SEND</a> | The optional common parameters form the SEND subset, used for sending a message (but not applicable to files). |


#### Example

A message using the identifier ANDREW.

```
SEND
IDM = ANDREW,
MSG = ‘ANDREW: call PETER ’,
PART = HQ,
TYPE = MESSAGE,
```
