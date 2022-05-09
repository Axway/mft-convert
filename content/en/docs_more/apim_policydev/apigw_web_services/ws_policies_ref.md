{
"title": "WS-Policy reference",
"linkTitle": "WS-Policy reference",
"weight": 11,
"date": "2019-10-17",
"description": "Reference information on WS-Policies."
}

## AsymmetricBinding WS-Policies

**AsymmetricBinding with Encrypted UsernameToken**: The service exposes an `AsymmetricBinding`
where the client and server use their respective X.509 v3 tokens to sign and encrypt the message. An encrypted `UsernameToken`
with hash password must be included in all messages from the client to the server.

**AsymmetricBinding with SAML 1.1 (Sender Vouches) Assertion and Signed Supporting Token**: The service is secured with an `AsymmetricBinding`
where the client and server use their respective X.509 v3 certificates to secure the message. The client must include a SAML 1.1 `Assertion` (sender vouches) in all messages it sends to the service.

**AsymmetricBinding with Signed and Encrypted UsernameToken**: The service exposes an `AsymmetricBinding`
where the client and server use their respective X.509 v3 tokens to sign and encrypt the message. A signed and encrypted `UsernameToken`
with plaintext password must be included in all messages from the client to the service.

**AsymmetricBinding with WSS 1.0 Mutual Authentication with X509 Certificates, Sign, Encrypt**: The service exposes an `AsymmetricBinding`
interface where the client and server use their respective X.509 v3 certificates for mutual authentication, signing, and encrypting.

**AsymmetricBinding with X509v3 Tokens**: The service exposes an `AsymmetricBinding`
where the client and server use their respective X.509 v3 tokens to sign and encrypt the message.

## Message-level WS-Policies

**Encrypt SOAP Body**: The SOAP body must be encrypted.

**Sign and Encrypt SOAP Body**: The SOAP body must be signed and encrypted.

**Sign SOAP Body**: The SOAP body must be encrypted.

## Oracle Web Services Manager WS-Policies

**WS-Security 1.0 Mutual Auth with Certificates**: `AsymmetricBinding`
where the client and server use their respective X.509 v3 certificates to secure the message.

**WS-Security 1.0 SAML with Certificates**: `AsymmetricBinding`
with SAML assertion as `SignedSupportingToken`.

**WS-Security 1.0 Username with Certificate**: `AsymmetricBinding`
with WS-Security `UsernameToken`
as `SignedSupportingToken`.

**WS-Security 1.1 Mutual Auth with Certificates**: `SymmetricBinding`
where the same X.509 v3 certificate is used to secure all messages between the client and the service.

**WS-Security 1.1 Username with Certificates**: `SymmetricBinding`
with a WS-Security `UsernameToken`
as a `SignedSupportingToken`. The message is endorsed with an asymmetric `Signature`.

**WS-Security SAML Token Over SSL**: `TransportBinding`
with a SAML Token as a `SupportingToken`.

**WS-Security UsernameToken Over SSL**: `TransportBinding`
with a WS-Security `UsernameToken`
as a `SupportingToken`.

## Simple WS-Policies

**SAML 1.1 Bearer**: The client must include a SAML 1.1 `Assertion`
(bearer) representing the requestor in all messages from the client to the service.

**Username SupportingToken Hash Password**: The client must authenticate with a WS-Security SAML `UsernameToken`
with hash password.

**Username SupportingToken No Password**: The client must authenticate with a WS-Security `UsernameToken`
without a password.

**Username SupportingToken Plaintext Password**: The client must authenticate with a WS-Security `UsernameToken`
with a plaintext password.

## SymmetricBinding WS-Policies

**SymmetricBinding with SAML 2.0 (Sender Vouches) Assertion and Endorsing Supporting Token**: The service exposes a `SymmetricBinding`
that requires the client to send a SAML 2.0 `Assertion`
to the service. An X.509 v3 token is also included in all messages from the client to the service as an `EndorsingSupportingToken`.

**SymmetricBinding with Signed and Encrypted UsernameToken**: The service uses a `SymmetricBinding`
where the client and service use the same X.509 v3 token to sign and encrypt the message. A signed and encrypted `UsernameToken`
with plaintext password must be included in all messages from the client to the service. The policy uses WSS SOAP Message Security 1.1 options.

**SymmetricBinding with WSS 1.1 Anonymous Authentication with X.509v3, Sign, Encrypt**: The service is secured by a `SymmetricBinding`
where the same X.509 v3 certificate is used to secure all messages between the client and the service. Derived keys are used for signing and encrypting and Signature Confirmation is required by the policy.

**SymmetricBinding with WSS 1.1 Mutual Authentication with X.509v3, Sign, Encrypt**: The service exposes a `SymmetricBinding`
where the same X.509 v3 certificate is used to secure all messages between the client and the service. The client also endorses the primary message signature using another X.509v3 certificate.

## TransportBinding WS-Policies

**SAML 1.1 Holder-of-Key over SSL**: The client includes a SAML 1.1 `Assertion`
(sender vouches) in all messages from the client to the service. The client provides an endorsing signature to prove that it is the holder-of-key. A `TransportBinding`
is used to sign and encrypt the message.

**SAML 1.1 Sender-Vouches over SSL**: The client includes a SAML 1.1 `Assertion`
(sender vouches) on behalf of the requestor to all messages from the client to the service. The service uses a `TransportBinding`
to ensure that all messages are signed and encrypted.

**SAML 2.0 Holder-of-Key over SSL**: The client includes a SAML 2.0 `Assertion`
(sender vouches) in all messages from the client to the service. The client provides an endorsing signature to prove that it is the holder-of-key. A `TransportBinding`
is used to sign and encrypt the message.

**SAML 2.0 Sender-Vouches over SSL**: The client includes a SAML 2.0 `Assertion` (sender vouches) on behalf of the requestor to all messages from the client to the service. The service uses a `TransportBinding`
to ensure that all messages are signed and encrypted.

**SSL Transport Binding**: The service is secured by SSL (HTTPS).

**Username Token over SSL with no Timestamp**: The service is secured over SSL (HTTPS), the client is authenticated with a `UsernameToken`, and no timestamp should be included in the `Security` header.

**Username Token over SSL with Timestamp**: The service is secured over SSL (HTTPS), the client is authenticated with a `UsernameToken`. The `Security`
header contains a timestamp.
