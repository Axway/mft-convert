{
    "title": "System environment",
    "linkTitle": "System specific functions",
    "weight": "210"
}This section describes the {{< TransferCFT/axwayvariablesComponentShortName  >}} architecture, the resources used, and the methods for controlling the {{< TransferCFT/axwayvariablesComponentShortName  >}} behavior in an OpenVMS environment.

{{< TransferCFT/axwayvariablesComponentShortName  >}} processes
--------------------------------------------------------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} is made up of a JOB that contains several processes. When a job is activated, a CFTMAIN process is created, either in batch mode or as a detached process, and creates sub-processes itself. All these processes are executed with the same UIC as the account in which {{< TransferCFT/axwayvariablesComponentShortName  >}} was installed and share two global memory sections.

{{< TransferCFT/axwayvariablesComponentShortName  >}} contains several processes, each of which has standard output files corresponding to SYS$OUTPUT and SYS$ERROR. These files are created in SYS$LOGIN and are named:

`<node>_<CFT_process>_XX.OUT `

And

`<node>_<CFT_process>_XX.ERR`

Where:

- &lt;node&gt; is the node on which {{< TransferCFT/axwayvariablesComponentShortName  >}} is executed

<!-- -->

- CFT_process is the {{< TransferCFT/axwayvariablesComponentShortName  >}} process name

<!-- -->

- XX is the group UIC number displayed in hexadecimal

These files can only be accessed in read mode after you have shut down {{< TransferCFT/axwayvariablesComponentShortName  >}}. {{< TransferCFT/axwayvariablesComponentShortName  >}} creates a new version each time it is started. These files are very important for troubleshooting in the event of a problem. Keep them following an incident, so that you can provide this information to the Technical Support Team.

During normal operations, these files may accumulate each time {{< TransferCFT/axwayvariablesComponentShortName  >}} is restarted. The administrator must delete these periodically, so that the disk is not unnecessarily saturated.

### Inter-process communication

{{< TransferCFT/axwayvariablesComponentShortName  >}} processes inter-communicate using the shared memory and two global sections, which can be seen by all users in the same group. Each process using {{< TransferCFT/axwayvariablesComponentShortName  >}}-specific inter-process communication functions is automatically associated with these two global sections which are:

- CFT_XX

<!-- -->

- CFT3_XX

Where XX represents the USER group number in hexadecimal.

In the system, the corresponding parameters are as follows:

- GBLPGFIL represents the number of global pages that can be created on the system

<!-- -->

- GBLSECTIONS represents the number of sections that can be created on the system

{{< TransferCFT/axwayvariablesComponentShortName  >}} creates approximately 2,000 global pages and two global sections.

Even if they do not use the same communication, CFTUTIL and the APIs are attached to the global sections when initialized.

{{< TransferCFT/axwayvariablesComponentShortName  >}} global sections
--------------------------------------------------------------------------

Transfer CFT uses shared memory buffers called CFT3_XX, created in the second global section, for internal communication requirements.

Transfer CFT has a default value of thirty 32 KB buffers reserved for its start sequence. Depending on the requirements, these buffers are broken down into smaller blocks. By default, Transfer CFT reserves 1920 global pages for Transfer CFT requirements, which may or may not be adequate, depending on the average Transfer CFT activity.

You can modify the default value of 30 buffers using CFT$MG_NB with an appropriate value between 30 and 60. Modifying the global pages on the system allows you to adapt Transfer CFT to your production constraints.
