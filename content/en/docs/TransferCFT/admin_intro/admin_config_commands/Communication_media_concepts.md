---

    title: Communication  media 
    linkTitle: CFTCOM - Communication media 
    weight: 250

---
<span style="color: #800000; font-weight: normal; text-decoration: none;">Related
topics</span>

- Command syntax
    [CFTCOM](../../../c_intro_userinterfaces/command_summary#CFTCOM)
- Parameter list
    [CFTCOM](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftcom)

<span id="About"></span>

## About the communication media CFTCOM

The CFTCOM object defines the communication media used by {{< TransferCFT/axwayvariablesComponentShortName  >}}.
You can define as many CFTCOM objects as needed. Depending on the system,
you can set the communication medium to:

- One or more shared
    files: requests are entered in a file, which is periodically scanned
    by the {{< TransferCFT/axwayvariablesComponentShortName >}}
- A synchronous communication
    medium supported by the local TCP/IP network (for remote usage, use REST API)

<span style="font-weight: bold;">****z/OS****</span>: You must define one communication
file for BATCH/TSO applications and one for CICS applications, if needed.

<span style="font-weight: bold;">****Windows****</span>: On a LAN, requests
made by Windows applications concerning a {{< TransferCFT/axwayvariablesComponentShortName  >}} that is not on the
same computer can only be accomplished via a file.

The CFTCOM command enables {{< TransferCFT/axwayvariablesComponentShortName  >}} to open the media communicating with
applications. Depending on the system, it is possible to use one or more
of the following media:

- One, or more, shared
    files (parameter setting TYPE = FILE)  
    Requests are entered in this file (or files) which CFT periodically
    scans. The presence of {{< TransferCFT/axwayvariablesComponentShortName >}} is not required to deposit requests unless
    such requests saturate the file. A CFTCOM command then defines a communication
    file and the frequency with which it is scanned by {{< TransferCFT/axwayvariablesComponentShortName >}}.

<!-- -->

- A synchronous communication
    medium supported by the TCP/IP network (parameter TYPE= TCPIP)

The communication media which can be used for each system are indicated
in the OS specific *Installation and Operations Guide*.

The declaration of the communication medium used by a requesting application
program must be consistent with the media opened by {{< TransferCFT/axwayvariablesComponentShortName  >}}:

- Command CONFIG
    TYPE = COM, ... of the CFTUTIL utility
- COM function of
    programming interfaces
- 7 option – {{< TransferCFT/axwayvariablesComponentShortName >}} characteristics

<span id="About_Service_Files_Medium"></span><span id="CFT_service_file_media"></span><span id="CFT_monitor_media"></span>

## {{< TransferCFT/axwayvariablesComponentShortName  >}} media files

You cannot change the name of the <span style="font-style: italic;">**parameter
file**</span>. It constitutes the {{< TransferCFT/axwayvariablesComponentShortName  >}} anchor point containing all the data
defined by the parameter setting, and in particular the names of the other
files and media. However, you can establish a correspondence between this
logical name and a physical name in the flow of Transfer CFT activation commands.

The names of the other {{< TransferCFT/axwayvariablesComponentShortName  >}} media are defined by parameter setting,
although {{< TransferCFT/axwayvariablesComponentShortName  >}} has default values.

****{{< TransferCFT/axwayvariablesComponentShortName  >}}
media by type of object and parameter****

<span id="Communication_media_characteristics"></span>

### Communication media characteristics

- For file communication:

The {{< TransferCFT/axwayvariablesComponentShortName  >}} can be inactive at the time
the commands assigned to it are issued, to the limit of the file size.
Commands are taken into account at the time the {{< TransferCFT/axwayvariablesComponentShortName  >}} is activated,
if a CFTCOM command relative to this communication file has been defined.
A communication file can be created by the CFTFILE command.

- For TCP synchronous
    mediums:
    -   Communication is only possible if the Transfer
        CFT is present.
        To retrieve the IDT and the IDTU values of the transfer, you can use the
        variables %\_CAT\_IDT% and %\_CAT\_IDTU%.

**Example**

```
SEND
PART = PART1
IDF = TEST1
 
Then...
LISTCAT
IDT = %_CAT_IDT%
```
