---
title: "Keys - PKIKEY"
linkTitle: "Keys - PKIKEY"
weight: 210
--- This page describes how to manage keys via the user interface.

> **Note**
>
> Some command line  parameters are not available in the user interface.

## About keys in {{< TransferCFT/suitevariablesTransferCFTName  >}}

A private key is comprised of both a private and public key component. You can use this private key, as it contains two keys, for both the server and the client. However, only the public key portion is used for the client.

The PKIKEY command is similar to the PKICER command. Parameters include:

| Parameter  | Description  |
| - - - | - - - |
| [PKIFNAME](../../../command_summary/parameter_intro/pkifname) | The PKI database file ($CFTPKU by default) *only in command line* |
| [ID](../../../command_summary/parameter_intro/id)  | The PKIKEY identifier  |
| [COMMENT](../../../command_summary/parameter_intro/comment)  | Free comment  |
| [STATE](../../../command_summary/parameter_intro/state)  | The state of the imported key (ACT or INACT). You cannot use deactivated keys (state=INACT) for SFTP  |
| [IKDATA]()  | Use base- 64 data instead of a file (where the format corresponds with ikform)  |
| [IKFORM](../../../command_summary/parameter_intro/iform)  | The key format (DER, PEM, PKCS8, SSH or KPRIV). The "SSH" value includes the SSH2 format and the ssh- rsa format  |
| [IKNAME](../../../command_summary/parameter_intro/ikname) | The key file to import *only in command line*  |
| [IKPUB]() | Text- only public key in ssh- rsa format *only in command line*  |
| [IKPASSW](../../../command_summary/parameter_intro/ikpassw) | The key file protection password in PKCS8 or encrypted PEM (PKCS #5)  |
| [MODE](../../../command_summary/parameter_intro/mode)  | The action to perform (CREATE, REPLACE, DELETE) *only in command line*  |

> **Note**
>
> See the PKIKEYGEN command for details on how to generate and use your own keys.

### Restrictions

- Transfer CFT does not support keys that contain comments, regardless of if you are directly referencing or importing them.
- Transfer CFT does not support private keys with passphrases.
- Transfer CFT supports the RSA digital signature algorithm; however, ECDSA and DSA are not supported.
