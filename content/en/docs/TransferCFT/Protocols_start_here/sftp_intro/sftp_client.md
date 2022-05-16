---

    title: Configure Transfer CFT as an SFTP client
    linkTitle: Configure the Transfer CFT SFTP client
    weight: 170

---
The supported operating systems are listed in the [Platform features](../../../datasheet) table.

To configure a {{< TransferCFT/suitevariablesTransferCFTName  >}} SFTP client:

1. Define the SFTP protocol.
1. Add the server public key in client profile.
1. Add the client key in the PKI database.
1. Define an SSH client profile.
1. Define a partner to use in a flow.
1. Set the type authentication.

## Define the protocol (CFTPROT) for the Transfer CFT client

The following parameters are used to define the client's SFTP protocol:

- TYPE: SFTP
- SSH: Link to the CFTSSH object (DIRECT=CLIENT)
- NET: link to the CFTNET 

****Example****

```
cftprot id = SFTP,

> TYPE = 'SFTP',
> SSH = 'SSH_DEFAULT',
> NET = NET0,

   ...
```

## Add the server public key in the client profile

Use the PKIKEY command to add the server public key in the database.

```
PKIKEY id=SRV_PUB_KEY, ikname='serv.pub', ikform=ssh
```

## Add the client key in the PKI database (PKIKEYGEN or PKIKEY)

You can use either the PKIKEYGEN command or the PKIKEY command to add the client key in the server database if you are using key authentication (or dual authentication). For more information, see <a href="../new_pki_keys_use" class="MCXref xref">Generate and manage keys</a>.

****Example****

```
PKIKEYGEN id=MY_KEY, keylen=2048
```

## Define the SSH client profile (CFTSSH)

This section you use CFTSSH to define a SSH profile in Transfer CFT. The CFTSSH definition contains the SSH parameter connection for SFTP in client mode. For more information, please see [Define CFTSSH](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
(SFTP).

- ID: Identifier of the object
- DIRECT=CLIENT
- SRVPUBKEY=Identifier of the key used to authenticate the server
- CLIPRIVKEY: Key Id of the client private key to use with key authentication.

****Example****

```
CFTSSH id = SSH_DEFAULT,

> DIRECT = CLIENT,
> CLIPRIVKEY = MY_KEY,
> SRVPUBKEY = ,
> ...

```

## Define the partner (CFTPART) for a flow

A CFTPART object represents an application, with one SFTP user per application (CFTPART = one application). To define a CFTPART object in client mode using SFTP:

- NSPART: Corresponds to your client login
- SAP: The remote SFTP server port
- HOST:The remote SFPT server host
- PROT: Refers to the SFTP protocol

> **Note**
>
> The following sections describe the type of partner authentication.

## Set the type of authentication

Select the appropriate type of authentication to use from the options listed in this section. For more information, see <a href="../sftp_keys_concepts" class="MCXref xref">SSH concepts</a>.

<span id="Password"></span>

### Password authentication

Use  one of the following methods to configure the client password:

- Clear text: When NSPASSW=&lt;the user password>, the client password is in clear text.

<!-- -->

- Uconf definition: When NSPASSW=\_AUTH\_, authentication is specified in <span class="code">`uconf:cft.server.authentication_method `</span>is used.

> **Note**
>
> If you do not define NSPASSW, Transfer CFT does not send a password to the server.

```
CFTPART id = USER1,

> prot = SFTP,
> sap = 1763,
> nspart = "user1",
> nspassw = "TheUser1Password", ...

CFTTCP id = USER1,

> host = <server host>,
> ...

```

![Client NSPART arrow to Server Login, Cient NSPASSW arrow to server Password](/Images/TransferCFT/sftp_client.png)

<span id="Key"></span>

### Key authentication

To configure how the client sends the key (CFTSSH):

- Identifier: CLIPRIVKEY refers to an identifier in the local PKI database.

This is how the client decides the CLIPRIVKEY to use for the SSH profile:

- To use a specific SSH profile, you must define it in CFTPART 
- If you do not define a specific SSH profile, then the client will use the one defined by default in CFTPROT 

This example illustrates a specific SSH profile (SSH\_USER2 below).

```
CFTPART id = USER2,

> ssh = SSH_USER2,
> sap = 1763,
> prot = SFTP,
> nspart = "user2", ...

 
CFTTCP id = USER2,
host = <remote host>, ...
 
CFTSSH id = SSH_USER2,

> direct = CLIENT,
> cliprivkey = USER2, ...

```

### Password and key authentication

When using **password and key** authentication:

- NSPASSW: Use one of the methods to configure how the client sends its password, as described [here](#Password)
- CLIPRIVKEY: Use this to configure how the client sends its key, as described [here](#Key)

<!-- -->

- ```
    CFTPART id = USER3,

    > ssh = USER3,
    > sap = 1763,
    > prot = SFTP,
    > nspart = "user3",
    > nspassw = "TheUser3Password",...

     
    CFTTCP id = USER3,

    > host = <remote host>, ...

     
    CFTSSH id = USER3,

    > direct = CLIENT,
    > cliprivkey = USER3, ...

    ```

<span id="Transcod"></span>

## Transcoding parameters

You can configure the conversion using FCHARSET/NCHARSET or FCODE/NCODE, where transcoding is performed if FCODE differs from NCODE, or if FCHARSET differs from NCHARSET. The FCHARSET/NCHARSET parameters, however, take precedence over FCODE/NCODE if both are defined. Additionally:

- The charset and transferred file code are exchanged between the requester and server for two Transfer CFTs.
- A transfer restart is forbidden if the FCHARSET/NCHARSET conversion is done at the server level.
- The NCODE parameter is available in CFTRECV as with CFTSEND.

See also, [Transcoding concepts](../#Transcod).

****Related topics****

[CFTSSH - Security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
