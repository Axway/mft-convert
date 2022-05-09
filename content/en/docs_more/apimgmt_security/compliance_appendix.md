{
"title": "Compliant configuration settings for Policy Studio",
"linkTitle": "Compliant configuration settings for Policy Studio",
"weight": 100,
"date": "2019-11-25",
"description": "Reference to compliant settings for FIPS, NIST Suite B, and NIST Suite B TS."
}

Policy Studio ships with a useful tool that allows you to scan a configuration and identify items that do not comply with the FIPS, Suite B, and Suite B Top Secret security standards.  For example, if you have a large configuration where the non-FIPS algorithm MD5 has been selected, you can run the FIPS Compliance Validation Tool to identify all occurrences of this algorithm.

You can click on the warning message in the report to jump directly to the offending configuration item.  Using the details in this topic, you can quickly enter or select a compliant algorithm or cipher suite to be FIPS-compliant.

While running in FIPS mode, the runtime blocks any attempts to use non-FIPS-compliant algorithms.  The Compliance Validation Tool should be used before a configuration is deployed to identify potential problems at configuration time rather than waiting to diagnose runtime errors.

## Compliance abbreviations

The following are common compliance abbreviations:

| Abbreviation | Meaning                                        |
|--------------|------------------------------------------------|
| FIPS         | Federal Information Processing Standards       |
| SB S         | Suite B Secret                                 |
| SB TS        | Suite B Top Secret                             |
| NIST         | National Institute of Standards and Technology |

## HTTPS interface, SMTP interface, ICAP server, Connection filter, Connect to URL filter

**Field**: Ciphers (Advanced SSL Tab)

FIPS compliant options:

* FIPS
* FIPS:!SSLv3
* FIPS:!SSLv3:!aNULL

Suite B compliant options:

* ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA
* ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256

Suite B TS compliant options:

* ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA
* ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384

### FIPS cipher suites

The default ciphers macro of `FIPS:!SSLv3:!aNULL`:

* Enables FIPS-compatible cipher suites only
* Explicitly blocks cipher suites that require SSLv3 or lower
* Forces the use of TLSv1.2 only
* Forbids unauthenticated cipher suites

The following table shows the SSL/TLS protocol version, key exchange, authentication, encryption, and MAC algorithms that are used by each supported cipher suite.

| Cipher Suite                  | SSL     | Kx         | Auth  | Enc         | MAC    |
|-------------------------------|---------|------------|-------|-------------|--------|
| ECDHE-RSA-AES256-GCM-SHA384   | TLSv1.2 | ECDH       | RSA   | AESGCM(256) | AEAD   |
| ECDHE-ECDSA-AES256-GCM-SHA384 | TLSv1.2 | ECDH       | ECDSA | AESGCM(256) | AEAD   |
| ECDHE-RSA-AES256-SHA384       | TLSv1.2 | ECDH       | RSA   | AES(256)    | SHA384 |
| ECDHE-ECDSA-AES256-SHA384     | TLSv1.2 | ECDH       | ECDSA | AES(256)    | SHA384 |
| DHE-DSS-AES256-GCM-SHA384     | TLSv1.2 | DH         | DSS   | AESGCM(256) | AEAD   |
| DHE-RSA-AES256-GCM-SHA384     | TLSv1.2 | DH         | RSA   | AES(256)    | SHA256 |
| DHE-RSA-AES256-SHA256         | TLSv1.2 | DH         | RSA   | AES(256)    | SHA256 |
| DHE-DSS-AES256-SHA256         | TLSv1.2 | DH         | DSS   | AES(256)    | SHA256 |
| ECDH-RSA-AES256-GCM-SHA384    | TLSv1.2 | ECDH/RSA   | ECDH  | AESGCM(256) | AEAD   |
| ECDH-ECDSA-AES256-GCM-SHA384  | TLSv1.2 | ECDH/ECDSA | ECDH  | AESGCM(256) | AEAD   |
| ECDH-RSA-AES256-SHA384        | TLSv1.2 | ECDH/RSA   | ECDH  | AES(256)    | SHA384 |
| ECDH-ECDSA-AES256-SHA384      | TLSv1.2 | ECDH/ECDSA | ECDH  | AES(256)    | SHA384 |
| AES256-GCM-SHA384             | TLSv1.2 | RSA        | RSA   | AESGCM(256) | AEAD   |
| AES256-SHA256                 | TLSv1.2 | RSA        | RSA   | AES(256)    | SHA256 |
| ECDHE-RSA-AES128-GCM-SHA256   | TLSv1.2 | ECDH       | RSA   | AESGCM(128) | AEAD   |
| ECDHE-ECDSA-AES128-GCM-SHA256 | TLSv1.2 | ECDH       | ECDSA | AESGCM(128) | AEAD   |
| ECDHE-RSA-AES128-SHA256       | TLSv1.2 | ECDH       | RSA   | AES(128)    | SHA256 |
| ECDHE-ECDSA-AES128-SHA256     | TLSv1.2 | ECDH/ECDSA | ECDH  | AES(128)    | SHA256 |
| DHE-DSS-AES128-GCM-SHA256     | TLSv1.2 | DH         | DSS   | AESGCM(128) | AEAD   |
| DHE-RSA-AES128-GCM-SHA256     | TLSv1.2 | DH         | RSA   | AESGCM(128) | AEAD   |
| DHE-RSA-AES128-SHA256         | TLSv1.2 | DH         | RSA   | AES(128)    | SHA256 |
| DHE-DSS-AES128-SHA256         | TLSv1.2 | DH         | DSS   | AES(128)    | SHA256 |
| ECDH-RSA-AES128-GCM-SHA256    | TLSv1.2 | ECDH/RSA   | ECDH  | AESGCM(128) | AEAD   |
| ECDH-ECDSA-AES128-GCM-SHA256  | TLSv1.2 | ECDH/ECDSA | ECDH  | AESGCM(128) | AEAD   |
| ECDH-RSA-AES128-SHA256        | TLSv1.2 | ECDH/RSA   | ECDH  | AES(128)    | SHA256 |
| ECDH-ECDSA-AES128-SHA256      | TLSv1.2 | ECDH/ECDSA | ECDH  | AES(128)    | SHA256 |
| AES128-GCM-SHA256             | TLSv1.2 | RSA        | RSA   | AESGCM(128) | AEAD   |
| AES128-SHA256                 | TLSv1.2 | RSA        | RSA   | AES(128)    | SHA256 |

