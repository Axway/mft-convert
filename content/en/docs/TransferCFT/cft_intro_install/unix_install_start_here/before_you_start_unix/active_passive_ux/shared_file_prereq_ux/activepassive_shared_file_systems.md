---
    title: "Using a shared file system"
    linkTitle: "Using a shared file system"
    weight: 220
---## Active/passive mode shared file systems

Active/passive mode requires a shared file system. Two typical shared file system implementations are NAS (network attached storage) and SAN (storage area network). This section describes GPFS, NFSv4, AWS EFS, and SMB/CIFS.

> **Note**
>
> In Transfer CFT, you can use any Portable Operating System Interface (POSIX) compliant shared file system for transferable application files.

## Using GPFS

GPFS, General Parallel File System, is the shared file system of choice for Transfer CFT. It provides high-speed file access for Transfer CFT.

<span id="Using"></span>

## Using NFS

The recommendations in this section apply to a Transfer CFT an active/passive architecture based on an NFS shared file system. As NFSv3 cannot detect host failures, we recommend that you use NFSv4.

- [Required NFS mount options](#Required)
- [Mount options summary](#Mount) 
- [Synchronous / asynchronous option impact](#Impact)
- [Tuning NFSv4 locking for node failover](#Tuning)
- [Troubleshoot an NFS lock daemon issue](#Troubles)

<span id="Required"></span>

### Required NFS mount options

#### Define the NFS version

If version 4 is not your NFS subsystem's default, you should specify version 4 when defining the mount options. Depending on your OS, use either the `vers `or `nfsvers` option.

#### Set the hard and nointr options

Mount NFS using the `hard `and `nointr` options. The `intr` mount option should not be available for NFSv4, but if you are in doubt, you should explicitly specify the `nointr `option.

#### Define file locking

Because Transfer CFT uses POSIX file locking services to synchronize shared files, make sure that the NFS clients report these locks to the NFS server. Depending on the NFS client, the corresponding option to tune may be called  `local_lock`,` llock`, or `nolock`.

- If` local_lock = all`, the NFS client assumes locks are local, and allows Transfer CFT to simultaneously start on both the active and passive hosts. As a result, Transfer CFT does not function properly.
- If `local_lock = none`, or if this option is not specified, the NFS client assumes that the locks are not local. This prevents Transfer CFT from starting on the second host when it is running on the first host.

Do not enable the local locking option.

#### Set the cto option

NFS implements a weak data consistency called "Close To Open consistency" or `cto`. This means that when a file is closed on a client, all modified data associated with the file is flushed from the server. If your NFS clients allow this behavior, be certain that the `cto `option is set.

<span id="Mount"></span>

#### Mount options summary

The following table summarizes the recommended NFS mount options. Note that depending on the OS platform, only one of the three locking options should be available.


| Recommended option  | Not recommended  |
| --- | --- |
| vers=4 (or nfsvers=4)  | not specified or value &lt;= 4  |
| hard (default)  | "soft" specified  |
| nointr (not the default)  | "intr" specified  |
| llock not specified  | "llock" specified  |
| lock (default)  | "nolock" specified  |
| local_lock=none (default)  | any other value specified  |
| cto (default)  | "nocto" specified  |


### Synchronous versus asynchronous option

To improve performance, NFS clients and NFS servers can delay file write operations in order to combine small file IOs into larger file IOs. You can enable this behavior on the NFS clients, NFS servers, or on both, using the `async `option. The `sync `option disables this behavior.

#### **Client**

On the client side, use the `mount `command to specify the `async/sync` option.

##### Async

The NFS client treats the `sync `mount option differently than some other file systems. If neither `sync `nor `async `is specified (or if `async `is specified), the NFS client delays sending application writes to the server until any of the following events occur:

- Memory limitations force reclaiming of system memory resources.
- Transfer CFT explicitly flushes file data (PeSIT synchronization points, for example).
- Transfer CFT closes a file.

This means that under normal circumstances, data written by Transfer CFT may not immediately appear on the server that hosts the file.

##### Sync

If the `sync `option is specified on a mount point, any system call that writes data to files on that mount point causes that data to be flushed to the server before the system call returns control to Transfer CFT. This provides greater data cache coherence among clients, but at a significant cost to performance.

#### Server

On the server side, use the `exports `command to specify the `async/sync` option (NFS server export table).

##### Async

The `async `option allows the NFS server to violate the NFS protocol and reply to requests before any changes made by that request have been committed to stable storage (the disk drive, for example), even if the client is set to `sync`. This option usually improves performance, however data may be lost or corrupted in the case of an unclean server restart, such as an NFS server crash.

This possible data corruption is not detectable at the time of occurrence, because the async option instructs the server to lie to the client, telling the client that all data was written to stable storage (regardless of the protocol used).

##### Sync

Enables replies to requests only after the changes have been committed to stable storage.

> **Note**
>
> For more information on these options, refer to NFS mount and export options in the UNIX man pages (for example, here).

<span id="Impact"></span>

#### Synchronous / asynchronous option impact


| Client  | Server  | Internal data  | Transferable data  | Performance  |
| --- | --- | --- | --- | --- |
| Sync  | Sync  | 1  | 1  | Low  |
| Sync  | Async  | 2 (secure the NFS server)  | 2 (secure the NFS server)  | Medium  |
| Async  | Sync  | 1 (if cft.server.catalog.<br /> sync.enable=Yes)  | 1 (when using sync points)  | Medium - high  |
| Async  | Async  | 3  | 3  | High  |


Legend:

- 1 = Secure
- 2 = Fairly secure
- 3 = Not secure
- Internal data = Transfer CFT runtime files, such as the catalog
- Transferable data = Files exchanged using Transfer CFT

<span id="Tuning"></span>

### 

<span id="Troubles"></span>

### Troubleshoot an NFS lock daemon issue with no error message

When transferring files that are located in a **N**etwork **F**ile **S**ystem, an NFS locking issue (lockd) may occur if the correct port is not open on the firewall.

****Symptom****

- Flow transfers hang in the phase `T` and phasestep `C`, with a timeout but no error message.

****Remedy****

- Check that the correct port for the lockd service is open on the firewall (default=4045).

### Troubleshoot the UID and GID

NFS uses UIDs and GIDs for all file permissions. Therefore during installation of a multihost multi-node architecture, the Transfer CFT user must have read and write access to any folder and files created on the other host or hosts. Additionally, all hosts in the implementation should use the same UID number.

For more information, please refer to the [NFS](http://nfs.sourceforge.net/nfs-howto/ar01s07.html#pemission_issues) documentation.

## Using AWS EFS

The recommendations in this section apply to a Transfer CFT multi-node, multi-host architecture based on an Amazon Web Services (AWS) Elastic File System (EFS) shared file system.

When using AWS EFS, you cannot set the server options; only the client is configurable.

This system is based on NFSv4. For more information on NFSv4, please see [Using NFS](#Using) .

This shared file system has features that impact performance, as compared to a traditional NFS:

- Distributed systems replicating data  
- Processing does not continue until all data is replicated

## Using SMB/CIFS

*Windows only*

SMB, Server Message Block, is a protocol used to share files across corporate intranets and the Internet, and is available as a shared file system when running Transfer CFT in a Windows environment.

****SMB recommendations for Transfer CFT****

- We recommend SMB 2 or higher.
- The minimum supported Samba version is 4.15.3 if Samba (smbd) is your SMB provider.
- If your SMB provider supports opportunistic locks (oplocks), we recommend that you disable this feature.

> **Note**
>
> CIFS, Common Internet File System, is an out-dated SMB protocol variant.
