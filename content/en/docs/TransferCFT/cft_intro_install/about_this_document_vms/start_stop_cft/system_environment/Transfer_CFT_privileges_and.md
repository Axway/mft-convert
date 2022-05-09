{
    "title": "Transfer CFT privileges and identifiers",
    "linkTitle": "Transfer CFT privileges and identifiers",
    "weight": "310"
}Specific privileges must be granted to the {{< TransferCFT/axwayvariablesComponentShortName  >}} account, depending on the options selected during installation. Some privileges must also be assigned to users wishing to deposit requests in the communication medium.

The following table describes the {{< TransferCFT/axwayvariablesComponentShortName  >}} user types and privileges.


| Name  | Type  | Meaning  |
| --- | --- | --- |
| OPER | Optional | Privilege for Transfer CFT exclusively. Used to implement the CFTLOG command NOTIFY parameter. |
| SYSNAM | Optional | Privilege for Transfer CFT exclusively. To be assigned if you are using the DECnet network. |
| BYPASS | Optional | Privilege for Transfer CFT exclusively. Used to implement the mechanism controlling user accesses to the files to be transferred. |
| CMKRNL | Optional | Privilege for Transfer CFT exclusively. Used by the monitor to submit the procedures from the transfer owner account. |
| SYSLCK | Optional | Privilege for Transfer CFT exclusively. Used to control concurrent accesses at system level when users from a group other than the Transfer CFT group wish to deposit requests in the communication file. |
| SHARE | Optional | Privilege for Transfer CFT exclusively. To be assigned if you are using Copilot. |
| PSI$DECLNAME | Optional | Identifier for the Transfer CFT exclusively. If this identifier is declared in SYSUAF, it must be assigned to the Transfer CFT account, so that the account can be registered on the TCP-IP network to receive incoming calls. |
| NET$EXAMINE | Mandatory | Identifier for the monitor. This identifier is used to obtain the Ethernet controller's hardware address, which is used when calculating the key.<br /> It can also be assigned to obtain this information via the ABOUT command. |


{{< TransferCFT/axwayvariablesComponentShortName  >}} quotas
-----------------------------------------------------------------

During installation, the {{< TransferCFT/axwayvariablesComponentShortName  >}} account is allocated quotas, which are mandatory for its execution. These quotas are based on the replies that you give in the VMSINSTAL procedure.

Some of these quotas are assigned to each process in the JOB, and all processes in the JOB share others. For quotas assigned to each process, the account must be allocated the quotas required for the processes making the most intensive use of system resources. For the other quotas, you must estimate the resources used for all processes in the JOB.

According to your operating environment, you are advised to track monitor activities, so that you can fine-tune the quotas, as required.

Quotas Used by {{< TransferCFT/axwayvariablesComponentShortName  >}}.


| Quota  | Level  | Use  |
| --- | --- | --- |
| ASTLM | Process  | The ASTs are used for inter-process communications and network monitoring. The processes managing the network ( CFTTCPS) make the most intensive use of ASTs.<br /> For each process, there is one AST for inter-process communications, one AST per protocol using this type of network (TCP/IP) multiplied by the number of resources (CFTNET) in the same class, and two ASTs for each session in progress. |
| BIOLM | Process  | Buffered I/Os are used for the file input/output operations and exchanges over the network.<br /> Each CFTTFIL process only performs one input/output at a given time.<br /> The processes managing the networks make the most intensive use of buffered I/Os.<br /> For each network process, there must be at least two buffered I/Os per active session. |
| BYTLM | JOB  | This resource is associated with the buffered I/O.<br /> The use made of this resource is determined by the size of the disk write buffers and data exchanged over the network (CFTPROT command SRUSIZE and RRUSIZE parameters).<br /> It also depends on the number of active sessions.<br /> For the network buffers, you must include the protocol-level overhead, depending on the protocol used (session encapsulation).<br />  |
| DIOLM | Process  | According to the VMS release, the file and network I/O system services are performed in direct or buffered I/O mode.<br /> Preference is given to buffered I/Os in the most recent releases of OpenVMS.<br /> This resource is therefore used less and less intensively. |
| ENQLM  | JOB  | Number of requests for locks managed by the DISTRIBUTED LOCK MANAGER at the same time.<br /> In particular, they are implemented by the RMS services.<br /> Transfer CFT uses two ENQs for each active process and two ENQs for each transfer in progress, in addition to one ENQ for the communication file access control mechanism.  |
| JTQUOTA | JOB | Memory quota of the logical name table allocated for the JOB.<br /> However, this resource is used by the various logical names defined at JOB level. |
| PGFLQUOTA | JOB | Maximum number of pages that the JOB can use in the system page file.<br /> This quota determines the amount of dynamic memory that can be allocated by the JOB.<br />  |
| PRCLM | Process | Maximum number of sub-processes that can be created by a process.<br /> According to your configuration, specify the CFTTCOM, CFTLOG, and CFTTPRO processes and the various CFTTFIL processes for CFTMAIN, and a sub-process per type of network used TCP/IP for the CFTTPRO process. |
| TQELM | JOB | The timeouts are used for inter-process communications. Transfer CFT uses one for each active process. |


### Calculating the {{< TransferCFT/axwayvariablesComponentShortName  >}} quotas

Depending on how you want to use Transfer CFT, the quotas required can vary.

Use these formulas to determine suitable values for the VMS-specific quotas to configure the Transfer CFT profile:

- prclm = 3 + maxtask + 8 (default = 19)

<!-- -->

- tqelm = 8 + maxtask (default = 16)

<!-- -->

- pgflquo = 180,000 + (10,000 \* maxtask) (default = 500,000)

<!-- -->

- jtquo = 7 168
