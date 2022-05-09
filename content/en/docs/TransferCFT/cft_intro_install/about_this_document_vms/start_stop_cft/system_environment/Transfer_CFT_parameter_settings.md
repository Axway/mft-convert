{
    "title": "Transfer CFT parameter settings",
    "linkTitle": "Transfer CFT parameter settings",
    "weight": "260"
}A communication media must be configured before starting {{< TransferCFT/axwayvariablesComponentShortName  >}}. The CFTCOM command defines the communication medium and its attributes (medium type, name, etc.).

```
CFTCOM
ID= identifier,
TYPE= {FILE &#124; TCP },
TYPE= TCP
PROTOCOL= {XHTTP &#124; XHTTPS},
HOST = string,
PORT = number ,
[ADDRLIST= (string,string…),]
[DISCTS= number ,]
```

Use the {{< TransferCFT/axwayvariablesComponentShortName  >}} API commands
-------------------------------------------------------------------------------

The following service requests are specific to TCP/IP:

- GETXINFO retrieves the report and information relating to the last transfer request (SEND or RECV).

<!-- -->

- CLOSEAPI frees system resources allocated by the command API.

To implement TCP/IP communication, you must:

- Configure Transfer CFT (CFTCOM command)

<!-- -->

- Set the API command parameters (COM function)

### COM function

Use the COM function of the command API to force the communication medium, type and name, from an application.

For example, the CFTUTIL command CONFIG TYPE=COM calls the COM function to configure a communication medium.

### Command API configuration file contents

A configuration file is a text file. It contains lines of instructions with one instruction per line. A comment line must start with a hash character: \#.

The following two parameters are mandatory:

- TYPE= {FILE &#124; TCP}
    -   Mandatory instruction used to declare the communication type.
- NAME= MediaName
    -   Mandatory instruction used to declare the name of the communication medium. Depending on the type, it is the name of the communication file for synchronous communication, the complete name of the TCP/IP communication channel (protocol://host:port).

For a TCP/IP communication type (TYPE = TCP), the protocol field indicates the protocol implemented on the TCP/IP layer. For more information see the PROTOCOL parameter of the command CFTCOM. The protocol must correspond to the one declared on the monitor. The host field indicates the name of the host or the IP address of the machine where the {{< TransferCFT/axwayvariablesComponentShortName  >}} you want to reach is executing (local host only for machine-internal communication). The port field indicates the listening port of the {{< TransferCFT/axwayvariablesComponentShortName  >}} to contact.

Example of API configuration file:

```
\# CFT API CONFIGURATION FILE
\# TCP/IP COMMUNICATION
TYPE=TCP
NAME=xhttp://localhost:5001
```

If a configuration file contains several communication medium definitions, the first definition is taken into account.

### GETXINFO function

Use this command API function to restore additional information that Transfer CFT returns for the SEND and RECV commands, via a structure of the type cftApiInf (file definition CFTAPI.H).

### CLOSEAPI function

Use this function to free system resources allocated by the command API (memory, network). It is strongly recommended that you call the CLOSEAPI function when the communication medium is TCP/IP.

### Return code

With TCP/IP, the command API returns a processing report (rc or CFTRC field of Z-RC).

### CFTUTIL

To use a TCP/IP communication media with {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must first enter the CONFIG command.

```
CONFIG
TYPE= {COM &#124; … },
[MEDIACOM= {FILE &#124; TCP},]
FNAME= string
```

The MEDIACOM parameter is optional. It accepts the values FILE and TCP.

If the value of the MEDIACOM parameter is TCP, the FNAME parameter indicates the name of the communication channel (protocol://host:port). If the MEDIACOM parameter is absent, the FNAME parameter indicates the name of a configuration file.

Examples:

```
config type=com,mediacom=tcp,fname=xhttp://172.17.5.1:6550
config type=com,fname=cft231prd/conftcp
```

### User group

When a file is used to communicate, it is accessed by a number of concurrent processes in both the write and read modes. Concurrent access to this file is controlled through the DISTRIBUTED LOCK MANAGER. By default, it is enabled for the group.

If users belonging to different groups are likely to submit commands in the communication file, it must be locked at system level. Therefore, you must:

- Grant the SYSLCK privilege to {{< TransferCFT/axwayvariablesComponentShortName  >}} and all users writing in the communication file
- Set the CFT_LOCK logical name to SYSTEM for the job in the CFTLOGIN.COM procedure using the following command for example:` $ define/job CFT_LOCK SYSTEM`

Any value other than SYSTEM only locks the file at group level. Users from other groups then have uncontrolled access to the file, which consequently may affect the contents.
