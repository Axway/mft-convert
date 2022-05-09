{
    "title": "Administrative configuration commands",
    "linkTitle": "Manage the configuration",
    "weight": "200"
}The topics in this section describe how to use command line operations to administer Transfer CFT configuration options, and is comprised of the following:


| Command  | Description  |
| --- | --- |
| [CFTACCNT](cftaccnt_concepts)  | Recording mode for statistical data  |
| [CFTCAT](catalog_parameter_concepts)  | Catalog attributes  |
| [CFTCOM](communication_media_concepts)  | Communication media  |
| [CFTLOG](../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftlog)  | Transfer log files  |
| [CFTNET](network_resource_concepts)  | Network resources  |
| [CFTPARM](cftparm_general_parameters)  | General parameters definition  |
| [CFTPROT](transfer_protocol_concepts)  | Transfer protocol  |


What is a service file medium
-----------------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} medium refers to any data support or local communication
means used by {{< TransferCFT/axwayvariablesComponentShortName  >}}. A distinction is made between the media:

- Accessed by the
    {{< TransferCFT/axwayvariablesComponentShortName  >}}
- Accessed by the
    {{< TransferCFT/axwayvariablesComponentShortName  >}} utility
- Used by the interactive
    functions
- Used by the programming
    interface

### Definitions

The following terms are used in this section:

- PHYSICAL FILE which
    describes the name and physical location of a file type medium
- If the file name
    begins with a reserved character, designated by ****char-file**** - see the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding to your OS,
    this is a logical name interpreted by {{< TransferCFT/axwayvariablesComponentShortName  >}}
- CFTIN and CFTOUT
    for standard task input/output
- TCP synchronous
    medium to designate a communication channel

The {{< TransferCFT/axwayvariablesComponentShortName  >}} medium names can be set up for the:

- Programming interface
    (catalog name, communication medium name with the {{< TransferCFT/axwayvariablesComponentShortName  >}}):
    by the OPEN and COM services
- {{< TransferCFT/axwayvariablesComponentShortName  >}}:
    CFTPARM, CFTCOM, CFTCAT, CFTLOG, CFTACCNT objects (the name of the Parameter
    file cannot be changed)
- {{< TransferCFT/axwayvariablesComponentShortName  >}} utility:
    CONFIG command
- Interactive functions:
    the customization function is used to set the names of the Parameter,
    Partner, Catalog and Log files, and the {{< TransferCFT/axwayvariablesComponentShortName  >}} communication medium

<span id="CFT_monitor_media"></span>

### {{< TransferCFT/axwayvariablesComponentShortName  >}} media files

You cannot change the name of the **parameter
file**. It constitutes the {{< TransferCFT/axwayvariablesComponentShortName  >}} anchor point containing all the data
defined by the parameter setting, and in particular the names of the other
files and media. However, you can establish a correspondence between this
logical name and a physical name in the flow of CFT activation commands.

The names of the other {{< TransferCFT/axwayvariablesComponentShortName  >}} media are defined by parameter setting,
although {{< TransferCFT/axwayvariablesComponentShortName  >}} has default values.

****{{< TransferCFT/axwayvariablesComponentShortName  >}}
media by type of object and parameter****


| Object  | Parameter  | File type described  |
| --- | --- | --- |
| CFTCAT  | FNAME  | Catalog file  |
| CFTCOM  | FNAME  | Communication medium  |
| CFTLOG  | FNAME<br /> AFNAME  | Log files  |
| CFTACCNT  | FNAME<br /> AFNAME  | Statistics files  |
| CFTPARM  | PARTFNAM  | Partner file (obsolete for Windows/Unix) |
| CFTXLATE  | FNAME  | File containing the description of a translation table  |
| CFTDEST  | FNAME  | File containing a list of partners  |
| CFTAUTH  | FNAME  | File containing a list of authorized or prohibited IDFs  |
| CFTPARM  | EXEC ...  | End-of-transfer procedures  |
| CFTLOG  | EXEC  | Log switching procedure  |
| CFTACCNT  | EXEC  | Statistics file switching procedure  |


Depending on the system, {{< TransferCFT/axwayvariablesComponentShortName  >}} supports the following communication
media:

- Direct organization
    file (relative)
- TCP synchronous
    medium

For a communication medium based on TCP/IP:

- Host name
- Configuration file
    name

Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding
to your OS.

<span id="Utility_media__CFTUTIL"></span>

### CFTUTIL media utility

Use the CFTUTIL utility command CONFIG to set the names of the media
it accesses, as presented in the table:


| CONFIG command<br/> TYPE parameter | File type described  |
| --- | --- |
| PARM  | Parameter file  |
| PART  | Partner file  |
| CAT  | Catalog file  |
| COM  | Communication medium  |
| INPUT  | Input data medium  |
| OUTPUT  | Output data medium  |


By default, in the absence of the CONFIG command, the media accessed
by the utility are defined in the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide*
corresponding to your OS.

Depending on the operating system, the media names may be physical files
or logical files. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding
to your OS.

Depending on the system, the {{< TransferCFT/axwayvariablesComponentShortName  >}} utility supports the following
communication media:

- Direct organization
    file (relative)
- TCP synchronous
    medium

For a communication medium based on TCP/IP:

- Host name
- Configuration file
    name

<span id="Programming_interface_media"></span>

### Programming interface media

The programming interface provides the following services:

- Access to the transfer
    catalog
- Dispatch of commands
    to CFT

The tables below list the media names by service for the COBOL and C
language applications.

#### **Media names by service in COBOL language interface**


| Service  | Command  | Parameter  | **File type described**  |
| --- | --- | --- | --- |
| CFTI  | OPEN  | D-CAT  | Catalog file  |
| CFTU  | COM  | D-COM  | CFT communication medium  |


#### **Media names by service in C language interface**


| Service  | Command  | Parameter  | File type described  |
| --- | --- | --- | --- |
| cftai  | OPEN  | cat  | Catalog file  |
| cftaix  | OPEN  | cat  | Catalog file  |
| cftau  | COM  | param  | {{< TransferCFT/axwayvariablesComponentShortName  >}} communication medium  |


The default names of the media accessed by the programming interface,
CATALOG file and COMMUNICATION medium, are defined in the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding to your OS.

Depending on the system, the programming interface supports the following
communication media:

- Direct organization
    file
- TCP synchronous
    medium

And in the case of a communication medium based on TCP/IP

- Host name
- Configuration file
    name

Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding to your
OS.

<span id="Interactive_function_media"></span>

### Interactive function media

The interactive function media are defined by the customization function
of this component. The table below indicates, for each field, the file
type described:


| Field  | File type described  |
| --- | --- |
| Parm  | Parameter file  |
| Part  | Partner file  |
| Cat  | Catalog files  |
| Log1  | Log files  |
| Log2  |   |
| Com  | Communication medium  |


If this function is not activated, the media accessed are those defined
at the time the product is installed.

Depending on the system, the interactive functions support the following
communication media:

- Direct organization
    file
- TCP synchronous
    medium

For a communication medium based on TCP/IP:

- Host name
- Configuration file
    name

Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}}*Operations Guide* corresponding to your
OS.
