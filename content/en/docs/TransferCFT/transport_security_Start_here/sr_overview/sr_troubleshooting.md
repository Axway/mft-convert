---

    title: Troubleshoot Secure Relay 
    linkTitle: Troubleshoot Secure Relay 
    weight: 280

---
Should you incur an issue, you can begin by checking for information in the following files:

- Secure Relay Master Agent log file: secure\_relay.ma.log\_fname = C:\\cft35\\runtime\\log\\xsrMaster.log
- Secure Relay Router Agent log file: located by default in the &lt;install\_dir>/SecureRelayRA/log/router.log
- Transfer CFT log messages: check for the messages CFTS63F and CFTS64I, which provide information about the SecureRelay status

You can refer to the [Secure Relay documentation](https://docs.axway.com/bundle/SecureRelay_271_AdministratorsGuide_allOS_en_HTML5/page/Content/AxwayStartPageRA_admin.htm) for additional information.

## Check Transfer CFT log messages...

If you find the following messages in the {{< TransferCFT/axwayvariablesComponentLongName  >}} log, you may want to check the **Possible cause**:

****Possible cause: No Router Agent available****

```
CFTS63F Secure Relay fatal error _ Secure Relay register error 2 (Error sending listen request to RA DMZ0: CFTS63F+com.axway.xsr.agent.master.context.router.RouterAgentContextException: MPX channel is currently not available)
```

****Possible cause: Java not set****

```
CFTS63F Secure Relay fatal error _ Java binary file not found:
CFTS63F Secure Relay fatal error _ Please set uconf:cft.jre.java_binary_path parameter
CFTI10F Init error _ failed to start the Secure Relay Master Agent CFTS63F Secure Relay fatal error _ (13) Permission denied (in UNIX environments)
```

****Possible cause: Problem related to secure\_relay.ma.ca\_cert\_fname****

```
CFTI09F Init error _ Communication process CFTI10F Init error _ failed to start the Secure Relay Master Agent
```

<span class="bold_in_para">****Possible cause: Firewall or SAP overlap issue****</span>

The following messages may display indicating a SAP overlap (SAP is already used) or that there is a firewall issue:

```
CFTCFTS63F Secure Relay fatal error _ Secure Relay register error 2 (Error sending listen request to RA DMZ0:
CFTS63F+com.axway.xsr.agent.master.context.router.RouterAgentContextException: MPX channel is currently not available)
CFTI22F CFTPROT=PESIT Register request failure CS=00000098
```

## Transfer CFT and the Master Agent fail to start

****Possible cause: After changing the MA certificate, the secure\_relay.ma.cert\_fname parameter points to an invalid file****

Transfer CFT fails to start and displays a message similar to the following in the<span class="code">` cft.out`</span> file:

```
Error accessing user certificate keystore file <certificate name> (password might be wrong): java.io.IOException: keystore password was incorrect
```

To  correct, delete or rename the file referenced by the <span class="code">`secure_relay.ma.cert_password_fname`</span> parameter (by default, XsrPwd.dat) prior to restarting {{< TransferCFT/suitevariablesTransferCFTName  >}}.
