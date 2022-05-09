{
    "title": "Active/passive installation - Windows",
    "linkTitle": "Active/passive installation",
    "weight": "190"
}This section describes how to install an active/passive architecture, as described in [About Multi-node architecture.](../../../../about_multinode)

> **Note**
>
> Note: Transfer CFT supports all POSIX file systems. Active/passive shared disks must be POSIX compliant!

A cluster installation of Transfer CFT without multi-node is an active/passive installation as described below:

- Install Transfer CFT using the Cluster installation architecture of the Installer and disable the multi-node architecture.
- Installation procedure must be executed on each host:
    -   The first host installation
    -   Additional hosts installation
- Transfer CFT binaries are installed on several hosts and runtime files are installed on a shared file system.
- Only runtime files are shared.
- You must configure the cluster after installation but before you can use the cluster. The procedure for cluster configuration varies depending on the platform on which the cluster is installed.
- At any given time:
    -   Only one host is active
    -   Only one Transfer CFT runtime environment is running on the active host

> **Note**
>
> Note: After installing applications in active/passive mode, you must implement the cft start, cft stop, and cft status scripts for the cluster.

****Shared Directory****

This is the path and name of the directory where you want to create a shared directory for the cluster installation. The shared directory is used to store product data files.

*Windows only* - When installing a Windows multi-host Transfer CFT architecture, we recommend that you use UNC notation, which defines the path to a shared folder using the format` \\server\sharename.`

****Installation Directory****

The path and name of the local directory where you want to install the first cluster.

Prerequisites
-------------

### Multi-node license keys

{{< TransferCFT/axwayvariablesComponentShortName  >}} in multi-node architecture requires a shared file system for use of a multi-node architecture on several hosts (active/active). Additionally, the system must be configured prior to the multi-node installation and the shared disk ready when starting the Copilot server.

> **Note**
>
> Note: See Shared file system prerequisites for details.

You can use a single key for a multi-node installation, as either:

- The hostname must not be defined for the key, or
- The hostname defined for the key matches the hostname of one of the hosts that composes the multi-node instance

Additionally, the key must have the cluster option.

### Download and uncompress

Download and unzip the {{< TransferCFT/suitevariablesTransferCFTName  >}} install package, as described in [Install Transfer CFT](../../../unix_install_start_here/before_you_start_unix).

### Customize

Create as many copies of the initialize.properties file as you have hosts in the installation. Customize the *n* initialize.properties file with the following parameters.


| CFT_Full_Hostname  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address.<br/> When you re installing a cluster, there are two ways to define this parameter:<br/> • If you do not set this in the silent file, the installation determines it (if the machine is correctly configured)<br/><br/> • Set the FQDN for each machine in the cluster, that is, for each host installation |
| --- | --- |
| Runtimedir  | The runtime directory must be in a shared directory.  |
| LoadBalancer_Host  | Specify the host address of the load balancer, which is the cluster's public IP address in an active/passive deployment.<br/> <blockquote> **Note**<br/> Note: The load balancer is used to connect to the Transfer CFT Copilot server.<br/> </blockquote>  |
| LoadBalancer_Port  | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server.  |


Install
-------

1. Start the installation.
1. Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber  >}}_Install_win-x86-64_BNXXXXXXXX.exe
1. ./Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber  >}}_Install_&lt;OS&gt;_&lt;BN&gt;.run
1. In the Installation Architecture screen, select **Cluster - first host**.
1. Complete the installation.
1. To add a host to create a multi-host installation, run the install `exe/bat` again. This time select **Cluster - Additional host**.

Silent installation
-------------------

This section describes the differences when installing using a silent file for a multi-host installation.

In the silent file (initialize.properties), you can use the same definitions as in the Transfer CFT 3.3.2 silent files.

Integrate Transfer CFT in a Microsoft Cluster Service (MSCS)
------------------------------------------------------------

### Prerequisites

Transfer CFT must be installed in service mode.

### Define Transfer CFT as a Generic Service Resource

Transfer CFT is a cluster-unaware application. However, you can integrate Transfer CFT in a cluster environment as a Generic Service Resource. Use the Microsoft **High Availability Wizard** to create a **Generic Service**, and from the **Select Service** dialog box select the Transfer CFT service.

### License key

The Transfer CFT license key refers to a specific machine, and is based on the machine's hostname. To allow Transfer CFT to start on both cluster nodes, you need one license key per node. Enter the two license keys in the `%CFTKEY%` file located on the shared disk, one key per line.
