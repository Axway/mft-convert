{
    "title": "Communication  media ",
    "linkTitle": "CFTCOM - Communication media ",
    "weight": "240"
}The CFTCOM object defines the communication media used by {{< TransferCFT/axwayvariablesComponentShortName  >}}.
You can define as many CFTCOM objects as needed.

Related
topics

- Command syntax
    [CFTCOM](../../../c_intro_userinterfaces/command_summary#CFTCOM)
- Parameter list
    [CFTCOM](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftcom)

<span id="About"></span>

About the communication media CFTCOM
------------------------------------

The CFTCOM command enables {{< TransferCFT/axwayvariablesComponentShortName  >}} to open the media that communicates with
applications. Depending on the system, you can use one or more
of the following media:

- One or more shared
    files (set TYPE = FILE). Requests are entered in a file or files that Transfer CFT periodically
    scans. The presence of {{< TransferCFT/axwayvariablesComponentShortName  >}} is not required to deposit requests unless
    such requests saturate the file. A CFTCOM command then defines a communication
    file and the frequency with which it is scanned by {{< TransferCFT/axwayvariablesComponentShortName  >}}.

<!-- -->

- A synchronous communication
    medium supported by the TCP/IP network (set TYPE= TCPIP)(for remote usage, use REST API)

<span id="About_Service_Files_Medium"></span><span id="CFT_service_file_media"></span><span id="CFT_monitor_media"></span>

{{< TransferCFT/axwayvariablesComponentShortName  >}} media files
----------------------------------------------------------------------

You cannot change the name of the **parameter
file**. It constitutes the {{< TransferCFT/axwayvariablesComponentShortName  >}} anchor point containing all the data
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

<!-- -->

- The {{< TransferCFT/axwayvariablesComponentShortName  >}} can be inactive at the time
    the commands assigned to it are issued, to the limit of the file size.
    Commands are taken into account at the time the {{< TransferCFT/axwayvariablesComponentShortName  >}} is activated,
    if a CFTCOM command relative to this communication file has been defined.
    A communication file can be created by the CFTFILE command.

<!-- -->

- For TCP synchronous
    mediums:
    -   Communication is only possible if the Transfer
        CFT is present.
        To retrieve the IDT and the IDTU values of the transfer, you can use the
        variables %_CAT_IDT% and %_CAT_IDTU%.

**Example**

```
CFTCOM ID = IDCOM2,
TYPE = TCPIP,
PROTOCOL = XHTTP,
HOST = localhost,
PORT = 1765
 
config TYPE=COM, MEDIACOM=TCPIP, FNAME=XHTTP://localhost:1765
SEND PART=PART1, IDF=TEST1
 
CFTU00I SEND _ Correct (IDT=H1018055 IDTU=A0000004 )
CFTU00I SEND _ Correct (part=PART1)
listcat idt=%_CAT_IDT%
```
