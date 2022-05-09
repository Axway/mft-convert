{
"title": "Additional encryption filters",
"linkTitle": "Additional encryption filters",
"weight": 95,
"date": "2019-10-17",
"description": "Additional encryption filters, including JWT and SMIME encryption and decryption."
}

## JWT decrypt filter

You can use the **JWT Decrypt** filter to decrypt encrypted JWTs.

Upon successful decryption, the filter removes all metadata, such as headers and encryption-specific information of the incoming encrypted JWT, and outputs the originally encrypted payload.

For example, when you decrypt the following JWE Payload:

```
eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZHQ00ifQ.

OKOawDo13gRp2ojaHV7LFpZcgV7T6DVZKTyKOMTYUmKoTCVJRgckCL9kiMT03JGe

ipsEdY3mx_etLbbWSrFr05kLzcSr4qKAq7YN7e9jwQRb23nfa6c9d-StnImGyFDb

Sv04uVuxIp5Zms1gNxKKK2Da14B8S4rzVRltdYwam_lDp5XnZAYpQdb76FdIKLaV

mqgfwX7XWRxv2322i-vDxRfqNzo_tETKzpVLzfiwQyeyPGLBIO56YJ7eObdv0je8

1860ppamavo35UgoRdbYaBcoh9QcfylQr66oc6vFWXRcZ_ZT2LawVCWTIy3brGPi

6UklfCpIMfIjf7iGdXKHzg.

48V1_ALb6US04U3b.

5eym8TW_c8SuK0ltJ3rpYIzOeDQz7TALvtu6UG9oMo4vpzs9tX_EFShS8iB7j6ji

SdiwkIr3ajwQzaBtQD_A.

XFBoMYUZodetZdvTiFvSkQ
```

The output is:

```
The true sign of intelligence is not knowledge but imagination.
```

{{< alert title="Note" color="primary" >}}The JWT Decrypt filter automatically detects whether the input JWT is encrypted with symmetric or asymmetric key and automatically uses the corresponding settings. For example, you can configure decryption with symmetric key and certificate; however, the filter uses the former or latter depending on the type of JWE it receives as input.{{< /alert >}}

Configure the following field on the **JWT Decrypt** window:

**Name**:

Enter an appropriate name for the filter to display in a policy.

**Token location**:

Enter the selector expression to obtain the JWT to be decrypted.

### Decryption using key selection

Optionally, configure the following fields in the **Key selection** section:

**X509 certificate**:

Select the certificate from the certificate store that is used to decrypt the payload.

**Selector expression**:

Alternatively, enter a selector expression to retrieve the alias of the certificate in the certificate store.

### Shared key selection details

Optionally, configure the following fields in the **Shared key selection** section:

**None**:

Select if you do not want to decrypt tokens that are encrypted with shared keys.

**Shared key**:

Enter the shared key that is used to encrypt the payload. The key should be given as a base64-encoded byte array.

**Selector expression**:

Alternatively, enter a selector expression to obtain the shared key. The value returned by the selector should contain:

* Byte array (possibly produced by a different filter)
* Base64-encoded byte array

## JWT encrypt filter

You can use the **JWT Encrypt** filter to encrypt arbitrary content (for example, a JWT claims set). The result of the encryption is called JSON Web Encryption (JWE).

JWE represents encrypted content using JSON-based data structures. The JWE cryptographic mechanisms encrypt and provide integrity protection for an arbitrary payload.

A JWE represents the following logical values:

* JOSE Header
* JWE Encrypted Key
* JWE Initialization Vector
* JWE Ciphertext
* JWE Authentication Tag

The encrypted content is outputted in JWE Compact Serialization format, which is produced by base64-encoding the logical values and concatenates them with a period (‘.’) in between:

```
BASE64URL(UTF8(JWE Protected Header)) ‚.‘

BASE64URL(JWE Encrypted Key) ‚.‘

BASE64URL(JWE Initialization Vector) ‚.‘

BASE64URL(JWE Ciphertext) ‚.‘

BASE64URL(JWE Authentication Tag)
```

For example, when you use the RSAES-OAEP algorithm and AES GCM encryption method to encrypt the following content (the JWE Payload):

```
The true sign of intelligence is not knowledge but imagination.
```

The following string is returned:

```
eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZHQ00ifQ.

OKOawDo13gRp2ojaHV7LFpZcgV7T6DVZKTyKOMTYUmKoTCVJRgckCL9kiMT03JGe

ipsEdY3mx_etLbbWSrFr05kLzcSr4qKAq7YN7e9jwQRb23nfa6c9d-StnImGyFDb

Sv04uVuxIp5Zms1gNxKKK2Da14B8S4rzVRltdYwam_lDp5XnZAYpQdb76FdIKLaV

mqgfwX7XWRxv2322i-vDxRfqNzo_tETKzpVLzfiwQyeyPGLBIO56YJ7eObdv0je8

1860ppamavo35UgoRdbYaBcoh9QcfylQr66oc6vFWXRcZ_ZT2LawVCWTIy3brGPi

6UklfCpIMfIjf7iGdXKHzg.

48V1_ALb6US04U3b.

5eym8TW_c8SuK0ltJ3rpYIzOeDQz7TALvtu6UG9oMo4vpzs9tX_EFShS8iB7j6ji

SdiwkIr3ajwQzaBtQD_A.

XFBoMYUZodetZdvTiFvSkQ
```

