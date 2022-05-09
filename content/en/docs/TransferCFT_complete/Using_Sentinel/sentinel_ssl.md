{
    "title": "Implement SSL with Sentinel ",
    "linkTitle": "Implement SSL with Sentinel",
    "weight": "250"
}This topic describes how to enable SSL connections between a Transfer CFT client and a Sentinel server when Central Governance is not implemented. To do this, you insert in the internal PKI base the CA certificate that is used to authenticate the Sentinel server.

Prerequisites
-------------

You require the root certificate of the Sentinel sever. See the *Axway Sentinel Installation Guide* for details.

To manage the SSL session, ensure that the values in the following Transfer CFT UCONF parameters match those defined for the Sentinel server or Event Router:

- ssl.ciphersuites
- ssl.version_min
- ssl.extension.enable_sni

Procedure
---------

Set the UCONF values as follows:

1. Enable the SSL connection.
1. Insert the Sentinel server's root certificate in the local PKI database in the appropriate format, for example DER.
1. Enter the root certificate value for the Sentinel server.
