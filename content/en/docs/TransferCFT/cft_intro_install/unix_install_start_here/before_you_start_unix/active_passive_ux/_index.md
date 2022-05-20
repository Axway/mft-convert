---
title: "Active/passive installation - Unix"
linkTitle: "Active/passive installation"
weight: 180
---This section describes how to install an active/passive architecture, as described in [About Multi-node architecture.](../../../../about_multinode)

## Prerequisites

Transfer CFT in multi-host architecture requires:

* A shared file system
* You must configure the system prior to the multi-node installation, and the shared disk should be ready when you start the Transfer CFT Copilot server. See [Shared file system prerequisites](../../../windows_install_start_here/before_you_start_win/n_active_active/shared_file_prereq_win) for details.

### License keys

{{< TransferCFT/axwayvariablesComponentShortName  >}} in multi-node architecture requires a shared file system for use of a multi-node architecture on several hosts (active/active). Additionally, the system must be configured prior to the multi-node installation and the shared disk ready when starting the Copilot server.

> **Note**
>
> See Shared file system prerequisites for details.

You can use a single key for a multi-node installation, as either:

* The hostname must not be defined for the key, or
* The hostname defined for the key matches the hostname of one of the hosts that composes the multi-node instance

Additionally, the key must have the cluster option.

### Download and uncompress

Download and unzip the {{< TransferCFT/suitevariablesTransferCFTName  >}} install package, as described in [Install Transfer CFT](../).

### Customize

Create as many copies of the initialize.properties file as you have hosts in the installation. Customize the *n* initialize.properties file with the following parameters.


| CFT_Full_Hostname  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address.<br/> When you re installing a cluster, there are two ways to define this parameter:<br/> • If you do not set this in the silent file, the installation determines it (if the machine is correctly configured)<br/><br/> • Set the FQDN for each machine in the cluster, that is, for each host installation |
| --- | --- |
| Runtimedir  | The runtime directory must be in a shared directory.  |
| LoadBalancer_Host  | Specify the host address of the load balancer, which is the cluster's public IP address in an active/passive deployment.<br/> <blockquote> **Note**<br/> The load balancer is used to connect to the Transfer CFT Copilot server.<br/> </blockquote>  |
| LoadBalancer_Port  | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server.  |


## Install

1. Start the installation.
1. Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_win-x86-64_BNXXXXXXXX.exe
1. ./Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_&lt;OS>_&lt;BN>.run
1. In the Installation Architecture screen, select **Cluster - first host**.
1. Complete the installation.
1. To add a host to create a multi-host installation, run the install `exe/bat` again. This time select **Cluster - Additional host**.

## Silent installation

When installing using a silent file for a multihost installation, in the silent file (initialize.properties) you can use the same other definitions as in the Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} silent files.

 
