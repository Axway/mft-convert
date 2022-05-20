---
title: "Update Transfer CFT"
linkTitle: "Update Transfer CFT"
weight: 170
---This section describes how to update Transfer CFT with a patch or service pack. You can manually perform the operation or use {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

## Download the update file

Download product updates from the [Axway support website](https://support.axway.com/) to the machine you where you want to perform the update. Please note that the update file is a zip file. Do not unzip this file.

## Impacted directories when updating a product

When you install a service pack, the contents of the home directory are updated, but the runtime directory remains untouched. This is so that your customizations, such as APIs, are not overwritten.

## Use {{< TransferCFT/PrimaryCGorUM  >}} for updates

You can easily perform {{< TransferCFT/suitevariablesTransferCFTName  >}} updates using {{< TransferCFT/suitevariablesCentralGovernanceName  >}} as of Transfer CFT 3.1.3. Refer to the Central Governance documentation for details.

Limitations:

- You cannot remove SP or patches via the {{< TransferCFT/suitevariablesCentralGovernanceName >}} interface.
- You cannot update {{< TransferCFT/axwayvariablesComponentLongName >}}s installed in multi-node/multi-hosts from {{< TransferCFT/PrimaryCGorUM >}}.

## Windows users

The same user that did the initial installation (or at least the same type of user) must start the update procedure. Additionally, if you install a service pack or patch for the installer, make sure all Windows services created by the installer are stopped.

#### Services modification

Some products support an installation in service mode with a user other than the default (Local System Account).

If the domain field is not shown in the products service configuration dialog, then it must be introduced in the username field, using this format:

` <domain>\<username>`

If it is a local user (a user that was created on the local machine) then the &lt;domain> field can be . or the &lt;hostname>.

****Example****

```
Local user: user1
.\\user1
<hostname>\\user1
Network user: user2
<domain_name>\\user2
```
<span id="Install"></span>

## Install a standard update

Stop {{< TransferCFT/axwayvariablesComponentShortName  >}} prior to installing a service pack or patch.

### Update in silent mode

Use the following command to update Transfer CFT in silent mode:

```
C:\\axway\\Transfer_CFT_ 3.10
_<Install\\SP\\Patch>_<OS>_<BN>.exe --mode unattended --installdir <installation_directory>
```

## Uninstall an update

This section describes uninstalling a patch or service pack.

To uninstall install the previous patch or service pack. For example, to remove Transfer CFT 3.4 SP2, from the Transfer CFT 3.4 SP1 kit, run the installation pointing to the Transfer CFT 3.4 SP2 installation directory. The installer detects and replaces the SP2 content, impacting only the `home `directory.

**Example**

```
C:\\axway\\Transfer_CFT_ 3.10
_SP1_<OS>_<BN>.exe --mode unattended
```

To verify, from the Transfer CFT &lt;runtime_dir> run the `about `command.

## Install patches and service packs in a multi-node, multi-host environment

This section describes the procedure to apply a patch or service pack on a multi-node architecture based on *N* hosts. You update a Transfer CFT multi-node architecture with multi-hosts using the same procedure as for a patch or service pack, one host at a time.

> **Note**
>
> Transfer CFT clusters can still run while performing an update.

1. Connect to the first host.
1. Stop all nodes running on this host by running the command: `copstop`  
    Copilot services are stopped, and local nodes are automatically re-started on the other hosts.
1. Check that the nodes are re-started by using the command: `CFTUTIL listnode`
1. Install the patch or the service pack as usual using {{< TransferCFT/suitevariablesTransferCFTName >}} installer as described in [Install a standard update](#Install).
1. Start Copilot services.
1. Connect to the next host and repeat the procedure starting as of ****Step 2**** (above).
