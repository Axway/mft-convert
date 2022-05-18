---
title: "Troubleshooting installations and upgrades"
linkTitle: "Troubleshoot installations and upgrades"
weight: 160
--- This page describes how to locate the various {{< TransferCFT/suitevariablesTransferCFTName  >}} installation logs when troubleshooting, and the files you made want to have ready if you need to contact Axway Support.

If an issue occurs while performing an installation or an upgrade, check for errors in the following places. Perform these steps before attempting to roll back in the case of an upgrade.

1. If you performed an upgrade from a version prior to v3.6, look for errors in the `install.log` located in the `axway.installer` directory.
1. Check for errors in the `install.log` in the `CFT` directory (if this is a new installation or you upgraded from a version that did not use the Axway installer).
1. Navigate to the runtime directory and check the `.up `folder contents.
1. Again from the runtime directory, check the `copupd `folder contents. This folder is only available after performing a {{< TransferCFT/suitevariablesTransferCFTName >}} update or upgrade when using {{< TransferCFT/suitevariablesCentralGovernanceName >}} or {{< TransferCFT/suitevariablesFlowManager >}}. In the `copupd `folder, navigate to the` log > install` file.
1. Be ready to supply all of these files to Axway support if you cannot troubleshoot the issue with any errors found in the logs.

## Transfer CFT Copilot server issues

### Copilot doesn't start

- Check that the port is not already used by another application.
- Close all active sessions, use the syntax: `copstop - f`
- Check that there are no orphan "`cop*`" processes. If there are, manually kill these processes.

{{< TransferCFT/axwayvariablesComponentShortName  >}} server

### Cannot start my {{< TransferCFT/axwayvariablesComponentShortName  >}}

- Check your {{< TransferCFT/axwayvariablesComponentShortName >}} log in Central Governance.
- From the local {{< TransferCFT/axwayvariablesComponentShortName >}} runtime, try to manually start the server. If you cannot manually start the server, refer to *[Support tools](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Troubleshooting/support_tools.htm)* in the {{< TransferCFT/axwayvariablesComponentLongName >}}{{< TransferCFT/suitevariablesDocTypeUser >}}.

### Runtime directory error

If for whatever reason an installation that uses symbolic links fails, once the silent files have been corrected, you must delete the {{< TransferCFT/suitevariablesTransferCFTName  >}} home installation directory to which the symbolic link points. Failing to do so causes the installer to go into upgrade mode.

Additionally, you cannot perform an installation in a directory if the runtime already exists.

### Problem with post- install step

If you get a **Warning***Problem running post- install step* message at the end of an installation, check the installation log file. The message `ERR: Unable to decrypt the password` indicates that there is an issue related to the PasswordsEncryptionKey.

1. In the `initialize.properties` file, locate all encrypted passwords and enter the passwords in clear text, then ensure that the `PasswordsEncryptionKey` field is empty.
1. Save the file.
1. Remove the files created by the unsuccessful installation.
1. Repeat the installation procedure.

## Multi- node multihost installation issues

### Second node or host fails to install

*UNIX*

While performing a multihost multi- node installation, where the user has a different UserId (UD) and GroupId (GID) for each host, you successfully install the first node on the first host, which creates the runtime on the shared disk. But the second node or host installation fails with the message:

Warning: The directory

/mnt/CFT36MNLIN_BN12807000/runtime

is not writable by the current user

To resolve this issue, using a different user perform the following commands on all hosts that are involved in the multihost, multi- node installation:

1. Open an SSH session on the machine and run the following command to change the UID, for example:  
    `sudo usermod - u 1005 ithomas`  
    Where `1005 `is the desired common UID, and `ithomas `is the user common to all of the UNIX hosts.
1. Run the command to change the GID, for example:  
    `sudo groupmod - g 1006 ithomas`  
    Where `1006 `is the desired common GID, and `ithomas `is the group to which the user `ithomas `belongs.

For more information, please refer to the [NFS](http://nfs.sourceforge.net/nfs- howto/ar01s07.html#pemission_issues) documentation.

## Installer does not run

When the /tmp directory and the user's homedir have the noexec option, InstallBuilder does not work:

EX:

Transfer_CFT_3.6_Install_linux- x86- 64_BN12807000.run - - mode unattended - - conf- file /opt/xip/cft36/initialize.properties

Unable to initialize installer.

The possible workarounds are therefore:

remove the noexec on one of the partitions

run the .run with a user who has the necessary rights (ex: sudo)

Create a temporary / home on an unprotected disk

sudo mkdir / instcft

sudo chown &lt;user>: &lt;user> / instcft

export HOME = / instcft => unprotected disk

./Transfer_CFT_3.7_Install_linux- x86- 64_BN13015241.run - - mode unattended

Solution 3 should work for the client as long as the HOME points to a directory where it has execution rig
