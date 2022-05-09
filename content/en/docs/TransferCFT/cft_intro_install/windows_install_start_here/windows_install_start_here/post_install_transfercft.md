{
    "title": "Post installation",
    "linkTitle": "Post installation",
    "weight": "190"
}After installing Transfer CFT, but before starting Transfer CFT and the Copilot server, you may need to perform the following tasks:

- [Perform an update](#updates)
- [Create a basic configuration](#Creating_a_basic_configuration)

Otherwise you can go to:

- [Start the Transfer CFT Copilot server](#Start)
- [Start Transfer CFT](#Start2)

For information on user rights, refer to the topic [Defining user rights UNIX](../../../unix_install_start_here/run_first_time_ux/run_first_time_ux/user_rights_and_interface_unix).

Installed directories and files
-------------------------------

{{% TransferCFT/snippets/home_dir_caution%}}
{{% TransferCFT/snippets/installer_files%}}

Standalone installations
------------------------

If you are not using {{< TransferCFT/suitevariablesTransferCFTName  >}} with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must provide a certificate in order to be able to use the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI. See [Using the web-based browser UI](../../../../c_intro_userinterfaces/web_copilot_ui#Connect2) page for details.

<span id="updates"></span>

Perform an update
-----------------

If you need to apply service packs or patches, refer to Update {{< TransferCFT/axwayvariablesComponentShortName  >}}.

If you have already started Transfer CFT or Copilot, stop these servers prior to performing an update.

<span id="Creating_a_basic_configuration"></span>

Create a basic configuration
----------------------------

This section describes how to create a basic Transfer CFT configuration
if you did not do so during the installation process. If you started Transfer CFT or Copilot, stop these servers before modifying the configuration.

### Update the profile

To add environment variables to your Transfer CFT profile
edit the following script:

- Windows: ` <CFTDIRRUNTIME>\profile.bat`
- UNIX: `./<CFTDIRRUNTIME>/profile`

### UCONF

To determine the Transfer CFT variable values list the values using
the command:

```
CFTUTIL listuconf
```

To change or update a value, start the Transfer CFT profile
and make modifications using either the Transfer CFT Copilot UI or command line UCONF tools. Refer to Transfer
CFT online
documentation for details.

For example, to change the user interface port:

```
CFTUTIL
uconfset id=copilot.general.serverport,value=1766
```

### Configuration

Before you can start Transfer CFT for the first time, Transfer CFT must
have a basic configuration. Typically this is created during installation or migration.

### License key

If you did not enter the license key during installation, you can enter it post installation in the cft.key file located in: `<CFTDIRRUNTIME>/conf/`

You can enter a single key or a list of keys in this file. In the configuration default
file, the variable `<$CFTKEY>` represents the `cft.key` file.

### {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafile and configuration

Check the values, especially the key value, hostname, and port for TCP,
in the following file:

- Windows and UNIX: `<CFTRUNTIME>/conf/cft-tcp.conf  `

Start the Transfer CFT profile and, to create the Transfer CFT
internal datafile and update the basic configuration, execute:

```
cftinit
cft-tcp.conf
```

To update the configuration at a later date, execute:

```
cftupdate
cft-tcp.conf
```

### User interface configuration

To view the Copilot user interface configuration, execute:

```
CFTUTIL
LISTUCONF id=copilot\*
```

To change this configuration, you update the hostname and listening
port for Transfer CFT UI using CFTUTIL
`uconfset`.

******Example******

```
CFTUTIL
uconfset id=copilot.general.serverhost, value="127.0.0.1"CFTUTIL
uconfset id=copilot.general.serverport, value="7000"
```

### Configuration for Service Mode

This option is only available on Windows systems and must be selected during the installation process configuration.

<span id="Start"></span>

Start the Transfer CFT Copilot server
-------------------------------------

If you have implemented {{< TransferCFT/PrimaryCGorUM  >}}, starting Copilot launches the registration process. For more information, see the topic [Registration overview](../../../../governance_services_intro/cg_register_overview).

******Windows******

1. Change directory to the runtime.
1. Execute profile.bat.
1. To start the Copilot server, enter: copstart
1. To check the Copilot status, enter: copstatus -v

******UNIX******

```
cd <CFTDIRRUNTIME>
. . /profile
copstart
http://myHostName:1766
```
<span id="Start2"></span>

Start Transfer CFT
------------------

If you have implemented {{< TransferCFT/PrimaryCGorUM  >}}, you can start and stop Transfer CFT via the Central {{< TransferCFT/suitevariablesGovernance  >}} user interface. Otherwise perform the following command from the Transfer CFT runtime directory.

Enter:

{{% TransferCFT/snippets/start_transfer_cft%}}
<span id="Service"></span>

Service mode set up when using {{< TransferCFT/PrimaryCGorUM  >}}
----------------------------------------------------------------------

Since you can start or restart a registered {{< TransferCFT/axwayvariablesComponentLongName  >}} from {{< TransferCFT/PrimaryCGorUM  >}}, when using service mode you must set the **CFT UI service** account so that it can start the **CFT service**. Therefore, perform one of the following two tasks:

- Add the account to the CFT server's **Local Administrator Group**, or
- Provide the rights to start the Transfer CFT Service as a non Local Admin user:
    -   Use a Sysinternals utility such as Process Explorer to change Windows service permissions. Run Process Explorer as administrator and locate the CFT service (CFTSRVLD.exe) process.
    -   Open the process properties and click the **Services** tab. Click the **Permissions** button and add the user or group in the opened window. Then select the permissions that you want to assign (Full Control) and click **OK** to save.
    -   ![](/Images/TransferCFT/services_rights.png)

{{% TransferCFT/snippets/register_w_cft%}}