### Suite B cipher suites

The following cipher suites are Suite B compliant.

| Cipher Suite                  | SSL     | Kx   | Auth  | Enc         | MAC    |
|-------------------------------|---------|------|-------|-------------|--------|
| ECDHE-ECDSA-AES128-GCM-SHA256 | TLSv1.2 | ECDH | ECDSA | AESGCM(128) | AEAD   |
| ECDHE-ECDSA-AES128-SHA256     | TLSv1.2 | ECDH | ECDSA | AES(128)    | SHA256 |
| ECDHE-ECDSA-AES128-SHA        | SSLv3   | ECDH | ECDSA | AES(128)    | SHA1   |

The Validation Tool will consider the following cipher strings as compliant:

* ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256
* ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA

{{< alert title="Note" color="primary" >}}The ECDHE-ECDSA-AES128-SHA cipher suite uses the SSLv3 protocol and is therefore not recommended.{{< /alert >}}

### Suite B TS cipher suites

The following cipher suites are Suite B TS compliant.

| Cipher Suite                  | SSL     | Kx   | Auth  | Enc         | MAC    |
|-------------------------------|---------|------|-------|-------------|--------|
| ECDHE-ECDSA-AES256-GCM-SHA384 | TLSv1.2 | ECDH | ECDSA | AESGCM(256) | AEAD   |
| ECDHE-ECDSA-AES256-SHA384     | TLSv1.2 | ECDH | ECDSA | AES(256)    | SHA384 |
| ECDHE-ECDSA-AES256-SHA        | SSLv3   | ECDH | ECDSA | AES(256)    | SHA1   |

The Validation Tool will consider the following cipher strings as compliant:

* ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384
* ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA

{{< alert title="Note" color="primary" >}}The ECDHE-ECDSA-AES256-SHA cipher suite uses the SSLv3 protocol and is therefore not recommended.{{< /alert >}}

## JMS service – Active MQ

It is possible to select any number of the ciphers listed in the FIPS column, which means that JMS service supports all the selected ciphers.

**Field**: Cipher suite

FIPS compliant options:

* SSL_DHE_RSA_WITH_3DES_EDE_CBC_SHA
* SSL_RSA_WITH_3DES_EDE_CBC_SHA
* TLS_DHE_DSS_WITH_AES_128_CBC_SHA
* TLS_DHE_DSS_WITH_AES_256_CBC_SHA
* TLS_DHE_RSA_WITH_AES_128_CBC_SHA
* TLS_DHE_RSA_WITH_AES_256_CBC_SHA
* TLS_ECDHE_ECDSA_WITH_3DES_EDE_CBC_SHATLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
* TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
* TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
* TLS_ECDH_ECDSA_WITH_3DES_EDE_CBC_SHA
* TLS_ECDH_ECDSA_WITH_AES_128_CBC_SHA
* TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA
* TLS_ECDH_RSA_WITH_3DES_EDE_CBC_SHA
* TLS_ECDH_RSA_WITH_AES_128_CBC_SHA
* TLS_ECDH_RSA_WITH_AES_256_CBC_SHA  
* TLS_RSA_WITH_AES_128_CBC_SHA
* TLS_RSA_WITH_AES_256_CBC_SHA

