---
title: "Multi-node architecture"
linkTitle: "Multi-node architecture"
weight: 160
---This topic describes the Transfer CFT multi-node feature, which provides you with horizontal scalability and high availability for failovers. See the <a href="" class="MCTextPopup popup popupHead">Active/active You configure two or more Transfer CFTs, up to four nodes, each having an independent workload. These nodes will then run as individual nodes until a fail over occurs.
When a fail over occurs, one of the other nodes takes over from the failed node. This secondary node offers services to new and existing transfer requests and tasks, until the original node resumes activity.
When the failed node restarts, a manual intervention required in Transfer CFT, it resumes its connections from the secondary or replacement node.
To summarize, when a fail over occurs the external Transfer CFT is oblivious to any change. The requests are directed to the sysplex distributor, which resubmits any uncommitted transactions to the replacement node. During fail back, the connection returns to the original node.</a> description for conceptual information.

Multi-node benefits for {{< TransferCFT/axwayvariablesComponentShortName  >}} include:

* Horizontal scalability: increased transfer flow capacity.
* High availability: active/active including patch management on an active cluster, and automatic node restart after fail over.
* Traffic management: load balancing between nodes using sysplex with VIPA distribution methods.

## Before you start

Check that the user that starts Copilot and Transfer CFT has:

* Read/write rights for the Transfer CFT instance files
* An OMVS segment and a TSO segment in their profile
* Rights to issue MODIFY and START console commands

****TSO version****

The user ID for the user that starts Copilot and Transfer CFT has a 7-characters maximum when using z/OS 2.2 or lower. Therefore, a user with an 8-character user ID cannot use TSO with a z/OS version lower than v2.3.

<span id="Prerequi"></span>

## Prerequisites

Transfer CFT in multi-node architecture can use a single key for a multi-node installation (which is stored in the PDS member: UPARM(PRODKEY)) as either:

* The hostname must not be defined for the key, or
* The hostname defined for the key matches the hostname of one of the hosts that composes the multi-node instance

Additionally, the key must have the cluster option.

* A shared file system for use of a multi-node architecture on several hosts. Additionally, the system must be configured prior to the multi-node installation and the shared disk ready when starting the Copilot server.
* The Transfer CFT configuration files (PARM, PART, COM, CAT) and services files (LOG, ACCNT) must implement SMS (system-managed storage).
* The SHARECAT parameter in the SGINSTAL macro (INSTALL(A12OPTS) and INSTALL(A12OPTSP) members) must be set to ‘NO’ or ‘INACT’.

## Architecture

The Transfer CFT multi-node architecture is based on hosts, nodes, and a shared file system. Regardless of the number of servers hosting the nodes from outside the cluster, the nodes are viewed as a single machine on a network.

In a z/OS environment, the multi-node architecture uses SYSPLEX components that include data sharing and a VIPA SYSPLEX distributor, along with LPAR (logical partition) resources.

**High level architectural overview**

![](/Images/TransferCFT/temp_lpar_cluster.png)

**What is a host?**

In a z/OS environment, the host is an LPAR located in a SYSPLEX distributor configuration.

**What is a node?**

A node is a Transfer CFT runtime running on a host. Multiple nodes are called a Transfer CFT cluster, which can run on one or multiple hosts. A server can host zero or multiple nodes. That said, Transfer CFT itself does not manage the round robin or other load distribution mechanisms.

**What is a shared file system?**

A shared file system is a file system resource that where data is shared by several nodes and/or hosts.

The shared file system for {{< TransferCFT/axwayvariablesComponentShortName  >}} z/OS is a DASD (direct-access storage device) in a SYSPLEX environment.

## Concepts

* Transfer CFT provides one **node manager** per host that monitors every node and checks that its nodes are active. If a node goes down, the node manager detects the inactivity and takes over that node's activity.
* For multiple nodes to be able to access the same files, using the same set configuration, the system requires the use of a shared file system. The shared disk provides communication, configuration, partners, data flows, databases and nodes. In the following illustration, you can see that the shared data includes parameter files and configuration settings.
* Incoming flow passes by a SYSPLEX distributor. However, *exiting* flow does not pass through the SYSPLEX distributor.
* In z/OS architecture, you use VIPA commands to perform all configuration tasks.

**Incoming flow from external Transfer CFT partner**

![](/Images/TransferCFT/zos_SysplexDistributor.png)

****Legend****

* The Transfer CFT z/OS SYSPLEX distributor receives an incoming call from an external Transfer CFT partner.
* The SYSPLEX distributor uses either the round robin or the weight active distribution method.
* The SYSPLEX distributor dispatches the incoming connection to one of the LPAR (host).

## Service descriptions

### Copilot

Copilot operates two services in z/OS, the node manager and the UI server. Only one Copilot runs per host.

