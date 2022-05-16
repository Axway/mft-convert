---

    title: Why  customize the kernel?
    linkTitle: Why customize the kernel?
    weight: 190

---
This topic explains the advantages of customizing the kernel, and additionally
presents the following related subjects:

- [Global
    memory segment](#Global_memory_segment)
- [Message
    queue depth](#Message_queue_depth)
- [Memory
    allocated to TCP](#Memory_allocated_to_TCP)
- [Number
    of files used by a process](#Number_of_files_used_by_a_process)

<span id="Global_memory_segment"></span>

### Global shared memory segment

By default, you can only create a global memory segment for a size defined
in terms of the kernel.

{{< TransferCFT/axwayvariablesComponentShortName  >}} can attempt to create a 32 MB global memory segment. Transfer
CFT uses this global memory for data exchanges during the execution of
various operational tasks. The 32 MB value represents the average size
necessary to support efficient performance without slowing the system,
due to memory saturation.

We strongly recommend that you change the kernel configuration, even
though {{< TransferCFT/axwayvariablesComponentShortName  >}} automatically adapts to the maximum size authorized
by the system. The reason for this is that if the memory is insufficient,
{{< TransferCFT/axwayvariablesComponentShortName  >}} slows down significantly.

In some cases, when receiving transfers from high-speed systems via
TCP, you may notice interlocks preventing {{< TransferCFT/axwayvariablesComponentShortName  >}} from running correctly.
If the capacity of the system cannot support the resulting overload, you
must reduce the number of concurrent transfers.

As the system can accommodate this modification, it is recommended that
the maximum size of the shared memory segment be systematically increased
to at least 32 MB.

<span id="Message_queue_depth"></span>

### Message queue depth

By default, some UNIX systems allow a maximum of 40 unread messages
to transit in a message queue.

To guarantee optimum performance levels, {{< TransferCFT/axwayvariablesComponentShortName  >}} maximizes its
use of the message queues. It may be that {{< TransferCFT/axwayvariablesComponentShortName  >}} requirements exceed
the system capacity. This is the case particularly over TCP networks,
when the remote monitor + network configuration allows a throughput exceeding
the capacities of the local system. This phenomenon becomes even more
likely if another application is also making intensive use of the message
queues.

<span id="Memory_allocated_to_TCP"></span>

### Memory allocated to TCP

When first installed, Open Server allocates a certain amount of memory
to allow the IP layer to operate correctly. Open Server adjusts this memory
allocation as required, up to a given maximum.

When this maximum is reached, following a request by a local application,
the request is refused. However, when this saturation is due to the network,
Open Server *waits* for space to be released on the local system
so that it can continue processing.

When this saturation phenomenon occurs, the remote {{< TransferCFT/headerfootervariableshflongproductname  >}} and network
permit an overall throughput that is too high for the local system, given
the close link between {{< TransferCFT/axwayvariablesComponentShortName  >}} and the data transiting on the network,
so this space cannot be released. The phenomenon is even more likely if
another application is also making intensive use of the network memory.

To ensure {{< TransferCFT/axwayvariablesComponentShortName  >}} operation, you must modify the kernel to increase
the size of the memory allocated to TCP.

<span id="Number_of_files_used_by_a_process"></span>

### Number of files used by a process

By default, some UNIX systems allow a process to only open 64 files
at the same time.

If you do not modify this limit, you cannot use {{< TransferCFT/axwayvariablesComponentShortName  >}} to its
full potential: 64 concurrent transfers + link channels + listening channels
+ trace channels &gt; 64 open files.

<span style="color: #000000;">To achieve 64 concurrent transfers, you must modify some of the kernel
properties so that it can open as many files as possible; 1024 is suggested.
</span>
