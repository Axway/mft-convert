---
    "title": "Using system users - UNIX",
    "linkTitle": "Using system users",
    "weight": "250"
---
This section describes two optional UNIX-specific tasks that you can perform to enable system user authentication and file system rights, and which are not mutually exclusive.

- [Enable the file user rights (USERCTRL)](#Manually)
    -   [Transfer CFT starts the CFTSU process](#Automati2)
    -   [User starts the CFTSUD process](#Manually2)
- [How to use system user authentication for the user interfaces](#Enable2)

<span id="Manually"></span>

Enable the file user rights (USERCTRL)
--------------------------------------

To enable file system rights in Unix, configure one of the following two scenarios to execute using the either the CFTSU or the CFTSUD process. See also, [USERCTRL](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/userctrl). Reminder, on UNIX due to case sensitivity CFTSU is not the same as the cftsu file, which is dedicated to the Transfer CFT Copilot server.

<span id="Automati2"></span>

### Transfer CFT starts the CFTSU process 

Perform the following steps so that when {{< TransferCFT/axwayvariablesComponentShortName  >}} starts the CFTSU process is automatically started.

1. Set the CFTPARM USERCTRL option to ****YES**** to enable.
1. Check that the UCONF `cft.unix.cftsu.isservice` parameter is set to `NO (default)`.
1. Log on as root.
1. Go to the Transfer CFT runtime and execute the profile.
1. Copy the CFTSU executable to a directory that is outside of the $CFTDIRINSTALL directory, for example `/opt/cft/`:  
    cp $CFTDIRINSTALL/bin/CFTSU /opt/cft/CFTSU  
    Ensure that the destination directory is outside of the $`CFTDIRINSTALL `directory to allow automatic updating when you apply a Transfer CFT service pack or update.
1. Set the UCONF `cft.unix.cftsu.fname` parameter to the directory used above, for example `/opt/cft/CFTSU`.  
    ```
    CFTUTIL uconfset id=cft.unix.cftsu.fname,value=/opt/cft/CFTSU
    ```
1. Execute the commands:  
    chown root:root /opt/cft/CFTSU  
    chmod u+s /opt/cft/CFTSU
1. Copy the cftsu_setup executable to the `$CFTDIRRUNTIME/bin` directory:  
    cp $CFTDIRINSTALL/bin/cftsu_setup $CFTDIRRUNTIME/bin/cftsu_setup
1. Execute the commands:  
    chown root:root $CFTDIRRUNTIME/bin/cftsu_setup  
    chmod u+s $CFTDIRRUNTIME/bin/cftsu_setup  
    The` cftsu_setup` is a necessary step as it is required if at a later date you want to update or upgrade.

> **Note**
>
> Note: After starting Transfer CFT, with a user other than root, the message CFTI34I PID=13653 /opt/cft/CFTSU Task started successfully (MQID=None) confirms that the CFTSU steps above were successful.

### CFTSU procedures for updates and upgrades

You do not need to repeat the ****Automatically start CFTSU process**** steps above when applying a Transfer CFT update after having performed this step at least once previously.

However, if you execute a version upgrade:

- If you were using the CFTSU in the `$CFTDIRINSTALL/bin` directory, change to use the owner who installed Transfer CFT.
- Move the CFTSU to a directory outside of the `$CFTDIRINSTALL/bin` directory.
- Apply the upgrade.
- Execute the ****Start CFTSU process**** steps above.

<span id="Manually2"></span>

### User starts the CFTSUD process

In this scenario, the root user must manually start the CFTSUD process as a service before starting {{< TransferCFT/axwayvariablesComponentShortName  >}}.

1. Set the CFTPARM USERCTRL option to ****YES**** (enabled).
1. Before starting {{< TransferCFT/axwayvariablesComponentShortName  >}}, set the uconf parameter `cft.unix.cftsu.isservice `to `yes`.
1. Set the uconf parameter to `cft.unix.cftsu.afunix=$(cft.runtime_dir)/run/SCFTSU`.

#### CFTSUD rights

Give special rights to the CFTSUD executable as follows:

1. Log on as root.
1. Execute following command:

    `chown root:root $CFTDIRINSTALL/bin/CFTSUD`

#### Start CFTSUD

The root user now starts the CFTSUD process and sets the AF_UNIX file owner, which you defined in `cft.unix.cftsu.afunix`. This results in {{< TransferCFT/axwayvariablesComponentShortName  >}} connecting via the CFTSUD process when started.

1. Log on as root.
1. Go to $CFTDIRRUNTIME directory.
1. Execute the {{< TransferCFT/axwayvariablesComponentShortName  >}} profile:

    `    . ./profile`

1. Start CFTSUD:

    `CFTSUD`

1. Set the AF_UNIX file owner as the user that starts {{< TransferCFT/axwayvariablesComponentShortName  >}}:

    `chown <cftuser>:<cftuser> <afunix_file> `

<span id="Enable2"></span>

How to use system user authentication for the user interfaces
-------------------------------------------------------------

There are two ways to enable the system user authentication:

- For the current web-based Transfer CFT UI: set `copilot.restapi.authentication_method=system`
- For the deprecated Transfer CFT UI (Copilot): set `copilot.misc.createprocessasuser=yes`

> **Note**
>
> Note: Note that the copilot.misc.createprocessasuser parameter is set to NO for Unix systems by default.

Proceed as follows:

1. Log on as root.
1. Execute the profile.
1. Copy the `cftsu` executable to the following directory, where you must first create the `cft `sub-folder:  
    cp $CFTDIRINSTALL/bin/cftsu /opt/cft/cftsu  
    The destination directory, for example `/opt/cft/`, must be outside of the $CFTDIRINSTALL directory to allow automatic updating when apply a SP or product update.
1. Execute the commands:  
    chown root:root /opt/cft/cftsu  
    chmod u+s /opt/cft/cftsu
1. Set the `copilot.unix.cftsu.fname` parameter to `/opt/cft/cftsu`, for example:  
    `CFTUTIL uconfset id=copilot.unix.cftsu.fname, value=/opt/cft/cftsu`
1. Copy the `cftsu_setup `executable to the` runtime_dir/bin` directory:  
    cp $CFTDIRINSTALL/bin/cftsu_setup $CFTDIRRUNTIME/bin/cftsu_setup
1. Execute the commands:  
    chown root:root $CFTDIRRUNTIME/bin/cftsu_setup  
    chmod u+s $CFTDIRRUNTIME/bin/cftsu_setup  
    The cftsu_setup is a necessary step to perform once, as it is required if at a later date you want to update or upgrade.

> **Note**
>
> Note: Once you perform the following procedure once, there is no need to repeat these steps when applying a Transfer CFT update (they are done automatically).
