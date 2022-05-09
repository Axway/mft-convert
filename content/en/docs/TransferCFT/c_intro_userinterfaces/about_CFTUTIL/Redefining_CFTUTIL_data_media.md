{
    "title": "Set the default CFTUTIL file names (CONFIG)",
    "linkTitle": "CONFIG - Set default file names",
    "weight": "200"
}The <span id="CONFIG_command"></span>CONFIG command redefines the data media
that the CFTUTIL utility uses. A medium refers to any data medium or local
communication means.

See [Defining
the communication media](../../../admin_intro/admin_config_commands/communication_media_concepts).

When activated, CFTUTIL uses the default data media such as master files,
log files, communication with the Transfer CFT, as defined in Transfer
CFT. See the specific *Operations Guide* that corresponds to your
Operating System.

You can use the CONFIG command when you want to read from or write
to a medium other than these default media. In this case, the CONFIG command
must be used before executing the other commands processed by CFTUTIL.

The CFTUTIL data media are designated in this command by the TYPE parameter.
There are two different types of media categories relating to this parameter:

The media interfacing
with those used by the Transfer CFT. The data associated with
these media is used by CFTUTIL to:

- Configure
    and query the Transfer CFT parameters with the option TYPE =PARM
- Configure
    and query the partners managed by the Transfer CFT with the option
    TYPE = PART
- Query the
    catalog used by the Transfer CFT with the option TYPE = CAT
- Communicate
    with the Transfer CFT with the option TYPE = COM, through
    the communication media managed by the Transfer CFT

The media specific
to CFTUTIL used to:

- Send the
    commands to be processed (by CFTUTIL) with the option TYPE = INPUT

For TYPE = PARM, TYPE = PART, and TYPE = CAT, the associated media can
only be files. The CONFIG command is used to modify the names of these
files (FNAME parameter).

For TYPE = INPUT and TYPE = OUTPUT, the CONFIG command is used to redirect
the CFTUTIL input or output data stream, to a file whose name is specified
by the FNAME parameter.

If the user modifies the name of the input file (TYPE = INPUT), subsequent
CFTUTIL commands are read in the new input medium mentioned.

For TYPE = COM and depending on the system involved, the associated
media can be:

- A file
- TCPIP communication

For a communication medium based on TCP/IP the media can be:

- Host name
- Configuration file
    name

If this file does not exist or does not have the correct syntax, you are notified only during the processing of the first transfer request.

By using this syntax, you can dynamically modify the communication media
since the file is analyzed at each new transfer request.

You use the CONFIG command to change the Transfer CFT
communication medium (MEDIACOM parameter). The media that can be used for a given system and the default communication
medium associated with this system. [Communication media](../../../admin_intro/admin_config_commands/communication_media_concepts)

**Command syntax**: [CONFIG](../../command_summary#CONFIG)


| Command and Parameters | Description |
| --- | --- |
| **CONFIG** command | Use this command to redefine the data media with which the CFTUTIL utility operates.  |
|  [FNAME](../../command_summary/parameter_intro/fname)<br/>  | For TYPE = {CAT &#124; INPUT &#124; OUTPUT &#124; PARM &#124; PART }<br/> Name of the file associated with the medium type accessed by CFTUTIL. |
|  [FNAME](../../command_summary/parameter_intro/fname)<br/>  | For TYPE = COM<br/> There must be a correspondence with the CFTCOM NAME parameter that defines the communication medium as seen from Transfer CFT.<br/> For a communication medium supported by TCP/IP (MEDIACOM=TCPIP) this is either:<br/> • A host name (string) using the format: &quot;protocol://machine:port&quot;, or<br/> • A configuration file (filename) |
|  [MEDIACOM](../../command_summary/parameter_intro/mediacom)  | Defines the communication medium type if this medium is relevant to the system. |
|  [TYPE](../../command_summary/parameter_intro/type#type_CONFIG)  | Defines the medium concerned. |


Examples
--------

****Example 1: redirect output****

This command redirects the CFTUTIL output (used for
querying the LISTPARM or LISTPART commands, for example) to the file with
the generic &lt;filename&gt;.

```
CONFIG       TYPE = OUTPUT,
FNAME = <filename>
```

****Example 2: define filename****

Use this command to define the file with the generic
&lt;filename&gt; as the Transfer CFT communication medium.

```
CONFIG      TYPE = COM,
MEDIACOM = FILE
FNAME = <filename>
```

****Example 3: MEDIACOM=FILE****

Use to select a specific communication file.

If the {{< TransferCFT/headerfootervariableshflongproductname  >}} configuration file refers to 2 communication files, for example cftcom1 and cftcom2:

```
CFTPARM ID=IDPARM0, ..., COM=(COM1,COM2),....
CFTCOM ID=COM1,TYPE=FILE,FNAME=$CFTDIRRUNTIME/data/cftcom1,...
CFTCOM ID=COM2,TYPE=FILE,FNAME=$CFTDIRRUNTIME/data/cftcom2,...
```

To select the second communication file in a CFTUTIL session, enter:

```
CONFIG TYPE=COM,MEDIACOM=FILE,FNAME=$CFTDIRRUNTIME/data/cftcom2
```

****Example 4: MEDIACOM=TCPIP****

Use to select a TCPIP communication media.

If the {{< TransferCFT/headerfootervariableshflongproductname  >}} configuration file refers to a communication file and a TCPIP communication media:

```
CFTPARM ID=IDPARM0, ..., COM=(COM,COMS),....
CFTCOM ID=COM1,TYPE=FILE,FNAME=_CFTCOM,...
CFTCOM ID=COMS,TYPE=TCPIP,PROTOCOL=XHTTP,HOST=localhost,PORT=1763...
```

To select the TCPIP communication media in a CFTUTIL session, enter:

```
CONFIG TYPE=COM,MEDIACOM=TCPIP,FNAME=XHTTP://localhost:1763
```