#### Node manager

The node manager monitors all nodes that are part of the Transfer CFT multi-node environment (independent of the host). The monitoring mechanism is based on locks provided by the resource enqueuing system.

Typically, when a node is not running correctly, the node manager tries to start it locally.

> **Note**
>
> Copilot can be registered to ARM (A12OPTS ARM=YES), but the Transfer CFTs cannot. If a host fails, the ARM will not restart that host's Copilot on another host.

### PORT statement (connection dispatcher)

The PORT statement with the SHAREPORT or SHAREPORTWLM option (Communications Server IP Configuration) allows you to run multiple nodes that listen at the same access points (interface/port pairs) on the same host.

* SHAREPORT: If you specify SHAREPORT when reserving a shared port for multiple listeners, TCP/IP allows multiple listeners to listen on the same combination of port and interface.
* SHAREPORTWLM: If you specify this parameter, it allows connections to be distributed based on WLM recommendations.

See the IBM documentation for complete details: <http://pic.dhe.ibm.com/infocenter/zos/v1r13/index.jsp>

### SYSPLEX distributor

This service dispatches an incoming connection to one of the hosts (LPAR) either using one of many methods, for example a round robin or weighted-active method.

### CFTCOM dispatcher

For outgoing calls, you can set the CFTCOM dispatcher to use either a round robin load balancing, or define a one-to-one relationship between a partner and a node. A one-to-one relationship ensures that for any given partner the transfers are kept in the correct chronological order. In the unified configuration, set the variable: **`cft.multi_node.cftcom.dispatcher_policy`**

* Round robin: **`round_robin` (default)**
* One-to-one:**` node_affinity`**

### Runtime files

All runtime data are stored on a shared file system.

The following databases are shared between nodes:

* Parameter internal datafile (CFTPARM)
* Partners internal datafile (CFTPART)
* PKI base (CFTPKI)
* Main communication media file (CFTCOM)
* Unified Configuration (UCONF)

The following databases are node specific, and the filename is flagged by the node identifier:

* Catalog (..CATALOG.N00,..CATALOG.N01,...)
* Communication media file (..COM.N00, ..COM.N01,...)
* Log files (..LOG1.N00, ..LOG2.N00, ..LOG1.N01, ..LOG2.N01, ,...)
* Account file (..ACCNT1.N00, ..ACCNT2.N00, ..ACCNT1.N01, ..ACCNT2.N01 ,...)

> **Note**
>
> When using multi-node architecture, the allocated space in the catalog file is 10% greater than when working in a standalone Transfer CFT.

## Recovery

### Node recovery

There are two possibilities when a node manager detects a node failure:

* If it is a *local* node fail over, the node manager automatically attempts to restart the node on the local server.
* If it is a *remote* node fail over, the node manager waits for the remote node manager to restart the node. If the remote node manager does not restart the node before the timeout, the local node manager restarts the node on the local server.

After the node is restarted, whether local or remote, it completes all transfer requests that were active when the failure occurred.

After a fail over, manual intervention is required to rebalance the cluster.

When a node manager fails but nodes are still active, after a timeout nodes are stopped and restarted on another server. A manual intervention is required to restore the node manager.

See [Rebalancing after a failover](multinode_setup_zos#Rebalanc) for an example.

### Transfer recovery

When a node receives an incoming request, be that a transfer receive, restart, acknowledgement or negative acknowledgement, if the corresponding transfer record cannot be found in the node's own catalog, the node requests the transfer record from other nodes through the CFTPRX task.

Possible scenarios include:

* If another node has the catalog record, the node retrieves it and performs the transfer.
* If no nodes have the record, an error is returned.
* If any one of the nodes does not respond, the requesting node continues to retry all nodes until the session's timeout. Once the timeout is reached, the node ends the connection. After this, the remote partner retries the request according to its retry parameters.

In the case of node failure during the transfer recovery process, the catalog record is locked in both catalogs until both nodes are available for recovery.

## Limitations

Note the following restrictions:

* The only network is TCP/IP.
* The use of the console interface commands can apply only to one specific node.
* Bandwidth control is calculated by node.
* Accounting statistics are generated by node.
* Nodes are not automatically re-balanced when a host is recovered after a failure.
* Duplicate file detection is not supported.
* If the parameter 'ARM' in SGINSTAL is set to 'YES':
    *   The Node Manager (CFTCOPL) will register to ARM.
    *   Transfer CFT (CFTMAIN) will not register to ARM.
* Secure Relay is not supported in a multi-node architecture.
* You can only have one communication media of the type "file".
* The SHARECAT parameter in the SGINSTAL macro (INSTALL(A12OPTS) and INSTALL(A12OPTSP) members) must be set to ‘NO’ or ‘INACT’.
