---
title: "Transfer CFT messages  and error codes"
linkTitle: "Messages "
weight: 250
---This section lists the different types of messages that {{< TransferCFT/axwayvariablesComponentLongName  >}} generates, and corrective actions when applicable. It begins with this section, which describes message formats, severity, and additional conventions used in this documentation.

## Message format

### Format in the documentation

{{< TransferCFT/axwayvariablesComponentShortName  >}} messages provide information on the status of the {{< TransferCFT/axwayvariablesComponentShortName  >}}. Messages have the general format and supporting information:


| The message severity is displayed | CFTxxx: the actual message that is displayed on {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| --- | --- |
| Explanation | The elements, such as variables, in the above message are detailed. |
| Consequence | Description of what happens to the {{< TransferCFT/axwayvariablesComponentShortName  >}}, or lists corrective actions. |
| Action  | If applicable, add corrective action here.  |


<span id="Message_format"></span>

### Format in the product

Earlier versions of {{< TransferCFT/axwayvariablesComponentShortName  >}} used a different message format
than the version 3.1.3 and higher. The error messages displayed in this document use the former, or earlier version, format. If your system uses
the CFTLOG parameter Format = V24,
the log display is as shown below:

```
CFTXXX: fixed text message <variables>
```

**Example**

****CFTLOG FORMAT=[V23,V24]****

- For V23: CFTT57I
    PART=&part IDF=&idf IDT=&idt &str transfer started
- For V24: CFTT57I
    &str transfer started   &lt;IDTU=&idtu
    PART=&part IDF=&idf IDT=&idt>

### Auto documented messages

Certain messages that are auto-documented, for example CFTA01I, CFTA02W, CFTA03E, CFTA04F, may not appear in this documentation. These messages are considered self-explanatory.

## Writing conventions

Messages are written according to the following conventions.

### Message description

The {{< TransferCFT/axwayvariablesComponentShortName  >}} messages use the format CFTxnns, for example CFTC01E. The elements that make up the message format are described in the following sections.

Where:

- x: message source
- nn: sequence number
- s: message severity

### Message source


| Code  | Meaning  |
| --- | --- |
| C  | Catalog: Access to the catalog  |
| E  | End: {{< TransferCFT/axwayvariablesComponentShortName  >}} shutdown phase  |
| F  | File: Access to files  |
| H  | External PeSIT: PeSIT protocol, non-SIT profile and CFT profile  |
| I  | Init: {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization phase  |
| N  | Network  |
| P  | Parameter: Access to parameter files |
| R  | Request: Requests that {{< TransferCFT/axwayvariablesComponentShortName  >}} received from CFTUTIL, applications, or interactive functions  |
| S  | System: System interface operations by the {{< TransferCFT/axwayvariablesComponentShortName  >}}  |
| T  | Transfers: Actions relating to transfers  |
| U  | CFTUTIL: Messages from the CFTUTIL utility  |
| W  | Miscellaneous: Various messages related to transfers  |
| X  | Security: Security system (only in the log)  |
| Y  | SSL: SSL protocol  |


### Sequence number

The sequence number is an index characterizing the message within a given class.

### Severity

The severity code is described in the following table.


| Code  | Indicates  |
| --- | --- |
| I  | Informational message only  |
| W  | An anomaly which may be an error  |
| E  | An error requiring correction (parameter setting or environment error)  |
| F  | A serious system error requiring the intervention of Product Support  |


### Categories

Transfer CFT messages belong to either a system or operating category:

- Operating: Includes all messages related to transfers and CRON jobs.
- System: Includes all internal messages, such as Transfer CFT start, stop, catalog, tasks, multi-node, and so on.

This applies to messages sent to the user defined in the notify parameter in CFTLOG object.

### Symbolic variables used in message text

The table below lists the symbolic variables used in message text.


| Code | Description |
| --- | --- |
| char | Alphanumeric character |
| cr | Function return code |
| cmd | Parameter setting or operator command name<br /> Example: CFTPARM, SEND |
| cpu_id | Host computer's CPU number |
| ctx | Internal context |
| diagn | Diagnostic code of a network error<br /> Specific to the access method and, in some cases, to the system<br /> Expressed in hexadecimal form |
| diagi | Internal Transfer CFT diagnostic code (DIAGI) of the catalog |
| diagp | Transfer CFT protocol diagnostic code (DIAGP) of the catalog |
| dest | Partner list identifier (CFTDEST command) |
| direct | Transmission direction |
| fname | File name |
| host | Physical address of the remote partner |
| id | Command identifier (value of the ID parameter) |
| idf | Model file identifier (CFTSEND/CFTRECV command) |
| idt | Transfer identifier |
| keyw | Keyword in a parameter setting command or an operator request<br /> Example: PART, DIRECT |
| local | Location of a network error:<br /> 1: local<br /> 0: remote |
| label | Freeform name relating to the software protection key |
| maxtrans | Number of transfers authorized at any one time |
| mode | Action requested |
| n | Numeric character |
| nb | Numeric code |
| ncr | General network error code |
| ncs | Network error code specific to the access method and system |
| net | Network resource identifier (CFTNET command) |
| part | Local partner identifier (CFTPART command) |
| prot | {{< TransferCFT/axwayvariablesComponentShortName  >}} protocol identifier (CFTPROT command) |
| pevent | Protocol event |
| pid | Process identifier |
| pstate | Protocol status |
| recov | General error recovery code (in the case of a network error), independent of the system or access method |
| reason | Reason code for a network error<br /> Specific to the access method and, in some cases, to the system<br /> Expressed in hexadecimal form |
| sappl | SAPPL parameter value (name of the sending application) |
| scs | System return code describing a system interface access error |
| state | Transfer status |
| str | Character string forming the message label |
| vfm | VFM base name |