Configure the following field on the **JWT Encrypt** window:

**Name**:

Enter an appropriate name for the filter to display in a policy.

### Encryption details

Configure the following fields **Encryption details** section:

**Token location**:

Enter a selector expression to retrieve the payload to be signed. The content can be JWT claims, encrypted token, or you can enter a different option.

**Key type**:

Select whether to encrypt using a asymmetric key, symmetric key, or JSON Web Key.

### Asymmetric key details

If you have selected the asymmetric key type, configure the following fields in the **Asymmetric** section:

**X509 certificate**:

Select the certificate from the certificate store that is used to encrypt the payload.

**Selector expression**:

Alternatively, enter a message attribute containing the alias of the certificate in the certificate store.

**Algorithm**:

Select the algorithm to use for encryption.

**Encryption method**:

Select the method to use for encryption.

### Symmetric key details

If you have selected the symmetric key type, configure the following fields on the **Symmetric** section:

**Shared key**:

Enter the shared key that should be used to encrypt the payload. The key should be given as a base64-encoded byte array and must must have a specific length depending on the algorithm selected for encryption:

| Algorithm                                                       | Key length          |
|-----------------------------------------------------------------|---------------------|
| AES key wrap with default initial value of 128 bit key (A128KW) | 16 bytes (128 bits) |
| AES key wrap with default initial value of 192 bit key (A192KW) | 24 bytes (192 bits) |
| AES key wrap with default initial value of 256 bit key (A256KW) | 32 bytes (256 bits) |
| Key wrap with AES GCM of 128-bit key (A128GCMKW)                | 16 bytes (128 bits) |
| Key wrap with AES GCM of 192-bit key (A192GCMKW)                | 24 bytes (192 bits) |
| Key wrap with AES GCM of 256-bit key (A256GCMKW)                | 32 bytes (256 bits) |
| Direct use of a shared symmetric key (dir)                      | 32 bytes (256 bits) |

**Selector expression**:

Alternatively, enter a selector expression to retrieve the shared key. The value returned by the selector should contain:

* Byte array (possibly produced by a different filter)
* Base64-encoded byte array

**Algorithm**:

Select the algorithm used to encrypt.

**Encryption method**:

Select the method used tp encrypt.

### JWK details

If you have selected the JWK key type, complete the following fields in the **JWK details** section:

**JSON Web key**:

Enter a selector expression to retrieve the JSON Web Key used to encrypt the payload. JWKs can be retrieved with the **Connect to URL** filter.

**Algorithm**:

Select the algorithm to use for encryption.

* To use the algorithm specified in the JWK, select **Always use JWK algorithm**. If no algorithm is specified within the JWK, the filter will fail.
* To always use the algorithm specified in the JWK (if one is defined), but default to the algorithm specified in the **Algorithm** field (if no algorithm is specified in the JWK), select **Use JWK algorithm if available, but default to the selected algorithm below if not**.
* To always use a specific algorithm (regardless if an algorithm is specified in the JWK), select **Always use the algorithm selected below**.

{{< alert title="Note" color="primary" >}}The encryption method should always be specified.{{< /alert >}}

**Encryption method**:

Select the method to use for encryption.

### FIPS restrictions

The following algorithms are not supported when API Gateway is in FIPS mode:

* Key wrap with AES GCM using 128-bit key (A128GCMKW)
* Key wrap with AES GCM using 192-bit key (A192GCMKW)
* Key wrap with AES GCM using 256-bit key (A256GCMKW)

The following encryption methods are not supported when API Gateway is in FIPS mode:

* AES GCM using 128-bit key (A128GCM)
* AES GCM using 192-bit key (A192GCM)
* AES GCM using 256-bit key (A256GCM)

## SMIME decryption

The **SMIME Decryption**
filter can be used to decrypt an encrypted Secure/Multipurpose Internet Mail Extensions (SMIME) message.

Complete the following fields to configure this filter:

**Name**:
Enter a name for the filter to display in a policy.

**Use Certificate to Decrypt**:
Check the box next to the certificate that you want to use to decrypt the encrypted PKCS#7 message with. The private key associated with this certificate is used to actually decrypt the message.

## SMIME encryption

You can use the **SMIME Encryption**
filter to generate an encrypted Secure/Multipurpose Internet Mail Extensions (SMIME) message. This filter enables you to configure the certificates of the recipients of the encrypted message. You can also configure advanced options such as ciphers and Base64 encoding.

Complete the following field:

**Name**:
Enter an appropriate name for the filter to display in a policy.

### Recipient settings

The **Recipients**
tab enables you to configure the certificates of the recipients of the encrypted SMIME message. Select one of the following options:

**Use the following certificates**:
This is the default option. Select the certificates of the recipients of the encrypted message. The public keys associated with these certificates are used to encrypt the data so that it can only be decrypted using the associated private keys.

**Certificate in attribute**:
Alternatively, enter the message attribute that contains the certificate of the recipients of the encrypted message. Defaults to the `certificate`
message attribute.

### Advanced SMIME encryption settings

The **Advanced**
tab includes the following settings:

**Cipher**:
Enter the cipher that you want to use to encrypt the message data. Defaults to the `DES-EDE3-CBC`
cipher.

**Content-Type**:
Enter the `Content-Type`
of the message data. Defaults to `application/pkcs7-mime`.

**Base64 encode**:
Select whether to Base64 encode the message data. This option is not selected by default.
