{
    "title": "CFTPARM  - General parameters",
    "linkTitle": "General parameters - CFTPARM",
    "weight": "170"
}The <span id="Defining_CFTPARM"></span>CFTPARM object is used to specify parameters
that control the operation of Transfer CFT as a whole and to select other
parameter setting commands to be used at run time. The [Symbolic variables](../../../command_summary/symbolic_variables) topic describes
how symbolic variables can be used in the identification and in the body
of these procedures.

Specific SSL, transport security, parameters are described in the topic
[Configuring
Transport Security: Start here](../../../../transport_security_start_here/configuring_transport_security_start_here).

****Related
topics****

- Command syntax
    [CFTPARM](../../../command_summary#CFTPARM)
- Object concepts
    [General parameters
    definition](../../../../admin_intro/admin_config_commands/cftparm_general_parameters)

Use this command to:

- Specify
    the parameters which control overall Transfer CFT operations
- Select
    the other parameter setting commands which should be taken into account
    during execution


| Parameter  | Description  |
| --- | --- |
| [CFTACCNT](../../../command_summary/parameter_intro/accnt)). |
| [see table](../../../command_summary/parameter_intro/bufsize)  | Size of the monitor internal buffer used to exchange data between monitor tasks, expressed in characters in bytes. |
| [Defining the catalog parameters](../../../command_summary/parameter_intro/cat)). |
| [Defining the communication media](../../../command_summary/parameter_intro/com)). |
| [COMMENT](../../../command_summary/parameter_intro/comment)  | Character comment field. |
| CTLPASSW | Obsolete. |
| [DEFAULT](../../../command_summary/parameter_intro/default) | Default identifier, generically indicated as &lt;defaut&gt;, of the CFTRECV, CFTSEND, and CFTXLATE commands. |
| [EXECRE](../../../command_summary/parameter_intro/execre) | Generic name of the file describing the procedures to be executed, following an incident (Error) occurring during a receive transfer, the transfer changing to the H or K state. |
| [EXECRF](../../../command_summary/parameter_intro/execrf) | Generic name of the file describing the procedures to be executed on completion of reception of a file. |
| [EXECRM](../../../command_summary/parameter_intro/execrm) | Generic name of the file describing the procedures to be executed on completion of reception of a message. |
| [EXECSE](../../../command_summary/parameter_intro/execse) | Generic name of the file describing the procedures to be executed, following an incident (Error) during a send transfer, the transfer changing to the H or K state. |
| [EXECSF](../../../command_summary/parameter_intro/execsf) | Generic name of the file describing the procedures to be executed on completion of the sending of a file. |
| [EXECSFA](../../../command_summary/parameter_intro/execsfa)  | Generic name of the file describing the procedures to be executed on receiving an acknowledgement (REPLY type message), following the sending of a file. |
| [EXECSM](../../../command_summary/parameter_intro/execsm)  | Generic name of the file describing the procedures to be executed on completion of the sending of a message. |
| [EXECSMA](../../../command_summary/parameter_intro/execsma)  | Generic name of the file describing the procedures to be executed on receiving an acknowledgement, REPLY type message, following the sending of a message. |
| [EXITBOT]()  | EXIT identifier. To activate a beginning-of-transfer EXIT task, this identifier must point to a CFTEXIT command. |
| [EXITEOT](../../../command_summary/parameter_intro/exiteot) | EXIT identifier. To activate an end-of-transfer EXIT task, this identifier must point to a CFTEXIT command. |
| [ID](../../../command_summary/parameter_intro/id)  | Identifier of the CFTPARM command. |
| [KEY](../../../command_summary/parameter_intro/key)  | The name of the indirection file preceded by the &lt;file-symb&gt; character, which is system specific, and containing the set of keys associated with the Transfer CFT. |
| [LENAPPL](../../../command_summary/parameter_intro/lenappl) | Length to be taken into account when comparing the file/message identifier, IDF or IDM, with the identifier of a CFTAPPL command.<br/> See, Security concepts: Start here. |
| [LOG](../../../command_summary/parameter_intro/log) | Identifier of the monitor event log file description command CFTLOG.<br/> If this parameter is not defined, the Transfer CFT writes logging messages to the standard output of the monitor. |
| [MAXTASK](../../../command_summary/parameter_intro/maxtask)  | Number of file access tasks authorized. |
| [MAXTRANS](../../../command_summary/parameter_intro/maxtrans) | {{% TransferCFT/snippets/parm_maxtrans%}}  |
| [NET](../../../command_summary/parameter_intro/net) | List of the identifiers of the description commands for network access methods and monitor network resources CFTNET. |
| [Managing](../../../command_summary/parameter_intro/npart) Protocols to check its size and format. |
| [PART](../../../command_summary/parameter_intro/part) | Local identifier, identifying the site on which Transfer CFT is executed.<br/> This parameter is an item of information appearing in the transfer catalog. |
| [PARTFNAM](../../../command_summary/parameter_intro/partfnam)  | Partner file name (obsolete for Windows/Unix). |
| [PROT](../../../command_summary/parameter_intro/protocol)  | Identifier of the Transfer CFT protocol description commands CFTPROT.<br/> Transfer CFT protocol refers to both file transfer application protocols and network access methods. |
| [RCVALLER](../../../command_summary/parameter_intro/rcvaller)  | Option to be used if an error occurs when receiving available files FILE=ALL option. |
| [SECFNAME](../../../command_summary/parameter_intro/secfname) | Name of the CFT internal security file. |
| [TRANTASK](../../../command_summary/parameter_intro/trantask) | Number of transfers in parallel performed by a file access task, above which a new task is created, if possible. |
| [TRKPART](../../../command_summary/parameter_intro/trkpart) | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [TRKRECV](../../../command_summary/parameter_intro/trkrecv) | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [TRKSEND](../../../command_summary/parameter_intro/trksend) | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| [USERCTRL](../../../command_summary/parameter_intro/userctrl) | Transferred file access control option. |
| [see table](../../../command_summary/parameter_intro/waitresp) | Timeout in seconds used during internal communication between monitor tasks.<br/> This parameter is used during a synchronous exchange of requests between two monitor tasks. After waitresp seconds without reply, the timeout is interrupted. A message CFTS09 is written in the log. The task in question is then stopped, CFTTCOM task, for example.<br/> During the initialization phase, this parameter checks the time allowed for each of the Transfer CFT monitor tasks to start. In the event of an insufficient value (case of a highly loaded computer), the Transfer CFT monitor initialization stops.<br/> The following table indicates the default value for each system. |
| [WAITTASK](../../../command_summary/parameter_intro/waittask)  | Time during which a file access task is inactive in minutes before being shut down. |


