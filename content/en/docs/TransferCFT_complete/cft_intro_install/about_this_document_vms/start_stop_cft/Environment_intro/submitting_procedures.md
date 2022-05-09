{
    "title": "Submit Transfer CFT OpenVMS procedures",
    "linkTitle": "Submit Transfer CFT VMS procedures",
    "weight": "260"
}A number of logical names are used to configure the Transfer CFT behavior with respect to the various procedures submitted by it. For more information, refer to the [MONIT_MEM utility]() section.

The following table describes the logical names for the procedures that control Transfer CFT behavior.


| Logical Name | Description |
| --- | --- |
| CFT_QUEUE | This logical name designates the name of the batch queue, in which the monitor submits the various procedures that it controls. It must be defined with the /JOB attribute. |
| CFT_SUBMIT | This logical name defines the behavior of the monitor when it submits an end of transfer procedure. If the logical name is set to USER, the procedures are submitted from the transfer owner account. |
| CFT_PRINT | This logical name is used to define a print queue, via which the procedure execution reports are printed. It must be defined with the /JOB attribute. |
| CFT_LOGDEL | When set to YES, this logical name is used to request VMS to delete the execution reports of the procedures that it submits. Any other value maintains the reports in the SYS$LOGIN directory. It must be defined with the /JOB attribute. |


### Controlling memory usage

Logical names are used to control Transfer CFT memory bottlenecks, refer to the [Delivered components](../../security_elements) section. The following table describes the logical names that are used to control Transfer CFT OpenVMS memory.


| Logical Name | Meaning |
| --- | --- |
| CFT$MEMCSTE | This logical name is used to define the amount of memory used by the monitor images in the Transfer CFT VMS JOB.<br /> Its default value is 1 100 000. |
| CFT$CATA | This logical name is used to define the percentage of available memory, as from which the monitor implements the bottleneck control mechanism.<br /> Its default value is 80. |
| CFT$COOL | This logical name is used to define the percentage of available memory, as from which the monitor is no longer in the alert status triggered by CFT$CATA.<br /> Its default value is 70. |
| CFT$MEMDLAY | This logical name is used to define a period after the monitor is no longer in the alert status, during which the monitor refuses any new incoming or outgoing connections, so that it can process the data already received. |
| CFT$MG_NB | This logical name is used to define the number of 32 KB buffers reserved by Transfer CFT VMS in the global section. |

