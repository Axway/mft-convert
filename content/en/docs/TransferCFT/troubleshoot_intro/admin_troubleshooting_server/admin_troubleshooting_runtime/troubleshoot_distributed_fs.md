---

    title: Troubleshoot distributed file systems
    linkTitle: Troubleshoot distributed file systems
    weight: 430

---
## NFS lock daemon issue with no error message

When transferring files that are located in a **N**etwork **F**ile **S**ystem, an NFS locking issue (lockd) may occur if the correct port is not open on the firewall.

****Symptom****

- Flow transfers hang in the phase <span class="code">`T`</span> and phasestep <span class="code">`C`</span>, with a timeout but no error message.

****Remedy****

- Check that the correct port for the lockd service is open on the firewall (default=4045).

## Multi-node multihost installation issues

### Second node or host fails to install

*UNIX*

While performing a multihost multi-node installation, where the user has a different UserId (UD) and GroupId (GID) for each host, you successfully install the first node on the first host, which creates the runtime on the shared disk. But the second node or host installation fails with the message:

Warning: The directory

/mnt/CFT36MNLIN\_BN12807000/runtime

is not writable by the current user

To resolve this issue, using a different user perform the following commands on all hosts that are involved in the multihost, multi-node installation:

1. Open an SSH session on the machine and run the following command to change the UID, for example:  
    <span class="code">`sudo usermod -u 1005 ithomas`</span>  
    Where `1005 `is the desired common UID, and <span class="code">`ithomas `</span>is the user common to all of the UNIX hosts.
1. Run the command to change the GID, for example:  
    `sudo groupmod -g 1006 ithomas`  
    Where <span class="code">`1006 `</span>is the desired common GID, and <span class="code">`ithomas `</span>is the group to which the user <span class="code">`ithomas `</span>belongs.

For more information, please refer to the [NFS](http://nfs.sourceforge.net/nfs-howto/ar01s07.html#pemission_issues) documentation.
