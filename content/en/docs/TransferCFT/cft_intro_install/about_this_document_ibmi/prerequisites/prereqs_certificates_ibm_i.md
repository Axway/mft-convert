---

    title: Certificates
    linkTitle: Certificates
    weight: 180

---
## Delivered certificates

The use of the default certificate supplied with {{< TransferCFT/axwayvariablesComponentShortName  >}} is strongly discouraged in a production environment. You should use your own certificates to enhance security.

> **Note**
>
> Caution  
> You must store certificates on the native side of the machine. Certificates located on the IFS partition are not supported.

## Create a PEM certificate for IBM i

For a PEM certificate, you must create a file with a record length equal to the size of the certificate in bytes. You can then upload the certificate to the newly created file.

****Example
&lt;/b>&lt;/p>****

In this example, assume that your certificate <span class="code">`2k_l1_user1_key.pem`</span> size is 1,191 bytes. Before uploading this certificate to the IBM i server, you would need to create a file with a record length of 1,191 bytes, as follows:

```
CRTPF FILE(YOURLIB/PEM_CERT) RCDLEN(1191)
```

You can use FTP, for example, to then upload <span class="code">`2k_l1_user1_key.pem`</span> to <span class="code">`YOURLIB/PEM_CERT`</span>.

- You must transfer PEM certificates in ASCII mode
- All other certificates can be transferred in binary mode

## Upload certificates to iSeries

You can use 3 type of certificates with Transfer CFT IBM i - PEM, DER, and P12, which must be stored on the native partition. Before you upload a certificate though, you need to know if it is binary or text, as the process differs depending on the format. For example, PEM certificates are in text, while the p12 certificates are in binary format.

### Using PEM (ASCII) certificates and keys

To use an ASCII certificate with {{< TransferCFT/axwayvariablesComponentLongName  >}}, perform the steps in this section.

On the Unix/Windows machine:

1. Use a text editor, such as Notepad, to open the certificate and modify it so that is has only one line, and save.
1. Use a text editor, such as Notepad, to open the private key and modify it so that is has only one line, and save.
1. Use FTP to upload the certificate and key files (in ASCII mode) to the iSeries machine.

For example:

```
FTP open <HOST>
cd CFTPROD
ascii
put USER.pem USERPEM
put USERK.pem USERKPEM
```

> **Note**
>
> If you have multiple certificates, repeat the process for each.

### Using binary certificates and keys

#### P12

Use FTP to upload the certificate file (in binary mode) to the iSeries machine. For example:

```
FTP OPEN <HOST>
cd CFTPROD
binary
put USER.P12 USERP12
```

#### DER

Use FTP to upload the certificate and key files (in binary mode) to the iSeries machine. For example:

```
FTP open <HOST>
cd CFTPROD
binary
put USER.der USERDER
put USERK.der USERKDER
```
