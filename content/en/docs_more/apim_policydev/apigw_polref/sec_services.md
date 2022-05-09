{
"title": "Security service filters",
"linkTitle": "Security service filters",
"weight": 110,
"date": "2019-10-17",
"description": "Filters for when API Gateway is acting as a security service."
}

## Encrypt and decrypt web services filter

The **Encrypt Web Service**
filter allows the API Gateway to act as an XML *encrypting*
web service, where clients can send up XML blocks to the API Gateway that are required to be encrypted. The API Gateway encrypts the XML data, replacing it with `<EncryptedData>` blocks in the message. The encrypted content is then returned to the client.

Similarly, the **Decrypt Web Service**
filter allows the API Gateway to act as an XML *decrypting*
web service, where clients can send up `<EncryptedData>` blocks to the API Gateway, which decrypts them and returns the plain-text data back to the client.

By deploying the API Gateway as a centralized encryption and decryption service, clients application can abstract out the security layer from their core business logic. This simplifies the logic of the client applications and makes the task of managing and configuring the security aspect a lot simpler because it is centralized.

Furthermore, the API Gateway's XML and cryptographic acceleration capabilities ensure that the process of encrypting and decrypting XML messages—a task that involves some very CPU-intensive operations—is performed at optimum speed.

To configure both the **Encrypt Web Service**
and **Decrypt Web Service**
filters, enter a descriptive name for the filter to display in a policy.

The settings for the filters are configured in the **XML-Encryption Settings** and **XML-Decryption Settings** filters in the Encryption category.

## STS web service filter

The **STS Web Service**
filter can be used to expose a Security Token Service (STS), allowing clients to obtain security tokens for use in your system.

## DSS signature generation filter

The **Sign Web Service**
filter enables the API Gateway to generate XML signatures as a service according to the OASIS Digital Signature Services (DSS) specification. The DSS specification describes how a client can send a message containing an XML signature to a DSS signature web service that can sign the relevant parts of the message, and return the resulting XML signature to the client.

The advantage of this approach is that the signature generation code is abstracted from the logic of the web service and does not have to be coded into the web service. Furthermore, a centralized DSS server provides a single implementation point for all XML signature related services, which can then be accessed by all services running in your API Gateway domain. This represents a much more manageable solution than one in which the security layer is coded into each service.

Complete the following fields:.

**Name**:
Enter a descriptive name for the filter to display in a policy.

**Signing Key**:
Click to select a private key from the certificate store. This key is used to perform the signing operation.

## DSS signature verification filter

The **Verify Sig Web Service**
filter enables the API Gateway to verify XML signatures as a service according to the OASIS Digital Signature Services (DSS) specification. The DSS specification describes how a client can send a message containing an XML signature to a DSS signature verification web service that can verify the signature and return the result of the verification to the client.

The advantage of this approach is that the signature verification code is abstracted from the logic of the web service and does not have to be coded into the web service. Furthermore, a centralized DSS server provides a single implementation point for all XML signature related services, which can then be accessed by all services running your system. This represents a much more manageable solution that one in which the security layer is coded into each web service.

Complete the following fields to configure the **Verify Sig Web Service**
filter.

**Name**:
Enter a descriptive name for the filter to display in a policy.

**Find Signing Key**:
The public key to be used to verify the signature can be retrieved from one of the following locations:

* **Via KeyInfo in Message**:
    The verification certificate can be located using the `<KeyInfo>`
    block in the XML signature. For example, the certificate could be contained in a `<BinarySecurityToken>`
    element in a WSSE security header. The `<KeyInfo>`
    section of the XML signature can then reference this `BinarySecurityToken`. The API Gateway can automatically resolve this reference to locate the certificate that contains the public key necessary to perform the signature verification.
* **Via Selector Expression**:
    The certificate used to verify the signature can be extracted from the message attribute specified in the selector expression (for example, `${certificate}`). The certificate must have been placed into the specified attribute by a predecessor of the **Verify Sig Web Service**
    filter.
* **Via Certificate in LDAP**:
    The certificate used to verify the signature can be retrieved from an LDAP directory. Click the button next to this field, and select a previously configured LDAP directory in the tree. To add an LDAP directory, right-click the **LDAP Connections**
    tree node, and select **Add an LDAP Connection**
    . Alternatively, you can configure LDAP connections under the **External Connections**
    node in the Policy Studio tree.
* **Via Certificate in Store**:
    Finally, the verification certificate can be selected from the certificate store. Click the **Select**
    button to view the certificate that has been added to the store. Select the verification certificate by selecting the check box next to it in the table.
