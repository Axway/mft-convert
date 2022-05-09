{
    "title": "Managing  certificates",
    "linkTitle": "Managing certificates",
    "weight": "160"
}<span id="What_you_will_find_in_this_book"></span>This topic
describes security certificates, and the following security concepts:

- [Overview
    of certificate standards](#Overview_of_Certificate_Standards)
- [Database
    protection](#Database_protection)

Central Governance certificate management
-----------------------------------------

When using Transfer CFT with Central Governance, certificate management is performed by {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. See[ {{< TransferCFT/suitevariablesCentralGovernanceName  >}} registration concepts](../../governance_services_intro/cg_register_overview) for details on certificates during installation and registration.

What is a certificate?
----------------------

A certificate is an electronic document used to identify an individual,
a server, a company, or other entity and to associate that identity with
a public key.

While communicating with any remote party, you need to be sure of the
correspondent’s identity before dealing with the data integrity and confidentiality.
In other words, forbidding the access to data and verifying that the data
received has not been modified is meaningless if you are not  sure
of the other party’s identity. Certificates help prevent the use of fake
public keys for impersonation. Only the public key certified by the certificate
will work with the corresponding private key possessed by the entity identified
by the certificate.

A certificate, more precisely a X509 certificate as defined by ISO,
is a data set that provides information about the owner and links them
it to his public key. To ensure that this information is correct, the
whole certificate is signed (data is hashed and the generated hash is
ciphered using an asymmetric algorithm such as RSA) by a third
party called the Certificate Authority (CA) which itself owns a public/private
key pair. It uses its private key to create the certificate signature
and makes its public key, which necessary to decrypt the signature, available
to anyone through its own certificate.

<span id="Overview_of_Certificate_Standards"></span>

Certificate standards overview
------------------------------

The following sections give an overview of certificate fields as defined
by ISO in the X509 standard.

Users don't usually need to be concerned about
the exact contents of a certificate. However, system administrators working
with certificates should be familiar with the information listed below
in this topic.

### Distinguished names

An X.509 v3 certificate binds a Distinguished Name
(DN) to a public key. A DN is a series of name-value pairs,
such as `uid=dupont`, that uniquely identify an entity. In
this case, it is called the certificate
subject.

For example, this could be a typical DN for a Axway employee:

`uid=dupont,e=dupont@Axway.com,cn=Albert Dupont ,o=Axway Corp.,c=FR`

The abbreviations before each equal sign in this example have the following
meanings:

- `uid`:
    User ID
- `e`:
    Mail address
- `cn`:The
    user's common name
- `o`:
    Organization
- `c`:
    Country

DNs may include a variety of other name/value pairs. They are used to
identify both certificate subjects and entries in directories that support
the Lightweight Directory
Access Protocol,
LDAP, the protocol generally used to access X.500 directories.

<span id="Certificate_fields"></span>

### Certificate fields

Every certificate consists of two sections, the data section and the
signature section as described below.

#### Certificate file description


| Certificate field  | Description  |
| --- | --- |
| Version  | The version of the encoded certificate value for X509v3 is 2.  |
| Serial Number  | An integer assigned by the CA. The issuer name and serial number must identify a unique certificate.  |
| Issuer Signature Algorithm  | The signature algorithm identifier used by the CA to sign the certificate (e.g., RSA with SHA-1).  |
| Issuer Distinguished Name | The DN of the entity who has signed and issued the certificate.  |
| Validity Period  | The dates on which the certificate becomes valid and on which the certificate ceases to be valid.  |
| Subject Distinguished Name  | The entity associated with the public key stored in the subject's public key field.  |
| Subject Public Key Information  | The public key and identifier algorithm with which the key is used.  |
| Issuer Unique Identifier (optional)  | To prevent the reuse of issuer name over time, a unique identifier is assigned to the CA.  |
| Subject Unique Identifier (optional)  | To prevent the reuse of subject name over time, a unique identifier is assigned to the CA.  |
| Extensions (optional)  | Extensions are additional information about the certificate. Extensions consist of three fields: type, criticality, and value. Type field is the ASN.1type of the data (for example, text string, numerical, date). The criticality flag is either critical or non-critical. If PKI client application cannot process a critical extension, the certificate will be rejected. The actual value is the value associated with the extension.  |


#### Signature description

The signature provides the following information:

- The cryptographic
    algorithm or cipher used by the issuing CA to create its own digital signature
- The CA's digital
    signature obtained by hashing all of the data in the certificate together
    and encrypting it with the CA's private key

****Example****

This displays the data and signature sections of a certificate in human-readable
format:

```
Certificate:
Data:
Version: v3 (0x2)
Serial Number: 3 (0x3)
Signature Algorithm: PKCS \#1 MD5 With RSA Encryption
Issuer: OU=Axway Certificate Authority, O=Axway, C=US
Validity:
Not Before: Fri Oct 17 18:36:25 2018
Not After: Sun Oct 17 18:36:25 2022
Subject: CN=Albert Dupont, OU=Marketing, O=Axway, C=US
Subject Public Key Info:
Algorithm: PKCS \#1 RSA Encryption
Public Key:
Modulus:
00:ca:fa:79:98:8f:19:f8:d7:de:e4:49:80:48:e6:2a:2a:86:
ed:27:40:4d:86:b3:05:c0:01:bb:50:15:c9:de:dc:85:19:22:
43:7d:45:6d:71:4e:17:3d:f0:36:4b:5b:7f:a8:51:a3:a1:00:
98:ce:7f:47:50:2c:93:36:7c:01:6e:cb:89:06:41:72:b5:e9:
73:49:38:76:ef:b6:8f:ac:49:bb:63:0f:9b:ff:16:2a:e3:0e:
9d:3b:af:ce:9a:3e:48:65:de:96:61:d5:0a:11:2a:a2:80:b0:
7d:d8:99:cb:0c:99:34:c9:ab:25:06:a8:31:ad:8c:4b:aa:54:
91:f4:15
Public Exponent: 65537 (0x10001)
Extensions:
Identifier: Certificate Type
Critical: no
Certified Usage:
SSL Client
Identifier: Authority Key Identifier
Critical: no
Key Identifier:
f2:f2:06:59:90:18:47:51:f5:89:33:5a:31:7a:e6:5c:fb:36:
26:c9
Signature:
Algorithm: PKCS \#1 MD5 With RSA Encryption
Signature:
6d:23:af:f3:d3:b6:7a:df:90:df:cd:7e:18:6c:01:69:8e:54:65:fc:06:
30:43:34:d1:63:1f:06:7d:c3:40:a8:2a:82:c1:a4:83:2a:fb:2e:8f:fb:
f0:6d:ff:75:a3:78:f7:52:47:46:62:97:1d:d9:c6:11:0a:02:a2:e0:cc:
2a:75:6c:8b:b6:9b:87:00:7d:7c:84:76:79:ba:f8:b4:d2:62:58:c3:c5:
b6:c1:43:ac:63:44:42:fd:af:c8:0f:2f:38:85:6d:d6:59:e8:41:42:a5:
4a:e5:26:38:ff:32:78:a1:38:f1:ed:dc:0d:31:d1:b0:6d:67:e9:46:a8:
dd:c4
```
<span id="Database_protection"></span>

Database protection
-------------------

A database record corresponds to a certificate, regardless of whether
it is from a root authority, an intermediate authority, or a user. In
the case of a user, the record also includes the private key associated
with the certificate's public key.

Records containing authority certificates, either root or intermediate,
are in plain format and simply sealed. Sealing is used to detect fraudulent
modifications, other than by the PKIUTIL utility.

Records containing user certificates are in plain format for the certificate
and encrypted for the private key. They are also sealed.

Private key encryption is based on a password that must be passed as
an argument in the PKICER command.

Complete CA certificate chains
------------------------------

In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and lower, you can perform a SSL transfer even if the certificate chain is not complete (not signed by a ROOT CA). However, for {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.0 and higher, the certificate chain must be complete for a transfer to succeed.

For more information, see [Unknown CA leads to a failed certificate verification](../../troubleshoot_intro/admin_troubleshooting_server/troubleshoot_security#Unknown)
