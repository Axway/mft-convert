{
    "title": "Start and stop the server JOBs",
    "linkTitle": "Post-installation",
    "weight": "170"
}This section presents JCL examples that you can use to create the JOBs necessary to run Transfer CFT. All of the JCLs are located in the **target.INSTALL** library.

- [Starting Transfer CFT JOB CFTMAIN](#Starting%20the%20CFTMAIN%20example)
- [Transfer CFT user interface server commands](#Transfer%20CFT%20user%20interface%20server)

> **Note**
>
> Note: If you installed Transfer CFT along with Central Governance, the uconf copilot.misc.cftstart.enable is automatically set to Yes. This allows Central Governance to control stopping and starting your Transfer CFT.

<span id="Starting the CFTMAIN example"></span>

Start Transfer CFT 
-------------------

The CFTMAIN JOB is an example of a JCL to start Transfer CFT. Beginning with the CFTMAIN sample, you can create JOBs to meet your operating requirements.

You can perform Transfer CFT commands using the CFTUTIL utility, the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface, or the console interface.

{{% TransferCFT/snippets/zos_start%}}

> **Note**
>
> Note: CFTMAIN must be APF authorized to start if the UCONF cft.mvs.monitor.check_apf variable is set to Yes. Otherwise, the Transfer CFT log displays CFTI01F CFT error CFT is not APF-authorized.

Stop Transfer CFT 
------------------

{{% TransferCFT/snippets/zos_stop%}}

### Restart

{{% TransferCFT/snippets/zos_restart%}}

### Status

Use the CFTPING in the target.INSTALL library to ping your {{< TransferCFT/axwayvariablesComponentShortName  >}}.

<span id="Transfer CFT user interface server"></span>

Transfer CFT Copilot server commands
------------------------------------

The Transfer CFT Copilot server is a sub component that is mandatory when using {{< TransferCFT/PrimaryCGorUM  >}}. Additionally, this server may function as the node manager when using multi-node.

### Starting the Copilot server

{{% TransferCFT/snippets/zos_copilot_start%}}

### Stopping user interface (Copilot) server

{{% TransferCFT/snippets/zos_copilot_stop%}}

### Checking the Copilot server status

You can use COPSTATU, for example, as the JCL statement to display the Transfer CFT Copilot server status in the current LPAR.

{{% TransferCFT/snippets/register_w_cft%}}
