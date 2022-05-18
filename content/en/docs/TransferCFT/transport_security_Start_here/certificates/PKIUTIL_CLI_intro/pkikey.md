---
title: "Using PKIKEY"
linkTitle: "Using PKIKEY"
weight: 280
--- This page describes how to use the PKIUTIL PKIKEY command.

## About PKIKEY

A private key is comprised of both a private and public key component. You can use this private key, as it contains two keys, for both the server and the client. However, only the public key portion is used for the client.

The PKIKEY command is similar to the PKICER command. Parameters include:

| Parameter  | Description  |
| --- | --- |
| PKIFNAME](../../../../c_intro_userinterfaces/command_summary/parameter_intro/pkifname) | The PKI database file ($CFTPKU by default) *only in command line* |
| [ID](../../../../c_intro_userinterfaces/command_summary/parameter_intro/id)  | The PKIKEY identifier  |
| [COMMENT](../../../../c_intro_userinterfaces/command_summary/parameter_intro/comment)  | Free comment  |
| [STATE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/state)  | The state of the imported key (ACT or INACT). You cannot use deactivated keys (state=INACT) for SFTP  |
| [IKDATA  | Use base- 64 data instead of a file (where the format corresponds with ikform)  |
| IKFORM](../../../../c_intro_userinterfaces/command_summary/parameter_intro/iform)  | The key format (DER, PEM, PKCS8, SSH or KPRIV). The "SSH" value includes the SSH2 format and the ssh- rsa format  |
| [IKNAME](../../../../c_intro_userinterfaces/command_summary/parameter_intro/ikname) | The key file to import *only in command line*  |
| [IKPUB | Text- only public key in ssh- rsa format *only in command line*  |
| [IKPASSW](../../../../c_intro_userinterfaces/command_summary/parameter_intro/ikpassw) | The key file protection password in PKCS8 or encrypted PEM (PKCS #5)  |
| [MODE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/mode)  | The action to perform (CREATE, REPLACE, DELETE) *only in command line*  |

> **Note**
>
> See the PKIKEYGEN command for details on how to generate and use your own keys.

### Restrictions

- Transfer CFT does not support keys that contain comments, regardless of if you are directly referencing or importing them.
- Transfer CFT does not support private keys with passphrases.
- Transfer CFT supports the RSA digital signature algorithm; however, ECDSA and DSA are not supported.

## Example uses

If you already have keys that you want to use, you can import them as described in the following sections.

****Import with PKCS8 format****

```
PKIUTIL PKIKEY ID=PRIVATE,COMMENT="My_note",IKFORM=PKCS8,IKPASSW="MyPassw", IKNAME=./conf/pki/private.pk8,MODE=CREATE
```

****Import with encrypted PEM (PKCS#5) format****

```
PKIUTIL PKIKEY ID=PRIVATE,COMMENT="My_note",IKFORM=PEM,IKPASSW="MyPassw", IKNAME=./conf/pki/private.pem,MODE=CREATE
```

`- - - - - BEGIN RSA PRIVATE KEY- - - - - `

`Proc- Type: 4,ENCRYPTED`

`DEK- Info: AES- 128- CBC,9E18D04529594FB617BC471F9958C8A7`

`<encrypted key data in base 64>`

`- - - - - END RSA PRIVATE KEY- - - - - - - - - `

> **Note**
>
> If a PEM encrypted key is generated using OpenSSL with FIPS, for example with "ssh- keygen", you cannot import it into Transfer CFT. To use this key, convert it to PKCS#8 using the command: openssl pkcs8 - topk8 - v2 aes128 - in &lt;key> - out &lt;key.pk8>

****Import with private.rsa format****

```
PKIUTIL PKIKEY ID=PRIVRSA, IKFORM=PEM, IKNAME=./private.rsa, MODE=CREATE
```

`- - - - - BEGIN RSA PRIVATE KEY- - - - - `

`MIICXwIBAAKBgQDDUPaQmmgTL90EaFPvzt9u/1AAxdeXKhTuH6QMTevV7dllkNHe `

`...`

`- - - - - END RSA PRIVATE KEY- - - - - `

****Import with public.ssh2 format****

```
PKIUTIL PKIKEY ID=PUBSSH2, IKFORM=SSH, IKNAME=./public.ssh2, MODE=CREATE
```

`- - - - BEGIN SSH2 PUBLIC KEY - - - - `

`AAAAB3NzaC1yc2EAAAADAQABAAAAgQDDUPaQmmgTL90EaFPvzt9u/1AAxdeXKhTuH`

`....`

`- - - - END SSH2 PUBLIC KEY - - - - `

****Import with public.ssh- rsa format****

```
PKIUTIL PKIKEY ID=PUBSSHRSA, IKFORM=SSH, IKNAME=./public.ssh- rsa, MODE=CREATE
```

`ssh- rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQDDUPaQmmgTL90EaFPvzt9u/1AAxdeXKhTuH6QMT...`

`BW4FzI2WRwuTK5vx4s2AF8+4wy7tKrR8kxHn2qnXB12ICh5/nnt2syjw== = KeyType=RSA Date=2017`

`0612 User=MyUser Comment=This is a free comment`

****Import with public.pem format****

```
PKIUTIL PKIKEY ID=PUBPEM, IKFORM=PEM, IKNAME=./public.pem, MODE=CREATE
```

> `- - - - - BEGIN PUBLIC KEY- - - - - `
>
> `MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDDUPaQmmgTL90EaFPvzt9u/1AA`
>
> `...`
>
> `- - - - - END PUBLIC KEY- - - - - `
