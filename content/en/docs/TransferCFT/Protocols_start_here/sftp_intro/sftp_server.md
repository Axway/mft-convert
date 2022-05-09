{
    "title": "Configure Transfer CFT as an SFTP server",
    "linkTitle": "Configure the Transfer CFT SFTP server",
    "weight": "150"
}{{% TransferCFT/snippets/sftp_os_limitation%}}

This section describes how to set up Transfer CFT to use as a server with the SFTP protocol.

To set up Transfer CFT as the server:

1. Set the partner protocol to SFTP.
1. Create send and receive templates.
1. Add the key in the PKI database.
1. Define the SSH parameter connection for SFTP.
1. Define a partner to use in a flow.
    -   Add the authentication to use.
    -   Define a default flow model identifier.
1. Optionally, set the transcoding parameters.

Set the protocol (CFTPROT)
--------------------------

You should define the following parameters to use the SFTP protocol with a partner:

- TYPE: SFTP
- SAP: TCP/IP port for the SFTP server
- SSH: Link to a CFTSSH object (DIRECT=SERVER)
- NET: Link to a CFTNET definition

****Example****

```
CFTPROT id = SFTP,

> TYPE = 'SFTP',
> SSH = 'SSH_DEFAULT',
> SAP = '1763',
> NET = NET0,

    ...
```

Create send and receive templates (CFTSEND/CFTRECV)
---------------------------------------------------

When Transfer CFT is acting in server mode, your require the following:

- If the client performs a `get `command, the Transfer CFT must use implicit mode (CFTSEND IMPL=YES) on the server side. If you do not have a model having IMPL=YES, the client cannot perform a download (get) and a `permission denied` error occurs.
- If a third-party SFTP client performs a `put `command, the Transfer CFT server must use open mode (FNAME= &NFNAME). If you do not have a CFTRECV model, the client cannot perform an upload (put) and a `permission denied` error occurs. If you want to additionally provide the IDF for the {{< TransferCFT/axwayvariablesComponentLongName  >}} server, you must use the following syntax `/idf/file` for the remote file where the IDF is `flow01.`

The root directory for the SFTP connection is the WORKINGDIR defined in the CFTSEND (IMPL=YES) or CFTRECV corresponding to the IDF. If both CFTSEND and CFTRECV are defined for that IDF but with different WORKINGDIR, there is a DIAGI 435 error for the Transfer CFT client, and an SFTP failure with an appropriate error message for other SFTP clients.

If you do not define the WORKINGDIR, {{< TransferCFT/axwayvariablesComponentLongName  >}} uses the current directory (typically the runtime directory).

You can use the following symbolic variables when defining the WORKINGDIR:

- &USERID: user login
- &NRPART: same as &USERID
- &PART: partner
- &HOME: home directory on UNIX, or the user directory on Windows (`C:\Users\<user>`)

****Example****

In this example, `user1 `can perform a `get `or `put `command using the space (WORKINGDIR) defined for `user1`.

```
CFTPART ID=USER1, IDF=FLOW01, NRPART="user1",...
CFTSEND IDF=FLOW01, IMPL=YES, workingdir=user1_space, fname=&nfname
CFTRECV IDF=FLOW01, workingdir=user1_space, fname=&nfname
```

### Define a default flow model for a partner (IDF)

This section describes how to specify a default flow model identifier (IDF) to use if a client does not provide a flow name. In {{< TransferCFT/suitevariablesTransferCFTName  >}}, this is the CFTPART default IDF.

```
CFTPART id=partner,IDF=<default_model_for_this_partner>,…
```

### Define the authorization list for a partner (CFTAUTH)

You can use the CFTAUTH command to restrict a user to certain flow templates (IDF) that correspond to a physical place where this user can only see the logical folder. The CFTAUTH IDF parameter designates a list
of authorized templates for send/receive transfers with a defined partner. You can use these parameters to limit the visibility for a given user. The SAUTH/RAUTH in the partner definition refers back to the CFTAUTH ID. For more information, see [CFTAUTH](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftauth).

