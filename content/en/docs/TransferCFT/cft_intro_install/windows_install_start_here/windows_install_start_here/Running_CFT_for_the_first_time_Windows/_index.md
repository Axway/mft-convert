{
    "title": "Running   Transfer CFT for the first time ",
    "linkTitle": "Running Transfer CFT for the first time",
    "weight": "200"
}The elements and tasks required to
start {{< TransferCFT/axwayvariablesComponentShortName  >}} for the first time include:

- [Set the environment](#Operations_to_perform_before_starting_CFT)
- [Start Transfer
    CFT using a command](#Starting_CFT)
- [Shut
    down {{< TransferCFT/axwayvariablesComponentShortName  >}} using a command](#Shutting_down_CFT)
- [Start or stop {{< TransferCFT/headerfootervariableshflongproductname  >}} via a user interface](#Start)
- [Service mode](#Service)
- [Start the CFTW desktop window](#Start2)

<span id="Operations_to_perform_before_starting_CFT"></span>

Set the environment
-------------------

After installing {{< TransferCFT/axwayvariablesComponentShortName  >}}
, but before starting {{< TransferCFT/axwayvariablesComponentShortName  >}} you should:

- Execute the `profile.bat` in the {{< TransferCFT/axwayvariablesComponentShortName  >}} runtime directory to define environment
    variables, or execute `profile.ps1` if you are using Windows PowerShell instead of Batch.
- Create a new set of Transfer
    CFT working files, parameters, partners, catalog, communication file, logs,
    use the sample configuration files cft-tcp.conf and cft-tcp-part.conf in the `runtime\conf` directory. You can configure these during the product installation or manually after installation.
- Use `cftinit <configuration_file>` &gt; and/or `cftupdate` to interpret the parameter and
    partner files.  
    ```
    cftinit conf\\cft-tcp.conf
    cftupdate conf\\cft-tcp-part.conf
    ```
      
    or  
    ```
    cftinit conf\\cft-tcp.conf conf\\cft-tcp-part.conf
    ```

> **Note**
>
> Caution  
> These commands generate an initial configuration by creating the configuration files. Any previous configurations, and any data in the communication file, catalog, or log files will be lost.

****Sample file details****

- `cft-tcp.conf`: Contains PARM object definitions (PARM, CAT, COM, LOG, ACCNT, PROT, SEND, RECV,...etc.).
- `cft-tcp-part.conf`: Contains partner definitions (CFTPART, CFTTCP, CFTSSL).

Delivered partners are:

- PARIS - NEW YORK
- LOOP
- LOOPSSL0

### Start and stop commands


| Commands  |
| --- |
| cft start  |
| cft stop  |
| cft status  |
| cft force-stop  |
| cft force-stop -kill  |


> **Note**
>
> Note: The cftstart and cftstop commands, from version 2.7.0 and earlier, are redirected to the standardized commands for continued compatibility.

<span id="Starting_CFT"></span>

Start {{< TransferCFT/axwayvariablesComponentShortName  >}} using a command
--------------------------------------------------------------------------------

If you have not already done so, from the runtime directory execute the `profile.bat` to set the {{< TransferCFT/axwayvariablesComponentShortName  >}} environment.
Then in the same `dos `session, enter the command: `cft start`

<span id="Shutting_down_CFT"></span>

Shut down {{< TransferCFT/axwayvariablesComponentShortName  >}} using a command
------------------------------------------------------------------------------------

You can use one of the following methods to shut down Transfer CFT:

- `CFTUTIL `utility  
    ```
    CFTUTIL shut fast=no
    *or*
    CFTUTIL shut fast=yes
    ```
- `cft  utility` using stop  
    ```
    cft stop
    ```

For more information, see the administrative commands in [Manage the Transfer CFT server](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/administration/start_stop_cft.htm).

<span id="Start"></span>

Start or stop {{< TransferCFT/headerfootervariableshflongproductname  >}} using a user interface
-----------------------------------------------------------------------------------------------------

You can also use either [Central Governance](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/CentralGov/operations/t_startCFT.htm) or the Transfer CFT Copilot UI to start or shut down {{< TransferCFT/headerfootervariableshflongproductname  >}}.

<span id="Service"></span>

Service mode
------------

You can retroactively install Service mode for {{< TransferCFT/axwayvariablesComponentShortName  >}}. Use the Installer ****Configure**** mode to install and uninstall the services for the {{< TransferCFT/axwayvariablesComponentShortName  >}} Server and {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot. To launch the Installer in **Configure** mode, from the ****Start**** menu select ****Axway Software &gt; Axway {{< TransferCFT/axwayvariablesComponentShortName  >}} &gt; Configure****.

<span id="Start2"></span>

Start the CFTW desktop window
-----------------------------

You can use the Windows utility `cftw.exe` to open a desktop window that displays the {{< TransferCFT/axwayvariablesComponentLongName  >}} log messages and processes list in separate tabs. The `cft start` command automatically launches this cftw.exe utility when the UCONF parameter `cft.nt.start_graphmode` is set to **`Yes `**(default value).

> **Note**
>
> Note: When Transfer CFT is running as a service, you must have the service configured to authorize desktop interaction, and you must manually launch the cftw.exe.

If {{< TransferCFT/axwayvariablesComponentLongName  >}}is not running, use the `-wait `option with cftw so that the utility waits for {{< TransferCFT/axwayvariablesComponentLongName  >}} to start instead of exiting immediately.

```
cftw.exe -w
```

As of {{< TransferCFT/axwayvariablesComponentLongName  >}} v3.0.1, a second cftw UCONF parameter,` cft.nt.cftw_display_log_messages`, is available. To display log messages in the cftw window, change the parameter setting from **No** (default value) to **Yes**.

For more information, see the administrative commands in [Manage the Transfer CFT server](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/administration/start_stop_cft.htm).
