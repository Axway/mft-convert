---

    title: Using FIPS
    linkTitle: Using FIPS
    weight: 180

---
## Concepts

Transfer CFT supports the use of a FIPS-compliant library. FIPS, Federal Information Processing Standard 140-2, is a standard that describes US Federal government requirements for certain IT products.

Requirements include a cryptographic module used in a security system to protect unclassified information in an IT system.

## Prerequisites

Before beginning:

- Refer to Licenses, in the openssl licenses/openssl-fips-1.2.txt directory, for redistribution terms.
- Verify that your Transfer CFT has the FIPS option included in the key.

## Procedure

To enable Transfer CFT to use FIPS-compliant algorithms:

1. Activate the FIPS compliance option in Transfer CFT key.
1. Set the uconf parameter **cft.fips.enable\_compliance** to **YES**.

### Option details

- Supported FIPS-compliant algorithms include:
    -   RSA 1024-4096 bits, 3DES, SHA-1, SHA-224, SHA-256, SHA-384, SHA-512, AES-128, AES-256

<!-- -->

- FIPS supports the following ciphers for SSL:
    -   61: TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256
    -   60: TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256
    -   59: TLS\_RSA\_WITH\_NULL\_SHA256
    -   53: TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA
    -   47: TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA
    -   10: TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA
    -   2: TLS\_RSA\_WITH\_NULL\_SHA
- The following non-FIPS-compliant algorithms are not supported:
    -   9: RSA DES SHA1
    -   5: TLS\_RSA\_WITH\_RC4\_128\_SHA RSA RC4 SHA1
    -   4: TLS\_RSA\_WITH\_RC4\_128\_MD5
    -   1: TLS\_RSA\_WITH\_NULL\_MD5
- Certificates using MD5 hashing are not accepted when FIPS is enabled.

## Troubleshooting

****Copilot SSL connection to Central Governance does not work when FIPS is enabled****

This issue occurs because the private key is encrypted using triple DES (by default). However, the certificate is encrypted using 40-bit RC2, which is not an approved FIPS algorithm. To remedy:

> In OpenSSL use the <span class="code">`pkcs12 -descert`</span> option to encrypt the PKCS12 certificate to triple DES (RC2-40). For example:
>
> ```
> pkcs12-export -in <your server cert>.pem -inkey <your server key>.pem -out mycert.p12 -descert
> ```

****Related topics****

- [About UCONF (the unified configuration tool)](../../../admin_intro/uconf)
