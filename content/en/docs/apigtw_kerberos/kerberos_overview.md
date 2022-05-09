{
"title": "Kerberos authentication",
"linkTitle": "Kerberos authentication",
"weight":"2",
"date": "2019-11-14",
"description": "Use the Kerberos authentication protocol to verify the identity of a user or host. The authentication is based on tickets used as credentials, allowing communication and proving identity in a secure manner even over a non-secure network."
}

For further security, the Kerberos protocol messages are also protected against eavesdropping and replay attacks.

Kerberos authentication is aimed primarily at a client–server model: a Kerberos client sends an authentication request to a Kerberos service. The authentication builds on symmetric-key cryptography, and it requires a trusted third party to facilitate the interactions between two parties. In API Gateway, the authentication is by default mutual: both the user and the server verify each other's identity.

On Windows 2000 and later, Kerberos authentication is the default authentication method when authenticating within an Active Directory domain. Linux operating systems include software for Kerberos authentication of users or services.

## Key concepts

* **Kerberos client** – a client (an application or even an end user) requiring access to a Kerberos service.
* **Kerberos service** – a server or an application providing something (a resource, a service, a process, data) a Kerberos client wants to access.
* **Key Distribution Center (KDC)** – a domain service located on a domain controller (such as Active Directory on Windows). In Kerberos, the KDC is a single process providing two services:
    * **Authentication Service (AS)** – authenticates the Kerberos client against the user database, and grants a Ticket Granting Ticket (TGT) for the client.
    * **Ticket Granting Service (TGS)** – validates the client is allowed to access the requested Kerberos service and issues a service ticket for that service. The TGS acts as the trusted third party in the Kerberos protocol.
* **Ticket Granting Ticket (TGT)** – an encrypted identification ticket with a limited validity period used for data traffic protection. The TGT is used to obtain a service ticket from the TGS. The TGT contains the the client/TGS session key, its expiration date, and the client's IP address protecting the client from man-in-the-middle attacks. The TGT is encrypted with the secret key of the TGS.
* **Service ticket** – an encrypted client-to-server ticket containing the client ID, client network address, validity period and client/server session key. A Kerberos client obtains this ticket from TGS after presenting a valid TGT. The service ticket is encrypted with the secret key of the Kerberos service.

## Flow description

![Kerberos_flow_overview](/Images/IntegrationGuides/KerberosIntegration/Kerberos_flow_overview.png)

1. A Kerberos client sends its user ID in a clear text message to the AS. The message does not include the client's password, nor its secret key based on the password.
2. The AS checks if the client is in the user database, and if found, generates the secret key for the client by hashing the client's password. The AS then sends a client/TGS session key and a TGT to the Kerberos client. The session key is encrypted with the secret key of the client.
3. The Kerberos client decrypts the client/TGS session key, and sends a request message – containing the TGT and the ID of the Kerberos service to be accessed – and an authenticator message – containing the client ID and the timestamp and encrypted with the client/TGS session key – to the TGS.
4. The TGS decrypts the TGT in the request message to retrieve client/TGS session key, and decrypts the authenticator message. The TGS verifies the Kerberos client is authorized to access the Kerberos service requested, and sends a service ticket and a client/server session key encrypted with the client/TGS session key to the Kerberos client.
5. The Kerberos client sends the service ticket and a new authenticator message encrypted with the client/server session key to the Kerberos service to be accessed.
6. The Kerberos service decrypts the service ticket to retrieve the client/server session key, then decrypts the authenticator message to retrieve the client's timestamp. The Kerberos service sends a service confirmation message including the timestamp and encrypted with the client/server session key back to the Kerberos client.
7. The Kerberos client decrypts the service confirmation message and verifies the timestamp is correct. The mutual authentication is now complete. The Kerberos client can now start issuing service requests, and the Kerberos service can provide the requested services for the client.

Kerberos authentication relies on a secure user database storing user IDs and passwords. Using secret keys for encryption requires that the password on the Kerberos client or Kerberos service must match the one stored in the database on the KDC. If the passwords do not match, the secret keys hashed from the passwords do not match either, and decrypting messages fails.

## Kerberos SPNEGO authentication

Kerberos authentication based on Simple and Protected Negotiation Protocol (SPNEGO) over HTTP refers to the use of the HTTP negotiate protocol to perform Kerberos authentication at the transport layer between a client and a service. The tokens required for the authentication are transmitted in HTTP headers. The SPNEGO specification suggests using SSL/TLS to provide confidentiality with the authentication mechanism. For more details on SPNEGO, see [SPNEGO-based Kerberos and NTLM HTTP Authentication](http://tools.ietf.org/html/rfc4559).

## Kerberos use cases

This guide provides examples of the following Kerberos scenarios:

* [Demo setup: API Gateway as both Kerberos client and service](/docs/apigtw_kerberos/kerberos_use_case_demo/)
* [API Gateway as a Kerberos client](/docs/apigtw_kerberos/kerberos_use_case_client/)
* [API Gateway as a Kerberos service](/docs/apigtw_kerberos/kerberos_use_case_service/)
* [API Gateway in Kerberos constrained delegation](/docs/apigtw_kerberos/kerberos_use_case_kcd/)
* [API Gateway in unconstrained credentials delegation](/docs/apigtw_kerberos/kerberos_use_case_ucd/)
