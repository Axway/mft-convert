{
    "title": "Multi-node architecture",
    "linkTitle": "Multi-node architecture",
    "weight": "160"
}This topic describes the {{< TransferCFT/axwayvariablesComponentShortName  >}} multi-node feature, which provides you with horizontal scalability and high availability for failovers. Following a brief review of terms, this topic describes:

- Active/Active clusters
- Active/Passive clusters
- Multi-node architecture
- Recovery and dispatching

****Terminology****

The {{< TransferCFT/axwayvariablesComponentShortName  >}} active-active architecture is based on hosts, nodes, and a shared file system.

*What is a host?* A host refers to a physical or virtual server located behind a load balancer, or other DNS mechanism. A server can host zero or multiple nodes. {{< TransferCFT/axwayvariablesComponentShortName  >}} itself does not manage the round robin or other load balancing mechanisms.

*What is a node?* A node is a {{< TransferCFT/axwayvariablesComponentShortName  >}} runtime running on a host. Multiple nodes are called a {{< TransferCFT/axwayvariablesComponentShortName  >}} cluster, which can run on one or multiple hosts.

*What is a shared file system?* A shared file system is a file system resource that is shared by several hosts.

Active/active cluster
---------------------

The Transfer CFT active/active architecture, multi-node with multi-host functionality, includes:

- Scalability to overcome node transfer and session restrictions (for example, the maximum number of simultaneous transfers per node).
- Automatic node restart after a host failure allowing another host to take over.
- Rolling updates (patch and service pack) to provide continuous availability.

These services provide more than a balance of the total load among nodes, in active/active if one host goes down the other hosts recuperates the entire load.

The active/active architecture requires a shared file system.

### Active/active features

- Able to use up to 8 nodes
- Available exclusively on Transfer CFT V3 (and higher)
- Supported platforms: Windows, Unix, and z/OS

Active/passive cluster
----------------------

The Transfer CFT active/passive architecture includes:

- High availability, that consists of one active host and one passive host. The passive host is available to take over the failed host. The services provided by the failed host are started on the passive node.  
- Rolling updates (patch and service pack) to provide continuous availability.  

The active/passive architecture requires a shared file system.

### Active/passive features

- Only one active host with only one runtime
- Available on all {{< TransferCFT/suitevariablesTransferCFTName  >}} versions
- Supported platforms: Windows Server with MSCS , Linux, Solaris, AIX /HACMP, and IBM i

Multi-node architecture
-----------------------

The Transfer CFT multi-node architecture is based on hosts, nodes, a shared file system and a load balancer. Regardless of the number of servers hosting the nodes from outside the cluster, all of the nodes are viewed as a single Transfer CFT instance.

> **Note**
>
> Note: This section, and the multi-node sections that follow, are based on an active/active installation.

The multi-node setup comprises:

- A ****shared file system**** (infrastructure), for multiple nodes to be able to access the same files using the same set configuration. The shared disk provides communication, configuration, partners, data flows, internal datafiles, and application transferable files.
- A ****load balancer**** (infrastructure), hardware or software, by which incoming connections can pass. The load balancer dispatches the incoming traffic between the different hosts. However, the outgoing traffic does not use the load balancer.
- One ****connection dispatcher**** per host (Copilot component), checks for incoming connections on the host it is running on and dispatches connections to nodes running on the same host. For z/OS platforms, refer to *VIPA load balancing* in the *Transfer CFT z/OS Installation and Operation Guide*.
- One ****node manager**** per host (Copilot component), which monitors all nodes. If a node goes down, the node manager detects the inactivity and restarts it if needed, while taking into account the activity of other node managers. The monitoring mechanism is based on locks provided by the file system lock manager or the resource lock manager. Additionally, the node manager has its own watchdog that is used to prevent incorrect behavior after a shared file system auto-unlock, for instance due to NFSv4 lease time. If the watchdog does not receive a keep-alive from the node manager, all local nodes are killed and relock is requested.
- One ****synchronous communication media dispatcher**** per host (Copilot component), which allows the use of the synchronous communication media feature in a multi-node environment.

********Normal functioning********

![](/Images/TransferCFT/normal_multinode.png)

********If Host A goes down, then Host B takes over Node 1********

********![](/Images/TransferCFT/host_down.png)********

Runtime files
-------------

All runtime data are stored on a shared file system.

The following internal datafiles are shared between nodes:

- Parameter internal datafiles (CFTPARM)
- Partners internal datafiles (CFTPART)
- Unified configuration (UCONF)
- PKI base (CFTPKU)
- Main communication media file (CFTCOM)

