---
    title: "Troubleshoot SFTP"
    linkTitle: "Troubleshoot SFTP"
    weight: 190
---******The supported operating systems are listed in the [Platform features](../../../datasheet) table.******

## Start Transfer CFT issues

### Error when starting {{< TransferCFT/suitevariablesTransferCFTName  >}}

If the following error displays:

`CFTN05E SFTP bind() failed: ECDSA, DSA, or RSA host key file must be set.  `

Check the SRVPRIVKEY ID (CFTSSH direct=server) parameter. Presently, only the RSA algorithm is supported; even if ECDSA and DSA are mentioned in the error message, these algorithms are not supported.

## Authentication issues

### Server authentication issues

****Client does not accept the server's public key****

The following is an example of a public key authentication issue where the client does not accept the server's public key.

```
Client side
CATALOG 
CLIENT_S SFH TH BIN J0219374 0 0 264 KEY
 
LOG 
CFTT75E Network connect reject <IDTU=A000000W PART=CLIENT_SFTP_WIN IDF=BIN IDT=J0219374 DIAGI=264>
CFTT82E Transfer aborted <IDTU=A000000W PART=CLIENT_SFTP_WIN IDF=BIN IDT=J0219374 DIAGI=264>
```

### Client authentication issues

****Invalid users or password issues****

Transfer CFT client authentication mismatches can lead to the following errors:

```
LOG 
CFTT75E Incorrect user or password <IDTU=A000000I PART=CLIENT_SFTP_WIN IDF=BIN IDT=J0218294 DIAGI=433>
CFTT82E Transfer aborted <IDTU=A000000I PART=CLIENT_SFTP_WIN IDF=BIN IDT=J0218294 DIAGI=433>
 
CATALOG
CLIENT_S SFH TH BIN J0218294 0 0 433 00000001
```

You may require a [trace](#Perform) on the Transfer CFT server for additional information (to see if the issue is user or password).

```
Invalid user:
CFTSFTP CFT.SFTP [3] S50000: User SERV_SFTP wants to authenticate with method SYSTEM
CFTSFTP CFT.SFTP [1] S50000: User SERV_SFTP not allowed to connect to the server
 
Invalid password:
CFTSFTP CFT.SFTP [3] S50000: User serv_SFTP wants to authenticate with method PASSWORD
CFTSFTP CFT.SFTP [1] S50000: User serv_SFTP not allowed to connect to the server
```

****SFTP client case sensitivity****

Remember that NSPART and NRPART are case sensitive when they are enclosed in quotes " ". For example, if the user name is `login`, then the CFTPART ID=PART,NSPART="login" and NRPART=login.

```
Error: Authentication failed.
Error: Critical error: Could not connect to server.
```

### Transfer fails

#### Enable implicit mode

You must define CFTSEND IDF=&lt;idf>,IMPL=YES when in server mode. Failing to do so leads to the following type of message:

```
CFTT16I _ No implicit send <PART=<part> IDF=<idf> > :
```

#### Working directory

Here the connection is interrupted because of a `workingdir `issue when connecting to the Transfer CFT SFTP via an SFTP client:

```
Error: Unable to open .: received failure with description 'The working directories in CFTSEND and CFTRECV for the IDF are not the same'
Error: Received unexpected end-of-file from SFTP server
Error: Cannot recover the folder contents
```

#### SAUTH/RAUTH

These parameters check the authorized IDF for the user. For example, in the following messages an error occurred because when performing a RECV (`get`) command, the IDF was not included in the remote authorization list.

****Server****

No catalog record, and in the log:

```
CFTT25E _ IDF not authorized <PART=SERV_SFTP IDF=AUSTIN>
CFTT82E Transfer aborted <IDTU=00000000 PART= IDF= IDT= DIAGI=413>
```

****Client****

Catalog record:

```
diagi=610 diagp=00000000
```

Log:

```
CFTF02E Remote file selection error <IDTU=A0000005 PART=CLIENT_SFTP_UNIX IDF=F
LOW IDT=J0913570>
CFTT82E Transfer aborted <IDTU=A0000005 PART=CLIENT_SFTP_UNIX IDF=
FLOW IDT=J0913570 DIAGI=610>
```

#### Open mode not allowed

When open mode is not enabled on a non-Transfer CFT client, a generic message displays:

```
Command: put "C:\\Users\\...\\MyFile.jpg" "MyFile.jpg"
Error: Received unexpected end-of-file from SFTP server
Error: File transfer failed
```

On the server side, additional information can be found in the log:

```
CFTT01E IDTU=00000000 PART=SERV_SFTP IDF=FLOW01 IDT= _ Open mode not allowed
CFTT82E Transfer aborted <IDTU=00000000 PART=SERV_SFTP IDF=FLOW01 IDT= DIAGI=404>
```

Catalog:

```
Diagi=110 with diagp=00000013
```

#### Client key does not correspond to server key

When using {{< TransferCFT/suitevariablesTransferCFTName  >}} as a client, the server's public key referenced by SRVPUBKEY (CFTSSH direct=Client) does not correspond to the server key.

```
CFTT82E+ DIAGP=KEY DIAGC=The client key doesn't correspond to the server key
```

Check that the public key stored in the PKI database corresponds with the server's (SRVPUBKEY value). This issue may occur due to a Transfer CFT limitation where when an SFTP server refers to multiple hostkeys (located in `etc/ssh/sshd_config`), the Transfer CFT related hostkey must be placed in the first position.

As shown in the following example, the Transfer CFT public key references the `ssh_host_rsa_key`, an error occurs:

```
HostKey /etc/ssh/ssh_host_rsa_key_not_in_CFT
HostKey /etc/ssh/ssh_host_rsa_key
```

### Resources

[Diagi:Diagnostic codes](../../../troubleshoot_intro/about_error_codes/about_diagnostic_codes/diagi_diagnostic_codes). Codes with a value between 001 and 499 indicate a local issue; values between 501 and 999 correspond to a fault reported by the partner. Therefore when troubleshooting if the code is greater than 500 it refers to a remote issue.

## Check updates to the configuration (delay)

The parameters used by SFTP in CFTPARM and CFTPART files are loaded in memory when {{< TransferCFT/suitevariablesTransferCFTName  >}} starts and updated every 10 seconds if there is a change in the file.

<span id="Perform"></span>

## Perform a trace

If you were not able to remedy the issue as described in the previous sections, you may want to perform an SFTP trace. After performing the following commands, you must restart {{< TransferCFT/suitevariablesTransferCFTName  >}}.

```
Windows
set XTRACE_CFT_SFTP_LEVEL=5
set XTRACE_OUTPUT_FILENAME=sftptrace.txt
 
UNIX
export XTRACE_CFT_SFTP_LEVEL=5
export XTRACE_OUTPUT_FILENAME=sftptrace.txt
```