<span id="Changing_the_initial_IDPARM_in_CFTUTIL"></span>

### Changing the initial IDPARM in CFTUTIL

The format that you use to enter commands in CFTUTIL is operating system
dependent. For more information, refer to the *Installation and Operations Gudie*that corresponds
with your operating system.

<span id="CFTPARM_command_line_examples"></span>

CFTPARM command line examples
-----------------------------

This topic lists and describes an example of the CFTPARM object using
command line operations to define the CFTPARM object.

****Example****

The CFTCAT, CFTCOM, CFTLOG, CFTACCNT, CFTNET and CFTPROT
parameter setting objects are not taken into account, during execution,
unless they were selected through the corresponding parameters: CAT, COM,
LOG, ACCNT, NET and PROT.

```
CFTPARM
CAT = CAT1,
COM = COM1,
ID = PARM1,
KEY = ‘XXXXXXXXXXXXXXXXXXXXX’,
NET = (),
PART = identifier,
PROT = (PESITCFT ),
BUFSIZE = 4096
DEFAULT = IDFDEF,
EXECRE = <filename4>,
EXECRF = <filename2>,
EXECSE = <filename3>,
EXECSF = <filename1>,
LOG = LOG1,
MAXTASK = 1,
MAXTRANS = 4,
NPART = MYCFT,
TRANTASK = 1,
WAITRESP = 500
```
