{
    "title": "Production related problems",
    "linkTitle": "Production related problems",
    "weight": "240"
}Problems can occur that are not specifically related to a transfer or group of transfers, such as:

- Cannot start Transfer CFT

<!-- -->

- The dynamic processes cannot be initialized

<!-- -->

- Monitor activities appear to be blocked

<!-- -->

- Some functions are not performed (procedures, and so on)

<!-- -->

- Communications cannot be established with Transfer CFT

<!-- -->

- Certain processes are missing

<!-- -->

- Network errors recur

Most malfunctions are caused by a {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration error, incorrectly defined quotas or privileges for the {{< TransferCFT/axwayvariablesComponentShortName  >}} account, inappropriately sized system parameters (such as GBLPAGES and GBLSECTIONS), and so on.

You can obtain information on the type of problem from the {{< TransferCFT/axwayvariablesComponentShortName  >}} log file.

If the problem is related to the use of an internal system service, you can find more information by querying the standard output files for the {{< TransferCFT/axwayvariablesComponentShortName  >}} tasks.

For the main CFTMAIN JOB process, consult either the batch report (CFT_LOG:CFTMAIN.LOG) or the files designated as output and error files if Transfer CFT is created as a detached process.

For {{< TransferCFT/axwayvariablesComponentShortName  >}} sub-processes, consult the .OUT and .ERR files in the {{< TransferCFT/axwayvariablesComponentShortName  >}} account login directory (SYS$LOGIN).

System call error messages
--------------------------

The specific part of {{< TransferCFT/axwayvariablesComponentShortName  >}} uses the OpenVMS system services to perform the operations required by {{< TransferCFT/axwayvariablesComponentShortName  >}}. If a call fails, an error message is sent to the standard output file of the process.

This message comprises:

- The date and time at which the error occurred

<!-- -->

- The name of the internal service concerned

<!-- -->

- A specific section of text describing the error

The message text may include the following symbolic variables:

- &cs: code returned by the system service

<!-- -->

- &chan: OpenVMS channel number

<!-- -->

- &cr: monitor-specific return code

<!-- -->

- &str: character string used by the service

<!-- -->

- &evt: character string representing the network event

<!-- -->

- &ref: reference to an internal monitor context

<!-- -->

- &val: decimal value

<!-- -->

- &net: type of network used

Values between square brackets ([...]) are expressed in hexadecimal, others in decimal.

For all these messages, when the message label refers to the &cs symbolic variable, it is a code returned by the system service described in the message. To determine the meaning of this code and the action to be taken, refer to the guide OpenVMS System Messages and Recovery Procedure.

In all other cases, contact the Technical Support Team and provide all possible information to help troubleshoot the problem.

### System messages common to all processes

These messages correspond to service calls common to all {{< TransferCFT/axwayvariablesComponentShortName  >}} processes. They can be found in the standard output files of all processes in {{< TransferCFT/axwayvariablesComponentShortName  >}}.

These messages correspond to common services specific to the {{< TransferCFT/axwayvariablesComponentShortName  >}}, but which call OpenVMS system services for execution purposes.

### CFTTFIL tasks system messages

The following messages correspond to calls to services specific to transferred files input/output. They can only be found in the standard output files of the various CFTTFIL tasks.

### Network server tasks system messages

The symbolic variables used:

- &NET: indicates that the message can be generated for all types of network

<!-- -->

- &FCT: function processed when the error occurred (generally a network request)

<!-- -->

- &msg: internal interchange block exchanged between the server task and CFTTPRO task

<!-- -->

- &service: label for a system service used by the task
