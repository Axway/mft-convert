{
    "title": "Running Transfer CFT for the first time UNIX",
    "linkTitle": "Post-installation",
    "weight": "150"
}The elements and tasks required to
start {{< TransferCFT/axwayvariablesComponentShortName  >}} for the first time include:

- [Set the environment](#Set)
- [Start and stopping Transfer
    CFT](#Configuring_CFT_)
- [Start or stop via a user interface](#Start2)

<span id="Set"></span>

Set the environment
-------------------

After installing {{< TransferCFT/axwayvariablesComponentShortName  >}}
, but before starting {{< TransferCFT/axwayvariablesComponentShortName  >}} you should:

- Execute the `profile` in the {{< TransferCFT/axwayvariablesComponentShortName  >}} runtime directory to define environment
    variables. Run: `‘. ./profile’`
- Create a new set of Transfer
    CFT working files, parameters, partners, catalog, communication file, logs,
    use the sample configuration files cft-tcp.conf and cft-tcp-part.conf in the `runtime/conf `directory. You can configure these during the product installation, or manually after installation.
- Use `cftinit <configuration_file>` &gt; and/or `cftupdate` to interpret the parameter and
    partner files.  
    ```
    cftinit conf/cft-tcp.conf
    cftupdate conf/cft-tcp-part.conf
    ```
      
    or  
    ```
    cftinit conf/cft-tcp.conf conf/cft-tcp-part.conf
    ```

> **Note**
>
> Caution  
> These commands generate an initial configuration by creating the configuration files. Any previous configurations, and any data in the communication file, catalog, or log files will be lost.

****Sample file details****

- `cft-tcp.conf`: Contains PARM object definitions (PARM, CAT, COM, LOG, ACCNT, PROT, SEND, RECV,...etc.)
- `cft-tcp-part.conf`: Contains partner definitions (CFTPART, CFTTCP, CFTSSL)

Delivered partners are:

- PARIS - NEW YORK
- LOOP
- LOOPSSL0

Start and stop commands
-----------------------

The following table lists the commands according to {{< TransferCFT/axwayvariablesComponentLongName  >}} version.


| Version 2.7.1 and higher  | Version 2.7.0 and lower  |
| --- | --- |
| cft start  | cftstart  |
| cft stop  | cftstop  |
| cft status  | cftstatus  |
| cft force-stop  | cftstop -kill  |
| cft force-stop –kill  | cftstop -forcedkill  |


> **Note**
>
> Note: The cftstart and cftstop commands, from version 2.7.0 and earlier, are redirected to the standardized commands for continued compatibility.

<span id="Configuring_CFT_"></span>

### Start up

You can start Transfer CFT with the `cft start `utility; see also Transfer CFT Management Utilities.

<span id="Shut"></span>

### Shut down

You can use one of the following methods to shut down Transfer CFT:

- The `CFTUTIL `utility
- The `cft ` utility

For more information, see the administrative commands in [Manage the Transfer CFT server](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/administration/start_stop_cft.htm).

<span id="Start2"></span>

Start or stop via a user interface
----------------------------------

You can also use either [Central Governance](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/CentralGov/operations/t_startCFT.htm) or the Transfer CFT to start or shut down {{< TransferCFT/headerfootervariableshflongproductname  >}}.
