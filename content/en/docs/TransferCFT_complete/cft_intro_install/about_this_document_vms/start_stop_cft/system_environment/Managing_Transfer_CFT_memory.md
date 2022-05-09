{
    "title": "Manage Transfer CFT memory",
    "linkTitle": "Manage Transfer CFT memory",
    "weight": "230"
}During transfers, the data in a file from the network is buffered by the CFTTPRO task while waiting for the CFTTFIL task to write the corresponding records to the disk.

At the same time, {{< TransferCFT/axwayvariablesComponentShortName  >}} accepts all the data from the network without any flow control. PeSIT and ODETTE can implement this control at protocol level.

In some operating conditions, the disks may be accessed intensively. Applications requiring frequent access to the disk are particularly affected when the BACKUP utility is used.

By default CFTTFIL uses the RMS parameters that are defined at the system level.

- Logical names CFT$MBCNT and CFT$MBFCNT enable you to modify the default values.
- CFT$MBCNT: Specifies a multi-block count (0 to 127) only for I/O record operations, where count is the number of blocks allocated for each I/O buffer
- CFT$MBFCNT: Specifies a default multi-buffer count (0 to 255) for local file operations, where count is the number of buffers to be allocated

Data from the network is buffered in dynamic memory, which is allocated by the CFTTPRO task as necessary. The quantity of memory that can be allocated by a JOB is determined by the PGFLQUOTA parameter associated with the user. It represents the total memory space that can be reserved by this JOB, including the images and stack.

If an excessive amount of data is buffered by the CFTTPRO task following a file access bottleneck, the task lacks sufficient memory and the system can no longer satisfy memory allocation requests. When this situation occurs, the sub-processes are shut down and the monitor is blocked.

You must allocate sufficient memory to {{< TransferCFT/axwayvariablesComponentShortName  >}} to prevent this problem from occurring. The maximum amount of memory likely to be used for the JOB, in the extreme case of disk access saturation, is calculated as Sum of the size of all JOB images plus approximately 10%.

However, according to your configuration and the networks involved, some images are used and some are not (in particular, the CFTLOG and CFTTCOM tasks, the various network server tasks and the maximum number of CFTTFIL tasks).

You must also include the data from the network for each receive file. The maximum amount of buffered data per transfer depends on the type of protocol:

- ODETTE: size of the network exchanges (CFTPROT RRUSIZE parameter) multiplied by the credit (CFTPROT RCREDIT parameter)

<!-- -->

- PeSIT: synchronization interval (CFTPROT RPACING parameter) multiplied by the synchronization window (CFTPROT RCHKW parameter)

If there is a low risk of the disk accesses being saturated on your system or the amount of memory that can be allocated to {{< TransferCFT/axwayvariablesComponentShortName  >}} is limited, you can specify a much lower value than the one provided above.

{{< TransferCFT/axwayvariablesComponentShortName  >}} is equipped with an internal configurable mechanism, which prevents it from reaching an insufficient memory status.

When the data from the network greatly exceeds the disk write capacities, {{< TransferCFT/axwayvariablesComponentShortName  >}} closes certain sessions before reaching the memory saturation threshold until the correct balance is established. New session requests are refused until {{< TransferCFT/axwayvariablesComponentShortName  >}} has resolved the bottleneck problems, while enabling it to terminate the remaining transfers in progress.

{{< TransferCFT/axwayvariablesComponentShortName  >}} behavior is determined using the value assigned to the following logical names.

- CFT$MEMCSTE This logical name corresponds to the number of bytes used by the various monitor images. Its value is calculated according to the first part of the formula described above. The logical name is used to calculate the amount of memory that can still be allocated by the monitor. When the monitor is started up, the available memory is calculated as being equal to the number of pages that can still be reserved by the JOB in the page file, multiplied by 512, minus the value of CFT$MEMCSTE. The default value of this constant is 1 100 000 bytes.
- CFT$CATA This logical name corresponds to the percentage of remaining memory that can be allocated by {{< TransferCFT/axwayvariablesComponentShortName  >}}. {{< TransferCFT/axwayvariablesComponentShortName  >}} bases the regulation mechanism described above on this percentage. The default value of this constant is 70.
- CFT$COOL This logical name corresponds to the percentage of remaining memory that can be allocated by {{< TransferCFT/axwayvariablesComponentShortName  >}}. When this percentage is reached, {{< TransferCFT/axwayvariablesComponentShortName  >}} considers that it has resolved its memory congestion problems and stops closing sessions. The default value of this constant is 80.

<!-- -->

- CFT$MEMDLAY This logical name corresponds to the period, during which {{< TransferCFT/axwayvariablesComponentShortName  >}} refuses to establish new connections after the memory status has returned to normal. The default value of this constant is 0000 00:10:00.00, which is 10 minutes.

All of these constants are set to their default value if the corresponding logical name is not defined.
