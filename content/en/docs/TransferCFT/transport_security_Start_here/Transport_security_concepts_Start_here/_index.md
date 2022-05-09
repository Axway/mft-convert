{
    "title": "Transport security concepts:  Start here",
    "linkTitle": "Transport security concepts",
    "weight": "140"
}This sub-book presents and defines transfer security concepts that are referred
to further in this document, as well as how these security items work. It is comprised of the following topics:

- [Extended
    catalog query APIs](extended_catalog_query_apis)
- Transport
    security symbolic variables

File transfer security is based on the following three principles, as described in this topic:

- [Privacy](#Privacy)
- [Integrity](#Integrity)
- [Authentication](#Authentication)

These three forms of security are independent and must be combined to
get a high level of security.

Additionally, the following two security related subjects are described:

- [Signature](#Signature)
- [Log](#Log)

<span id="Privacy"></span>

Privacy
-------

Privacy, also referred to as confidentiality, consists in protecting
data from being revealed to parties it was not meant for. As a rule, this
is accomplished by ciphering transferred data.

Two different cryptographic techniques are often used and combined:

### Symmetrical encryption

Symmetrical
encryption consists in using a unique key to encrypt and decrypt a
message. Both connected parties then share a key that is used to encrypt
and decrypt data. The task of privately choosing the key before ciphering
data is more problematic as the connected parties share a single key that
cannot be clearly transmitted to others.

********Symmetrical encryption********

![View of symetrical key ](/Images/TransferCFT/encrypt_key.gif)

### Asymmetrical encryption

Asymmetrical
encryption consists in having a pair of keys where each key
by itself can decrypt messages ciphered using the other key. This technique
solves the problem of exchanging the key between connected parties. When
two parties want to communicate using this encryption mechanism, each
party publishes one of its keys and keeps the other one secret.

When using SSL or TLS protocols, a symmetric encryption mechanism is
used to cipher data (DES, triple DES, RC2, RC4,
and AES). A key used by that mechanism is generated for each session
and exchanged during the handshake phase. They are transmitted using an
asymmetrical encryption mechanism, which is slower but more secure (RSA)
than a symmetric encryption mechanism.

********Asymmetrical encryption********

![View of public and private key encryption](/Images/TransferCFT/image004.gif)

<span id="Integrity"></span>

Integrity
---------

Integrity consists in assuring that the received data has not been altered
or modified. Generally, integrity checking is done by adding to the sent
data an abstract of that data. When receiving a message, the receiver
makes an abstract of the message and compares it with the one sent. If
the abstracts match, message integrity is assumed to be correct. Otherwise,
it is not.

The message abstract is commonly called the message digest or the hash
code. The procedure used to create that hash code is called the digest
function or the hash function.

When using SSL or TLS, a hash code, called MAC (Message
Authentication Code),
is added to each message sent. The standard hash functions used are SHA-1
or MD5.

********Hash function********

![](/Images/TransferCFT/image005.gif)

<span id="Authentication"></span>

Authentication
--------------

Authentication is the process of verifying a claimed identity. The most
widely used authentication mechanisms are:

- Password request
- Proof request

Password requests are associated with user login requests. When you
have sent a login name, you are then prompted for your password.

Proof requests are associated with asymmetrical encryption mechanisms.
After you have sent your public key, or any information relevant enough
to get the public key, such as a certificate, you are sent a random encrypted
message. If you can decrypt the message, your identity is verified because
you own the unique private key associated with the public key.

********Proof request![](/Images/TransferCFT/image006.gif)********

When you use TLS and SSL protocols, the client encrypts the symmetrical
key with the server’s public key. If the server can decrypt the key, the
client is recognized. Otherwise, dialog is not possible and the connection
is closed. When requested, the server authenticates the client through
a signature process.

<span id="Signature"></span>

Signature
---------

Signing messages is a technique that mixes both integrity and authentication.
It consists in sending a ciphered hash code with the message.

When receiving a signed message, both the sender identity and data integrity
are verified by decrypting the signature and comparing the result with
the calculated hash code. If the hash code is correct, data integrity
is accepted, and the sender is as claimed because he had the correct key
to encrypt the digest.

********Integrity and authentication********

![](/Images/TransferCFT/temp_integrity_and_auth.png)

<span id="Log"></span>

Log
---

Log messages enable you to monitor secure transfer sessions.

The following two messages indicate whether the SSL
option was applied when starting up the monitor. Remember that SSL
is a protection key option.

`CFTI18I SSL Protocol Option is authorizedCFTI25I Init complete _ Security active [ SSL ]`

Other messages are used to notify you of secure sessions that have been
opened/closed and the parameters negotiated.