Suite B compliant options:

None as all ciphers in CBC mode.

Suite B TS compliant options:

None as all ciphers in CBC mode.

## JMS service – IBM MQ

The IBM MQ provider only allows you to select one of the following FIPS-compliant ciphers.

**Field**: Cipher suite

FIPS compliant options:

* SSL_RSA_WITH_3DES_EDE_CBC_SHA
* SSL_RSA_WITH_AES_128_CBC_SHA256SSL_RSA_WITH_AES_256_CBC_SHA256

Suite B compliant options:

None as all ciphers in CBC mode.

Suite B TS compliant options:

None as all ciphers in CBC mode.

## Embedded Active MQ

See [JMS service – Active MQ](#jms-service-active-mq).

## PGP Encrypt and Sign

The following symmetric key encryption and hash algorithms are compliant.

**Field**: Symmetric Key Algorithm

FIPS compliant options:

* AES128
* AES192
* AES256
* CAST5
* TRIPLE_DES

Suite B compliant options:

N/A.

Suite B TS compliant options:

N/A.

Despite the fact that the PGP Encrypt filter supports the Suite B and Suite B TS symmetric block ciphers AES256 and AES384, the PGP specification mandates the use of CFB (Cipher FeedBack) mode for symmetric encryption.  Since the NIST recommendation states that GCM is the preferred mode, the PGP Encrypt filter cannot be Suite B compliant.

**Field**: HASH Algorithm

FIPS compliant options:

* SHA1
* SHA224
* SHA256
* SHA384
* SHA512

Suite B compliant options:

* SHA256

Suite B TS compliant options:

* SHA384

## XML Encryption filter, XML Encryption Settings filter, XML Decryption, XML Decryption Settings filter

**FIPS compliance**: All algorithms available on the XML Encryption Settings filter are FIPS-compliant.

**Suite B and Suite B TS compliance**: The XML Encryption filter supports the XML Encryption 1.0 specification, which only supports symmetric ciphers in CBC (Cipher Block Chaining) mode, whereas Suite B requires GCM (Galois Counter Mode).  The XML Encryption support is, therefore, not Suite B compliant.

## XML Signature Generation filter

**FIPS compliance**: All algorithms available on the XML Signature Generation filter are FIPS-compliant.

**Suite B and Suite B TS compliance**: Symmetric signatures are not Suite B compliant because the required key wrap algorithms use 3DES or AES in CBC mode.

The following asymmetric Signature Methods are compliant.

| Field            | Suite B     | Suite B TS  |
|------------------|-------------|-------------|
| Signature Method | EcdsaSha256 | EcdsaSha384 |
| Digest Algorithm | Sha256      | Sha384      |

## XML Signature Verification filter

**FIPS compliance**: The filter is FIPS-compliant for verification of all XML Signatures.

**Suite B and Suite B TS compliance**: The XML Signature Verification filter uses the WS-SecurityPolicy-based Algorithm Suite configuration setting to mandate the crypto algorithms used on the XML Signature it is verifying.  These algorithms are not compliant because:

* Suite B requires the Signature Method to be ECDSAwithSHA256 or ECDSAwithSHA384, but only RsaSha1 is supported by the WS-SecurityPolicy algorithms.
* Only the SHA1 and SHA256 digest algorithms are supported by the Algorithm Suites, neither of which are Top Secret compliant.
* The algorithm suites mandate the use of KwRsa15 or KwRsaOaep, which both use 3DES or AES in CBC mode.

## KeyInfo configuration

The KeyInfo configuration is available on several filters, for example, XML Signature Generation, XML Encryption Settings, SAML Attribute/Authentication/Authorization Assertion Validation filters.

The following Security Token Reference mechanisms all use SHA1 and are not Suite B or Suite B TS compliant:

* EncryptedKeySHA1
* ThumbprintSHA1
* Kerberosv5APREQSHA1

## Create Thumbprint filter

**FIPS compliance**: All available digest algorithms are FIPS-compliant.

**Suite B compliance**: You must select SHA-256 to be compliant.

**Suite B TS compliance**: You must select SHA-384 to be compliant.

## S/MIME Encrypt filter

Any one of the following ciphers can be entered in the **Cipher** field to be compliant.

**Field**: Ciphers (Advanced SSL Tab)

FIPS compliant options:

* AES-128-CBC
* AES-128-CFB
* AES-128-CFB1
* AES-128-CFB8
* AES-128-CTR
* AES-128-ECB
* AES-128-GCM
* AES-128-OFB
* AES-128-XTS
* AES-192-CBC
* AES-192-CFB
* AES-192-CFB1
* AES-192-CFB8
* AES-192-CTR
* AES-192-ECB
* AES-192-GCM
* AES-192-OFB
* AES-256-CBC
* AES-256-CFB
* AES-256-CFB1
* AES-256-CFB8
* AES-256-CTR
* AES-256-ECB
* AES-256-GCM
* AES-256-OFB
* AES-256-XTS
* AES128
* AES192
* AES256
* DES-EDE3
* DES-EDE3-CBC
* DES-EDE3-CFB
* DES-EDE3-CFB1
* DES-EDE3-CFB8
* DES-EDE3-OFB
* DES3
* id-aes128-GCM
* id-aes192-GCM
* id-aes256-GCM

Suite B compliant options:

* id-aes128-GCM
* aes-128-gcm

Suite B TS compliant options:

* id-aes256-GCM
* aes-256-gcm

## S/MIME Decrypt filter

It is not possible to tell at configuration time if this filter is not compliant because the algorithm used to encrypt the message is encapsulated in a PKCS#7 structure as the value of the `keyEncryptionAlgorithm` parameter.  It is important to note that the API Gateway runtime will block any non FIPS-compliant algorithms. See the [S/MIME Encrypt filter](#s-mime-encrypt-filter) for a complete list of compliant algorithms.

## S/MIME Sign filter

**FIPS compliance**: The S/MIME Sign Filter uses SHA-1 internally to sign messages.  This algorithm is not configurable, but is FIPS-compliant.

**Suite B and Suite B TS compliance**: Because SHA-1 is not Suite B compliant and this algorithm is not configurable, the S/MIME Sign filter is not Suite B or Suite B TS compliant.

## S/MIME Verify filter

The S/MIME Verify filter assumes the use of SHA-1 and so is FIPS-compliant, but not Suite B or Suite B TS compliant.

## HTTP Digest Authentication filter

The HTTP Digest Authentication specification mandates the use of MD5 to digest the user name and password sent up in the HTTP Authorization header.  Therefore, it is not FIPS, Suite B, or Suite B TS compliant.

## Scripting filter

Custom scripting code is not covered by this tool and must be scanned manually.

## WS-Security Username Authentication

The WSS Username specification mandates the use of SHA-1 to digest the user’s password.

*Section 3.1 Usernames and Passwords* states:

> Passwords of type PasswordDigest are defined as being the Base64 encoded, **SHA-1 hash value**, of the UTF8 encoded password (or equivalent). However, unless this digested password is sent on a secured channel or the token is encrypted, the digest offers no real additional security over use of wsse:PasswordText.

## Kerberos settings

**FIPS compliance**: If the `krb5.conf` file contains a `default_tgs_enctypes` property with any of the following values, it is not FIPS-compliant:

* des-cbc-md5
* des-cbc-crc
* rc4-hmac

**Suite B and Suite B TS compliance**: Kerberos encryption algorithms are not Suite B or Suite B TS compliant.

## Kerberos Service Authentication

**FIPS compliance**: It is not possible to tell at configuration time if a Kerberos Service Authentication configuration item is FIPS-compliant since it depends on the algorithms negotiated at runtime.

**Suite B and Suite B TS compliance**: No Kerberos Service Authentication configuration is Suite B compliant because the required algorithms are not supported.

## SiteMinder SMHost configuration

**FIPS compliance**: If the SiteMinder `SMHost.conf` file contains one of the following prefixes on the value of the `sharedsecret` property, it is not FIPS-compliant:

* {RC4}
* {RC2}
* {DES}

**Suite B and Suite B TS compliance**: The SiteMinder configuration is not Suite B compliant since the required algorithms are not supported.

## Database Authentication Repository

**FIPS compliance**: The **Hash Algorithm** (used to hash the client's password) cannot be set to MD5 since this algorithm is not FIPS-compliant.

**Suite B and Suite B TS compliance**: The Database Authentication Repository is not Suite B compliant if you have elected to hash passwords by checking the **Hash client password** radio button since neither MD5 nor SHA1 are Suite B compliant.
