{
    "title": "Transfer CFT communications",
    "linkTitle": "Transfer CFT communications",
    "weight": "250"
}You can communicate with {{< TransferCFT/axwayvariablesComponentShortName  >}} using a file  or a TCP/IP connection.

> **Note**
>
> Note: When using a multi-node architecture, the TCP/IP communication option is not available.

### Communication file

A communication file is a relative file created using the CFTUTIL or Copilot utility. It has a specific internal structure and must be accessible to all users wishing to submit requests to the monitor.

### TCP/IP connection

A TCP/IP communication medium is asynchronous, or connected.

An application cannot submit a service request via a synchronous communication medium unless {{< TransferCFT/axwayvariablesComponentShortName  >}} is active; you must first establish a TCP/IP connection between application and {{< TransferCFT/axwayvariablesComponentShortName  >}}.

{{< TransferCFT/axwayvariablesComponentShortName  >}} supplies an execution report for each service request. {{< TransferCFT/axwayvariablesComponentShortName  >}} supplies additional data such as the unique identifier of the transfer for transfer requests (SEND and RECV). This identifier is used for tracking.
