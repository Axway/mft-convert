{
    "title": "Transfer CFT logical names",
    "linkTitle": "Transfer CFT logical names",
    "weight": "300"
}{{< TransferCFT/axwayvariablesComponentShortName  >}} uses a number of logical names to designate the resources or define an operating mode.

Default Transfer CFT directory
------------------------------

Transfer CFT recognizes the following logical name, which must be defined so that {{< TransferCFT/axwayvariablesComponentShortName  >}} can locate images.

CFT_IMAGE  
This logical name points to a directory containing all {{< TransferCFT/axwayvariablesComponentShortName  >}} images. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_EXE:, which must be defined with the /JOB attribute.

### Other {{< TransferCFT/axwayvariablesComponentShortName  >}} directories

The installation procedure creates a directory structure in which the supplied files are located. Each directory is defined by a logical name to make it easier to use. These logical names are not mandatory, but are used in the examples provided and the CFTLOGIN.COM procedure.

The following table lists the logical names that are defined in the standard {{< TransferCFT/axwayvariablesComponentShortName  >}} product, located in the D$CFT_RUN:[profile]CFTLOGIN.COM procedure.


| Logical Name  | Description  |
| --- | --- |
| D$CFT | Logical name pointing to the {{< TransferCFT/axwayvariablesComponentShortName  >}} installation directory.<br />  |
| D$CFT_INST  | Logical name pointing to the Transfer CFT HOME directory.  |
| D$CFT_RUN  | Logical name pointing to the Transfer CFT RUNTIME directory.  |
| CFT_DAT | Directory containing the Transfer CFT configuration definition files, catalog and communication file.<br />  |
| CFT_EXE | Directory containing all the monitor images and the procedure used to link the {{< TransferCFT/axwayvariablesComponentShortName  >}} images.<br />  |
| CFT_LOG | Directory containing the monitor log and accounting files.<br />  |
| CFT_PROC | Directory containing the end of transfer procedure, and log and account switching samples.<br />  |
| CFT_RECV | Directory used in the samples provided to receive the files transferred.<br />  |
| CFT_SCEN | Directory containing Transfer CFT configuration samples.<br />  |
| CFT_SEND | Directory used in the samples provided for the files sent.<br />  |
| CFT_UTL | Directory containing the procedures used to define and modify the {{< TransferCFT/axwayvariablesComponentShortName  >}} environment.<br />  |
| CFT_PKI  | Directory containing certificate samples.  |
| CFT_API  | Directory containing API samples.  |
| CFT_EXIT  | Directory containing exit samples.  |
| CFT_ACCNT  | Directory containing account files.  |


### Standard {{< TransferCFT/axwayvariablesComponentShortName  >}} service files

All {{< TransferCFT/axwayvariablesComponentShortName  >}} service files are designated by standard logical names known to {{< TransferCFT/axwayvariablesComponentShortName  >}}, or the associated utilities, and used by default.

The following table describes the logical names for {{< TransferCFT/axwayvariablesComponentShortName  >}} standard service files.


| Standard Logical Name  | Meaning  |
| --- | --- |
| CFTUCONF  | Client configuration file.  |
| CFTUCONFDEF  | Transfer CFT dictionary values. Do ****not**** modify these values.  |
| CFTPARM | Transfer CFT configuration file. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_DAT:CFTPARM.INX. |
| CFTPART | Transfer CFT partner file. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_DAT:CFTPART.INX. |
| CFTCATA | Transfer CFT catalog file. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_DAT:CFTCATA.REL. |
| CFTCOM | Transfer CFT communication file. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_DAT:CFTCOM.REL. It must be defined with the /JOB attribute. |


### Additional {{< TransferCFT/axwayvariablesComponentShortName  >}} files

By default, the following logical names are not known to {{< TransferCFT/axwayvariablesComponentShortName  >}} or utilities. They are declared explicitly in the configuration. However, they are used by the samples provided and the CFTLOGIN.COM procedure.

The following table lists the logical names that are used in the supplied {{< TransferCFT/axwayvariablesComponentShortName  >}} files.


