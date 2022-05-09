{
    "title": "Defining user rights Windows",
    "linkTitle": "Defining user rights ",
    "weight": "230"
}Before you can start {{< TransferCFT/axwayvariablesComponentShortName  >}} from the {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot server, the Copilot server must be started. Additionally you will need rights to log on to this server. The overall process requires that you:

- [Define rights before starting the {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot server](#Define_rights_before_starting_Transfer_CFT)
- [Define rights before starting {{< TransferCFT/axwayvariablesComponentShortName  >}}](#Define_rights_before_starting_Transfer_CFT)
- [Define a domain user](#Define%20domain%20user)
- [Define folder rights](#Define)
- [Define system user access](#Define2)

To start the {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot server you must give each Windows user read/write rights for the {{< TransferCFT/axwayvariablesComponentShortName  >}} installation folder.

You can opt to control the file-access permissions and the batch execution environment by setting the UCONF copilot.misc.createprocessasuser identifier as follows:

- no: Any user who logs on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server will have their processes identified as the user who started the {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot server.

<!-- -->

- yes: Any user who logs on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server will have their processes identified as their own.


| copilot.misc.createprocessasuser  | PassPort AM<br/> status | Rights to define  |
| --- | --- | --- |
|  <br/> no | Not activated  | No need to set rights. All Windows users can log on to the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server.  |
| no | Activated  | If PassPort AM is activated, you must use a PassPort AM user to log on. Check if the AM user has the rights to manage {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| yes  | Not activated  | <span id="Define user rights in Windows"></span>The Windows user who is going to log on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server, must have read and write rights for {{< TransferCFT/axwayvariablesComponentShortName  >}} install folder.<br/> Some user rights must be assigned to the user who launched the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server to permit other Windows users to log on. This is true except if it is the local system account when working in the service mode. The user rights to assign are:<br/> • Adjust memory quotas for a process<br/> • Impersonate a client after authentication (only on Windows 2008)<br/> • Replace a process level token<br/> • Create a token object<br/> To define user rights:<br/> • In a dos command window, enter <code>lusrmgr.msc</code> to open the system users list. Check available users.<br/> • In a dos command window, enter <code>secpol.msc</code> to open the Local Security Policy window.<br/> • Select ****Security Settings &gt; Local Policies &gt; User Rights Assignment****.<br/> • Double-click the required right.<br/> • Click ****Add user or group**** and define.<br/> • Close and re-open the Windows session to take into account the modifications. |
| yes  | Activated  | Some user rights must be assigned to the user who starts the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server to allow other Windows users to log on, unless it is the local system account working in service mode. The user rights are:<br/> • Adjust memory quotas for a process<br/> • Impersonate a client after authentication (only on Windows 2008)<br/> • Replace a process level token<br/> • Create a token object<br/> • In a dos command window, type <code>lusrmgr.msc</code> to open the system users list. Check available users.<br/> • In a dos command window, type <code>secpol.msc</code> to open the Local Security Policy window.<br/> • Select ****Security Settings &gt; Local Policies &gt; User Rights Assignment****.<br/> • Double-click the required right.<br/> • Click Add user or group and define.<br/> • Close and re-open the Windows session to take into account the modifications.<br/> Additionally, the user who wants to log on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server must exist both in the Windows system and PassPort AM. The Windows system performs the user authentication, and PassPort AM checks the other rights.<br/> <blockquote> **Note**<br/> The PassPort user name is case-sensitive.<br/> </blockquote>  |


<span id="Define_rights_before_starting_Transfer_CFT"></span>

Define folder rights before starting {{< TransferCFT/axwayvariablesComponentShortName  >}}
-----------------------------------------------------------------------------------------------

The Windows user who is going to start the {{< TransferCFT/axwayvariablesComponentShortName  >}} server requires read/write rights on the {{< TransferCFT/axwayvariablesComponentShortName  >}} installation folder and read/write/modify rights on the runtime folder.

> **Note**
>
> Note: If you are using either Central Governance or Flow Manager, the user name must exist in PassPort AM and have rights to manage Transfer CFT. Remember that the PassPort user name is case-sensitive.

<span id="Define domain user"></span>

Define domain user
------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} supports domain user accounts, which allows a service to use Windows service security features.

### File action executed for applicative users

To enable for file actions, check that the USERID user has access to the transfer destination directory. To do this, copy the rights from the user's rights table and create a token object.

### Post-transfer procedure executed for applicative users

To enable for post-transfer procedures, check that the USERID user has rights to execute end-of-transfer procedures. To do this, copy the rights from the user's rights table and create a token object.

<span id="Define"></span>

Define folder rights
--------------------

To be able to start the {{< TransferCFT/axwayvariablesComponentShortName  >}} server and the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI, you must give each user read and write rights for {{< TransferCFT/axwayvariablesComponentShortName  >}}.

### Service mode login

If you are working in service mode, you must have Log on as a service authority.

1. In a dos command window, type secpol.msc to open the Local Security Policy window.
1. Select Security Settings &gt; Local Policies &gt; User Rights Assignment.
1. Double-click Log on as a service.
1. Click Add user or group and define.

<span id="Define2"></span>

Define system user access
-------------------------

### System user enabled

If copilot.misc.createprocessasuser=yes in UCONF , or Createprocessasuser=yes in [MISC], the user starting the {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot server must do the following tasks to allow other users to log on. Additionally, those users must exist in the Windows system users list.

- Adjust memory quotas for a process
- Simulate a client after authentication (only on Windows 2008)
- Replace a process level token

### Procedure

1. In a dos command window, type lusrmgr.msc to open the system users list. Check available users.
1. In a dos command window, type secpol.msc to open the Local Security Policy window.
1. Select Security Settings &gt; Local Policies &gt; User Rights Assignment.
1. Double-click the required right.
1. Click Add user or group and define.

PassPort AM is activated

If PassPort AM is active (am.type=PassPort in UCONF), the user must exist both in the Windows system users list and PassPort AM users list. The Windows system user performs the authentication, and PassPort AM performs the other rights checks.

> > **Note**
> >
> > Note: The PassPort user name is case-sensitive.

### System user deactivated with PassPort AM management

If copilot.misc.createprocessasuser=no in UCONF, all system users have the right to log on.

- PassPort AM is activated
- If PassPort AM is active (am.type=PassPort in [UCONF](../../../../../admin_intro/uconf/uconf_parameters)), you must be a defined PassPort AM user to log on.

> **Note**
>
> Note: The PassPort user name is case-sensitive.

****Related topics****

[About PassPort AM](../../../../../internal_a_m_start_here/about_passport_am)

[UCONF parameters](../../../../../admin_intro/uconf/uconf_parameters)
