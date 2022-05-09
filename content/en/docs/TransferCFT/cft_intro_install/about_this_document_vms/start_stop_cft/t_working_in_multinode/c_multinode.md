{
    "title": "About multi-node architecture",
    "linkTitle": "About multi-node architecture",
    "weight": "260"
}This topic describes the Transfer CFT multi-node feature, which provides you with horizontal scalability and high availability for fail overs. See also [Active/active You configure two or more Transfer CFTs, up to four nodes, each having an independent workload. These nodes will then run as individual nodes until a fail over occurs. When a fail over occurs, one of the other nodes takes over from the failed node. This secondary node offers services to new and existing transfer requests and tasks, until the original node resumes activity. When the failed node restarts, a manual intervention required in Transfer CFT, it resumes its connections from the secondary or replacement node. To summarize, when a fail over occurs the external Transfer CFT is oblivious to any change. The requests are directed to the load balancer, which resubmits any uncommitted transactions to the replacement node. During fail back, the connection returns to the original node.]() for conceptual information.

Multi-node benefits include:

- Horizontal scalability: increased transfer flow capacity
- High availability: active/active including patch management on an active cluster, and automatic node restart after fail over
- Traffic management: balancing load between nodes through an external load balancer or DNS

> **Note**
>
> Note: Transfer CFT supports the following OS for muti-node architecture: Windows-x86, Linux-x86, AIX-power, HPUX-parisc, HPUX-ia64, Sun-SPARC, Sun-x86.

Prerequisites
-------------

Transfer CFT in multi-node architecture requires:

- One key per node must have the cluster option, and if there is more than one host you require at least one valid key per host
- A shared file system for use of a multi-node architecture on several hosts. Additionally, the system must be configured prior to the multi-node installation and the shared disk ready when starting the Transfer CFT Copilot server.

Architecture
------------

The Transfer CFT multi-node architecture is based on hosts, nodes, and a shared file system.

Regardless of the number of servers hosting the nodes from outside the cluster, the nodes are viewed as a single machine on an internal network.

****What is a host?****

A host refers to a physical or virtual server located behind a load balancer, or other DNS mechanism. A server can host zero or multiple nodes. Transfer CFT itself does not manage the round robin or other load balancing mechanism.

****What is a node?****

A node is a Transfer CFT runtime running on a host. Multiple nodes are called a Transfer CFT cluster, which can run on one or multiple hosts.

****What is a shared file system?****

A shared file system is a file system resource that is shared by several hosts.

### General architectural overview

- Transfer CFT provides a ****node manager**** per host that monitors every node and checks that its nodes are active. If a node goes down, the node manager detects the inactivity and takes over that node's activity.
- In addition to the node manager Transfer CFT provides a ****connection dispatcher**** that listens on the Transfer CFT server port, and monitors incoming traffic.
- For multiple nodes to be able to access the same files, using the same set configuration, the system requires the use of a shared file system. The shared disk provides communication, configuration, partners, data flows, internal datafiles and nodes.
- Incoming flow passes by a load balancer. Note that the exiting flow does not pass through an external load balancer.

Architectural component details
-------------------------------

### Copilot

Copilot operates three services: the node manager, the connection dispatcher, the UI server. Only one Copilot runs per host.

#### Node manager

The node manager monitors all nodes that are part of the Transfer CFT multi-node environment (independent of the host). The monitoring mechanism is based on locks provided by the file system lock manager.

When a node is not running correctly, the node manager tries to start it locally.

#### Connection dispatcher

The connection dispatcher checks for inbound connections, and dispatches requests to each node.

On startup, the Transfer CFT nodes connect to the connection dispatcher service and register their TCP interface/port pairs. The service listens to these access points and passes incoming connections to the various Transfer CFT nodes.

The connection dispatcher service allows running multiple nodes, which listen at the same access points (interface/port pairs) on the same host.

You can set the dispatcher to use either a round-robin load balancing, or define a one-to-one relationship between a partner and a node. A one-to-one relationship ensures that for any given partner the transfers are kept in the correct chronological order.

### Run-time files

All runtime data are stored on a shared file system.

The following internal datafiles are shared between nodes:

- Parameter internal datafile (CFTPARM)
- Partners internal datafile (CFTPART)
- PKI base (CFTPKU)
- Main communication media file (CFTCOM)
- Unified Configuration (UCONF)

The following internal datafiles are node specific, and the filename is flagged by the node identifier:

- Catalog (cftcata00, cftcata01,...) located in &lt;cft_runtime_dir&gt;/data
- Communication media file (cftcom00, cftcom01,...) located in &lt;cft_runtime_dir&gt;/data
- Log files (cftlog00, cftlog01,...) located in &lt;cft_runtime_dir&gt;/log
- Output file (cft00.out, cft01.out,...) located in &lt;cft_runtime_dir&gt;/run
- Account file (cftaccnt00, cftaccnt01,...) located in &lt;cft_runtime_dir&gt;/accnt

Node recovery overview
----------------------

There are two possibilities when a node manager detects a node failure:

- If it is a local node fail over, the node manager automatically attempts to restart the node on the local server.
- If it is a remote node fail over, the node manager waits for the remote node manager to restart the node. If the remote node manager does not restart the node  before the timeout, the local node manager will restart the node on the local server.

After the node is restarted, whether local or remote, it will complete all transfer requests that were active when the failure occurred.

After a fail over, manual intervention is required to rebalance the cluster.

When a node manager fails but nodes are still active, after a timeout nodes are stopped and restarted on another server. A manual intervention is required to restore the node manager.

Transfer recovery overview
--------------------------

When a node receives an incoming request - a transfer receive, restart, acknowledgement or negative acknowledgement - if the corresponding transfer record cannot be found in the node's own catalog, the node requests the transfer record from other nodes through the CFTPRX task.

Possible scenarios include:

- If another node has the catalog record, the node retrieves it and perform the transfer.
- If no nodes have the record, an error is returned.
- If any one of the nodes does not respond, the requesting node continues to retry all nodes until the session's timeout. Once the timeout is reached, the node ends the connection. After this, the remote partner retries the request according to its retry parameters.

In the case of node failure during the transfer recovery process, the catalog record is locked in both catalogs until both nodes are available for recovery.

### Limitations and restrictions

The synchronous communication media is not available in multi-node environment. If a synchronous communication media is enabled when the multi-node architecture is enabled, the Transfer CFT will not be able to start. Additionally note the following restrictions:

- Transfer acceleration is not available in server mode.
- Bandwidth control is calculated by node.
- Accounting statistics are generated by node.
- Nodes are not automatically re-balanced when a host is recovered after a failure.
- Duplicate file detection is not supported.
