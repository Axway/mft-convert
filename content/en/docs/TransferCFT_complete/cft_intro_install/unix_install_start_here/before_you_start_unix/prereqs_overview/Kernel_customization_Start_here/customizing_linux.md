{
    "title": "Linux: Customize the kernel",
    "linkTitle": "Linux: Customize the kernel",
    "weight": "210"
}This section describes suggested kernel customizations to perform prior to an installation to better enable Transfer CFT operations.

Kernel customization parameters
-------------------------------

The following table lists IPC tuning parameters to consider customizing and recommended values.

> **Note**
>
> Note: To aid in correctly calculating semaphores, remember that each Transfer CFT has two semaphores per instance.


| Parameter  | Recommended<br/> value | Description  |
| --- | --- | --- |
| msgmax  | 8192  | Maximum size of a message in bytes.  |
| msgmnb  | 32768  | Maximum size in bytes on a single IPC message queue.  |
| msgmni  | 48 per {{< TransferCFT/axwayvariablesComponentLongName  >}} instance<br/> See [NOTE](#note_linux)** | Maximum number of IPC message queue resources allowed.<br/> You require as many message queues as processes per Transfer CFT instance (when using multiple instances, multiply the number of instances by the number of Transfer CFT processes). |
| shmall<br/> (shmall*PAGE_SIZE) | 2097152<br/> (8 GB when page_size = 4096) | The total amount of shared memory available on the system is 2097152*4096 bytes (shmall*PAGE_SIZE) which is 8 GB.<br/> This number may be affected by the use of a very large number of Transfer CFT instances. |
| shmmax  | 33554432: for fewer than 512 transfers in parallel<br/> 67108864: for more than 512 transfers in parallel | Maximum size in bytes for a shared memory segment.  |
| shmmni  | 4096  | Number of shared memory segment identifiers in the system.<br/> For each Transfer CFT instance you need 2 shared memory segments, so when using multiple instances or multi-node, multiply the number of instances by 2. |
| shmseg  | 10  | Maximum number of shared memory segments per process.  |
| semmsl  | 250  | Maximum number of IPC semaphores per set.  |
| semmns  | 32000  | Number of IPC system-wide semaphores.  |
| semopm  | 100  | Maximum number of semaphore operations that can be performed per semop(2).  |
| semmni  | 128  | Maximum number of IPC system-wide semaphore sets.<br/> For each Transfer CFT instance you need 2 semaphore sets, so when using multiple instances or multi-node, multiply the number of instances by 2. |


> **Note**
>
> Note: \*\*This value is based on the number of processes started by the Transfer CFT. This minimum is typically 7, but can be in excess of 40 depending on the values for maxtask, sslmtask, Sentinel if enabled, one task for each exit, etc.
