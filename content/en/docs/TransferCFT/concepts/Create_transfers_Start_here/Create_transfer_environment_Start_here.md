{
    "title": "Creating  the transfer environment",
    "linkTitle": "Creating the transfer environment",
    "weight": "200"
}Model file parameters
---------------------

CFTSEND objects include parameters controlling the access to the data
to be sent and the send transfer process.

A CFTSEND object, *default command excluded,* controls the sending
of the model file with the same identifier, where the IDF equals the ID
of the CFTSEND object.

This command is taken into account in one or other of the following
send transfer cases:

- Send transfer following
    an explicit request SEND IDF = ...
- Send transfer using
    the implicit SEND mechanism; this mechanism automatically handles the
    receive requests sent without warning by a remote partner (server mode
    exclusively)

To cover both these cases for a given model file (IDF), two CFTSEND
commands with the same ID are required:

- One containing
    the parameter IMPL = NO
- The other containing
    IMPL = YES

> **Note**
>
> Note: The default CFTSEND object for its part applies to all the other
> model files in the case of an explicit SEND: it is taken into account
> whenever the SEND IDF does not correspond to a CFTSEND command containing
> IMPL = NO.

For clarity, it is recommended to group all CFTSEND commands in a parameter
setting source file, placing the default CFTSEND object in the first or
last position.

The CFTSEND command is used to specify, for each IDF model file:

- The name and the
    local physical characteristics of the file containing the data to be sent.
    At the start of a send transfer, these parameters are used by Transfer
    CFT:
- To control
    access to the data to be sent  
      
    Only the characteristics of the file which do not vary from one transfer
    to another and which Transfer CFT cannot locate automatically, are generally
    specified. For example, when the same local physical filename is always
    associated with the IDF model file, it is logical to specify this name
    by indicating an FNAME parameter in the CFTSEND command

<!-- -->

- As default
    values for the file network characteristics, if such values are not explicitly
    specified (see below)
- The file network
    characteristics: values to be sent to the partner, in protocol parameters,
    to describe the file (a physical filename can even be sent - see the definition
    of the ****Open**** mode further on).  
      
    The physical characteristics that Transfer CFT is able to locate automatically
    for the local file, can be considered as default values for the corresponding
    CFT SEND parameters (example: local record length: FLRECL), parameters
    which themselves consist of default values for the network characteristics
    (example: record length sent to the partner: NLRECL).  
      
    Also, when the transfer is initiated by a local SEND command explicitly
    including one or more parameters which are also included in the CFTSEND
    command, according to the value of the FORCE parameter:

<!-- -->

- If FORCE =
    NO, the parameters of the SEND command take precedence over the parameters
    of the CFTSEND object for all the parameters common to both commands,
    CFTSEND only supplies the default values to SEND
- If FORCE =
    YES, the parameters of the CFTSEND object take precedence over the parameters
    of the SEND command. For details on what parameters take precedence refer
    to the [FORCE](../../../c_intro_userinterfaces/command_summary/parameter_intro/force) parameter
    in the command index
- The actions to
    be performed locally:
- Call when transferring
    to a user-written file EXIT task
- Action to be
    taken on the data handled by the monitor during the transfer: translation,
    compression
- Action performed
    by the {{< TransferCFT/axwayvariablesComponentShortName  >}} on the file sent, after the transfer
- Call to a procedure
    to be executed on completion of the transfer, etc
- Miscellaneous parameters
    controlling the execution of transfers, including:
- Authorized
    time slot
- Default user
    id associated with transfers

<span id="Transfer_environment_parameters"></span>

Transfer environment parameters
-------------------------------

This section lists the parameters involved when creating the file transfer
environment. The transfer environment parameters can be divided into the
following categories:

- Protection of parameters
    with values: FORCE
- Identification
    parameters:
    -   General: ID,
        USERID, GROUPID
    -   PeSIT protocol
        related (SIT profile, PeSIT D CFT profile or PeSIT E): RAPPL, SAPPL
    -   PeSIT protocol
        related (PeSIT D CFT profile or PeSIT E): RUSER, SUSER

<!-- -->

- Free parameters
    for the {{< TransferCFT/axwayvariablesComponentShortName  >}} user:
    -   Sent to the
        receiver: PARM, SAPPL
    -   For local use:
        COMMENT, OPERMSG, DELETE, NOTIFY

<!-- -->

- Execution control
    parameters:
    -   General: IMPL,
        STATE, PRI
    -   User: EXEC,
        EXIT
    -   Schedule management:
        MINDATE, ...TCYCLE

<!-- -->

- Data processing
    parameters: NCOMP, XLATE
- Parameters associated
    with the file sent:
    -   File management:
        FACTION
    -   Physical name:
        FNAME
    -   Physical characteristics
        (whole file): FSPACE, FORG, FTYPE, FCODE, FDISP
    -   Physical characteristics
        (records): FRECFM, FLRECL, FBLKSIZE

<!-- -->

- File parameters
    for the partner, according to the protocol:
    -   Physical name:
        NFNAME
    -   Physical characteristics
        (whole file): NSPACE, NTYPE, NCODE
    -   Physical characteristics
        (records): NRECFM, NLRECL, NBLKSIZE, NKEYLEN, NKEYPOS

For the partner file parameters, Nxxxxx of SEND and CFTSEND:

- The parameters
    are optional.
- They are used to
    set the values of the information sent - file name and/or physical characteristics - when provided in the message units (FPDUs) of the protocol used (or
    of the protocol profile used).
- This information
    is used by the receiver monitor according to its specific possibilities:
    -   Pending the version and OS, see the description of these possibilities
        at the level of the CFTRECV command and in the relevant Operations Guide.
- In all cases:
    the values sent must be valid for the receiver partner.
