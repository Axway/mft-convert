---
title: "Configuring  transport security: Start here"
linkTitle: "Configuring transport security"
weight: 150
--- This section describes how to configure Transfer CFT objects transport
security configuration. The following topics describe the objects that
are involved in the Transfer CFT transport security configuration. The
CFTPARM, CFTPROT, and CFTPART objects include parameters that are directly
associated with SSL.

- CFTPARM
    configuration
- CFTPART
    configuration
- [CFTPROT
    configuration](../../c_intro_userinterfaces/about_cftutil/configuring_cft_start_here/cftprot_command_line)
- [CFTSSL
    configuration](transport_security_cftssl)
- [Integrated
    PKI](#Implementing_a_PKI)
- [Delivered SSL templates](#Using)

This topic
provides basic information that you need to use transport security on
your Transfer CFT. The following security elements are defined
in this topic:

- [About
    transport security](#About_Transport_Security)
- [Security
    profiles](#Security_profiles)
- [Implementing
    a PKI](#Implementing_a_PKI)

<span id="About_Transport_Security"></span>

## About transport security

Transport security implementation is a Transfer CFT option that is protected
by the software key.

Implementing transport security has no impact on the applications using
the Transfer CFT services. It is imposed by the Transfer CFT configuration
and requires the installation of a public key infrastructure.

The SSL and TLS protocols come in between the file exchange protocol
(PeSIT, ODETTE) and the communication protocol TCP. All the functions
provided by the file exchange protocol are therefore conserved when security
is implemented (compression, additional transfer- associated message, open
mode,...).

The SSL and TLS protocols define the security suite, or cipher suite,
concept. A suite is identified by a number and designates one:

- Authentication
    algorithm
- Encryption algorithm
    to ensure confidentiality
- Hash
    algorithm to ensure integrity

The suites supported by Transfer CFT are described in the following
table:

| Suite  | Order used | Authentication  | Confidentiality  | Integrity  |
| - - - | - - - | - - - | - - - | - - - |
| 49199 **  | 1  | ECDHE + RSA authentication  | AES- 128 GCM  | SHA- 256  |
| 49200 **  | 2  | ECDHE + RSA authentication  | AES- 256 GCM  | SHA- 384  |
| 49191 **  | 3  | ECDHE + RSA authentication | AES- 128  | SHA- 256  |
| 49192**  | 4  | ECDHE + RSA authentication  | AES- 256  | SHA- 384  |
| 156 **  | 5  | RSA authentication  | AES 128 GCM  | SHA- 256  |
| 157 **  | 6  | RSA authentication  | AES 256 GCM  | SHA- 384  |
| 60*  | 7  | RSA authentication (512, 1024, 2048, or 4096)  | AES- 128  | SHA- 256  |
| 61*  | 8  | RSA authentication (512, 1024, 2048, or 4096)  | AES- 256  | SHA- 256  |
| 47 &lt;/td&gt;  | 9  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | AES- 128 &lt;/td&gt;  | SHA- 1 &lt;/td&gt;  |
| 53  | 10  | RSA authentication (512, 1024, 2048, or 4096)  | AES- 256  | SHA- 1  |
| 10 &lt;/td&gt;  | 11  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | Triple DES &lt;/td&gt;  | SHA- 1 &lt;/td&gt;  |
| 5 &lt;/td&gt;  | 12  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | SHA- 1 &lt;/td&gt;  |
| 4 &lt;/td&gt;  | 13  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | MD5 &lt;/td&gt;  |
| 59*  | 14  | RSA authentication (512, 1024, 2048, or 4096)  | None  | SHA- 256  |
| 2 &lt;/td&gt;  | 15  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | SHA- 1 &lt;/td&gt;  |
| 1 &lt;/td&gt;  | 16  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | MD5 &lt;/td&gt;  |

> **Note**
>
> \* To comply with security standards, as of Transfer CFT version 3.2.0 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively. This means that you cannot negotiate a session with another partner (monitor) that is using a TLS version lower than 1.2 with these cipher suites.

> **Note**
>
> \*\* These cipher suites are only available for Transfer CFT 3.2.2 and higher and are restricted to use with TLS 1.2.

Transfer CFT only processes X.509 certificates with an RSA signature.
Authentication can be one- way or mutual. It is one- way if only the server,
in physical connection terms, is authenticated by the client via its certificate.
It is mutual if the client is also authenticated by the server via its
certificate. It is the server that decides whether the client must be
authenticated.

<span id="Security_profiles"></span>

### Security profiles

A security profile combines the parameters used to negotiate the opening
of an SSL or TLS session. It is associated with the definition of a protocol
(CFTPROT command) in the following conditions:

For incoming calls, or server mode:

- One single profile per CFTPROT command
- Additional controls may be associated for each partner

For outgoing calls, or client mode:

- One default security profile per CFTPROT command
- A different profile may be forced for each partner

<span id="Implementing_a_PKI"></span>

### Implementing a PKI

Transfer CFT can be interfaced with an external PKI (Xcert…) or integrated
PKI. To interface Transfer CFT with
an external PKI, you must create a Transfer CFT user exit from the supplier
API.

The Transfer CFT integrated PKI provides
a certificate management solution comprising a:

- Certificate database
- Certificate database
    management utility (PKIUTIL)

The PKIUTIL certificate database management utility offers functions
allowing you to:

- Create and delete
    the database
- Install a trusted
    certificate in the database
- Install a user
    certificate and its associated private key in the database
- Delete a certificate
    from the database
- Enable/disable
    a certificate
- Display the database
    content, according to various criteria (authorities, validity date,...)

The certificate import command accepts the following formats:

- PEM, Privacy
    Enhanced Mail,
    of the Base 64 variety
- DER
- PKCS, Public
    Key Cryptographic
    Standards:
- PKCS #7 Cryptographic
    Message Syntax Standard used for encrypting the data
- PKCS#12 Personal
    Information Exchange Syntax Standard used for storing and transporting
    private keys, certificates, and so on
- PKCS#8 used
    for storing the private keys, encrypted or decrypted

Transfer CFT then accesses
this database whenever it needs to locate a user certificate, an authority
certificate - which is essential for checking the certificates received,
or a user private key. The private keys are encrypted in the certificate
database. Encryption is performed on the basis of a password supplied
by the user when the key was imported. This password is not recorded,
but must be declared in the Transfer CFT configuration.

Transfer CFT also offers an operating mode in which only confidentiality
and integrity are provided without certificate- based authentication. In
this operating mode, a PKI does not need to be implemented.

<span id="Using"></span>

### Using delivered SSL templates

Transfer CFT no longer delivers certificates and private key samples in the product packaging. However Transfer CFT does provide a template, which you can update and use as described in the delivered samples (`$CFTDIRRUNTIME\conf`). The file cft- pki.conf describes how to insert a certificate in the local database in the various formats. The file cft- tcp.conf contains general configuration, and commented SSL profiles.
