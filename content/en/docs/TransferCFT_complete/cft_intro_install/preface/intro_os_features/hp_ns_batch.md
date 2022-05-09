{
    "title": "Transfer CFT batch procedures",
    "linkTitle": "Batch procedures",
    "weight": "220"
}Transfer CFT supports batch procedure from both native and OSS environments. This section describes the rules that determine the type of batch procedure and how it is executed.

How it works
------------

In the absence of TACL indicators in the first line of the batch procedure, the procedure is executed as a script in the OSS environment. If TACL indicators are present, certain characters and rules determines what happens next in the native environment (either Direct TACL processing or NetBatch processing).

The **Decision rules** section explains which characters indicate that it is a TACL procedure, and the exact type of processing that is used.

A batch procedure can contain a reference to Transfer CFT variables. The character used to indicate a variable is:

- &
    -   OSS script
    -   Example: &FNAME
- ^
    -   The same character as in the previous Tandem version
    -   TACL procedure
    -   Example: ^FNAME

Refer to *Transfer CFT User Guide* for a complete list of Transfer CFT variables.

![](/Images/TransferCFT/temp_batch_processing.png)

### Decision rules

If the first line of the skeleton procedure begins with one of the following characters, it is a TACL procedure.

- ==
- ?
- \#FRAME
- \#PUSH

The characters following the initial " == " either set certain information for the TACL procedure or determine if the procedure is sent to the NetBatch interface for processing.

- == CFT^BT^FORCE^TACL ==
    -   A direct TACL procedure execution
    -   This execution type performed by default
    -   This parameter is kept to ensure compatibility with existing batch procedures
- <span id="CFT^BT^FORCE^ZBAT"></span>== CFT^BT^FORCE^ZBAT ==
    -   Use NetBatch Interface
    -   To specify a given environment, you can declare it in the first line of the actual procedure. Add the following optional values in the first line of the skeleton procedure, in the order listed. If no values are declared, the [UCONF](#UCONF) default values are used.
        -   NetBatch process
        -   Job name
        -   Attachment-set
    -   For each field add a delimiter such as a colon (:), comma (,), or equal sign (=) followed by the parameter's value

**Example**

` == CFT^BT^FORCE^ZBAT : $ZBA1 , JOBNAME , SETNAME ==`

- NetBatch Process = $ZBA1
- Job name = JOBNAME
- Attachment Set = SETNAME

### Processing

1. Regardless of if the procedure is OSS or native, {{< TransferCFT/axwayvariablesComponentLongName  >}} creates a temporary file with the following locations and naming conventions:

- OSS: The same as on {{< TransferCFT/axwayvariablesComponentLongName  >}} Unix: /tmp/CFTxxxx  
- Native: On the {{< TransferCFT/axwayvariablesComponentLongName  >}} default [subvolume](#subvolumeUD): CTMPnnnn

`2> filenames $DATA14.CFT32BUD.*`

`           $DATA14.CFT32BUD`

`CTMP0001  CTMP0002  CTMP0003  CTMP0004`

{{< TransferCFT/axwayvariablesComponentLongName  >}} copies the skeleton in the temporary file, replacing variables with their real values (transfer information, file names, etc.).

Depending on the type of procedure, {{< TransferCFT/axwayvariablesComponentLongName  >}}:

- Starts the script (OSS)
- Starts the TACL direct processing
- Puts the temporary file in NetBatch for execution

### Delete temporary files

The started procedure MUST delete the temporary files, regardless of the environment.

- OSS
    -   rm $0
    -   rm $0.err
- NATIVE
    -   \#PURGE [\#IN]
    -   The same BTPURGE procedure as in the previous version is delivered and can be executed:

<span id="UCONF"></span>

### UCONF parameters

The following unified configuration parameters are specific to HP Nonstop.

Parameter

Default

Description

<span id="cft.guardian.cftwrk"></span>cft.guardian.cftwrk

The default working directory for the TACL and NETBATCH scripts.

The parameter is set with the default value of “`<``subvolume``>UD`”
(see [Guardian files](#Guardian)) during the Guardian
files installation.

<span id="cft.guardian.process_name_prefix"></span>cft.guardian.process_name_prefix

LA

The first two letters of the Guardian process names.

Each Transfer CFT process is assigned a name using this prefix and a
suffix, which depends on the executable name.

For instance, using the default setting, CFTLOG is run with the
name $LALOG with the Guardian convention (or /G/LALOG with the OSS
convention).

If empty, no Guardian process name is given.

If you plan to run several instances of Transfer CFT at the same time on
the same machine, you should assign each instance a unique value.

<span id="cft.guardian.processor"></span>cft.guardian.processor

-1

Processor on which Transfer CFT is started.

-  
    -1 indicates that Transfer CFT is started on the processor
    from which the start-up command is executed
- Processor number

<span id="cft.guardian.backup_processor"></span>cft.guardian.backup_processor

-1

Backup processor on which Transfer CFT is started.

- -1 indicates that no processor number is assigned
- Backup processor number

<span id="cft.guardian.priority"></span>cft.guardian.priority

-1

Guardian execution priority of the CFT processes.

- -1 means that CFT is started with the
    execution priority of the parent process.
- Process priority

<span id="cft.guardian.hometerm"></span>cft.guardian.hometerm

The Guardian home terminal for Transfer CFT processes.

- An empty value means that CFT is started with
    the OSS shell’s home terminal
- The value should be either set using the
    Guardian form ($ZTNT.\#PTY4, $TTY), or the OSS form
    (/G/ZTNT/\#PTY, /G/TTY)

cft.guardian.tcpip_resolver_name

The TCPIP resolver name for Transfer CFT processes. Equivalent to the Guardian
DEFINE TCPIP^RESOLVER^NAME.

If set, the value should be a Unix pathname pointing to an existing resolver file.

cft.guardian.tacl.processor

-1

Processor on which TACL is started.

- -1 means that TACL is started on same
    processor where Transfer CFT is running
- Processor number

cft.guardian.tacl.backup_processor

-1

Backup processor on which TACL is started. Valid only if
different from -1 and from the cft.guardian.tacl.value.

cft.guardian.tacl.priority

90

Priority of a started TACL.

cft.guardian.tacl.output

'$S.\#ABTT'

Output destination of the started TACL.

cft.guardian.tacl.home_terminal

'$ZHOME'

TACL home terminal.

cft.guardian.netbatch.process

'$ZBAT'

NetBatch process with which you send a request.

You can override this value in the [first line of a TACL procedure](#CFT%5EBT%5EFORCE%5EZBAT).

cft.guardian.netbatch.jobname_prefix

'ZBBT'

A jobname prefix used to build a jobname having 8 characters, and
comprised of:

- This prefix
- A suffix that is composed of the last
    characters of the temporary file name

You can override this value in the [first line of a TACL procedure](#CFT%5EBT%5EFORCE%5EZBAT).

<span id="cft.guardian.netbatch.attachment_set"></span>cft.guardian.netbatch.attachment_set

'NBASCFTLI'

NetBatch attachment-set name.

You can override this value in the [first line of a TACL procedure](#CFT%5EBT%5EFORCE%5EZBAT).

cft.guardian.netbatch.priority

90

Priority of TACL run started.

cft.guardian.netbatch.selpri

4

JOB selection priority.

cft.guardian.netbatch.output

'$S.\#ABTZ'

Output destination of the started TACL.
