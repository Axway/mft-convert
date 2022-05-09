{
    "title": "Set up RACF security software",
    "linkTitle": "Set up RACF security software",
    "weight": "280"
}This section describes how to set up RACF software to provide security control for file handling operations. Transfer CFT z/OS uses the SAF security interface, and is compatible with the security software packages that use this interface, in particular RACF.

File handling operations control
--------------------------------

By default, Transfer CFT implements file handling operations under its own authority.

Transfer CFT systematically checks the file access rights, both for its own access rights and for the user accounts. A 91300038 or 80000913 error code is returned for any unsuccessful access attempt.

You can activate more elaborate control in the following cases:

- To request the submission of an end-of-send procedure under the authority of the transfer requester

<!-- -->

- To request the submitting an end-of-receive procedure under the authority of the transfer receiver

<!-- -->

- To request opening a file to be sent under the authority of the transfer requester

<!-- -->

- To request opening a file to be received under the authority of the transfer receiver

Activate RACF authorization control
-----------------------------------

The SAF services call (RACF or equivalent) is systematic.

Advanced use of RACF functions can be implemented when the following conditions are fulfilled:

- Transfer CFT is an authorized program

<!-- -->

- The corresponding option is activated in the SGINSTAL installation options

<!-- -->

- The CFTPARM USERCTRL=YES parameter is set

For more information, see [CFTPARM](../../../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftparm).

### RACF and the Internet

RACF is required when the Internet interface is used to check the PASSWORDs. The interface cannot check the passwords if:

- It is not authorized

<!-- -->

- SAF is not available

In either of these cases, you can log in by entering a random value in the PASSWORD field. You then have the privileges of the JOB where you are logged on.

### RACF and the JAVA user interface

The JAVA interface connects to the Transfer CFT Copilot server. It is identified by the pair USERID/PASSWORD, even when connecting from OPEN/MVS. The interface cannot verify passwords, if either:

- Password is not authorized

<!-- -->

- SAF is not available

In either of these cases, you can enter any value in the PASSWORD field to connect. You then have privileges for the JOB you are connected to.

> **Note**
>
> Note: Transfer CFT z/OS accepts passwords in lower case using RACF or Top-Secret.

<span id="RACF pas"></span>

### RACF password phrase support

{{< TransferCFT/axwayvariablesComponentLongName  >}} supports the use of a password phrase (RACF) in the user interface, JPIUTIL, REST API, and synchronous API services.

****Limitations****

- When using the user interface with RACF to enter your password phrase, the confirm password pop-up window does not display after the password phrase expiration.
- The RACF password phrase can consist of up to 30 characters.

### Access USS files

Specific configuration is required to access USS files:

- Transfer CFT must be able to transfer USS files.

<!-- -->

- The Transfer CFT Copilot server must be able to access USS files.

We recommend that you:

- Assign a UID other than 0 to Transfer CFT and the Copilot server.

<!-- -->

- Give READ access to the BPX.SERVER resource in the RACF FACILITY class.

****Example of PERMIT****

```
PERMIT PBX.SERVER CLASS(FACILITY) ID(CFT) ACCESS(READ) ID(COPILOT)
```
