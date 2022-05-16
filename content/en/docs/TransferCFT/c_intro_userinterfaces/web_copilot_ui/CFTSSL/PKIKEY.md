---

    title: Keys - PKIKEY
    linkTitle: Keys - PKIKEY
    weight: 220

---
This page describes how to manage keys via the user interface.

> **Note**
>
> Some command line  parameters are not available in the user interface.

## About keys in {{< TransferCFT/suitevariablesTransferCFTName  >}}

A private key is comprised of both a private and public key component. You can use this private key, as it contains two keys, for both the server and the client. However, only the public key portion is used for the client.

The PKIKEY command is similar to the PKICER command. Parameters include:


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/pkifname">PKIFNAME</a>: | The PKI database file ($CFTPKU by default) *only in command line* |
| <a href="../../../command_summary/parameter_intro/id">ID</a>:  | The PKIKEY identifier  |
| <a href="../../../command_summary/parameter_intro/comment">COMMENT</a>:  | Free comment  |
| <a href="../../../command_summary/parameter_intro/state">STATE</a>:  | The state of the imported key (ACT or INACT). You cannot use deactivated keys (state=INACT) for SFTP  |
| <a href="">IKDATA</a>:  | Use base-64 data instead of a file (where the format corresponds with ikform)  |
| <a href="../../../command_summary/parameter_intro/iform">IKFORM</a>:  | The key format (DER, PEM, PKCS8, SSH or KPRIV). The "SSH" value includes the SSH2 format and the ssh-rsa format  |
| <a href="../../../command_summary/parameter_intro/ikname">IKNAME</a>: | The key file to import *only in command line*  |
| <a href="">IKPUB</a>: | Text-only public key in ssh-rsa format *only in command line*  |
| <a href="../../../command_summary/parameter_intro/ikpassw">IKPASSW</a>: | The key file protection password in PKCS8 or encrypted PEM (PKCS #5)  |
| <a href="../../../command_summary/parameter_intro/mode">MODE</a>:  | The action to perform (CREATE, REPLACE, DELETE) *only in command line*  |


> **Note**
>
> See the PKIKEYGEN command for details on how to generate and use your own keys.

### Restrictions

- Transfer CFT does not support keys that contain comments, regardless of if you are directly referencing or importing them.
- Transfer CFT does not support private keys with passphrases.
- Transfer CFT supports the RSA digital signature algorithm; however, ECDSA and DSA are not supported.
