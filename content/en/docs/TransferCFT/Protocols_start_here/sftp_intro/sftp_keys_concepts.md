---

    title: SSH concepts
    linkTitle: SSH concepts
    weight: 150

---
The supported operating systems are listed in the [Platform features](../../../datasheet) table.

The SSH protocol is an encrypted protocol that uses a client-server model to authenticate two parties and encrypt the data between them.

The server listens on a designated port for connections, and is responsible for negotiating the secure connection, authenticating the connecting party, and spawning the correct environment if the credentials are accepted.

The client begins the TCP handshake with the server, negotiates the secured connection, checks that the server's identity matches previously recorded information, and provides credentials to authenticate.

An SSH session is established in two steps:

- Agree on and establish encryption to protect future communication.
- Authenticate the user and determine if access to the server should be granted.

If you are already familiar with SSH concepts are ready for details on configuring the CFTSSH object, please see <a href="../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh" class="MCXref xref">CFTSSH - Security profile</a>.

> **Note**
>
> If you are ready to generate a key pair, you can go directly to Use PKIKEYGEN to generate and import a key pair.

## Negotiating a secured session

To establish a secured session between the client and the server:

- When the client tries to connect to the server, the server returns the encryption protocols and supported versions (CFTSSH).
- The server additionally sends an asymmetric public key to the client so that the client can verify the authenticity of the server.
- The client and the server can then use an algorithm to share a symmetrical key to create the secured communication.

## Authenticating the user

Once the secured communication is established, the client must be authenticated. Authentication methods for SSH include passwords, keys, and a combined password/key authentication.

## Define the CFTSSH object

Use the CFTSSH command to describe a Transfer CFT security profile. The CFTSSH definition contains the SSH parameter connection for SFTP either in server or client mode. See [CFTSSH - security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh) for details.

## Transcoding in SFTP

The character conversion in text mode can be done at the requester or server level, either in a send or receive (as with PeSIT). You can configure the conversion using FCHARSET/NCHARSET or FCODE/NCODE, where transcoding is performed if FCODE differs from NCODE, or if FCHARSET differs from NCHARSET. The FCHARSET/NCHARSET parameters, however, take precedence over FCODE/NCODE if both are defined. Additionally:

- The charset and transferred file code are exchanged between the requester and server for two Transfer CFTs.
- A transfer restart is forbidden if the FCHARSET/NCHARSET conversion is done at the server level.
- The NCODE parameter is available in CFTRECV as with CFTSEND.

****SFTP versions****

- SFTP 3 and lower: There is no flag to open a file in text mode, so the text mode is selected through the IDF's FTYPE parameter. The newline conversion can be specified on the client side.
- SFTP 4 and higher: The client indicates if the transfer is done in binary or text mode. This overrides the IDF's FTYPE parameter. The newline conversion is done on the client side to accommodate the server requirement.
