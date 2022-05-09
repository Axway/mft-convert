{
    "title": "Install services in command line",
    "linkTitle": "Install services in command line",
    "weight": "130"
}After adding a Windows service in command line, the default system user is the user that started the service. To define a specific user, you must edit the service properties in the Services page.

> **Note**
>
> Note: If you plan to integrate Transfer CFT with Central Governance and also plan to use Service mode, please refer to the additional instructions in Service mode set up when using Central Governance.

****Windows only****

Install services
----------------

### {{< TransferCFT/axwayvariablesComponentShortName  >}} services

1. To install the {{< TransferCFT/axwayvariablesComponentShortName  >}} service, access the {{< TransferCFT/axwayvariablesComponentShortName  >}} directory:

    `cd %TransferCFT_directory%`

1. Enter the following:

    `cscript /nologo home\bin\cftsrvin.vbs n=CFT36`

    Where n= &lt;CFT plus the current version of {{< TransferCFT/axwayvariablesComponentShortName  >}}&gt;

### Copilot services

From the {{< TransferCFT/axwayvariablesComponentShortName  >}} home directory, run:

`copsrv.exe -install <service_name> <displayname> <cftdirruntime>`

******Example******

For {{< TransferCFT/axwayvariablesComponentShortName  >}} version {{< TransferCFT/axwayvariablesComponentVersion  >}} Copilot you would enter:

`c:\CFT36\Transfer_CFT\home\bin>copsrv.exe -install CFT_Copilot36 CFT_Copilot36 c:\CFT36\Transfer_CFT\runtime`

### Activate services

Using CFTUTIL activate the services for both Transfer CFT and Copilot with the uconf `service `configuration parameters.

****Example****

```
uconfset id=cft.nt.service_name, value=CFT36
uconfset id=cft.nt.service_mode, value=yes
uconfset id=copilot.nt.service_name, value=CFT_Copilot36
uconfset id=copilot.nt.service_mode, value=yes
```
<span id="Service"></span>
