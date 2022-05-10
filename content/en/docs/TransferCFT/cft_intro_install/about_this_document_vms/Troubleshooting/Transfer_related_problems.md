---
    "title": "Transfer related problems",
    "linkTitle": "Transfer related problems",
    "weight": "230"
---
These problems cause {{< TransferCFT/axwayvariablesComponentShortName  >}} to abort a transfer. For the {{< TransferCFT/axwayvariablesComponentShortName  >}} operator, one or more transfers fail, but the remaining activity is normal. This may be an exceptional or recurring phenomenon.

The key diagnostic elements are the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog and log file.

The problem may be local and due to the transfer configuration (CFTSEND and CFTRECV commands), the network resource configuration (CFTNET command), the protocol configuration (CFTPROT command), production problems (quotas, privileges, file or network problems) or the protocol-level dialog with your partner.

### Querying the catalog

Once you have identified the transfer at fault, query the catalog. The DIAGI field provides global information on the source of the error. Some of these codes are protocol-specific.

When the error is local, that is the error condition causing the transfer to be aborted occurred on your system, the DIAGP field in the catalog provides additional system-specific information. Otherwise, the DIAGP field contains a code that is standardized for the protocol between you and the partner site.

The DIAGI codes corresponding to local and remote errors range from 0 to 499 and 600 to 999 respectively. If a remote error occurs, you must contact the partner site, so that you can both determine the cause of the error. The error can be corrected by taking action on either side of the link.

The various types of error that can occur locally during a transfer are:

- Internal monitor errors: DIAGI code between 0 and 3

<!-- -->

- Errors relating to transferred files: DIAGI code between 100 and 142

<!-- -->

- Errors in the protocol-level dialog between the two transfer monitors: DIAGI code between 220 and 240

<!-- -->

- Network-related errors: DIAGI code between 301 and 303

<!-- -->

- Monitor configuration errors: DIAGI code between 401 and 460

<!-- -->

- Non-referenced errors: DIAGI code equal to 399

The way in which the DIAGP field is interpreted varies according to the type of error. To determine the meaning of the code, refer to the sub-book Codes, Diagnostics and Messages in the {{< TransferCFT/axwayvariablesComponentShortName  >}} online documentation.

### Querying the {{< TransferCFT/axwayvariablesComponentShortName  >}} log file

The transfer record may not appear in the catalog, even though the partner site has informed you of a problem, or the intended transfer has not been performed. This is often the case in server mode, when the problem occurs even before the transfer starts (mutual identification refused, file creation error, and so on). Therefore, you must query the {{< TransferCFT/axwayvariablesComponentShortName  >}} log file.

The {{< TransferCFT/axwayvariablesComponentShortName  >}} log file contains a trace of all events that occurred during monitor operations from the last file switch.

The sub-book Messages in the Codes, Diagnostics Codes and Messages in the {{< TransferCFT/axwayvariablesComponentShortName  >}} online documentation provides the format and a description for each event.

### TCP/IP network

In the case of the TCP/IP network, reason and diagnostic codes are returned by the task serving the TCP/IP network for {{< TransferCFT/axwayvariablesComponentShortName  >}}. The code structure is specific to CFT. For more information refer to TCP/IP Network, in Network Codes in the {{< TransferCFT/axwayvariablesComponentShortName  >}} online documentation.

### Interpreting a system code

A code in the CS, NCS or SCS category is a code that has been returned by the system service used to perform the operation that caused the error. Transfer CFT displays the code in hexadecimal format.

The system code is specific to OpenVMS and is therefore not documented in the {{< TransferCFT/axwayvariablesComponentShortName  >}} guides. To determine the meaning, you must refer to the *OpenVMS System Messages and Recovery Procedures Reference Manual*. The error codes are listed with their mnemonics.

To obtain the mnemonic from the code, proceed as follows:

 

```
$ write sys$output f$message("%x00018292")
%RMS-E-FNF, file not found
$
```

You can often determine the cause of the error from this code and take corrective action.

### Interpreting a network code

When the error is network-related (DIAGI code between 301 and 303), the DIAGP field is defined with a code specific to the type of network used.

The various network accounting files may feature additional information on the cause of the error.

If the error is indicated as being local (the code has the L HH HHH format), the reason and diagnostic fields do not correspond to network-standard codes. It is a problem related to the use of the network interface and not of the network itself.

If the reason code is equal to 2, the diagnostic code may be 1 or 2. If the reason code is equal to 3, the diagnostic code is a system code, depending on the type of problem.

The following table describes the reason and diagnostic codes.

 


| Reason  | Diagnostic  | Meaning  | Action  |
| --- | --- | --- | --- |
| 2 | 1 | Internal memory allocation problem.<br /> Insufficient dynamic memory for the JOB. | Increase the JOB PGFLQUOTA or implement the memory control mechanism, refer to section 3.2. |
| 2 | 2 | Maximum number of VCs declared for the resource (configuration CFTNET command) reached. | Modify your subscription and increase the value of the CFTNET command MAXCNX parameter defining the network resource, or wait until one of the VCs of the resource used is released for another transfer. |
| 3 | System code | System code returned by the network access system services.<br /> To determine the meaning of this code, refer to paragraphs SYS$QIO, SYS$ASSIGN, SYS$CANCEL and SYS$DASSGN in the System Services Reference Manual, or the programming guide for the corresponding network. | According to the meaning of the code, refer to the OpenVMS guide entitled System Messages and Recovery Procedures. |