The following internal datafiles are node specific, and the filename is flagged by the node identifier:

- Catalog (cftcata00, cftcata01,...) located in &lt;cft_runtime_dir&gt;/data
- Communication media file (cftcom00, cftcom01,...) located in &lt;cft_runtime_dir&gt;/data
- Log files (cftlog00, cftlog01,...) located in &lt;cft_runtime_dir&gt;/log
- Output file (cft00.out, cft01.out,...) located in &lt;cft_runtime_dir&gt;/run
- Account file (cftaccnt00, cftaccnt01,...) located in &lt;cft_runtime_dir&gt;/accnt

> **Note**
>
> Caution  
> Transfer CFT is sensitive to the shared file system's performance, as transfer requests perform concurrent access to the database (COM, catalog, parameters). We recommend a high-performance shared file system, a solid-state drive (SSD), a dedicated network link between the clients and the file system server, and low latency &lt; 2ms.

Failover recovery
-----------------

### Node failover

There are two possibilities when a node manager detects a node failure:

- If it is a ****local**** node failover, the node manager automatically attempts to restart the node on the local server.
- If it is a ****remote**** node failover, the node manager waits for the remote node manager (if it is still active) to restart the node. If the remote node manager does not restart the node  before the timeout, the local node manager restarts the node on the local server.

After the node is restarted, whether local or remote, it completes all transfer requests that were active when the failure occurred.

- After a fail over, manual intervention is required to rebalance the nodes between the hosts.

### Transfer failover

When a node receives an incoming request - a transfer receive, restart, acknowledgement or negative acknowledgement - if the corresponding transfer record cannot be found in the node's own catalog, the node requests the transfer record from other nodes through the CFTPRX task.

Possible scenarios include:

- If another node has the catalog record, the node retrieves it and performs the transfer.
- If no nodes have the record, an error is returned.
- If any one of the nodes does not respond, the requesting node continues to retry all nodes until the session's timeout. Once the timeout is reached, the node ends the connection. After this, the remote partner retries the request according to its retry parameters.

If a node fails during the transfer recovery process, the catalog record is locked in both catalogs until both nodes are available for recovery.

How local request dispatching works
-----------------------------------

In a multi-node architecture there is one primary communication file, and then as many secondary communication files as nodes to control request dispatching.

****How does communication between the primary file and secondary files occur?****

The dispatcher (CFTMCOM) process controls the relationship between the primary COM file and the specific secondary COM files.

****What happens in the case of a communication breakdown?****

No breakdown occurs because there is no communication between the nodes. If the dispatcher fails, another active node takes over as the dispatcher.

****When and how is the transfer request removed from the communication file?****

- It is removed from the primary communication file once the dispatcher has written it to the secondary communication file.
- It is then removed from the secondary file once CFTMAIN has processed the transfer request.

****How are requests dispatched?****

There are two types of requests, which may be handled differently depending on their type:

1. Transfer requests are dispatched on a single node, according to the defined dispatching rule. The dispatching rule is defined by a UCONF value, the `cft.multi_node.cftcom.dispatcher_policy` parameter.
1. Dispatching for administrative types of requests are broadcast to all active nodes. For example, using the HALT command to stop a transfer request applies to all nodes.

#### Client transfer request

![](/Images/TransferCFT/client_multinode_new.png)

How remote request dispatching works
------------------------------------

All remote requests are going through a load balancer, server transfers, SOAP web-service requests, REST API requests, UI requests (Copilot applet application).

> **Note**
>
> Note: For z/OS platforms, refer to VIPA load balancing in the Transfer CFT z/OS Installation and Operation Guide.

#### Server transfer dispatching

![](/Images/TransferCFT/server_multi.png)

#### Web API request dispatching

![](/Images/TransferCFT/api_multinode.png)

Troubleshooting multi-node issues
---------------------------------

For information on troubleshooting multi-node issues, please refer to [Troubleshoot multi-node](../troubleshoot_intro/admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_multinode).

****Related topics****

- [Multi-node commands](multi_node_commands)
- [Managing multi-node]()
- [Unified configuration parameters](../cft_intro_install/about_this_document_zos/c_multinode_zos/multi_node_parameters)
- [Configure multi-node simultaneous transfers](../concepts/about_parallel_transfers/multi_node_simultaneous_transfers)
- [Using a shared file system](../cft_intro_install/unix_install_start_here/before_you_start_unix/n_active_active/shared_file_prereq_ux/activeactive_shared_file_systems)
