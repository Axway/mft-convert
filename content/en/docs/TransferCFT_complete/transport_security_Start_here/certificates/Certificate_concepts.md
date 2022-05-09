{
    "title": "Certificate concepts",
    "linkTitle": "Certificate concepts",
    "weight": "180"
}This topic describes the following certificate principles:

- [Establishing
    a trust relationship](#Establishing_a_trust_relationship)
- [CA
    hierarchy](#CA_hierarchy)
- [Certificate
    syntax and formats](#Certificate_syntax_and_formats)

<span id="Establishing_a_trust_relationship"></span>

Establishing a trust relationship
---------------------------------

Certificate Authorities
(CAs) are entities that validate identities and issue certificates. They
can be either independent third parties or organizations running their
own certificate-issuing server software.

Any client or server software that supports certificates maintains a
collection of trusted CA certificates. These CA certificates determine
which other certificates the software can validate, in other words, which
certificate issuers the software can trust. In the simplest case, the
software can validate only certificates issued by one of the CAs for which
it has a certificate.

It is also possible for a trusted CA certificate to be part of a chain
of CA certificates, each one issued by the CA above it in a certificate
hierarchy.  See
for further details about Public Key Infrastructure.

<span id="CA_hierarchy"></span>

CA hierarchy
------------

In large organizations, it may be appropriate to delegate the responsibility
for issuing certificates to several different certificate authorities.
For example, the number of certificates required may be too large for
a single CA to maintain. Also, different organizational units may have
different policy requirements, or it may be important for a CA to be physically
located in the same geographic area as the people to whom it is issuing
certificates.

You can delegate certificate-issuing responsibilities to subordinate
CAs. The X.509 standard includes a model for setting up a hierarchy of
CAs as shown in the following figure.

********CA hierarchy********

![Relationship between the Root CA and related certificates, such as Asia and Europe](/Images/TransferCFT/certificates3.gif)

In this model, the root CA is at the top of the hierarchy. The root
CA certificate is a self-signed
certificate. That means the certificate is digitally signed
by the same entity, the root CA, which the certificate identifies. The
CAs that are directly subordinate to the root CA have CA certificates
signed by the root CA. CAs under the subordinate CAs in the hierarchy
have their CA certificates signed by the higher-level subordinate CAs.

Organizations have a great deal of flexibility in terms of the way they
set up their CA hierarchies. The above figure is just one example; many
other configurations are possible.

<span id="Certificate_chains"></span>

### Certificate chains

CA hierarchies are reflected in certificate chains. A certification
path is an ordered sequence of certificates between the end entity,
a leaf, and the trusted point in the hierarchy, for example, the root.
The result forms a certificate chain that begins at the end entity and
ends at the root CA.

A certificate chain traces a path of certificates from a branch to the
root of the hierarchy. A certificate chain is formed that way:

- Each certificate
    is followed by the certificate of its issuer
- Each certificate
    contains the name (DN) of that certificate's issuer, which is the same
    as the subject name of the next certificate in the chain

********Certificate chains********

![View of Trusted Root CA with some related certificates being Untrusted certificates](/Images/TransferCFT/certificates2.gif)

In the above figure, the Engineering CA certificate contains the DN
of the CA (European CA), that issued that certificate. European CA's DN
is also the subject name of the next certificate in the chain.

Each certificate is signed with its issuer’s private key. The signature
can be verified with the public key in the issuer's certificate (which
is the next certificate in the chain).

In the above figure, the public key in the certificate for the European
CA can be used to verify the European CA's digital signature on the certificate
for the Engineering CA.

<span id="Verifying_a_certificate_chain"></span>

### Verifying a certificate chain

Certificate chain verification is the process of making sure a given
certificate chain is well formed, valid, properly signed, and trustworthy:

1. Certificate validity period
    is checked against the current time provided by the verifier's system
    clock.
1. The issuer's certificate is
    located. The source can be either the verifier's local certificate database
    (on that client or server) or the certificate chain provided by the subject
    (for example, over an SSL connection).
1. The certificate signature is
    verified using the public key in the issuer's certificate.
1. If the issuer's certificate
    is trusted by the verifier in the verifier's certificate database, verification
    stops successfully here. Otherwise, the issuer's certificate becomes the
    one that needs to be verified, and chain verification returns to step
    1 to start again, but with this new certificate.

The following figure presents an example of this process.

********Verifying a certificate chain********

![Validity checks on Untrusted Authorities, where Root CA is a Trusted Authority](/Images/TransferCFT/certificate1.gif)

 

<span id="Certificate_syntax_and_formats"></span>

Certificate syntax and formats
------------------------------

X509 Certificates are defined in ISO x509 according to a descriptive
syntax called ASN1 (Abstract Syntax Notation
One). The purpose of this syntax
is to give a system an independent object description tool. In other words,
this syntax allows the description of objects used by applications in
heterogeneous environments.

The details of ASN1 is outside the scope of this document but further
details are available in the ISO /IEC8824-1. To be exploitable by a remote
application, supported by any kind of machine, transmitted data (described
with ASN1) must be encoded. The encoding rule must be the same on each
communicating application but the way this encoding rule is implemented
is case dependant (hardware dependent, OS dependent,...).

The encoding rule commonly used by applications dealing with x509 certificates
and public key infrastructures is the Distinguished Encoding
Rule (DER). The goal of the DER is to give a data set described
with ASN1 syntax a unique encoded value, and therefore reduce ambiguity.

<span id="RSA_Public_key_cryptography_standards"></span>

### RSA Public key cryptography standards

The RSA laboratories have defined various standards about certificates
based on ASN1 descriptions. Their aim is to provide a means for systems
participating in Public Key Infrastructures to inter-operate.

Furthermore, two data structures that are useful when you want to transfer
Certificate Information have been completely detailed: the PKCS7 and PKCS12
structures.

#### The PKCS7 Standard

The PKCS7 standard defines an envelope for transferring all kinds of
data. Such a transfer can involve cryptographic parameters since data
may be ciphered, signed, and so on. Therefore, the PKCS7 envelope includes
cryptographic information. For instance the ciphering algorithm that is
used when data is ciphered and the tools needed to authenticate the sender's
identity when data is signed,  its
user certificate with the whole certification chain. In a degenerated
case, when the PKCS7 structure data field is empty, it is a means of disseminating
certificates.

Commonly used file extensions are p7s.

#### The PKCS8 Standard

The PKCS8 standard defines a structure used to store private key information.
Private key information includes a private key for some public-key algorithm
and a set of attributes. You can use a password-based algorithm to encrypt
the key.

#### The PKCS12 Standard

After processing the certification operation, which generally consists
in generating a pair of keys, signing the user certificate and sending
the information he had called for back to this user, the certificate authority
needs a secure way to send the user both the generated certificate and
the private key. In the same way as PKCS7, the PKCS12 standard defines
an envelope. This one contains password based ciphered data.

Therefore, in order to open the envelope and access the data, you need
to give a password required to generate a symmetric key used for deciphering.

Commonly used file extensions are p12.

#### Base64 Encoding

While using mailing to transfer data, you may be constrained to encode
it with printable character representation only. The base64 encoding
converts binary data to printable characters. This enables you to DER
encode a certificate and to use base 64 to encode the result. For instance,
the base64 is mandatory for compatibility with MIME.

A commonly used file extension is pem.

TLS certificate concepts
------------------------

TLS certificates require the following key usages:

- A TLS CA certificate must have the **Certificate sign** key usage
- A TLS server certificate must have the **Key encipherment** key usage
- A TLS client certificate must have the **Digital signature** key usage

Where:

- If a certificate does not have a key usage, it is assumed that ALL key usage are applicable
- Some ciphers, such as Diffie-Hellman, require **Key agreement** for key usage