| Logical Name  | Meaning  |
| --- | --- |
| CFTLOG<br /> CFTLOGA | {{< TransferCFT/axwayvariablesComponentShortName  >}} log files.<br /> CFT_LOG:CFTLOG.LOG and CFT_LOG:CFTLOG.LOGA.<br /> These must be defined with the /JOB attribute. |
| CFTACCNT<br /> CFTACCNTA | Transfer CFT accounting files.<br /> CFT_LOG:CFTACCNT.LOG and CFT_LOG:CFTACCNT.LOGA. |
| CFTPKU  | PKI base file. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_PKI:CFTPKI.INX.  |
| CFTEXAM  | VMS Share User API samples. The CFTLOGIN.COM procedure supplied with the product defines it as CFT_EXE:CFTEXAM.EXE.  |


### Submitting {{< TransferCFT/axwayvariablesComponentShortName  >}} procedures

A number of logical names are used to configure the {{< TransferCFT/axwayvariablesComponentShortName  >}} behavior with respect to the various procedures submitted by it. For more information, see the[MONIT_MEM utility]()  section.

The following table describes the logical names for the procedures that control {{< TransferCFT/axwayvariablesComponentShortName  >}} behavior.


| Logical Name  | Description  |
| --- | --- |
| CFT_QUEUE | This logical name designates the name of the batch queue, in which the monitor submits the various procedures that it controls. It must be defined with the /JOB attribute. |
| CFT_SUBMIT | This logical name defines the behavior of the monitor when it submits an end of transfer procedure. If the logical name is set to USER, the procedures are submitted from the transfer owner account. |
| CFT_PRINT | This logical name is used to define a print queue, via which the procedure execution reports are printed. It must be defined with the /JOB attribute. |
| CFT_LOGDEL | When set to YES, this logical name is used to request VMS to delete the execution reports of the procedures that it submits. Any other value maintains the reports in the SYS$LOGIN directory. It must be defined with the /JOB attribute. |


### Control memory usage

Logical names are used to control {{< TransferCFT/axwayvariablesComponentShortName  >}} memory bottlenecks, refer to the [Delivered components](../../security_elements) section. The following table describes the logical names that are used to control {{< TransferCFT/axwayvariablesComponentShortName  >}} memory.


| Logical Name  | Meaning  |
| --- | --- |
| CFT$MEMCSTE | This logical name is used to define the amount of memory used by the monitor images in the Transfer CFT JOB.<br /> Its default value is 1 100 000. |
| CFT$CATA | This logical name is used to define the percentage of available memory, as from which the monitor implements the bottleneck control mechanism.<br /> Its default value is 80. |
| CFT$COOL | This logical name is used to define the percentage of available memory, as from which the monitor is no longer in the alert status triggered by CFT$CATA.<br /> Its default value is 70. |
| CFT$MEMDLAY | This logical name is used to define a period after the monitor is no longer in the alert status, during which the monitor refuses any new incoming or outgoing connections, so that it can process the data already received. |
| CFT$MG_NB | This logical name is used to define the number of 32 KB buffers reserved by Transfer CFT in the global section. |


### Concurrent access to the communication file

If users belonging to a group other than the {{< TransferCFT/axwayvariablesComponentShortName  >}} group want to submit commands in the {{< TransferCFT/axwayvariablesComponentShortName  >}} communication file, you must enable the concurrent access control mechanism at system level. The CFT_LOCK logical name is used to implement this mechanism. For more information, see the [Transfer CFT parameter settings]() section.

### CFT_LOCK

If the CFT_LOCK logical name is set to SYSTEM, the concurrent access control mechanism is enabled at system level. Otherwise, the control is performed at group level. The SYSLCK privilege is required to implement the mechanism. The logical name must be defined with the /JOB attribute.

### Optimize receive file write operations

When receiving files, {{< TransferCFT/axwayvariablesComponentShortName  >}} allows you to optimize the way in which they are written, by performing disk accesses in virtual block mode instead of requesting that RMS write each record. The CFT$BLOCKIO logical name is used to implement this mechanism. The increased performance levels are particularly noticeable when writing files containing small-sized records.

### CFT$BLOCKIO

If this logical name is not defined or its substitution begins with the letter N, {{< TransferCFT/axwayvariablesComponentShortName  >}} writes the records received through RMS. In all other cases, the records are written in virtual block mode. The logical name must be defined with the /JOB attribute.
