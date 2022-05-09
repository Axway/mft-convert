{
    "title": "Transfer CFT z/OS performance",
    "linkTitle": "Optimize performance",
    "weight": "270"
}This section introduces performance issues to consider when using Transfer CFT in a z/OS environment.

<span id="Memory usage"></span>

Memory usage
------------

### File transfer server

The 24–bit addressed memory (JCL REGION parameter) is used as follows:

- Approximately 1,000 k used by the programs and memory areas.

<!-- -->

- Each transfer task requires an additional 4 k.

The 31–bit addressed memory is used as follows:

- Approximately 64 Mbytes are used by the programs and the memory zones.

<!-- -->

- Each transfer task requires an additional 1 Mbyte.

<!-- -->

- Each open BSAM file uses two buffers, the size of which is equal to BLKSIZE.

<!-- -->

- Each receive transfer uses (CFTPROT PACING\*CHKW) receive buffers; the use of high values with a large number of concurrent transfers may cause Transfer CFT z/OS to abort (ABENDS878).

<!-- -->

- The CFTSSL module needs an extra 4 Mbytes.

<!-- -->

- The installation of Sentinel requires an increase in the size of the execution area according to the configuration file options selected, and according to the number of messages to be processed.

Transfer CFT z/OS delays additional transfer creation when:

- 64 K of 24-bit memory is not available – message: CFTA02E
- 1 MB of 31-bit memory is not available – messages: CFTA01W for a transfer TASK (TFIL) or CFTA01E

The 64–bit addressed memory is used as follows:

- Approximately 512 Mbytes are allocated at startup
- 8 to 12 MB are allocated for each transfer
- Transfer CFT may use up to 20 Gbytes of storage above the bar

You can control the 64-bit usage with the MEMLIMIT=value JCL parameter. When the value is less than 512M, the use of 64-bit storage is disabled. In this case, Transfer CFT will handle a smaller number of parallel transfers and use more (20%) CPU cycles.

Even if Transfer CFT is running in a single address space, it may use a very large amount of virtual storage. You should monitor the z/OS use of page datasets.

### Transfer CFT Copilot server 

The 24–bit addressed memory is used as follows:

- Approximately 1400 k used by the programs and memory areas

The 31–bit addressed memory is used as follows:

- Each logged-in user requires an additional 10 Mbytes

<span id="Disk input/output"></span>

Disk input/output
-----------------

Disk I/O operations are performed in the following cases:

- When each BSAM block is read, or written

<!-- -->

- When a BSAM block is written at each synchronization point and a TCLOSE is sent for the file in write mode

<!-- -->

- When each catalog record is written every UPDAT synchronization points  
    (CFTCAT parameter value)

<!-- -->

- When the first record in each CFTCOM file is read every WSCAN seconds  
    (CFTCOM value)

<!-- -->

- Input/output management for HFS files is performed under the control of the OMVS address space. Using RECFM=V to write HFS files is usually faster than using RECFM=U

### Used z/OS dataspace

Transfer CFT z/OS keeps a copy of the Transfer CFT CATALOG in a dataspace. As a result:

- Transfer CFT does not reread the records.
- Transfer CFT uses CFTCAT 4\*RECNB pages in z/OS auxiliary storage.
- If A12OPTS SHARECAT=YES is specified, other Transfer CFT applications read the catalog primarily from this cache (messages CFCA03I and CFCA04I on the system console). In this case, stopping Transfer CFT may be delayed if another user is sharing this cache (messages CFCA06W and CFCA01).
- A 2-GB data space is allocated for everyunit of 131,000 records in the catalog.

### Release unused disk space in received files

In z/OS the Transfer CFT releases unused disk space in the files received if Transfer CFT is APF authorized (using wfname).

<span id="DF/SMS compression"></span>

DF/SMS compression
------------------

Transfer CFT in z/OS is partially compatible with file compression managed by DF/SMS.

- Read: Transfer CFT recalculates the size of the non-compressed file from DF/SMS information.
- Create: Transfer CFT requests the space required to write a non-compressed file. If possible, Transfer CFT then frees space not used at the end of transfer.

You cannot restart a receive transfer for a DF/SMS compressed file.

<span id="Automatic Restart Manager services"></span>

Automatic Restart Manager Service
---------------------------------

Transfer CFT authorized programs subscribe to the z/OS Automatic Restart Manager Service. These programs are restarted automatically depending on the rules defined in the ARM component.

The component names have the following format:

- CFTMAIN: Xidparm, where idparm is the name of the parameter passed to Transfer CFT (shortened to 15 characters).

To modify the element name:

1. Add a DD card attributing it the DDNAME ‘CFTARMID’.
1. Provide a single unique record, of which the first 16 bytes are used as substitutions for Xidparm.

The RESTART is delayed for one minute to handle the TCP/IP network sessions.

<span id="LE diagnostic files"></span>

LE diagnostic files 
--------------------

Certain Transfer CFT modules use a diagnostic file generated by LE. This file is CEEDUMP, an LE-formatted dump file.

<span id="VIPA"></span>

Virtual IP address support (VIPA)
---------------------------------

This section describes the specific parameters to use Transfer CFT with VIPA. You must customize the VIPA features in the TCPPARMS file of the IBM TCP/IP stack.

#### Dynamic VIPA

> **Note**
>
> Note: The LOWPORT parameter is deprecated.

Transfer CFT is fully compatible with dynamic VIPA. There are two methods available to use dynamic VIPA with Transfer CFT:

- Use the IBM program MODDVIPA to create or delete a dynamic VIPA definition. This is the mode of operations required for Transfer CFT GUI.
- Use the CFTNET SRCPORTS=(1-65535) value in Transfer CFT parameters. A SAF security profile update may be required in this case. Please read IBM SC31-8775 for more information.

#### Source VIPA

Transfer CFT is compatible with source VIPA. To enable, configure this feature in the TCP/IP parameters, and code CFTNET SRCPORTS=value with value &lt;2.

#### Summary

- When SRCPORTS=(‘0 - 65535’), only Source VIPA is enabled.
- When SRCPORTS=(‘1 - 65535’), both Source and Dynamic VIPA are enabled.

> **Note**
>
> Note: For ascending compatibility with Transfer CFT parameters, LOWPORT=0 (or 1) is still supported in the CFTNET and will automatically set the SRCPORTS with the correct value, but only if it is not already set in the macro!

A source CFTNET as below:

`CFTNET ID=xxxx`

`...`

`LOWPORT = 0,`

`.....`

`Will result in:`

`CFTNET ID=xxxx,`

`...`

`SRCPORTS = (0 - 65535),`

`...`

> **Note**
>
> Note: To avoid TCP/IP source port conflict with Sentinel, see the Sentinel Configuration instructions.

### TCP/IP stack affinity

If you want to ensure that Transfer CFT has an affinity to a particular TCP/IP stack, use the _BPXK_SETIBMOPT_TRANSPORT environment variable.

The environment variable _BPXK_SETIBMOPT_TRANSPORT located in UPARM(CNFENV) can be uncommented and set with the TCP/IP stack to be used by CFT.

### Modify record format to improve performance

To improve the performance, we recommend using the variable file format (set this in the CFTRECV object, ID=xxx,FRECFM=V). You should set the FBLKSIZE to less than or equal to 27992, which is the value we recommend you use. This results in:

- The EXCP number is divided by 4.
- The CPU time is lowered.
- The memory usage may either decrease or increase, depending on the FBLKSIZE used.
