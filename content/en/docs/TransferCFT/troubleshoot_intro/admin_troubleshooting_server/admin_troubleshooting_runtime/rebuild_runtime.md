---
title: "Rebuild the runtime"
linkTitle: "Rebuild the runtime"
weight: 370
---This section describes how to rebuild the unified configuration environment. You may need to use this process after accidentally erasing critical files, to deal with corrupted files, or after a disk failure.

<span id="Create_regenerate_runtime_uconf"></span>

## Create or regenerate runtime uconf environment

**UNIX and Windows only**

******UNIX syntax******

`cftruntime [-h&#124;--help&#124;-p&#124;--profile&#124;-n <name>&#124;--uconf&#124;--inst] <cft-install-dir> <cft-runtime-dir> [--mac=yes&#124;no]`

Where:

* -p --profile : Regenerates the profile file
* -n &lt;name> : Regenerates the profile file with &lt;name> of your choice
* --uconf: Regenerates the uconf file.
* -h --help: Displays this help.
* --runtime: Creates a new runtime environment.
* --inst: Creates an initial runtime environment, only when run by the Synchrony Installer.

******Example******

In a UNIX environment, regenerate the uconf settings as follows:

```
cftruntime --uconf /home/Transfer_CFT/home /home/Transfer_CFT/runtime
```

******Windows syntax******

`cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name>&#124;-uconf&#124;-inst]`

Where:

* cft-install-dir: Is the full Transfer CFT install path and must exist.
* cft-runtime-dir: Is the full Transfer CFT runtime path and does not exist.

Usage:

* -profile: Creates a new profile.bat and backs up the old one.
* -name: Creates a new profile with the &lt;name> of your choice.
* -uconf: Regenerates the uconf file.
* -inst: Creates the initial runtime environment, which is used exclusively by the Installer.

> **Note**
>
> You must use double quotes when indicating a path that contains spaces.

******Example 1******

In a Windows environment, create a new runtime called `runtime2`:

```
cftruntime c:\\AxwayCFT38\\Transfer_CFT\\home c:\\AxwayCFT36\\Transfer_CFT\\runtime2
```

******Example 2******

In a Windows environment, regenerate the `cftuconf.dat` uconf settings as follows:

```
cftruntime c:\\AxwayCFT38\\Transfer_CFT\\home c:\\AxwayCFT36\\Transfer_CFT\\runtime –uconf
```

## How to disable UCONF integrity control

To disable integrity control for the UCONF dictionary, add the `-mac=no` parameter to the `cftruntime `command either when creating the runtime environment or regenerating the uconf file:

Unix syntax:

* `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> --uconf --mac=no`
* `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> --inst --mac=no`

Windows syntax:

* `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> -uconf -mac=no`
* `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> -inst -mac=no`
