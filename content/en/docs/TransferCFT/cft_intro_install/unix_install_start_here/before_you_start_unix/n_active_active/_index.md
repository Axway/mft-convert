---
title: "Active/active installation - Unix"
linkTitle: "Active/active installation"
weight: 170
--- This section describes how to install active/active failover, as described in [About Multi- node architecture.](../../../../about_multinode)

## Multi- node license keys

{{< TransferCFT/axwayvariablesComponentShortName  >}} in multi- node architecture requires a shared file system for use of a multi- node architecture on several hosts (active/active). Additionally, the system must be configured prior to the multi- node installation and the shared disk ready when starting the Copilot server.

> **Note**
>
> See Shared file system prerequisites for details.

You can use a single key for a multi- node installation, as either:

- The hostname must not be defined for the key, or
- The hostname defined for the key matches the hostname of one of the hosts that composes the multi- node instance

Additionally, the key must have the cluster option.

### Download and uncompress

Download and unzip the {{< TransferCFT/suitevariablesTransferCFTName  >}} install package, as described in [Install Transfer CFT](../).

### Customize

Create as many copies of the initialize.properties file as you have hosts in the multi- node installation. Customize the *N* initialize.properties file with the following parameters to enable a multi- node installation.

| CFT_Full_Hostname  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address.<br/> When you're installing a cluster, there are two ways to define this parameter:<br/> • If you do not set this in the silent file, the install determines it (if the machine is correctly configured)<br/><br/> • Set the FQDN for each machine in the cluster, that is, for each host installation |
| --- | --- |
| Runtimedir  | The runtime directory must be in a shared directory.  |
| Multinode_Enable  | Enable the multi- node architecture.<br/> To use a multi- node architecture, you must define the multi- node option in the initialize.properties file. |
| Multinode_Number  | Enter the number of nodes.  |
| LoadBalancer_Host  | Specify the host address of the load balancer.<br/> When using an ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. |
| LoadBalancer_Port  | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server. When using ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. |

## Install

1. Start the installation.
1. Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_linux- x86- 64_BNXXXXXXXX.exe
1. ./Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_&lt;OS>_&lt;BN>.run
1. In the Installation Architecture screen, select **Cluster - first host**.
1. Complete the installation.
1. To add a host to create a multi- host, multi- node installation, run the install again. This time select **Cluster - Additional host**.

## Silent installation

This section describes the differences when installing using a silent file for a multi- node installation.

If the silent file architecture is either first_host or additional_host, there are two important parameters:

- multinode_hostname
- multinode_host_address

By default, these 2 attributes are automatically completed if they are not specified in the silent file.

During the runtime creation and for each host, the multinode_hostname is used as the basis for the cft.multi_node.hostnames, where cft.multi_node.hostnames=Cluster_short_hostname1, Cluster_short_hostname2, Cluster_short_hostname3, Cluster_short_hostname4. Using the same logic, the multinode_host_addresss is used to create each _ cft.multi_node.hostnames.\*\*\*\*.host uconf attribute.

In the silent file (initialize.properties), you can use the same other definitions as in the Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} silent files.

## Commands

See the [Multi- node commands and management](../../../../about_multinode/multi_node_commands) section for details on using {{< TransferCFT/suitevariablesTransferCFTName  >}} commands and cluster management.

## Troubleshooting

Please see [Troubleshoot multi- node](../../../../troubleshoot_intro/admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_multinode).
