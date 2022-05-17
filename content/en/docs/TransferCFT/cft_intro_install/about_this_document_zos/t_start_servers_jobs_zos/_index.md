---
    title: "Start and stop the server JOBs"
    linkTitle: "Post-installation"
    weight: 170
---This section presents JCL examples that you can use to create the JOBs necessary to run Transfer CFT. All of the JCLs are located in the **target.INSTALL** library.

- [Starting Transfer CFT JOB CFTMAIN](#Starting%20the%20CFTMAIN%20example)
- [Transfer CFT user interface server commands](#Transfer%20CFT%20user%20interface%20server)

> **Note**
>
> If you installed Transfer CFT along with Central Governance, the uconf copilot.misc.cftstart.enable is automatically set to Yes. This allows Central Governance to control stopping and starting your Transfer CFT.

<span id="Starting the CFTMAIN example"></span>

## Start Transfer CFT 

The CFTMAIN JOB is an example of a JCL to start Transfer CFT. Beginning with the CFTMAIN sample, you can create JOBs to meet your operating requirements.

You can perform Transfer CFT commands using the CFTUTIL utility, the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface, or the console interface.

Start the CFTMAIN JCL in the target.INSTALL library.

> **Note**
>
> CFTMAIN must be APF authorized to start if the UCONF cft.mvs.monitor.check_apf variable is set to Yes. Otherwise, the Transfer CFT log displays CFTI01F CFT error CFT is not APF-authorized.

## Stop Transfer CFT 

The following are commands that you can use to stop Transfer CFT outside of a customized JCL (such as the delivered sample JCL, ******CFTSTOP******, in the target.INSTALL library).

**Normal stop**

```
/F <Jobname>,SHUT FAST=NO *or* SHUT FAST=YES
```

**Quick stop**

Enter the operator command:

```
/P < Transfer CFT
Jobname>
```

\- or -

```
/F < Transfer CFT
Jobname>,SHUT FAST=YES
```

**Force** {{< TransferCFT/axwayvariablesComponentShortName  >}} **shut down**

```
/F < Transfer CFT
Jobname>,SHUT FAST=KILL
```

### Restart

The following command restarts Transfer CFT outside of a customized JCL. Enter the operator command:

```
/F Jobname,SHUT RESTART=YES
```

### Status

Use the CFTPING in the target.INSTALL library to ping your {{< TransferCFT/axwayvariablesComponentShortName  >}}.

<span id="Transfer CFT user interface server"></span>

## Transfer CFT Copilot server commands

The Transfer CFT Copilot server is a sub component that is mandatory when using {{< TransferCFT/PrimaryCGorUM  >}}. Additionally, this server may function as the node manager when using multi-node.

### Starting the Copilot server

COPRUN is an example of a JCL statement that starts the Transfer CFT Copilot server. The server can be started as a Start Task. The Transfer CFT Copilot server STEPLIB, and then JOBLIB should be defined as an APF. If it is not defined as an APF, no RACF check can be performed. This results in no log-on check being available and all requests are done with the user associated with the server JOB.

When the `copilot.misc.CreateProcessAsUser` variable is set, STEPLIB or JOBLIB can be non-APF. Only a {{< TransferCFT/suitevariablesCentralGovernanceName  >}}/PassPort user can sign on to Copilot user interface.

> **Note**
>
> When the ‘cft.mvs.copilot.check_apf’ uconf variable is set to ‘Yes’, CFTCOPL must be APF authorized to start.

LOG message: `+CFTI42E Copilot must be APF-authorized.`

> **Note**
>
> CFTCOPL must be APF authorized to start if the UCONF cft.mvs.copilot.check_apf variable is set to Yes. Otherwise, the Transfer CFT log displays CFTI42E Copilot must be APF-authorized.

### Stopping user interface (Copilot) server

COPSTOP is an example of the JCL stop statement for the Transfer CFT UI server. You can also stop the Transfer CFT UI server using the operator command pause (/P jobname) for the server-associated task.

### Checking the Copilot server status

You can use COPSTATU, for example, as the JCL statement to display the Transfer CFT Copilot server status in the current LPAR.

## Register with {{< TransferCFT/PrimaryCGorUM  >}}

If you intend to implement {{< TransferCFT/PrimaryCGorUM  >}}, please refer to the {{< TransferCFT/axwayvariablesComponentLongName  >}} *User's Guide &gt; [*Register with* {{< TransferCFT/PrimaryCGorUM  >}}](https://docs.axway.com/bundle/TransferCFT_36_UsersGuide_allOS_en_HTML5/page/Content/cft_installation/migrate/register_CG.htm)* page for registration details.
