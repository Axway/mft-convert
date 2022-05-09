{
    "title": "Communication media - CFTCOM  ",
    "linkTitle": "Communication media - CFTCOM ",
    "weight": "200"
}This topic describes the CFTCOM object and parameters. You can use this
command to define the communication media used by Transfer CFT.

****Related
topics****

- Command syntax
    [CFTCOM](../../../command_summary#CFTCOM)
- Object concepts
    [About the
    communication media](../../../../admin_intro/admin_config_commands/communication_media_concepts)

For a description of the object and CFTCOM concepts, go to [CFTCOM
communication media concepts](../../../../admin_intro/admin_config_commands/communication_media_concepts).

As described in the [CFTCOM
concepts: Start here](../../../../admin_intro/admin_config_commands/communication_media_concepts), you can set the communication medium to:

- [File](#TYPE=FILE)
- [TCP/IP](#TYPE=TCPIP)

First define the TYPE, and then define the corresponding communication media parameters.

<span id="TYPE=FILE"></span>

### TYPE=FILE

`NAME = filename`

Designates the communication file name.  

`[ WSCAN = { 60   &#124; n}] {1..3600}`

Communication file scanning time in seconds. Determines the time taken for the Transfer CFT
to process a command. The optimum value is a tradeoff between the desired
response time and the computer’s workload.

<span id="TYPE=TCPIP"></span>

### TYPE=TCPIP

Use CFTCOM TCP/IP for synchronous Transfer CFT communication on the local network.

`[ DISCTS = n ]`

Without a request , the timeout in seconds before
freeing a channel opened by a client.

`HOST = string1..64`

The IP address of the local resource, localhost or 127.0.0.1.

`PROTOCOL = { XHTTP }`

Request/reply protocol implemented on the TCP/IP layer is XHTTP; and HTTP protocol
variant, property of Axway Software.

`PORT = n`

Listening port on the networks defined in the HOST
parameter.

<span id="Defining_CFTCOM_TCPIP"></span>

CFTCOM TCPIP
------------

This table describes the parameters to define the CFTCOM object when the communication
type is TCPIP.


| Parameters  | Description  |
| --- | --- |
| [HOST](../../../command_summary/parameter_intro/host) | Networking IP address of the local resource. |
| [ID](../../../command_summary/parameter_intro/id)  | Identifier of the CFTCOM object. |
| [MODE](../../../command_summary/parameter_intro/mode) | Action to do in the parameter or partner base. This parameter applies to all commands that affect CFT bases. |
| [PORT](../../../command_summary/parameter_intro/port) | Listening port of the network. |
| [PROTOCOL](../../../command_summary/parameter_intro/protocol) | Defines the remote TCP network resource |
| [TYPE](../../../command_summary/parameter_intro/type) | Transfer CFT communication means. |


<span id="Defining_CFTCOM_FILE"></span>

CFTCOM FILE
-----------

This table describes the parameters to define the CFTCOM object when the communication
type is FILE.


| Parameters  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id)  | Identifier of the CFTCOM command. |
| [MODE](../../../command_summary/parameter_intro/mode) | Action to do in the parameter or partner base. This parameter applies to all commands that affect Transfer CFT bases. |
| [TLVCEXEC](../../../command_summary/parameter_intro/tlvcexec)  | Batch to execute when the alert ends.  |
| [TLVCLEAR](../../../command_summary/parameter_intro/tlvclear)  | Level below which the alert ceases, as a percentage of filling, where 0% indicates the file is empty and 100% that it is full. |
| [TLVWARN](../../../command_summary/parameter_intro/tlvwarn)  | Command file usage limit before issuing an alert, as is a percentage of filling, where 0% indicates the file is empty, and 100% that it is full.<br/> Once this limit is reached, the CFTCOM/TLVWEXEC is executed. |
| [TLVWEXEC](../../../command_summary/parameter_intro/tlvwexec)  | Batch to execute when CFTCOM/TLVWARN is reached.  |
| [TLVWRATE](../../../command_summary/parameter_intro/tlvwrate)  | The minimum amount of time, in seconds, to wait before resending an alert.  |
| [TYPE](../../../command_summary/parameter_intro/type)  | Transfer CFT communication means. |
| [WSCAN](../../../command_summary/parameter_intro/wscan) | The frequency, in seconds, with which the Transfer CFT scans the communication file. |


****Example****

TYPE=FILE

```
CFTCOM ID = IDCOM1,
TYPE = FILE
NAME = <filename>,
WSCAN = 120
```
