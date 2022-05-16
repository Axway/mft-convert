---

    title: CFTPARM  - General parameters
    linkTitle: CFTPARM - General parameters
    weight: 390

---
The <span id="Defining_CFTPARM"></span>CFTPARM object is used to specify parameters
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
| <a href="../../../command_summary/parameter_intro/accnt">ACCNT</a> | Identifier of the description command of the statistical data record of the transfers (<a href="../../../../admin_intro/admin_config_commands/cftaccnt_concepts">CFTACCNT</a>). |
| <a href="../../../command_summary/parameter_intro/bufsize">BUFSIZE</a> <br/> <a href="../../../command_summary/parameter_intro/bufsize">see table</a>  | Size of the monitor internal buffer used to exchange data between monitor tasks, expressed in characters in bytes. |
| <a href="../../../command_summary/parameter_intro/cat">CAT</a> | Identifier of the command describing catalog file management (<a href="../../../web_copilot_ui/conf_intro/cftcat">Defining the catalog parameters</a>). |
| <a href="../../../command_summary/parameter_intro/com">COM</a>  | List of the identifiers of the communication media description commands (<a href="../../../../admin_intro/admin_config_commands/communication_media_concepts">Defining the communication media</a>). |
| <a href="../../../command_summary/parameter_intro/comment">COMMENT</a>  | Character comment field. |
| CTLPASSW | Obsolete. |
| <a href="../../../command_summary/parameter_intro/default">DEFAULT</a> | Default identifier, generically indicated as &lt;defaut&gt;, of the CFTRECV, CFTSEND, and CFTXLATE commands. |
| <a href="../../../command_summary/parameter_intro/execre">EXECRE</a> | Generic name of the file describing the procedures to be executed, following an incident (Error) occurring during a receive transfer, the transfer changing to the H or K state. |
| <a href="../../../command_summary/parameter_intro/execrf">EXECRF</a> | Generic name of the file describing the procedures to be executed on completion of reception of a file. |
| <a href="../../../command_summary/parameter_intro/execrm">EXECRM</a> | Generic name of the file describing the procedures to be executed on completion of reception of a message. |
| <a href="../../../command_summary/parameter_intro/execse">EXECSE</a> | Generic name of the file describing the procedures to be executed, following an incident (Error) during a send transfer, the transfer changing to the H or K state. |
| <a href="../../../command_summary/parameter_intro/execsf">EXECSF</a> | Generic name of the file describing the procedures to be executed on completion of the sending of a file. |
| <a href="../../../command_summary/parameter_intro/execsfa">EXECSFA</a>  | Generic name of the file describing the procedures to be executed on receiving an acknowledgement (REPLY type message), following the sending of a file. |
| <a href="../../../command_summary/parameter_intro/execsm">EXECSM</a>  | Generic name of the file describing the procedures to be executed on completion of the sending of a message. |
| <a href="../../../command_summary/parameter_intro/execsma">EXECSMA</a>  | Generic name of the file describing the procedures to be executed on receiving an acknowledgement, REPLY type message, following the sending of a message. |
| <a href="">EXITBOT</a>  | EXIT identifier. To activate a beginning-of-transfer EXIT task, this identifier must point to a CFTEXIT command. |
| <a href="../../../command_summary/parameter_intro/exiteot">EXITEOT</a> | EXIT identifier. To activate an end-of-transfer EXIT task, this identifier must point to a CFTEXIT command. |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | Identifier of the CFTPARM command. |
| <a href="../../../command_summary/parameter_intro/key">KEY</a>  | The name of the indirection file preceded by the &lt;file-symb&gt; character, which is system specific, and containing the set of keys associated with the Transfer CFT. |
| <a href="../../../command_summary/parameter_intro/lenappl">LENAPPL</a> | Length to be taken into account when comparing the file/message identifier, IDF or IDM, with the identifier of a CFTAPPL command.<br/> See, Security concepts: Start here. |
| <a href="../../../command_summary/parameter_intro/log">LOG</a> | Identifier of the monitor event log file description command CFTLOG.<br/> If this parameter is not defined, the Transfer CFT writes logging messages to the standard output of the monitor. |
| <a href="../../../command_summary/parameter_intro/maxtask">MAXTASK</a>  | Number of file access tasks authorized. |
| <a href="../../../command_summary/parameter_intro/maxtrans">MAXTRANS</a> | The maximum authorized number of transfers in parallel. When using multi node, this is the number of transfers per node. |
| <a href="../../../command_summary/parameter_intro/net">NET</a> | List of the identifiers of the description commands for network access methods and monitor network resources CFTNET. |
| <a href="../../../command_summary/parameter_intro/npart">NPART</a>  | Default network identifier of the local site.<br/> Default value of the NSPART parameter of the CFTPART command.<br/> As this name is sent by some file transfer protocols, refer to the Transfer CFT <a href="../../../../protocols_start_here">Managing</a> Protocols to check its size and format. |
| <a href="../../../command_summary/parameter_intro/part">PART</a> | Local identifier, identifying the site on which Transfer CFT is executed.<br/> This parameter is an item of information appearing in the transfer catalog. |
| <a href="../../../command_summary/parameter_intro/partfnam">PARTFNAM</a>  | Partner file name (obsolete for Windows/Unix). |
| <a href="../../../command_summary/parameter_intro/protocol">PROT</a>  | Identifier of the Transfer CFT protocol description commands CFTPROT.<br/> Transfer CFT protocol refers to both file transfer application protocols and network access methods. |
| <a href="../../../command_summary/parameter_intro/rcvaller">RCVALLER</a>  | Option to be used if an error occurs when receiving available files FILE=ALL option. |
| <a href="../../../command_summary/parameter_intro/secfname">SECFNAME</a> | Name of the CFT internal security file. |
| <a href="../../../command_summary/parameter_intro/trantask">TRANTASK</a> | Number of transfers in parallel performed by a file access task, above which a new task is created, if possible. |
| <a href="../../../command_summary/parameter_intro/trkpart">TRKPART</a> | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/trkrecv">TRKRECV</a> | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/trksend">TRKSEND</a> | Specification of how much detail Transfer CFT provides Sentinel about transfers. Transfer CFT sends detail about the transfers in the form of tracked instances. |
| <a href="../../../command_summary/parameter_intro/userctrl">USERCTRL</a> | Transferred file access control option. |
| <a href="../../../command_summary/parameter_intro/waitresp">WAITRESP</a> <a href="../../../command_summary/parameter_intro/waitresp">see table</a> | Timeout in seconds used during internal communication between monitor tasks.<br/> This parameter is used during a synchronous exchange of requests between two monitor tasks. After waitresp seconds without reply, the timeout is interrupted. A message CFTS09 is written in the log. The task in question is then stopped, CFTTCOM task, for example.<br/> During the initialization phase, this parameter checks the time allowed for each of the Transfer CFT monitor tasks to start. In the event of an insufficient value (case of a highly loaded computer), the Transfer CFT monitor initialization stops.<br/> The following table indicates the default value for each system. |
| <a href="../../../command_summary/parameter_intro/waittask">WAITTASK</a>  | Time during which a file access task is inactive in minutes before being shut down. |


<span id="Changing_the_initial_IDPARM_in_CFTUTIL"></span>

### Changing the initial IDPARM in CFTUTIL

The format that you use to enter commands in CFTUTIL is operating system
dependent. For more information, refer to the *Installation and Operations Gudie*that corresponds
with your operating system.

<span id="CFTPARM_command_line_examples"></span>

## CFTPARM command line examples

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
