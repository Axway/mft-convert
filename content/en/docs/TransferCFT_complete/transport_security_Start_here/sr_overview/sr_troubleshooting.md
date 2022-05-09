{
    "title": "Troubleshoot Secure Relay ",
    "linkTitle": "Troubleshoot Secure Relay ",
    "weight": "270"
}Should you incur an issue, you can begin by checking for information in the following files:

- Secure Relay Master Agent log file: secure_relay.ma.log_fname = C:\\cft35\\runtime\\log\\xsrMaster.log
- Secure Relay Router Agent log file: located by default in the &lt;install_dir&gt;/SecureRelayRA/log/router.log
- Transfer CFT log messages: check for the messages CFTS63F and CFTS64I, which provide information about the SecureRelay status

You can refer to the [Secure Relay documentation](https://docs.axway.com/bundle/SecureRelay_271_AdministratorsGuide_allOS_en_HTML5/page/Content/AxwayStartPageRA_admin.htm) for additional information.

Check Transfer CFT log messages...
----------------------------------

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

****Possible cause: Problem related to secure_relay.ma.ca_cert_fname****

```
CFTI09F Init error _ Communication process CFTI10F Init error _ failed to start the Secure Relay Master Agent
```

****Possible cause: Firewall or SAP overlap issue****

The following messages may display indicating a SAP overlap (SAP is already used) or that there is a firewall issue:

```
CFTCFTS63F Secure Relay fatal error _ Secure Relay register error 2 (Error sending listen request to RA DMZ0:
CFTS63F+com.axway.xsr.agent.master.context.router.RouterAgentContextException: MPX channel is currently not available)
CFTI22F CFTPROT=PESIT Register request failure CS=00000098
```

Transfer CFT and the Master Agent fail to start
-----------------------------------------------

****Possible cause: After changing the MA certificate, the secure_relay.ma.cert_fname parameter points to an invalid file****

Transfer CFT fails to start and displays a message similar to the following in the` cft.out` file:

```
Error accessing user certificate keystore file <certificate name> (password might be wrong): java.io.IOException: keystore password was incorrect
```

To  correct, delete or rename the file referenced by the `secure_relay.ma.cert_password_fname` parameter (by default, XsrPwd.dat) prior to restarting {{< TransferCFT/suitevariablesTransferCFTName  >}}.

****Possible cause: After changing the Master Agent certificate, the secure_relay.ma.cert_fname parameter points to an invalid file****

****Possible cause: The Secure Relay certificate is expired (either the CA or user certificate)****

How to check for expired certificates
-------------------------------------

Perform the following commands for the indicated product:

### On Transfer CFT

`openssl x509 -in <secure_relay.ma.ca_cert_fname> -noout -text`

`openssl pkcs12 -in <secure_relay.ma.cert_fname> -nokeys -passin pass:<secure_relay.ma.cert_password>  &#124; openssl x509 -noout -enddate`

### On the Secure Relay Router Agent

`openssl pkcs12 -in <secure_relay router agent  cert fname> -nokeys -passin pass: <secure_relay router agent  cert password>  &#124; openssl x509 -noout -enddate`

How to fix expired certificates
-------------------------------

> **Note**
>
> Note: Stop involved products before performing this procedure and restart afterward.

### Secure Relay Master Agent on Transfer CFT

1. Check the location and name of the certificates and encryption file in the following uconf parameters:
    -   secure_relay.ma.ca_cert_fname
    -   secure_relay.ma.cert_fname
    -   secure_relay.ma.cert_password_fname
1. Replace certificates as needed.
1. Set the uconf parameters listed above so that they point to the new certificates.
1. Rename or remove the old file that the secure_relay.ma.cert_password_fname parameter referenced.

### On the Secure Relay RA/XSR

1. Check the location and name of the certificates and encryption files.
1. Go to &lt;SecureRelayRAInstallationDirectory&gt;/conf/configuration.xml and check:  
    \*&lt;CACertificate&gt;\*CA_for_RA.der&lt;/CACertificate&gt;  
    \*&lt;UserCertificate&gt;\*USER_for_RA.p12&lt;/UserCertificate&gt;  
    \*&lt;PasswordFile&gt;\*XsrPwd.dat&lt;/PasswordFile&gt;
1. Replace the certificates as needed.
1. Update configuration parameters (path and filename) with the new certificates.
1. Regenerate password file as follows:
    1.  Enter the new password in a text file - for example, pwd.txt.
    2.  Rename the existing XsrPwd.dat by XsrPwd.dat.bak from &lt;SecureRelayRAInstallationDirectory&gt;/bin/SRencryptPwd pwd.txt XsrPwd.dat
    3.  Copy the new XsrPwd.dat to the path identified for &lt;PasswordFile&gt; (configuration.xml)
