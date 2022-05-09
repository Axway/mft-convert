{
    "title": "How to enable system users - Windows",
    "linkTitle": "Using system users ",
    "weight": "240"
}This section describes two optional Windows-specific tasks that you can perform to enable system user authentication and file system rights, and which are not mutually exclusive.

- [Enable the file user rights (USERCTRL)](#Enable3)
- [How to use system user authentication for the user interfaces](#How)

You can only use the **user authentication for Copilot** when implementing the deprecated Java based Copilot UI or Web Services. This sort of authentication does not apply to the current Transfer CFT UI or to REST API usage.

<span id="Enable3"></span>

Enable the file user rights (USERCTRL)
--------------------------------------

When implementing file user rights with [USERCTRL](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/userctrl), you must run Transfer CFT as a service or start {{< TransferCFT/suitevariablesTransferCFTName  >}} with administrator privileges.

> **Note**
>
> Caution  
> When USERCTRL=YES, access to UNC or mapped file drives (as opposed to local files) are performed by the user who started Transfer CFT and not the owner of the transfer.

The Windows user who is going to perform transfers must have read and write rights for the files to be transferred.

Some user rights must be assigned to the user who launched the {{< TransferCFT/axwayvariablesComponentShortName  >}} server to permit other Windows users to perform transfers.

To assign user rights:

- Adjust memory quotas for a process
- Impersonate a client after authentication (only on Windows 2008)
- Replace a process level token
- Create a token object

To define user rights:

1. In a dos command window, enter `lusrmgr.msc `to open the system users list. Check available users.
1. In a dos command window, enter `secpol.msc` to open the Local Security Policy window.
1. Select ****Security Settings**** &gt; ****Local Policies**** &gt; ****User Rights Assignment****.
1. Double-click the required right.
1. Click ****Add user or group**** and define.
1. Close and re-open the Windows session to take into account the modifications.

<span id="How"></span>

How to use system user authentication for the user interfaces
-------------------------------------------------------------

There are two ways to enable the system user authentication:

- For the current web-based Transfer CFT UI: set `copilot.restapi.authentication_method=system`
- For the deprecated Transfer CFT UI (Copilot): set `copilot.misc.createprocessasuser=yes`

> **Note**
>
> Note: Note that the copilot.misc.createprocessasuser parameter is set to Yes for Windows systems by default.

When enabling user authentication for Copilot, you must run the Transfer Copilot server as a service. The following information applies except if you are using the local system account when working in service mode.

### Prerequisites

The user rights to assign are:

- Adjust memory quotas for a process
- Impersonate a client after authentication (only on Windows 2008)
- Replace a process level token
- Create a token object

### Define user rights

1. In a dos command window, enter `lusrmgr.msc` to open the system users list. Check available users.
1. In a dos command window, enter `secpol.msc` to open the Local Security Policy window.
1. Select ****Security Settings**** &gt; ****Local Policies**** &gt; ****User Rights Assignment****.
1. Double-click the required right.
1. Click ****Add user or group**** and define.
1. Close and re-open the Windows session to take into account the modifications.

Additionally, the user who wants to log on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server must exist both in the Windows system and {{< TransferCFT/suitevariablesCentralGovernanceName  >}} (or PassPort AM). The Windows system performs the user authentication, and {{< TransferCFT/suitevariablesCentralGovernanceName  >}} (or PassPort AM) checks the other rights.

> **Note**
>
> Note: If using Central Governance, the user name is case-sensitive.
