{
"title": "Security features",
"linkTitle": "Security features",
"weight": 30,
"date": "2019-11-25",
"description": "Summary of the main security features of API Gateway, API Manager, and API Portal."
}

## Secure connections

The following secure connections are available:

* Connections between all product components – API Gateway, API Portal, Node Manager, and Policy Studio – are SSL-secured by default.
* Connections to databases and LDAP servers can be SSL-secured.
* Connections to Apache Cassandra can be SSL-secured.
* Connections made to audit servers can be made over SSL.
* Connections to other Axway products (for example, PassPort) can be SSL-secured.
* All inbound connections to API Gateway can be configured to be received over one-way or two-way SSL.
* All outbound connections made by API Gateway to destination web services can be made over one-way or two-way SSL where the destination supports SSL.
* API Manager custom routing policy templates use TLS version 1.2 with strong FIPS-compliant ciphers by default for SSL/TLS connections.
* Inbound and outbound FTP connections can be made secure by using either FTPS or SFTP.
* SMTP and POP connections can be made over SSL or TLS.
* API Gateway can send messages to an Internet Content Adaptation Protocol (ICAP) server for content adaptation over a mutually authenticated SSL connection.
* In all cases where API Gateway is delegating authentication and authorization decisions to a third-party identity management (IM) server, the connection to that server can be made over SSL/TLS where it is supported by the server.

## API Gateway password management features

The following passwords are stored securely by API Gateway:

* **Administrator user passwords**: The administrator user's passwords are stored in the `INSTALL_DIR/apigateway/conf/adminUsers.json` file as a base 64-encoded salted hash of the plain text password. The salt is a 16-byte value generated using the SHA1PRNG pseudo-random number generator algorithm. The algorithm used is provided by JCE and is PBKDF2 with HMACSHA256 using a key length of 256 bits. The algorithm repeats the digest of the password along with the salt for 102400 iterations. A new salt is used for each password hash, which results in different password hashes for the same password.
* **Passwords stored in the Entity Store**: All sensitive data (local user store passwords, private keys and their passwords, and passwords required to connect to third-party services) can be [encrypted in the Entity Store](/docs/apim_administration/apigtw_admin/general_passphrase/). The process behind the encryption uses Password-Based Encryption (PBE) with the Entity Store passphrase:

    1. A high entropy master key (MK) of size 32 bytes is created using PBKDF2 with inputs of the entity store passphrase, a 16 byte salt generated using a pseudo random number generator (PRNG), and a 100,000 iteration count.
    2. An MK = PBKDF2 (HMAC-SHA512, passphrase, unique salt, iteration count, MK length) is used as key material input into the generation of a data protection key (DPK).
    3. The DPK of size 32 bytes is derived from the MK again using PBKDF2, unique random salt and MK.
    4. DPK = PBKDF2 (HMAC-SHA512, MK, unique salt, iteration count, DPK length). The number of iterations is 2 for the DPK generation to maintain acceptable performance in the runtime environment.

    To encrypt plaintext, we generate a cipher using AES/GCM encryption and the DPK as the secret key, then we store the unique salt with the cipher text.

* **API Manager user passwords**: API Manager users are stored in KPS. The encryption mechanism and algorithms used to store sensitive data in KPS is inherited from the API Gateway Entity Store. The encryption for KPS data is applied only when the corresponding Entity Store is protected with a passphrase.

## API Portal password management features

API Portal does not store credential information for API Manager users.

API Portal stores the Public API user password encryption key securely in a given directory specified by the user during installation.

API Portal stores Joomla! administrator passwords securely in the database. They are hashed using the Blowfish algorithm, resulting in a 60 character string that contains:

