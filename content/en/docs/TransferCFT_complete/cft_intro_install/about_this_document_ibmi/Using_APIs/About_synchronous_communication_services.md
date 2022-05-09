{
    "title": "Synchronous  communication services",
    "linkTitle": "Synchronous communication services",
    "weight": "260"
}This topic describes {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication services.

Description of functions
------------------------


| Function | Use |
| --- | --- |
| COM | Set the communication medium |
| GETXINFO | Retrieve information concerning the last transfer made from a synchronous request after a request of the following types: SEND, RECV, HALT, KEEP, START, RESUME, DELETE, END, SUBMIT, SWITCH, PURGE.<br/> The information is stored in a cftApiInf-type structure:<br/> • Transfer state<br/> • Diagnostic<br/> • Diagnostic protocol<br/> • Value of the PART field of CFTPARM<br/> • Transfer identifier (IDT)<br/> • Local transfer identifier (IDTU)<br/> • Transfer type (single, cyclical, diffusion list, collection, file group)<br/> • Public reference of the transfer (only for a single transfer in Send)<br/> The GETXINFO action returns an error if the communication medium is not synchronous.<br/> <blockquote> **Note**<br/> Note: The public reference of the transfer is a character string of variable length. In the PESIT protocol, it contains 'pi13.pi3.pi4.pi11.pi12.pi61.pi62'.<br/> </blockquote>  |