In the following example, defining a CFTAUTH creates visibility for **`FLOW01`** and **`FLOW02`** for the `user2 `client.

```
CFTPART ID=USER2, IDF=(FLOW02), NRPART="user2", SAUTH=AUTH2, RAUTH=AUTH2...
CFTAUTH ID=AUTH2, IDF=(FLOW01,FLOW02)
 
CFTSEND IDF=FLOW01, IMPL=YES, workingdir=flow1_space, fname=&nfname
 
CFTSEND IDF=FLOW02, IMPL=YES, workingdir=flow2_space, fname=&nfname
CFTRECV IDF=FLOW02, workingdir=user2_space, fname=&nfname
```

The view for a third-party software client would resemble the following (where the flow identifier displays, but not the physical folder name). In this example, the `workingdir `is relative to the runtime directory.

> ![](/Images/TransferCFT/sftp_view_client.png)

If you enter an \* (asterisk),
all model files (IDFs) can be used. If the provided IDF is not in one of the two lists, the connection is rejected and the {{< TransferCFT/suitevariablesTransferCFTName  >}} client returns a DIAGI 411.

See the examples in [SFTP use case examples](../cftssh_example#top).

If the provided IDF does not belong to either the SAUTH or RAUTH list, on the server side, the connection is rejected and the {{< TransferCFT/suitevariablesTransferCFTName  >}} client returns a DIAGI 413.

> **Note**
>
> For a put command on the client side, the IDF must be defined in the RAUTH on the server side. For a get command, the IDF must be defined in the SAUTH on the server side.

> **Note**
>
> If you are using both the SAUTH and RAUTH parameters, then you must use the same value for the SAUTH (sending files) and RAUTH (receiving files) for a given CFTPART definition. This means that the client will see only the directories that are authorized by these parameters.

Add the server key in the PKI database (PKIKEYGEN or PKIKEY)
------------------------------------------------------------

You can use either the PKIKEYGEN command or the PKIKEY command to add the server key in the database. For more information, see [Generate and manage keys](../new_pki_keys_use).

****Example****

```
PKIKEYGEN id=MY_KEY, keylen=2048
```

Define the SSH server profile (CFTSSH)
--------------------------------------

This section you use CFTSSH to define a SSH profile in Transfer CFT. The CFTSSH definition contains the SSH parameter connection for SFTP in server mode.

- ID: Identifier of the object
- DIRECT=SERVER
- SRVPRIVKEY: Key Id containing the server private key (RSA) to use with key authentication.
    -   If SRVPRIVKEY(CFTSSH direct=server) is not set, {{< TransferCFT/axwayvariablesComponentLongName  >}} cannot start, and the message displays `CFTN05E SFTP bind() failed: ECDSA, DSA, or RSA host key file must be set`.

****Example****

```
CFTSSH id = SSH_DEFAULT,

> DIRECT = SERVER,
> SRVPRIVKEY = MY_KEY,
> ...

```

Define a partner (CFTPART) for the flow
---------------------------------------

A CFTPART object represents an application, with one SFTP user per application (CFTPART = one application). To define a CFTPART object in server mode using SFTP:

- NRPART: Corresponds to the client login. You cannot have 2 partners with the same NRPART value.
- PROT: Refers to the SFTP protocol

> **Note**
>
> The following sections detail authentication for the partner in flows.

Set the type of authentication
------------------------------

Select the appropriate type of authentication to use from the options listed in this section. For more information, please see [SSH session concepts](../sftp_keys_concepts)
(SFTP).

<span id="Password"></span>

### Password authentication

There are two ways for you to configure how the server will check the client password:

- Clear text: When NRPASSW=&lt;the user password&gt;, the client password is in clear text.

<!-- -->

- Uconf definition: When NRPASSW=_AUTH_, authentication is specified in `uconf:cft.server.authentication_method `is used.

> **Note**
>
> If you do not define NRPASSW, there is no password authentication.

```
CFTPART id = USER1,

> prot = SFTP,
> nrpart = "user1",
> nrpassw = "TheUser1Password", ...

CFTTCP id = USER1,

> host = 127.0.0.1,
> ...

```
<span id="Key"></span>

### Key authentication

To configure how the server check the client key (CFTSSH), define:

- Identifier: CLIPUBKEY can refer to an identifier in the local PKI database.

Which CLIPUBKEY is used for the SSH profile is determined as follows:

- To use a specific SSH profile, you must define it in CFTPART 
- If you do not define a specific SSH profile, then the server uses the one defined by default in CFTPROT 

This example illustrates a specific SSH profile (SSH_USER2 below).

![](/Images/TransferCFT/KEY_AUTH1.png)

```
CFTPART id = USER2,

> ssh = SSH_USER2,
> prot = SFTP,
> nrpart = "user2", ...

 
CFTTCP id = USER2,
host = 127.0.0.1, ...
 
CFTSSH id = SSH_USER2,

> direct = SERVER,
> clipubkey = USER2, ...

```

> **Note**
>
> Remember that you must import the client's public key in the server's database as shown below:

```
PKIUTIL pkikey id='USER2', ikname='user2.pub', ikform='ssh'
```

### Two-factor (dual) authentication

When using **password and key** two-factor (dual) authentication:

- NRPASSW: Use one of the password authentication methods to configure how the server will check the client password, as described [here.](#Password)
- CLIPUBKEY: To configure how the server will check the client key, as described [here.](#Key)  
    ![](/Images/TransferCFT/SFTP_DUALAUTH.png)

<!-- -->

- ```
    CFTPART id = USER3,

    > ssh = USER3,
    > prot = SFTP,
    > nrpart = "user3",
    > nrpassw = "TheUser3Password",...

     
    CFTTCP id = USER3,

    > host = 127.0.0.1, ...

     
    CFTSSH id = USER3,

    > direct = SERVER,
    > clipubkey = USER3, ...

    ```

Server mode password authentication using restricted flow models
----------------------------------------------------------------

The following diagrams illustrate the previous configuration concepts.

![](/Images/TransferCFT/sftp_cft.jpg)

****Server mode key and password authentication using restricted flow models****

![](/Images/TransferCFT/sftp_server_double.png)

<span id="Transcod"></span>

Transcoding parameters
----------------------

The character conversion in text mode can be done at the requester or server level, either in a send or receive command. You can configure the conversion using FCHARSET/NCHARSET or FCODE/NCODE, where transcoding is performed if FCODE differs from NCODE, or if FCHARSET differs from NCHARSET. The FCHARSET/NCHARSET parameters, however, take precedence over FCODE/NCODE if both are defined. Additionally:

- The charset and transferred file code are exchanged between the requester and server for two Transfer CFTs.
- A transfer restart is forbidden if the FCHARSET/NCHARSET conversion is done at the server level.
- The NCODE parameter is available in CFTRECV as with CFTSEND.

****SFTP versions****

- SFTP 3 and lower: There is no flag to open a file in text mode, so the text mode is selected through the IDF's FTYPE parameter. The newline conversion can be specified on the client side.
- SFTP 4 and higher: The client indicates if the transfer is done in binary or text mode. This overrides the IDF's FTYPE parameter. The newline conversion is done on the client side to accommodate the server requirement.

If the remote path is absolute (root directory), {{< TransferCFT/suitevariablesTransferCFTName  >}} uses the root directory as the IDF. In this example, `flow01 `corresponds to the IDF for this partner.

```
put LocalFile01.txt /flow01/RemoteFile01.txt
```

****Related topics****

[CFTSSH - Security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh)
