{
    "title": "Using FIPS",
    "linkTitle": "Using FIPS",
    "weight": "170"
}Concepts
--------

Transfer CFT supports the use of a FIPS-compliant library. FIPS, Federal Information Processing Standard 140-2, is a standard that describes US Federal government requirements for certain IT products.

Requirements include a cryptographic module used in a security system to protect unclassified information in an IT system.

Prerequisites
-------------

Before beginning:

- Refer to Licenses, in the openssl licenses/openssl-fips-1.2.txt directory, for redistribution terms.
- Verify that your Transfer CFT has the FIPS option included in the key.

Procedure
---------

To enable Transfer CFT to use FIPS-compliant algorithms:

1. Activate the FIPS compliance option in Transfer CFT key.
1. Set the uconf parameter **cft.fips.enable_compliance** to **YES**.

### Option details

- Supported FIPS-compliant algorithms include:
    -   RSA 1024-4096 bits, 3DES, SHA-1, SHA-224, SHA-256, SHA-384, SHA-512, AES-128, AES-256

<!-- -->

- FIPS supports the following ciphers for SSL:
    -   61: TLS_RSA_WITH_AES_256_CBC_SHA256
    -   60: TLS_RSA_WITH_AES_128_CBC_SHA256
    -   59: TLS_RSA_WITH_NULL_SHA256
    -   53: TLS_RSA_WITH_AES_256_CBC_SHA
    -   47: TLS_RSA_WITH_AES_128_CBC_SHA
    -   10: TLS_RSA_WITH_3DES_EDE_CBC_SHA
    -   2: TLS_RSA_WITH_NULL_SHA
- The following non-FIPS-compliant algorithms are not supported:
    -   9: RSA DES SHA1
    -   5: TLS_RSA_WITH_RC4_128_SHA RSA RC4 SHA1
    -   4: TLS_RSA_WITH_RC4_128_MD5
    -   1: TLS_RSA_WITH_NULL_MD5
- Certificates using MD5 hashing are not accepted when FIPS is enabled.

Troubleshooting
---------------

****Copilot SSL connection to Central Governance does not work when FIPS is enabled****

This issue occurs because the private key is encrypted using triple DES (by default). However, the certificate is encrypted using 40-bit RC2, which is not an approved FIPS algorithm. To remedy:

> In OpenSSL use the `pkcs12 -descert` option to encrypt the PKCS12 certificate to triple DES (RC2-40). For example:
>
> ```
> pkcs12-export -in <your server cert>.pem -inkey <your server key>.pem -out mycert.p12 -descert
> ```

****Related topics****

- [About UCONF (the unified configuration tool)](../../../admin_intro/uconf)
