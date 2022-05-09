{
    "title": "CFTSSH - Security profile",
    "linkTitle": "SSH Security Profiles - CFTSSH",
    "weight": "190"
}Use the CFTSSH command to describe a security profile.

> **Note**
>
> Note: When using the SFTP protocol, the CFTSSH definition contains the SSH connection parameters for server or client mode.

Server
------

The CFTSSH object parameters for a server definition (DIRECT = SERVER).


| Parameter  | Description  |
| --- | --- |
| ID = identifier | Identifier of the security profile. |
| CIPHLIST = {(num, num, ..)} | List of allowed ciphers (encryption methods).<br/> Each value defines three algorithms:<br/> • Authentication algorithm<br/> • Encryption algorithm<br/> • Sealing algorithm<br/> This list is compared with the list proposed by the client in order of preference, for the purpose of determining the suite to be negotiated.<br/> {{< TransferCFT/axwayvariablesComponentLongName  >}} supports the following: aes256-ctr, aes192-ctr, aes128-ctr, aes256-cbc, aes192-cbc, aes128-cbc, 3des-cbc, blowfish-cbc.<br/> <blockquote> **Note**<br/> If the field is empty, the default list is: aes256-ctr, aes192-ctr, aes128-ctr, aes256-cbc, aes192-cbc, aes128-cbc.<br/> </blockquote>  |
| CLIPUBKEY  | When DIRECT=SERVER<br/> Key Id containing the client public key (RSA). When defined, the Transfer CFT server checks that the client public key referenced in CLIPUBKEY matches the public key provided by the client. If an error occurs, the connection is rejected with a DIAGI 433. |
| Comment  | Free comment.  |
| DIRECT<br/>  | The security profile is applicable in this mode (SERVER). |
| HMAC  | List of accepted HMAC (keyed-hash message authentication code).<br/> • Choose from the following: hmac-sha2-512, hmac-sha2-256, hmac-sha1, none.<br /> <br/> • If the field is empty, the default list is hmac-sha2-512, hmac-sha2-256, hmac-sha1. |
| KEYEXCHG  | List of key exchange methods to use.<br/> Choose from the following: curve25519-sha256@libssh.org, ecdh-sha2-nistp256, diffie-hellman-group14-sha1, diffie-hellman-group1-sha1. |
| MODE = {REPLACE &#124; CREATE &#124; DELETE} | Action for the command. For DELETE mode, the command is deleted from the PARAMETERS database; only the ID and DIRECT parameters are required. |
| ORIGIN = string  | This parameter indicates the object's origin.  |
| SRVPRIVKEY  | When DIRECT=SERVER:<br/> Key Id containing the server private key (RSA) to use with key authentication. |


Client
------

The CFTSSH object parameters for a client definition (DIRECT = CLIENT).


| Parameter  | Description  |
| --- | --- |
| ID = identifier | Identifier of the security profile. |
| CIPHLIST = {(num, num, ..)} | List of allowed ciphers (encryption methods).<br/> Each value defines three algorithms:<br/> • Authentication algorithm<br/> • Encryption algorithm<br/> • Sealing algorithm<br/> This list is compared with the list proposed by the client in order of preference, for the purpose of determining the suite to be negotiated.<br/> {{< TransferCFT/axwayvariablesComponentLongName  >}} supports the following: aes256-ctr, aes192-ctr, aes128-ctr, aes256-cbc, aes192-cbc, aes128-cbc, 3des-cbc, blowfish-cbc.<br/> <blockquote> **Note**<br/> If the field is empty, the default list is: aes256-ctr, aes192-ctr, aes128-ctr, aes256-cbc, aes192-cbc, aes128-cbc.<br/> </blockquote>  |
| CLIPRIVKEY  | When DIRECT=CLIENT Key Id containing the client private key (RSA) to use with key authentication. When defined, Transfer CFT uses key authentication. If an error occurs, the connection is rejected with a DIAGI 433.  |
| Comment  | Free comment.  |
| DIRECT | The security profile is applicable in this mode (CLIENT). |
| HMAC  | List of accepted HMAC (keyed-hash message authentication code).<br/> • Choose from the following: hmac-sha2-512, hmac-sha2-256, hmac-sha1, none.<br /> <br/> • If the field is empty, the default list is hmac-sha2-512, hmac-sha2-256, hmac-sha1. |
| KEYEXCHG  | List of key exchange methods to use. Choose from the following: curve25519-sha256@libssh.org, ecdh-sha2-nistp256, diffie-hellman-group14-sha1, diffie-hellman-group1-sha1.  |
| MODE = {REPLACE &#124; CREATE &#124; DELETE} | Action for the command. For DELETE mode, the command is deleted from the PARAMETERS database; only the ID and DIRECT parameters are required. |
| ORIGIN = string  | This parameter indicates the object's origin.  |
| SRVPUBKEY  | When DIRECT=CLIENT:<br/> Key Id containing the server public key (RSA) for the server. When defined, the Transfer CFT client checks that the public key referenced by SRVPUBKEY matches the key provided by the server.<br/> If an error occurs, the connection is rejected with a DIAGI 264. |


****Example 1****

This example demonstrates an SSH default profile that has no client key authentication (CLIPUBKEY is not defined). The server private key is referenced by the SRVPRIVKEY parameter (SSH_PRIV_KEY in the example). The SRVPRIVKEY value is a key identifier that corresponds to a key stored in the local PKI database.

```
CFTSSH ID = 'SSH_DEFAULT',
 DIRECT = 'SERVER',
 SRVPRIVKEY = 'SSH_PRIV_KEY',
...
 MODE = 'REPLACE'
```

****Example 2****

The next example demonstrates an SSH default profile that uses client key authentication (CLIPUBKEY is defined). The client public key is referenced by the CLIPUBKEY parameter (SSH_PUB_KEY). The CLIPUBKEY value is a key identifier that corresponds to a key stored in the local PKI database.

****Example 3****

The server private key is referenced by the SRVPRIVKEY parameter (SSH_PRIV_KEY in the example). The value is also a key identifier that corresponds to a key stored in the local PKI database.

```
CFTSSH ID = 'SSH_DEFAULT',
 DIRECT = 'SERVER',
 SRVPRIVKEY = 'SSH_PRIV_KEY',
  CLIPUBKEY = 'SSH_PUB_KEY',
 MODE = 'REPLACE'
```

****Example 4****

This example demonstrates an SSH default profile with no server key authentication (SRVPUBKEY is not defined), but where there is no client key authentication (CLIPRIVKEY is not defined).

```
CFTSSH ID = 'SSH_DEFAULT',
 DIRECT = 'CLIENT',
  MODE = 'REPLACE'
```

****Example 5****

This example demonstrates an SSH default profile with server key authentication (SRVPUBKEY is defined), but where there is no client key authentication (CLIPRIVKEY is not defined). The server public key is referenced by the SRVPUBKEY parameter (SSH_PUB_KEY in the example). The SRVPUBKEY value is a key identifier that corresponds to a key stored in the local PKI database.

```
CFTSSH ID = 'SSH_DEFAULT',
  DIRECT = 'CLIENT',
 SRVPUBKEY = 'SSH_PUB_KEY',
  MODE = 'REPLACE'
```

****Example 6****

This example demonstrates an SSH default profile with server key authentication (SRVPUBKEY is defined) and client key authentication (CLIPRIVKEY is defined).

- The server public key is referenced by the SRVPUBKEY parameter (SSH_PUB_KEY). The SRVPUBKEY value is a key identifier that corresponds to a key stored in the local PKI database.
- The client private key is referenced by the CLIPRIVKEY parameter (SSH_PRIV_KEY in this example). The CLIPRIVKEY value is a key identifier that corresponds to a key stored in the local PKI database.

```
CFTSSH ID = 'SSH_DEFAULT',
DIRECT = 'CLIENT',
SRVPUBKEY = 'SSH_PUB_KEY',
CLIPRIVKEY = 'SSH_PRIV_KEY',
MODE = 'REPLACE'
```
