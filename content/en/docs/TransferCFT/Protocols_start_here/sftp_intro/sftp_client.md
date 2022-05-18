---
title: "Configure Transfer CFT as an SFTP client"
linkTitle: "Configure the Transfer CFT SFTP client"
weight: 160
--- ******The supported operating systems are listed in the [Platform features](../../../datasheet) table.******

To configure a {{< TransferCFT/suitevariablesTransferCFTName  >}} SFTP client:

1. Define the SFTP protocol.
1. Add the server public key in client profile depending on the type of authentication.
1. Add the client key in the PKI database depending on the type of authentication.
1. Define an SSH client profile.
1. Define a partner to use in a flow.
1. Set the type authentication.
1. Optionally, set the transcoding parameters.

## Define the protocol (CFTPROT) for the Transfer CFT client

Use the following parameters to define the client's SFTP protocol:

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

## Add the server public key in the PKI database

Use the PKIKEY command to add the server public key in the PKI database. For more information, see [Generate and manage keys](../new_pki_keys_use).

```
PKIKEY id=SRV_PUB_KEY, ikname='serv.pub', ikform=ssh
```

## Add the client private key in the PKI database (PKIKEYGEN or PKIKEY)

You can use either the PKIKEYGEN command or the PKIKEY command to add the client private key in the server database if you are using key authentication (or dual authentication). For more information, see [Generate and manage keys](../new_pki_keys_use).

****Example****

```
PKIKEYGEN id=MY_KEY, keylen=2048
```

## Define the SSH client profile (CFTSSH)

Use CFTSSH to define a SSH profile in Transfer CFT. The CFTSSH definition contains the SSH parameter connection for SFTP in client mode. For more information, see [Define CFTSSH](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
(SFTP).

- ID: Identifier of the object
- DIRECT=CLIENT
- SRVPUBKEY=Identifier of the key used to authenticate the server. If this is not set, there is no authentication control.
- CLIPRIVKEY: Key Id of the client private key to use with key authentication or two- factor (dual) authentication.

****Example****

```
CFTSSH id = SSH_DEFAULT,

> DIRECT = CLIENT,
> CLIPRIVKEY = MY_KEY,
> SRVPUBKEY = ,
> ...

```

## Define the partner (CFTPART) for the flow

A CFTPART object represents an application with one SFTP user per application (CFTPART = one application). To define a CFTPART object in client mode using SFTP:

- NSPART: Corresponds to your client login
- SAP: The remote SFTP server port
- HOST:The remote SFPT server host
- PROT: Refers to the SFTP protocol

## Set the type of authentication

Select the type of authentication to use from the options listed in this section. For more information, see [SSH session concepts](../sftp_keys_concepts).

<span id="Password"></span>

### Password authentication

Use  one of the following methods to configure the client password:

- Clear text: When NSPASSW=&lt;the user password>, the client password is in clear text.

<!- - - - >

- Uconf definition: When NSPASSW=_AUTH_, authentication is specified in `uconf:cft.server.authentication_method `is used.

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

This example illustrates a specific SSH profile (SSH_USER2 below).

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

### Two- factor (dual) authentication

When using **password and key** two- factor (dual) authentication:

- NSPASSW: Use one of the methods to configure how the client sends its password, as described [here.](#Password)
- CLIPRIVKEY: Use this to configure how the client sends its key, as described [here.](#Key)

<!- - - - >

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

The character conversion in text mode can be done at the requester or server level, either in a send or receive command. You can configure the conversion using FCHARSET/NCHARSET or FCODE/NCODE, where transcoding is performed if FCODE differs from NCODE, or if FCHARSET differs from NCHARSET. The FCHARSET/NCHARSET parameters, however, take precedence over FCODE/NCODE if both are defined. Additionally:

- The charset and transferred file code are exchanged between the requester and server for two Transfer CFTs.
- A transfer restart is forbidden if the FCHARSET/NCHARSET conversion is done at the server level.
- The NCODE parameter is available in CFTRECV as with CFTSEND.

****SFTP versions****

- SFTP 3 and lower: There is no flag to open a file in text mode, so the text mode is selected through the IDF's FTYPE parameter. The newline conversion can be specified on the client side.
- SFTP 4 and higher: The client indicates if the transfer is done in binary or text mode. This overrides the IDF's FTYPE parameter. The newline conversion is done on the client side to accommodate the server requirement.

## Restart a transfer

Since transfers are activated by the client, a restart will only work from the client side.

- There is only one entry in the catalog for the transfer. Use the file name to identify the transfer identifier during the restart.
- When a send is restarted by the client, {{< TransferCFT/axwayvariablesComponentLongName >}} checks that the file is still available on the server. Therefore a restart is only possible when the transfer is configured in Open Mode.

****Related topics****

[CFTSSH - Security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
