---
title: "Active/active installation - Windows"
linkTitle: "Active/active installation"
weight: 180
---This section describes how to install active/active failover, as described in [About Multi-node architecture.](../../../../about_multinode)

## Prerequisites

When installing a Transfer CFT multi-node architecture in Windows, the user performing the installation must:

- Be a domain user who is part of the Administrators group.
- Be the same user for all machines.
- Have all rights (create/modify/delete) to the shared disk on all machines when {{< TransferCFT/axwayvariablesComponentLongName >}} is installed in a multi-host architecture.

> **Note**
>
> You must configure the system prior to the multi-node installation, and the shared disk should be ready when you start the Transfer CFT Copilot server. See Shared file system prerequisites for details.

## Overview

A cluster installation of Transfer CFT with multi-node (HA):

- Install Transfer CFT and enable the multi-node architecture.
- Installation procedure must be executed on each host:
    -   The first host installation sets the Shared directory, the &lt;Transfer_CFT Shared> directory and all of the Transfer CFT configurations.
    -   During each hosts installation (meaning additional nodes), you are prompted to specify the shared directory.
- Transfer CFT binaries are installed on several hosts and runtime files are installed on a shared file system.
- Only runtime files are shared.
- At any given time:
    -   One or several hosts are active.
    -   All Transfer CFT runtime environments (Transfer CFT nodes) are running.

> **Note**
>
> Transfer CFT binaries can be patched on each host one after the other without stopping the Transfer CFT instance (all of the Transfer CFT nodes).

****Shared directory****

This is the path and name of the directory where you want to create a shared directory for the cluster installation. The shared directory is used to store product data files.

*Windows only* - When installing a Windows multi-host Transfer CFT architecture, we recommend that you use UNC notation, which defines the path to a shared folder using the format` \\server\sharename.`

> **Note**
>
> Transfer CFT supports all POSIX file systems.

****Installation directory****

The path and name of the local directory where you want to install the first cluster.

## Before you start

### License keys

{{< TransferCFT/axwayvariablesComponentShortName  >}} in multi-node architecture requires a shared file system for use of a multi-node architecture on several hosts (active/active). Additionally, the system must be configured prior to the multi-node installation and the shared disk ready when starting the Copilot server.

> **Note**
>
> See Shared file system prerequisites for details.

You can use a single key for a multi-node installation, as either:

- The hostname must not be defined for the key, or
- The hostname defined for the key matches the hostname of one of the hosts that composes the multi-node instance

Additionally, the key must have the cluster option.

### Download and uncompress

Download and unzip the {{< TransferCFT/suitevariablesTransferCFTName  >}} install package, as described in [Install Transfer CFT](../../../unix_install_start_here/before_you_start_unix).

### Customize

Create as many copies of the initialize.properties file as you have hosts in the multi-node installation. Customize the *N* initialize.properties file with the following parameters to enable a multi-node installation.


| CFT_Full_Hostname  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address.<br/> When you re installing a cluster, there are two ways to define this parameter:<br/> • If you do not set this in the silent file, the install determines it (if the machine is correctly configured)<br/><br/> • Set the FQDN for each machine in the cluster, that is, for each host installation |
| --- | --- |
| Runtimedir  | The runtime directory must be in a shared directory.  |
| Multinode_Enable  | Enable the multi-node architecture.<br/> To use a multi-node architecture, you must define the multi-node option in the initialize.properties file. |
| Multinode_Number  | Enter the number of nodes.  |
| LoadBalancer_Host  | Specify the host address of the load balancer.<br/> When using an ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. |
| LoadBalancer_Port  | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server. When using ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. |


## Installation overview

1. Start the installation.
1. Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_win-x86-64_BNXXXXXXXX.exe/bat
1. ./Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_&lt;OS>_&lt;BN>.run
1. In the Installation Architecture screen, select **Cluster - first host**.
1. Complete the installation.
1. To add a host to create a multi-host, multi-node installation, run the install `exe/bat` again. This time select **Cluster - Additional host**.

After installation, but before you can use the cluster installation, you must configure the high-availability operations. The procedure for cluster configuration varies depending on the platform on which the cluster is installed.

## Silent installation

This section describes the differences when installing using a silent file for a multi-node installation.

If the silent file architecture is either first_host or additional_host, there are two important parameters:

- multinode_hostname
- multinode_host_address

By default, these 2 attributes are automatically completed if they are not specified in the silent file.

During the runtime creation and for each host, the multinode_hostname is used as the basis for the cft.multi_node.hostnames, where cft.multi_node.hostnames=Cluster_short_hostname1, Cluster_short_hostname2, Cluster_short_hostname3, Cluster_short_hostname4. Using the same logic, the multinode_host_addresss is used to create each _ cft.multi_node.hostnames.\*\*\*\*.host uconf attribute.

In the silent file (initialize.properties), you can use the same other definitions as in the Transfer CFT 3.3.2 silent files.

## Commands

See the [Multi-node commands and management](../../../../about_multinode/multi_node_commands) section for details on using {{< TransferCFT/suitevariablesTransferCFTName  >}} commands and cluster management.

## Troubleshooting

Please see [Troubleshoot multi-node](../../../../troubleshoot_intro/admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_multinode).
