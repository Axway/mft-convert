---

    title: Troubleshoot  installation and registration
    linkTitle: Installation and registration
    weight: 290

---
This section lists some possible post-installation issues along with corresponding corrective actions when applicable. If corrective actions do not remedy the issue, check the **Support tools** section for more information, or contact support at [support.axway.com](https://support.axway.com/).

## Transfer CFT installation

### Installation fails due to InstallBuilder issue

**UNIX systems**

You cannot run the InstallBuilder if the **noexec mount option** is set on both the<span class="code">` /tmp `</span>directory and the user's <span class="code">`homedir`</span>. For example, the installation will fail if you are running unattended mode for a target machine where the user has this mount option set on both directories.

Workarounds include:

- Remove the <span class="code">`noexec `</span>option from one of the partitions.
- Run the<span class="code">` .run`</span> with a user who has the necessary rights, for example <span class="code">`sudo`</span>.
- Create a temporary <span class="code">`/home`</span> directory on an unprotected disk:

```
sudo mkdir /instcft
sudo chown <user>:<user> /instcft
export HOME=/instcft (unprotected disk)
./Transfer_CFT_3.7_Install_linux-x86-64_BN13015241.run --mode unattended
```

## {{< TransferCFT/PrimaryCGorUM  >}}

<span id="Troubles"></span>

### Troubleshoot the registration

If Copilot starts, but the Transfer CFT either does not display in the {{< TransferCFT/PrimaryCGorUM  >}} **Product List** or registers in error:

- Verify the Central Governance IP address (or FQDN) used in the Transfer CFT configuration.
- On the computer running Transfer CFT, check that you can reach Central Governance at the IP address used in the Transfer CFT configuration.
- Check that the Transfer CFT appears in the Central Governance logs. If not, typically this is because the Transfer CFT is unable contact Central Governance.
- In {{< TransferCFT/PrimaryCGorUM >}} check **Administration > Services** to ensure that Central Governance is correctly started.
- Verify the shared secret for Central Governance used in the Transfer CFT configuration.

> **Note**
>
> See the Central Governance documentation for additional information and details.

### Cannot register {{< TransferCFT/suitevariablesTransferCFTName  >}} with {{< TransferCFT/suitevariablesCentralGovernanceName  >}} (error: (1103))

If you have the following error in the Transfer CFT log:

`XFBCOPT S99015 Central Governance: sending self-registration request`

**`XFBCOPT S99015 navigatorNotifSend: Central Governance: sending self-registration request error: (1103)`**

`Handshake failed (1) SSL_ERROR_SSL(1) - error:14090086:SSL routines: ssl3_get_server_certificate:certificate verify failed`

This is a Certificate Signing Request / CSR failure because the {{< TransferCFT/suitevariablesTransferCFTName  >}} truststore does not contain the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} root &gt;CA certificate. Perform the steps described in [If CAs change after Transfer CFT registration](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/CFT/cft_registration/t_change_cft_ca.htm).

### Generic registration issue with Central Governance or Flow Manager

You can use the CFTUTIL CHECK command to validate the coherence of parameters, partners, and the Transfer CFT PKI database.

```
CHECK CONTENT=BRIEF|FULL, FOUT=FileName
```

See also, <a href="../../../c_intro_userinterfaces/about_cftutil/check_command" class="MCXref xref">Use the check command</a>.

### Registration or Copilot connection issue

While trying to troubleshoot a non-working registration, you may find there are not enough http traces to diagnose connection issues.

To have more request details between {{< TransferCFT/suitevariablesCentralGovernanceName  >}} and {{< TransferCFT/suitevariablesTransferCFTName  >}} registration or between heartbeats, set the $\_JAVA\_OPTIONS environment variables as shown below (on UNIX in this example).

1. Requests from {{< TransferCFT/suitevariablesCentralGovernanceName >}} to {{< TransferCFT/suitevariablesTransferCFTName >}} (call to web services):  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.apache.commons.logging.Log=org.apache.commons.logging.impl.SimpleLog"`</span>  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.apache.commons.logging.simplelog.defaultlog=debug"`</span>  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.apache.commons.logging.simplelog.showdatetime=true"`</span>  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.apache.commons.logging.simplelog.log.org.apache.href=debug"`</span>
1. Requests from {{< TransferCFT/suitevariablesTransferCFTName >}} to {{< TransferCFT/suitevariablesCentralGovernanceName >}} (registration /heartbeat):  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.eclipse.jetty.util.log.class=org.eclipse.jetty.util.log.StdErrLog"`</span>  
    <span class="code">`export _JAVA_OPTIONS=$_JAVA_OPTIONS" -Dorg.eclipse.jetty.LEVEL=DEBUG"`</span>
1. Restart Central Governance to activate the changes.
1. Ensure the <span class="bold_in_para">****Transfer CFT Connector****</span> service in {{< TransferCFT/suitevariablesCentralGovernanceName >}} is in debug mode:  
    <span class="code">`cli > serviceLog -l DEBUG -name "Transfer CFT Connector"`</span>
1. Return to info mode:  
    <span class="code">`cli > serviceLog -l INFO -name "Transfer CFT Connector"`</span>
1. Check the logs in:  
    <span class="code">`$CG_HOME/runtime/com.axway.nodes.cftconnector_{un_UUID}/uma/logs/provider.log.*`</span>

### Registration fails after installing in service mode when using a firewall

****Windows only, firewall enabled****

Transfer CFT cannot register in Central Governance when installing Copilot in service mode.

- Preventive measure: Deactivate the firewall to perform the registration.
- Workaround: If you encounter this error, perform the following steps to register:
    1.  Stop the Copilot Windows service.
    2.  Manually start the service in a DOSBOX to register.
    3.  Accept the authorization from the Windows firewall.

### Re-register with {{< TransferCFT/PrimaryCGorUM  >}}

When Central Governance sends the SSL certificates to {{< TransferCFT/axwayvariablesComponentShortName  >}}, the UCONF <span class="code">`cg.registration_id parameter`</span> is set to a positive integer. If an error occurs, the registration process ends in error. To repeat the registration, perform the following steps:

1. Stop {{< TransferCFT/axwayvariablesComponentShortName >}}.
1. Stop Copilot.
1. Set the UCONF <span class="code">`cg.registration_id`</span> to its default value (-1) using the unset command:
1. Start the <span class="axway_variablesComponent Short Name axway_variablesComponent Short Name">Transfer CFT</span> Copilot. Copilot starts the registration process.

******More information******

For more information on {{< TransferCFT/PrimaryCGorUM  >}}, refer to the {{< TransferCFT/PrimaryCGorUM  >}} {{< TransferCFT/PrimaryCGversion  >}} documentation.

All {{< TransferCFT/PrimaryCGorUM  >}} interoperability parameters are configured with uconf value settings. For more information, refer to the topic UCONF {{< TransferCFT/PrimaryCGorUM  >}} parameters.
