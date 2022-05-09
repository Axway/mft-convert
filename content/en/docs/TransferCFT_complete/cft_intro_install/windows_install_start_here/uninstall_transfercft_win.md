{
    "title": "Uninstall ",
    "linkTitle": "Uninstall",
    "weight": "150"
}This page describes how to uninstall Transfer CFT. When you uninstall a Transfer CFT, you lose the complete Transfer CFT
configuration. To avoid this, save your environment (samples, exits, etc.) before removing the Transfer CFT.

Modes
-----

****Interactive****

`uninstall.exe`

****Silent****

`uninstall.exe --mode unattended`

Procedure
---------

1. Stop the server you want to uninstall.
1. Enter the appropriate command from the path of the Transfer CFT to uninstall. For example, for the interactive mode on a Windows system: ` C:\axway\my_cft\uninstall.exe`
1. A message displays asking if you want to uninstall; click **Yes** to continue.  
    You are reminded to check that all Transfer CFT processes are stopped.
1. A message displays asking if you want to delete the runtime. Selecting **Yes** removes the runtime folder and all of its contents. If you are in a cluster installation, or want to keep information stored in the runtime, select **No**.
1. Click **Next** to uninstall, and **Finish** to exit.

### About uninstalling in Windows

The same user that did the initial installation (or at least the same type of user) must start the uninstall procedure.

### Services modification

Some products support an installation in service mode with a user other than the default (Local System Account).

If the domain field is not shown in the products service configuration dialog, then it must be introduced in the username field, using this format:

```
<domain>\\<username>
```

If it is a local user (a user that was created on the local machine) then the &lt;domain&gt; field can be . or the &lt;hostname&gt;.

****Example****

Local user: user1

`.\user1`

`<hostname>\user1`

Network user: user2

`<domain_name>\user2`
