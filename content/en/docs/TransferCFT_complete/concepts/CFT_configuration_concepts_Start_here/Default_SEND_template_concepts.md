{
    "title": "Creating  a SEND template",
    "linkTitle": "Default send templates CFTSEND",
    "weight": "170"
}This topic describes the
CFTSEND template object, used to specify the default values for:

- The name and local
    physical characteristics of the file to send
- The network characteristics
    of the file to send to the partner
- The actions to
    perform locally during and after a transfer (translation, compression,
    call to a user EXIT, an end-of-transfer procedure...)
- An authorized time
    slot and default user associated with the transfers

There is no limit to the number of CFTSEND objects that you can create
in the Ongoing CFTSEND object. In this topic, you create a template
CFTSEND object. The identifier must correspond to the file identifier
(idf) or, if this parameter is not defined, the default parameter of the
CFTPARM object.

Certain default template objects cannot be deleted. The template CFTSEND and CFTRECV objects can not be deleted if they are used by another configuration.

****Related
topics****

- Command syntax
    [CFTSEND](../../../c_intro_userinterfaces/command_summary#CFTSEND)
- Parameter list
    [CFTSEND - Default
    SEND](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftsend)

<span id="CFTSEND_parameter_details"></span>

CFTSEND parameter details
-------------------------

<span id="Parameters_Associated_with_a_Model_File"></span>

### Parameters Associated with a Model File

CFTSEND commands include parameters controlling the access to the data
to be sent and the send transfer process.

A CFTSEND command (*default command
excluded*) controls the sending of the model file with the same
identifier (IDF equal to the ID of the CFTSEND command).

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

The *default* CFTSEND command for its part applies to all the other
model files in the case of an explicit SEND: it is taken into account
whenever the SEND IDF does not correspond to a CFTSEND command containing
IMPL = NO.

For clarity, it is recommended to group all CFTSEND commands in a parameter
setting source file, placing the "default" CFTSEND command in
the first or last position.

The CFTSEND command is used to specify, for each model file (IDF):

- The name and the
    local physical characteristics of the file containing the data to be sent  
    At the start of a send transfer, these parameters are used by Transfer
    CFT:
- To control
    access to the data to be sent  
    In this case, only the characteristics of the file which do not vary
    from one transfer to another and which {{< TransferCFT/axwayvariablesComponentShortName  >}} cannot locate automatically,
    are generally specified.  
    For example, when the same local physical filename is always associated
    with the model file (IDF), it is logical to specify this name by indicating
    an FNAME parameter in the CFTSEND command

<!-- -->

- As default
    values for the file "network" characteristics, if such values
    are not explicitly specified (see below)
- The file network
    characteristics: values to be sent to the partner, in protocol parameters,
    to describe the file (a physical filename can even be sent - see open mode).  
      
    The physical characteristics that {{< TransferCFT/axwayvariablesComponentShortName  >}} is able to locate automatically
    for the local file, can be considered as default values for the corresponding
    CFT SEND parameters (example: local record length: FLRECL), parameters
    which themselves consist of default values for the network characteristics
    (example: record length sent to the partner: NLRECL).  
      
    When the transfer is initiated by a local SEND command explicitly including
    one or more parameters which are also included in the CFTSEND command,
    according to the value of the FORCE parameter:

<!-- -->

- If FORCE =
    NO, the parameters of the SEND command take precedence over the parameters
    of the CFTSEND command (for all the parameters common to both commands,
    CFTSEND only supplies the default values to SEND)
- If FORCE =
    YES, the parameters of the CFTSEND command take precedence over the parameters
    of the SEND command
- The actions to
    be performed locally:
    -   Call when transferring
        to a user-written "file EXIT" task
    -   Action to be
        taken on the data handled by the monitor during the transfer: translation,
        compression
    -   Action performed
        by the monitor on the file sent, after the transfer
    -   Call to a procedure
        to be executed on completion of the transfer, etc
- Miscellaneous parameters
    controlling the execution of transfers, including:
    -   Authorized
        time slot
    -   Default user
        id associated with transfers

<span id="About_default_SEND_parameters"></span>

About default SEND parameters
-----------------------------

This section  explains
certain categories of the SEND parameters. The parameters can be classified
as follows:

Protection of parameters
with values: FORCE

Identification
parameters:

- General: ID,
    USERID, GROUPID
- PeSIT protocol
    related (SIT profile, PeSIT D CFT profile or PeSIT E): RAPPL, SAPPL
- PeSIT protocol
    related (PeSIT D CFT profile or PeSIT E): RUSER, SUSER

Free parameters
for the {{< TransferCFT/axwayvariablesComponentShortName  >}} user:

- Sent to the
    receiver: PARM, SPART
- For local use:
    COMMENT, OPERMSG, DELETE, NOTIFY

Execution control
parameters:

- General: IMPL,
    STATE, PRI
- User: EXEC,
    EXIT
- Schedule management:
    MINDATE, ...TCYCLE

Data processing
parameters: NCOMP, XLATE

Parameters associated
with the file sent:

- File management:
    FACTION
- Physical name:
    FNAME
- Physical characteristics
    (whole file): FSPACE, FORG, FTYPE, FCODE, FDISP
- Physical characteristics
    (records): FRECFM, FLRECL, FBLKSIZE

File parameters
for the partner, according to the protocol:

- Physical name:
    NFNAME
- Physical characteristics
    (whole file): NSPACE, NTYPE, NCODE
- Physical characteristics
    (records): NRECFM, NLRECL, NBLKSIZE, NKEYLEN, NKEYPOS

Concerning the file parameters for the partner (Nxxxxx of SEND and CFTSEND):

- The parameters
    are optional
- They are used to
    set the values of the information sent - file name and/or physical characteristics - when provided in the message units (FPDUs) of the protocol used (or
    of the protocol profile used)
- This information
    is used by the receiver monitor and the values sent must be valid for
    the receiver partner

Sending a file  
----------------

The example below illustrates the relationships to be established between the user SEND command
and the CFTSEND and CFTPART parameter setting commands.

****Correspondence
between the SEND command (file) and {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter setting****

If there is no CFTSEND command with an
identifier ID = FI, the default characteristics indicated in the CFTSEND
ID = &lt;*default*&gt; command, are used to supplement the ones indicated
in the SEND command, as required.

![](/Images/TransferCFT/send_a_file.GIF)
