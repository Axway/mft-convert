{
    "title": "System configuration and output ",
    "linkTitle": "System configuration and output",
    "weight": "210"
}About the output file
---------------------

Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} HP NonStop reassembles all of the Transfer CFT process output in a main file, `runtime/run/cft.out`, whereas version 2.3.2 used either spooler output (one per process) or event messages (EMS).

About the system configuration file
-----------------------------------

The system configurations file (CFGSYS) found in the previous version no longer exists in this version. The information that was in this file is now either obsolete (for example, processes output) or is managed by:

- Runtime/profile (sets environment variables)
- Transfer CFT uconf file (contains, for example, all parameters related to end of transfer procedure or server executions)
