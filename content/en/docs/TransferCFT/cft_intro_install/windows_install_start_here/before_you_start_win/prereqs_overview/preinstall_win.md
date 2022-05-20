---
title: "Windows-specific prerequisites "
linkTitle: "Windows-specific prerequisites"
weight: 170
---The following are tasks to perform or issues to address before installing Transfer CFT.

## Windows requirements

The Windows installation directory must not contain any sub-folders or files that are owned by another user.

## Hardware and software requirements

Before installing {{< TransferCFT/axwayvariablesComponentShortName  >}} Windows check the following:

* OS version
* Communication
    system type

These selections affect the minimum hardware and software requirements
for the product and may be inter-dependent.

{{< TransferCFT/axwayvariablesComponentShortName  >}} is based on an external network layer, which must be installed before operating the product. And note that if other applications are running at the same time as {{< TransferCFT/axwayvariablesComponentShortName  >}},
the RAM requirement needs to be increased.

## Apply a service pack or patch, or upgrade using Central Governance

To upgrade or install a Transfer CFT Service Pack or patch from Central Governance, you must:

* Run the Transfer CFT Copilot as administrator.
* Disable the Windows User Account Control (UAC).

## Configure Windows UAC

User Account Control (UAC) is an option to add security infrastructure on Windows operating systems.

For Windows versions that support UAC, Windows Vista, Windows Server 2008, Windows 7, Windows Server 2008 R2, and Windows 2012, you must disable the UAC when using {{< TransferCFT/PrimaryCGorUM  >}} to apply patches, service packs or upgrades for Transfer CFT.

1. From the **Start** menu, type UAC and click to search.
1. In the User Control Account settings pop-up window, set the slider to **Never Notify**.
1. Click **OK**.
1. Reboot to make the change effective.

For Windows versions prior to the versions listed above, perform the following steps to add yourself in ****Log on as a service group****:

1. Navigate to ****Start**** **&gt;** ****Control Panel**** **&gt;** ****Administrative Tools**** **&gt;** ****Local Security Policy****.
1. From the tree, select ****Local Policies**** **&gt;** ****User Rights Assignment**** **&gt;** ****Log on as a service****.

> **Note**
>
> On some systems the path may be Start &gt; Control Panel &gt; System and Security &gt; Administrative Tools &gt; Local Security Policy.

### Set administration rights if UAC is enabled

If you do **not** disable the UAC, the installer requires administration rights at installation. You must be part of a group of administrators, or have an administrator user account. The installer detects the type of user, and sends the appropriate message:

* Setup with administrator user account: Accept or decline if you want to make changes to your computer.
* Setup with standard user account: Enter your administrator password first to continue.

## Tasks that required elevated rights

The following tasks require that you have elevated rights and fail if one of the above setup options was not performed:

* Create or remove shortcuts in Start menu or desktop
* Create or remove Windows services
* Installing in %SystemRoot% or %ProgramFiles%
* Running product scripts that require elevated rights

<span id="Windows"></span>

## User rights prerequisites

There are certain Windows-specific tasks you must perform to enable system user authentication and file system rights before using Transfer CFT or the Copilot server.

The following sections describe how to:

* [Define rights before starting](../../../windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win#Define_rights_before_starting_Transfer_CFT)
* [Define a domain user](../../../windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win#Define%20domain%20user)
* [Define folder rights](../../../windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win#Define)
* [Define system user access](../../../windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win#Define2)

## Run as administrator

The user who installs or upgrades Transfer CFT must be an administrator on the system where you are performing the installation.

## Service mode prerequisites

Note the following prerequisites and limitations when installing {{< TransferCFT/axwayvariablesComponentLongName  >}} in service mode.

* If you install {{< TransferCFT/axwayvariablesComponentLongName >}} in service mode, to launch the service on a specific account you must grant that user service rights to log in.  
    To grant this right, navigate to **Administrative Tools > Local Security Policy > Local Management > Local Policies**, and select **User Rights Assignment**. Then grant the user **Logon as a Service**.
* If you install {{< TransferCFT/axwayvariablesComponentLongName >}}in service mode but want to launch the service on a Local System Account, be aware that you cannot start Transfer CFT from the Copilot UI.
* See also, [Shared file system prerequisites](../../n_active_active/shared_file_prereq_win).

## Using a mapped drive

### Create a mapped drive

This section describes how to create a mapped drive using the **psexec** tool, which you can use with an active/active cluster or a standalone installation.

To map network shares to a drive accessible to Windows Services, you must log in as the NT AUTHORITY\\SYSTEM account.

1. Download the [](http://technet.microsoft.com/en-us/sysinternals/bb842062.aspx)[Sysinternals Suite from Microsoft](https://technet.microsoft.com/en-us/sysinternals/bb842062.aspx), and unzip it to a directory.
1. Open a command window and start a session as Administrator (Run as administrator).
1. Go to the unzipped directory containing the Sysinternals Suite executable:
1. Log in as the NT AUTHORITY\\SYSTEM account:
1. In the new window, create the persistent mapped drive.
1. Provide the credentials for a user having access to the shared folder.
1. Create a **Startup script** that contains only the command from step 5 (Create the persistent mapped drive), and implement using the instructions in the Microsoft article: [Assign Computer Startup scripts](https://technet.microsoft.com/en-us/library/cc770556(v=ws.11).aspx).

### Remove a mapped drive

To remove a mapped drive:

1. Launch a command prompt as Administrator (run as administrator).
1. Go to the unzipped directory containing the Sysinternals Suite executable with command:
1. Log on as the NT AUTHORITY\\SYSTEM account:
1. In the new window, delete the mapped drive:
