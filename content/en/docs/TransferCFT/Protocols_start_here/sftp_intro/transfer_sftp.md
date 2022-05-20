---
title: "SFTP quick start!"
linkTitle: "SFTP quick start!"
weight: 130
---******The supported operating systems are listed in the [Platform features](../../../datasheet) table.******

This page provides a bare-bones configuration for you to begin to explore using SFTP for file transfers. When you are ready for a more advanced configuration, refer to the [Client configuration](../sftp_client) and [Server configuration](../sftp_server) pages. If you prefer video step instructions, please see the [How to configure an SFTP flow](#How) demo below.

## What you need

An installed Transfer CFT that acts as the server, and FileZilla (or similar) to use as the client.

## Procedure


| **On the Transfer CFT server**  |   |
| --- | --- |
| 1. Generate and import the server keys.  | Generate the server's public/private key pair using the <code>pkikeygen </code>utility, which automatically puts the key pair in the PKI database (CFTPKU).<br/> ```  PKIUTIL pkikeygen id=SRV_PRIV_KEY, keylen=2048 ```  |
| 2. Interpret the template for the server configuration.  | From the runtime directory, interpret the <code>cft-sftp.conf</code> template (click [here]() to view the template). Remember, Transfer CFT and the Transfer CFT Copilot server must be stopped.<br/> ```  cftinit conf/cft-sftp.conf ```  |
| **On the client**  |   |
| 3. Enter the server connection details.  | Start FileZilla and enter the following connection details: • Host: sftp://&lt;host address of the {{< TransferCFT/suitevariablesTransferCFTName  >}} server&gt;<br/> • Port: 1763 (if you used the SAP from the template)<br/> • Username: user1<br/> • Password: TheUser1Password<br /> <br/> <blockquote> **Note**<br/> Tip The username and password are case sensitive.<br/> </blockquote><br/> Click **Quickconnect** to connect. Click **OK** to accept the pop-up to accept the key.<br /> ![](/Images/TransferCFT/fz_client_popup.png)  |
| 4. Drag and drop files to <code>FLOW01 </code>or <code>FLOW02 </code>to perform file transfers.  | ![](/Images/TransferCFT/fz_client.png) |


## Check the results

You can check the Transfer CFT log for details.

```
SSH handshake:
CFTY51I SSH server session established CTX=32000 PROT=SFTP Version=SSH2 Cipher in=aes256-ctr Cipher out=aes256-ctr Key
CFTY51I+exchange=curve25519-sha256@libssh.org hmac in=hmac-sha2-256 hmac out=hmac-sha2-256
SSH authentication using a password:
CFTT70I The user user1 is connecting with method Password
CFTY53I SFTP server session established CTX=32000 PROT=SFTP Version=3 Authentication=Password CFT Client=No
SSH algorithms:
CFTY51I SSH server session established CTX=33000 PROT=SFTP Version=SSH2 Cipher in=aes256-ctr Cipher out=aes256-ctr Key
CFTY51I+exchange=curve25519-sha256@libssh.org hmac in=hmac-sha2-256 hmac out=hmac-sha2-256
CFTT70I The user user1 is connecting with method Password
CFTY53I SFTP server session established CTX=33000 PROT=SFTP Version=3 Authentication=Password CFT Client=No
CFTH56I SFTP Server session opened <PART=USER1 IDS=33000 HOST=127.0.0.1>
SFTP data transfer:
CFTT57I Server transfer started <IDTU=A0000001 PART=USER1 IDF=FLOW01 IDT=C2610430 IDS=33000>
CFTT58I Server transfer ended <IDTU=A0000001 PART=USER1 IDF=FLOW01 IDT=C2610430 IDS=33000>
CFTT54I Server file deselected <IDTU=A0000001 PART=USER1 IDF=FLOW01 IDT=C2610430>
Received file location:
CFTT88I+<IDTU=A0000001 WORKINGDIR=sftp/user1/flow01 FNAME=FLOW01/stdio NBC=147998 DURATION=0s>
```
<span id="How"></span>

## How to configure an SFTP flow - video
