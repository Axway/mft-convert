---

    title: SFTP quick start!
    linkTitle: SFTP quick start!
    weight: 140

---
The supported operating systems are listed in the [Platform features](../../../datasheet) table.

This page provides a bare-bones configuration for you to begin to explore using SFTP for file transfers. When you are ready for a more advanced configuration, refer to the [Client configuration](../sftp_client) and [Server configuration](../sftp_server) pages.

## What you need

An installed Transfer CFT that acts as the server, and FileZilla (or similar) to use as the client.

## Step overview

1. Generate and import the server keys.
1. Interpret the template for the server configuration.
1. Use the provided connection details in the client.
1. Drag and drop your file!

## Tasks on the {{< TransferCFT/suitevariablesTransferCFTName  >}} server

### 1. Generate and import keys

Generate the server's public/private key pair using the <span class="code">`pkikeygen `</span>utility, which automatically puts the key pair in the PKI database (CFTPKU).

```
PKIUTIL pkikeygen id=SRV_PRIV_KEY, keylen=2048
```

### 2. Interpret the predefined SFTP template

From the runtime directory, interpret the <span class="code">`cft-sftp.conf`</span> template (click [here]() to view the template). Remember, Transfer CFT and the Transfer CFT Copilot server must be stopped.

```
cftinit conf/cft-sftp.conf
```

This example uses the most basic type of authentication. However, the <span class="code">`cft-sftp.conf`</span> template includes examples of multiple types of authentication, as described in detail in [SSH concepts](../sftp_keys_concepts).

## Tasks on the FileZilla client

### 3. Enter server connection details

1. Start Filezilla and enter the following connection details:

- Host: sftp://&lt;host address of the {{< TransferCFT/suitevariablesTransferCFTName >}} server>

- Port: 1763 (if you used the SAP from the template)

- Username: user1

- Password: TheUser1Password  

    > **Note**
    >
    > Tip  
    > The username and password are case sensitive.

Click **Quickconnect** to connect.

Click **OK** to accept the pop-up to accept the key.  
![](/Images/TransferCFT/fz_client_popup.png)

### 4. Perform a file transfer

Drag and drop files to <span class="code">`FLOW01 `</span>or <span class="code">`FLOW02 `</span>to perform file transfers.

![](/Images/TransferCFT/fz_client.png)

You can check the Transfer CFT log for details.

```
00 19/03/26 10:42:52 CFTY51I SSH server session established CTX=32000 PROT=SFTP Version=SSH2
Cipher in=aes256-ctr Cipher out=aes256-ctr Key
00 19/03/26 10:42:52 CFTY51I+exchange=curve25519-sha256@libssh.org hmac in=hmac-sha2-256
hmac out=hmac-sha2-256
00 19/03/26 10:42:52 CFTT70I The user user1 is connecting with method Password
00 19/03/26 10:42:52 CFTY53I SFTP server session established CTX=32000 PROT=SFTP Version=3
Authentication=Password CFT Client=No
00 19/03/26 10:42:52 CFTH56I SFTP Server session opened <PART=USER1 IDS=32000
HOST=127.0.0.1>
00 19/03/26 10:43:08 CFTY51I SSH server session established CTX=33000 PROT=SFTP Version=SSH2
Cipher in=aes256-ctr Cipher out=aes256-ctr Key
00 19/03/26 10:43:08 CFTY51I+exchange=curve25519-sha256@libssh.org hmac in=hmac-sha2-256
hmac out=hmac-sha2-256
00 19/03/26 10:43:08 CFTT70I The user user1 is connecting with method Password
00 19/03/26 10:43:08 CFTY53I SFTP server session established CTX=33000 PROT=SFTP Version=3
Authentication=Password CFT Client=No
00 19/03/26 10:43:08 CFTH56I SFTP Server session opened <PART=USER1 IDS=33000
HOST=127.0.0.1>
00 19/03/26 10:43:08 CFTW09I CFTRECV FLOW01 <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430 NIDF=FLOW01>
00 19/03/26 10:43:08 CFTT53I Server file created <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430>
00 19/03/26 10:43:08 CFTT55I Server file opened <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430>
00 19/03/26 10:43:08 CFTT57I Server transfer started <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430 IDS=33000>
00 19/03/26 10:43:08 CFTT58I Server transfer ended <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430 IDS=33000>
00 19/03/26 10:43:08 CFTT58I Transfer deselected <PART=USER1 IDS=33000 IDF=FLOW01
IDT=C2610430>
00 19/03/26 10:43:08 CFTT56I Server file closed <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430>
00 19/03/26 10:43:08 CFTT54I Server file deselected <IDTU=A0000001 PART=USER1 IDF=FLOW01
IDT=C2610430>
00 19/03/26 10:43:08 CFTT88I+<IDTU=A0000001 WORKINGDIR=sftp/user1/flow01
FNAME=FLOW01/stdio NBC=147998 DURATION=0s>
```
