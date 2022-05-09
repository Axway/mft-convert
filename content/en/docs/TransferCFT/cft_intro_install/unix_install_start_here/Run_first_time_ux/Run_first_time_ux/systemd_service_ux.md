{
    "title": "Configure systemd services",
    "linkTitle": "Configure systemd services",
    "weight": "210"
}This section describes the steps you must perform to start and stop, or update, Transfer CFT from {{< TransferCFT/PrimaryCGorUM  >}} with Linux `systemd `services defined.

> **Note**
>
> Note: To perform the authorization procedures described in this page, you may want to refer to the sudoers documentation that corresponds with your Linux operating system.

Start the service using Central Governance or Copilot
-----------------------------------------------------

Perform the following steps to configure the Transfer CFT `systemd `service to start via {{< TransferCFT/PrimaryCGorUM  >}} or Copilot, where:

- The {{< TransferCFT/axwayvariablesComponentLongName  >}} systemd service is called: **`cft`**
- The command to start this service is: `sudo systemctl start cft`

1. Authorize the {{< TransferCFT/axwayvariablesComponentLongName  >}} user to start the service without requiring a password. On Ubuntu/Debian, use the `visudo `command to add a file called `cft `to `/etc/sudoers.d/`:
1. Then add the following line:
1. Create a script in the `$CFTDIRRUNTIME/conf` directory, for example called `copcftstart`, as follows:
1. Set the UCONF `copilot.misc.cftstart` parameter to `conf/copcftstart`.

Update using Central Governance with systemd services defined
-------------------------------------------------------------

Configure support for the `systemd `service to apply updates via Central Governance, where:

- The systemd service created for {{< TransferCFT/axwayvariablesComponentLongName  >}} is called: `cft`
- The systemd service created for Copilot is called: `cftcopilot`

Authorize the user that operates {{< TransferCFT/axwayvariablesComponentLongName  >}} to start and stop both services without requiring a password.

On Ubuntu/Debian, use the `visudo `command to add a file called `cft `to `/etc/sudoers.d/`:

```
sudo visudo -f /etc/sudoers.d/cft
```

Add the following lines:

```
<USERNAME> ALL=NOPASSWD:/bin/systemctl start cft
<USERNAME> ALL=NOPASSWD:/bin/systemctl stop cft
<USERNAME> ALL=NOPASSWD:/bin/systemctl start cftcopilot
<USERNAME> ALL=NOPASSWD:/bin/systemctl stop cftcopilot
```

Create a script in the `$CFTDIRRUNTIME/conf` directory for each command as follows:

1. Define a `copcftstart `script that starts the Transfer CFT service:

```
\#!/bin/sh
sudo systemctl start cft
rm $0
```

1. Define a `copcftstop `script that stops the {{< TransferCFT/axwayvariablesComponentLongName  >}} service:

```
\#!/bin/sh
sudo systemctl stop cft
rm $0
```

1. Define a `copcopilotstart `script that starts the Copilot service:

```
\#!/bin/sh
sudo systemctl start cftcopilot
rm $0
```

1. Define a `copcopilotstop `script that stops the Copilot service:

```
\#!/bin/sh
sudo systemctl stop cftcopilot
rm $0
```

Set the following UCONF parameters, which reference the scripts to start and stop both services.

```
uconfset id=copilot.misc.cftstart, value=conf/copcftstart
uconfset id=copilot.misc.cftstop, value=conf/copcftstop
uconfset id=copilot.misc.copstart, value=conf/copcopilotstart
uconfset id=copilot.misc.copstop, value=conf/copcopilotstop
```
