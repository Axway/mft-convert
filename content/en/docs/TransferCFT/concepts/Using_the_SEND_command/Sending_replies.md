---

    title: Use the SEND acknowledgement commands  
    linkTitle: Sending a REPLY
    weight: 220

---
<span id="About_the_SEND_REPLY_Command"></span>

## 

## Sending an acknowledgement

This topic describes the SEND REPLY command, which sends a positive acknowledgement.

The SEND TYPE = REPLY command
initiates the sending of a reply type message
to a previous transfer from the partner that the message was
intended for. The partner monitor interprets this message as a transfer
acknowledgement.

The REPLY sender must define the IDT parameter corresponding
to the identifier of the transfer to be acknowledged. This facility may
be used, for example, after processing a file received, or a message, in order to inform the sender that
all the operations related to this receive transfer have been correctly
accomplished.

The protocol used to acknowledge a transfer must be the same as the
one used for the transfer. A transfer acknowledgement is a protocol concept.

## Sending a negative acknowledgement

This topic describes the SEND NACK command, which sends a negative acknowledgement.

Via negative acknowledgments, the
final partner signals to the initial sender of the file that application
errors were detected.

If the initial sender does not support this function, as for example in earlier versions of Transfer CFT, the final partner does not transmit the
negative acknowledgement and the Transfer CFT log file displays:

```
CFTT93W PART=XFB1 IDS=00008 Negative ack not supported by server
```

******Example******

```
cftutil send
type=nack,
part=&part,
idm=nack,
msg=recu,
idt=&idt
```


| Parameters  | Description  |
| --- | --- |
|   | Command summary: Use the SEND TYPE = REPLY command to initiate the sending of a message of a particular type. This message is a reply to a previous transfer from the partner. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/exec">EXEC</a> | Filename. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/ida">IDA</a>  | Local transfer identifier assigned by the user or user application. The maximum length is 64 characters. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idm">IDM</a>  | Message identifier. The value of this identifier is unrestricted. |
| <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/idu">IDT</a>  | Identifier of the original transfer acknowledged by this message. |
| ****<a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/msg">MSG</a>**** | Message |
| ******<a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/part">PART</a> ****** | Transfer partner identifier. |
| ****<a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/pri">PRI</a>**** | Request selection priority. |
| ******<a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/type">TYPE</a> = REPLY****** | Characterizes a reply send transfer. |
| Other parameters  | For optional parameters common to SEND: see the <a href="../../../c_intro_userinterfaces/command_summary#SEND">SEND</a> command.  |

