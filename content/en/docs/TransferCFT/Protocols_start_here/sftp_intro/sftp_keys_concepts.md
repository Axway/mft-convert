---
    "title": "SSH session concepts",
    "linkTitle": "SSH concepts",
    "weight": "140"
---
{{% TransferCFT/snippets/sftp_os_limitation%}}

An SSH session is established in two steps:

- Agree on and establish encryption to protect future communication.
- Authenticate the user and determine if access to the server should be granted.

If you are already familiar with SSH concepts are ready for details on configuring the CFTSSH object, please see [CFTSSH - Security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh).

> **Note**
>
> Note: If you are ready to generate a key pair, you can go directly to Use PKIKEYGEN to generate and import a key pair.

### Negotiating a secured session

To establish a secured session between the client and the server:

- The client tries to connect to the server, and the server returns the encryption protocols and supported versions (CFTSSH).
- The server additionally sends an asymmetric public key to the client so that the client can verify the authenticity of the server.
- The client and the server then use an algorithm to share a symmetrical key to create the secured communication.

### Authenticating the user

Once the secured communication is established, the client must be authenticated. Authentication methods for SSH include passwords, keys, and a combined password/key authentication (two-factor or dual authentication method).

### Define the CFTSSH object

Use the CFTSSH command to describe a Transfer CFT security profile. The CFTSSH definition contains the SSH parameter connection for SFTP either in server or client mode. See [CFTSSH - security profile](../../../c_intro_userinterfaces/web_copilot_ui/cftssl/cftssh) for details.