* Algorithm identifiers (for example, 2y\$ for Blowfish).
* Cost parameter (for example, 10\$ for cost 10) – Specifies the number of iterations (as a power of two) the key and the salt will be hashed. For example, for cost of 10, the total iterations will be 2\^10 = 1024 iterations. The value of the cost on API Portal is always 10.
* Salt (for example, q5MkhSBtlsJcNEVsYh64a) – Consists of 128 randomly generated bits that are base-64 encoded with a 22 characters alphabet.
* The hash (for example, .aCluzHnGog7TQAKVmQwO9C8xb.t89F.) – Consists of 184 bits that are base-64 encoded with 31 characters alphabet.

## API Gateway certificate management features

Certificate management can either be performed internally in the product or using Axway PassPort.

### PassPort certificate management

API Gateway can integrate with Axway PassPort to authenticate users against the PassPort certificate store.

The connection between API Gateway and PassPort requires that communication is performed over a secure connection using two-way SSL authentication. This means that the PassPort server must be able to identify and trust the client connection and this trust is established by registration.

The first connection from API Gateway to PassPort initiates registration. A public-private key pair is created and a Certificate Signing Request (CSR) is submitted to PassPort. While the CSR is pending, the repository is unable to process any requests. However, registration is a one-off event, and when complete, it does not need to be repeated.

For more information on PassPort integration, see the [API Gateway PassPort Interoperability Guide](https://docs.axway.com/bundle/APIGateway_77_PassPort_InteropGuide_allOS_en_HTML5).

### Certificate storage, validation, and revocation

The following features are available for API Gateway:

* The product can validate certificates using CRL, OCSP, and XKMS.
* It is possible to validate the thumbprint, attributes, and chain of a given certificate using the out-of-the-box filters.
* Local private keys can be stored in the local certificate store and can be encrypted with the Entity Store passphrase using the PBE with SHA and 3DES (for example, pbeWithSHAAnd3_KeyTripleDES_CBC) algorithm.
* The local private keys can be stored in a Hardware Security Module (HSM). For example, Thales nShield Solo or Safenet Luna SA.

## API Gateway identity and access management features

Identity and access management (IAM) can be performed internally in the product using Axway PassPort or a range of other third-party Identity Management products.

### PassPort access management

The following PassPort access management features are available:

* RBAC (Role-Based Access Control) is used.
* The users and associated passwords can be stored in a PassPort database.  Alternatively, they can be retrieved from an LDAP directory or from Active Directory.
* An SSO option is available. PassPort includes a reverse proxy and a token manager to provide an SSO option only for Axway products.
* The product can authenticate and authorize users for specific resources against PassPort.
* See the Axway PassPort documentation, available from Axway Support at [https://support.axway.com](https://support.axway.com/), for a complete list of features in this product.

### When not using PassPort

The following features are available for API Gateway:

* Store users locally in the local user store.
* Store users in databases, LDAP directories, or Key Property Stores (KPS).
* Integrate with third-party Identity Management products used to authenticate and authorize users, including: CA SiteMinder, CA SOA Security Manager, Entrust GetAccess, Oracle Access Manager, Oracle Entitlements Server, RADIUS servers, RSA Access Manager, Sun Access Manager, and Tivoli.
* Support for authentication and authorization of users with SAML, WS-Security Username Tokens, WS-Trust, and other WS-\* standards.
* Authentication and authorization schemes, including: HTTP basic and HTTP digest authentication, cookie-based sessions, HTML form authentication, SSL, Kerberos, X.509 certificate attribute authorization, XACML, OAuth 2.0 and OpenID Connect, and RBAC authorization against an LDAP directory.

## API Gateway other security features

The following features are available for API Gateway:

* Support for security standards, including: OAuth 2.0, OpenID Connect, Kerberos, XML Signature, XML-Encryption, SAML, WS-Trust, SMIME encryption/decryption, SMIME signing/verification, PGP signature/verification and encryption/decryption, WS-Trust, XACML, and WS-Policy.
* API Gateway can scan for viruses using the ClamAV, Sophos, and McAfee anti-virus scanners.
* Other security features include IP address validation, message throttling, XML complexity, content-type validation, regexp-based input validation, XML schema validation, JSON validation, and timestamp validation.
