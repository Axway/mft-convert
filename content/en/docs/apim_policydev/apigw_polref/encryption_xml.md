{
"title": "XML encryption filters",
  "linkTitle": "XML encryption filters",
  "weight": 91,
  "date": "2019-10-17",
  "description": "XML encryption and decryption filters and settings."
}

## XML encryption settings filter

The API Gateway can XML encrypt an XML message so that only certain specified recipients can decrypt the message. XML encryption is a W3C standard that enables data to be encrypted and decrypted at the application layer of the OSI stack, thus ensuring complete end-to-end confidentiality of data.

The **XML-Encryption Settings** filter
should be used in conjunction with the **XML-Encryption**
filter, which performs the encryption. The **XML-Encryption Settings**
generates the `encryption.properties`
message attribute, which is required by the **XML-Encryption**
filter.

### XML encryption overview

XML encryption facilitates the secure transmission of XML documents between two application endpoints. Whereas traditional transport-level encryption schemes, such as SSL and TLS, can only offer point-to-point security, XML encryption guarantees complete end-to-end security.

Encryption takes place at the application-layer, and so the encrypted data can be encapsulated in the message itself. The encrypted data can therefore remain encrypted as it travels along its path to the target Web service.

#### Elements

Before explaining how to configure the API Gateway to encrypt XML messages, it is useful to examine an XML encrypted message. The following example shows a SOAP message containing information about Axway:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Body>
    <getCompanyInfo xmlns="www.axway.com">
       <name>Company</name>
       <description>XML Security Company</description>
    </getCompanyInfo>
  </s:Body>
</s:Envelope>
```

After encrypting the SOAP Body, the message is as follows:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <Security xmlns="http://schemas.xmlsoap.org/ws/2003/06/secext" s:actor="Enc">
     <!-- Encapsulates the recipient's key details -->
     <enc:EncryptedKey xmlns:enc="http://www.w3.org/2001/04/xmlenc#"
        Id="00004190E5D1-7529AA14" MimeType="text/xml"
      <enc:EncryptionMethod Algorithm="http://www.w3.org/2001/04xmlenc#rsa-1_5">
        <enc:KeySize>256</enc:KeySize>
      </enc:EncryptionMethod>
      <enc:CipherData>
        <!-- The session key encrypted with the recipient's public key -->
        <enc:CipherValue>AAAAAJ/lK ... mrTF8Egg==</enc:CipherValue>
      </enc:CipherData>
      <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
        <dsig:KeyName>sample</dsig:KeyName>
        <dsig:X509Data>
          <!-- The recipient's X.509 certificate -->
          <dsig:X509Certificate>MIIEZzCCA0 ... fzmc/YR5gA</dsig:X509Certificate>
        </dsig:X509Data>
      </dsig:KeyInfo>
      <enc:CarriedKeyName>Session key</enc:CarriedKeyName>
      <enc:ReferenceList>
        <enc:DataReference URI="#00004190E5D1-5F889C11"/>
      </enc:ReferenceList>
     </enc:EncryptedKey>
    </Security>
  </s:Header>
  <enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#"
        Id="00004190E5D1-5F889C11" MimeType="text/xml"
        Type="http://www.w3.org/2001/04/xmlenc#Element">
    <enc:EncryptionMethod Algorithm="http://www.w3.org/2001/04xmlenc#aes256-cbc">
      <enc:KeySize>256</enc:KeySize>
    </enc:EncryptionMethod>
   <enc:CipherData>
    <!-- The SOAP Body encrypted with the session key -->
    <enc:CipherValue>E2ioF8ib2r ... KJAnrX0GQV</enc:CipherValue>
   </enc:CipherData>
   <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
    <dsig:KeyName>Session key</dsig:KeyName>
   </dsig:KeyInfo>
  </enc:EncryptedData>
<s:Envelope>
```

The most important elements are as follows:

* `EncryptedKey`:
    The `EncryptedKey`
    element encapsulates all information relevant to the encryption key.
* `EncryptionMethod`:
    The `Algorithm`
    attribute specifies the algorithm used to encrypt the data. The message data (`EncryptedData`) is encrypted using the Advanced Encryption Standard (AES) *symmetric cipher*, but the session key (`EncryptedKey`) is encrypted with the RSA *asymmetric*
    algorithm.
* `CipherValue`:
    The value of the encrypted data. The contents of the `CipherValue`
    element are always Base64 encoded.
* `DigestValue`:
    Contains the Base64-encoded message-digest.
* `KeyInfo`:
    Contains information about the recipient and his encryption key, such as the key name, X.509 certificate, and Common Name.
* `ReferenceList`:
    This element contains a list of references to encrypted elements in the message. It contains a `DataReference`
    element for each encrypted element, where the value of a URI attribute points to the `Id`
    of the encrypted element. In the previous example, the `DataReference`
    URI attribute contains the value `#00004190E5D1-5F889C11`, which corresponds with the `Id`
    of the `EncryptedData`
    element.
* `EncryptedData`:
    The XML elements or content that has been encrypted. In this case, the SOAP `Body`
    element has been encrypted, and so the `EncryptedData`
    block has replaced the SOAP `Body`
    element.

#### Asymmetric and symmetric cryptography

Now that you have seen how encrypted data can be encapsulated in an XML message, it is important to discuss how the data is encrypted. When a message is encrypted, it is encrypted in such a manner that only the intended recipients of the message can decrypt it.

By encrypting the message with the recipient public key, the sender can be guaranteed that only the intended recipient can decrypt the message using his private key, to which he has sole access. This is the basic principle behind *asymmetric cryptography*. In practice, however, encrypting and decrypting data with a public-private key pair is a notoriously CPU-intensive and time consuming affair. Because of this, asymmetric cryptography is seldom used to encrypt large amounts of data.

The following steps show a more typical encryption process:

1. The sender generates a one-time *symmetric*
    (or session) key which is used to encrypt the data. Symmetric key encryption is much faster than asymmetric encryption, and is far more efficient with large amounts of data.
2. The sender encrypts the data with the symmetric key. This same key can then be used to decrypt the data. It is therefore crucial that only the intended recipient can access the symmetric key and consequently decrypt the data.
3. To ensure that nobody else can decrypt the data, the symmetric key is encrypted with the recipient's *public key*.
4. The data (encrypted with the symmetric key), and session key(encrypted with the recipient's public key), are then sent together to the intended recipient.
5. When the recipient receives the message, he decrypts the encrypted session key using his *private key*. Because the recipient is the only one with access to the private key, only he can decrypt the encrypted session key.
6. Armed with the decrypted session key, the recipient can decrypt the encrypted data into its original plaintext form.

**XML-Encryption Settings** filter requires a preceding filter to populate the message attribute with the symmetric key. To add the preceding filter, right-click **XML-Encryption Settings** filter, select **Create Predecessor**, and select the filter to populate the message attribute.

The following filters generate the `symmetric.key` message attribute:

* HTTP Header
* SMIME Verify
* XML Signature Verification
* XML Signature Generation
* Insert SAML Authorization Assertion
* SSL
* XML Decryption

The most typical example is to sign the message with a symmetric key using an **XML Signature Generation** filter before encrypting it with **XML Encryption Filter** using the settings from the **XML Encryption Settings** filter. You can configure the **XML Signature Generation** filter to generate a symmetric key to sign the message symmetrically and automatically populate the `symmetric.key` message attribute with the generated key.

Now that you understand the structure and mechanics of XML Encryption, you can configure the API Gateway to encrypt egress XML messages. The next section describes how to configure the tabs on the **XML Encryption Settings**
screen.

### Encryption key settings

The settings on the **Encryption Key**
tab determine the key to use to encrypt the message, and how this key is referred to in the encrypted data. The following configuration options are available:

**Generate Encryption Key**:
Select this option to generate a symmetric key to encrypt the data with.

**Encryption Key from Selector Expression**:
If you have already used a symmetric key in a previous filter (for example, a **Sign Message**
filter), you can reuse that key to encrypt data by selecting this option and specifying a selector expression to obtain the key (for example, `${symmetric.key}`).

Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, a Key Property Store (KPS), or environment variable).

**Include Encryption Key in Message**:
Select this option if you want to include the encryption key in the message. The encryption key is encrypted for the recipient so that only the recipient can access the encryption key. You may choose not to include the symmetric key in the message if the API Gateway and recipient have agreed on the symmetric encryption key using some other means.

**Specify Method of Associating the Encryption Key with the Encrypted Data**:
This section enables you to configure the method by which the encrypted data references the key used to encrypt it. The following options are available:

* **Point to Encryption Key with Security Token Reference**:
    This option creates a `<SecurityTokenReference>`
    in the `<EncryptedData>`
    that points to an `<EncryptedKey>`.
* **Embed Symmetric Key Inside Encrypted Data**:
    Place the `<xenc:EncryptedKey>`
    inside the `<xenc:EncryptedData>`
    element.
* **Specify Encryption Key via Carried Keyname**:
    Place the encrypted key's carried keyname inside the`<dsig:KeyInfo>` / `<dsig:KeyName>`
    of the `<xenc:EncryptedData>`.
* **Specify Encryption Key via Retrieval Method**:
    Refer to a symmetric key using a retrieval method reference from the`<xenc:EncryptedData>`.
* **Symmetric Key Refers to Encrypted Data**:
    The symmetric key refers to `<xenc:EncryptedData>`
    using a reference list.

**Use Derived Key**:
Select this option if you want to derive a key from the symmetric key configured above to encrypt the data. The `<enc:EncryptedData>`
has a `<wsse:SecurityTokenReference>`
to the`<wssc:DerivedKeyToken>`. The `<wssc:DerivedKeyToken>`
refers to the `<enc:EncryptedKey>`. Both `<wssc:DerivedKeyToken>`
and `<enc:EncryptedKey>`
are placed inside a `<wsse:Security>`
element.

### Key info settings

The **Key Info**
tab configures the content of the `<KeyInfo>` section of the generated `<EncryptedData>`
block. Configure the following fields on this tab:

**Do Not Include KeyInfo Section**:
This option enables you to omit all information about the certificate that contains the public key that was used to encrypt the data from the `<EncryptedData>`
block. In other words, the `<KeyInfo>`
element is omitted from the `<EncryptedData>`
block.

This is useful where a downstream Web service uses an alternative method to decide what key to use to decrypt the message. In such cases, adding certificate information to the message may be regarded as an unnecessary overhead.

**Include Certificate**:
This is the default option, which places the certificate that contains the encryption key inside the `<EncryptedData>`. The following shows an example of a `<KeyInfo>`
that has been produced using this option:

```xml
<enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
  <dsig:KeyInfo>
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=Sample...</dsig:X509SubjectName>
        <dsig:X509Certificate>
           MIIEZDCCA0yg
             ....
           RNp9aKD1fEQgJ
      </dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</enc:EncryptedData>
```

**Expand Public Key**:
The details of the public key used to encrypt the data are inserted into a `<KeyValue>`
block. The `<KeyValue>`
block is only inserted when this option is selected.

```xml
<enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
  ...
  <dsig:KeyInfo>
    <dsig:X509Data>
     <dsig:X509SubjectName>CN=Sample...</dsig:X509SubjectName>
     <dsig:X509Certificate>MIIE .... EQgJ</dsig:X509Certificate>
    </dsig:X509Data>
    <dsig:KeyValue>
      <dsig:RSAKeyValue>
        <dsig:Modulus>AMfb2tT53GmMiD...NmrNht7iy18=</dsig:Modulus>
        <dsig:Exponent>AQAB</dsig:Exponent>
      </dsig:RSAKeyValue>
    </dsig:KeyValue>
  </dsig:KeyInfo>
</enc:EncryptedData>
```

**Include Distinguished Name**:
If this is selected, the Distinguished Name of the certificate that contains the public key used to encrypt the data is inserted in an `<X509SubjectName>`
element as shown in the following example:

```xml
<enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
  ...
  <dsig:KeyInfo>
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=Sample,C=IE...</dsig:X509SubjectName>
      <dsig:X509Certificate>MIIEZDCCA0yg...RNp9aKD1fEQgJ</dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</enc:EncryptedData>
```

**Include Key Name**:
This option enables you insert a key identifier, or `<KeyName>`, to allow the recipient to identify the key to use to decrypt the data. Enter an appropriate value for the `<KeyName>`
in the **Value**
field. Typical values include Distinguished Names (DName) from X.509 certificates, key IDs, or email addresses. Specify whether the specified value is a **Text value**
of a **Distinguished name attribute**
by selecting the appropriate radio button.

```xml
<enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
  ...
  <dsig:KeyInfo>
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=Sample,C=IE...</dsig:X509SubjectName>
      <dsig:X509Certificate>MIIEZDCCA0yg...RNp9aKD1fEQgJ</dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</enc:EncryptedData>
```

**Put Certificate in an Attachment**:
The API Gateway supports SOAP messages with attachments. By selecting this option, you can save the certificate containing the encryption key to the file specified in the input field. This file can then be sent along with the SOAP message as a SOAP attachment.

From previous examples, it is clear that the user's certificate is usually placed inside a `<KeyInfo>`
element. However, in this example, the certificate is contained in an attachment, and not in the `<EncryptedData>`. Clearly, you need a way to reference the certificate from the `<EncryptedData>`
block, so that the recipient can determine what key it should use to decrypt the data. This is the role of the `<SecurityTokenReference>`
block.

The `<SecurityTokenReference>`
block provides a generic mechanism for applications to retrieve security tokens in cases where these tokens are not contained in the SOAP message. The name of the security token is specified in the `URI`
attribute of the `<Reference>`
element.

```xml
<enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
  ...
  <dsig:KeyInfo>
    <wsse:SecurityTokenReference xmlns:wsse="http://schemas.xmlsoap.org/ws/...">
      <wsse:Reference URI="c:myCertificate.txt"/>
      </wsse:SecurityTokenReference>
  </dsig:KeyInfo>
</enc:EncryptedData>
```

When the message is sent, the certificate attachment is given a `Content-Id`
corresponding to the `URI`
attribute of the `<Reference>`
element. The following example shows the wire format of the complete multipart MIME SOAP message. It should help illustrate how the `<Reference>`
element refers to the `Content-ID`
of the attachment:

```
POST /adoWebSvc.asmx HTTP/1.0
Content-Length: 3790
User-Agent: API Gateway
Accept-Language: en
Content-Type: multipart/related; type="text/xml";
              boundary="----=Multipart-SOAP-boundary"

------=Multipart-SOAP-boundary
Content-Id: soap-envelope
Content-Type: text/xml; charset="utf-8";
    SOAPAction=getQuote

<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
     ...
  <enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#">
     ...
    <dsig:KeyInfo>
     <ws:SecurityTokenReference xmlns:ws="http://schemas.xmlsoap.org/ws/...">
        <ws:Reference URI="c:myCertificate.txt"/>
     </ws:SecurityTokenReference>
    </dsig:KeyInfo>
  </enc:EncryptedData>
...
</s:Envelope>

------=Multipart-SOAP-boundary
Content-Id: c:myCertificate.txt
Content-Type: text/plain; charset="US-ASCII"

MIIEZDCCA0ygAwIBAgIBAzANBgkqhki
....
7uFveG0eL0zBwZ5qwLRNp9aKD1fEQgJ
------=Multipart-SOAP-boundary-
```

**Security Token Reference**:
A `<wsse:SecurityTokenReference>`
element can be used to point to the security token used to encrypt the data. If you wish to use a `<wsse:SecurityTokenReference>`, enable this option, and select a Security Token Reference type from **Reference Type**
drop-down list.

The `<wsse:SecurityTokenReference>` (in `<dsig:KeyInfo>`) may contain a `<wsse:Embedded>`
security token. Alternatively, the `<wsse:SecurityTokenReference>`, (in the `<dsig:KeyInfo>`), may refer to a certificate using a `<dsig:X509Data>`. Select the appropriate button, **Embed**
or **Refer**, depending on whether you want to use an embedded security token or a referred one.

If you have configured the `SecurityContextToken (sct)`
mechanism from the **Security Token Reference**
drop-down list, you can select to use an **Attached SCT**
or an **Unattached SCT**. The default option is to use an **Attached SCT**, which should be used in cases where the SCT refers to a security token inside the `<wsse:Security>`
header. If the SCT is located outside the `<wsse:Security>`
header, you should select the **Unattached SCT**
option.

You can make sure to include a `<BinarySecurityToken>`
(BST) that contains the certificate (that contains the encryption key) in the message by selecting the **Include BinarySecurityToken**
option. The BST is inserted into the WS-Security header regardless of the type of Security Token Reference selected from the list.

Select **Include TokenType**
if you want to add the `TokenType`
attribute to the `SecurityTokenReference`
element.

When using the Kerberos Token Profile standard, and the API Gateway is acting as the initiator of a secure transaction, it can use Kerberos session keys to encrypt a message. The `KeyInfo`
must be configured to use a Security Token Reference with a `ValueType`
of `GSS_Kerberosv5_AP_REQ`. In this case, the Kerberos token is contained in a `<BinarySecurityToken>`
in the message.

If the API Gateway is acting as the recipient of a secure transaction, it can also use the Kerberos session keys to encrypt the message returned to the client. However, the `KeyInfo`
must be configured to use a Security Token Reference with `ValueType`
of `Kerberosv5_APREQSHA1`. When this is selected, the Kerberos token is not contained in the message. The Security Token Reference contains a SHA1 digest of the original Kerberos token received from the client, which identifies the session keys to the client.

When using the WS-Trust for SPENGO standard, the Kerberos session keys are not used directly to encrypt messages because a security context with an associated symmetric key is negotiated. This symmetric key is shared by both client and service and can be used to encrypt messages on both sides.

### Recipient settings

XML Messages can be encrypted for multiple recipients. In such cases, the symmetric encryption key is encrypted with the public key of each intended recipient and added to the message. Each recipient can then decrypt the encryption key with their private key and use it to decrypt the message.

The following SOAP message has been encrypted for 2 recipients (*axway_1*
and *axway_2*). The encryption key has been encrypted twice: once for
*axway_1* using its public key, and a second time for *axway_2* using its public key:

{{< alert title="Note" color="primary" >}}The data itself is only encrypted once, while the encryption key must be encrypted for each recipient. For illustration purposes, only those elements relevant to the above discussion have been included in the following XML encrypted message.{{< /alert >}}

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <Security xmlns="http://schemas.xmlsoap.org/ws/2003/06/secext" s:actor="Enc Keys">
      <enc:EncryptedKey xmlns:enc="http://www.w3.org/2001/04/xmlenc#"
          Id="0000418BBB61-A692675C" MimeType="text/xml">
         ...
       <enc:CipherData>
         <!-- Enc key encrypted with axway_1's public key and base64-encoded -->
         <enc:CipherValue>AAAAAExx1A ... vuAhCgMQ==</enc:CipherValue>
       </enc:CipherData>
       <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
         <dsig:KeyName>axway_1</dsig:KeyName>
       </dsig:KeyInfo>
       <enc:CarriedKeyName>Session key</enc:CarriedKeyName>
       <enc:ReferenceList>
          <enc:DataReference URI="#0000418BBB61-D4495D9B"/>
       </enc:ReferenceList>
      </enc:EncryptedKey>
      <enc:EncryptedKey xmlns:enc="http://www.w3.org/2001/04/xmlenc#"
           Id="#0000418BBB61-D4495D9B" MimeType="text/xml">
            ...
        <enc:CipherData>
          <!-- Enc key encrypted with axway_2's public key and base64-encoded -->
          <enc:CipherValue>AAAAABZH+U ... MrMEEM/Ps=</enc:CipherValue>
        </enc:CipherData>
        <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
          <dsig:KeyName>axway_2</dsig:KeyName>
        </dsig:KeyInfo>
        <enc:CarriedKeyName>Session key</enc:CarriedKeyName>
          <enc:ReferenceList>
             <enc:DataReference URI="#0000418BBB61-D4495D9B"/>
          </enc:ReferenceList>
        </enc:EncryptedKey>
    </Security>
  </s:Header>
  <enc:EncryptedData xmlns:enc="http://www.w3.org/2001/04/xmlenc#"
       Id="0000418BBB61-D4495D9B" MimeType="text/xml"
       Type="http://www.w3.org/2001/04/xmlenc#Element">
    <enc:EncryptionMethod Algorithm="http://www.w3.org/2001/04xmlenc#aes256-cbc">
      <enc:KeySize>256</enc:KeySize>
    </enc:EncryptionMethod>
    <enc:CipherData>
      <!-- SOAP Body encrypted with symmetric enc key and base64-encoded -->
      <enc:CipherValue>WD0TmuMk9 ... GzYFeq8SM=</enc:CipherValue>
    </enc:CipherData>
    <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
      <dsig:KeyName>Session key</dsig:KeyName>
    </dsig:KeyInfo>
  </enc:EncryptedData>
</s:Envelope>
```

There are two `<EncryptedKey>`
elements, one for each recipient. The `<CipherValue>`
element contains the symmetric encryption key encrypted with the recipient's public key. The encrypted symmetric key must be Base64-encoded so that it can be represented as the textual contents of an XML element.

The `<EncryptedData>`
element contains the encrypted data, along with information about the encryption process, including the encryption algorithm used, the size of the encryption key, and the type of data that was encrypted (for example, whether an element or the contents of an element was encrypted).

Click the **Add**
button to add a new recipient for which the data is to be encrypted. Configure the following fields on the **XML Encryption Recipient**
dialog:

**Recipient Name**:
Enter a name for the recipient. This name can then be selected on the main **Recipients**
tab of the filter.

**Actor**:
The `<EncryptedKey>`
for this recipient is inserted into the specified SOAP actor/role.

**Use Key in Message Attribute**:
Specify the message attribute that contains the recipient's public key that is used to encrypt the data. By default, the `certificate`
attribute is used. Typically, this attribute is populated by the **Find Certificate**
filter, which retrieves a certificate from any one of a number of locations, including the Certificate Store, an LDAP directory, HTTP header, or from the message itself.

If you want to encrypt the message for multiple recipients, you must configure multiple **Find Certificate**
filters (or some other filter that can retrieve certificates). Each **Find Certificate**
filter retrieves a certificate fora single recipient and store it in a unique message attribute.

For example, a **Find Certificate**
filter called **Find Certificate for Recipient1**
filter could locate Recipient1's certificate from the Certificate Store and store it in a `certificate_recip1`
message attribute. You would then configure a second **Find Certificate**
filter called **Find Certificate for Recipient2**
, which could retrieve Recipient2's certificate from the Certificate Store and store it in a `certificate_recip2`
message attribute.

On the **Recipients**
tab of the **XML Encryption Settings**
filter, you would then configure two recipients. For the first recipient (Recipient1), you would enter `certificate_recip1`
as the location of the encryption key, while for the second recipient (Recipient2), you would specify `certificate_recip2`
as the location of the encryption key.

{{< alert title="Note" color="primary" >}}If the API Gateway fails to encrypt the message for any of the recipients configured on the **Recipients**
tab, the filter fails.{{< /alert >}}

### What to encrypt settings

The **What to Encrypt**
tab is used to identify parts of the message that must be encrypted. Each encrypted part is replaced by an `<EncryptedData>`
block, which contains all information required to decrypt the block.

You can use any
combination of **Node Locations**, **XPaths**, and the nodes contained in a **Message Attribute**
to specify the nodes that are required to be encrypted.

{{< alert title="Note" color="primary" >}}There is a difference between encrypting the element and encrypting its content. When encrypting the element, the entire element is replaced by the `<EncryptedData>`
block. This is not recommended if you wish to encrypt the SOAP Body. If this element is removed from the SOAP message, the message may not be a valid SOAP message.{{< /alert >}}

Element encryption is more suitable when encrypting security blocks, (for example, WS-Security Username tokens and SAML assertions) that may appear in a WS-Security header of a SOAP message. In such cases, replacing the element content (for example, a `<UsernameToken>`
element) with an `<EncryptedData>`
block does not affect the semantics of the WS-Security header.

If you wish to encrypt the SOAP Body, you should use element content encryption, where the children of the element are replaced by the `<EncryptedData>`
block. In this way, the message can still be validated against the SOAP schema.

When using **Node Locations**
to identify nodes that are to be encrypted, you can configure whether to encrypt the element or the element contents on the **Locate XML Nodes**
dialog. To encrypt the element, select the **Encrypt Node**
radio button. Alternatively, to encrypt the element contents, select **Encrypt Node Content**.

If you are using XPath expressions to specify the nodes that are to be signed, be careful not to use an expression that returns a node and all its contents. The **Encrypt Node**
and **Encrypt Node Content**
options are also available when configuring XPath expressions on the **Enter XPath Expression**
dialog.

### Advanced XML encryption settings

The **Advanced**
tab on the main **XML-Encryption Settings**
screen enables you to configure some of the more complicated settings regarding XML-Encryption. The following settings are available:

**Algorithm Suite Tab**:
The following fields can be configured on this tab:

**Algorithm Suite**:
WS-Security Policy defines a number of *algorithm suites*
that group together a number of cryptographic algorithms. For example, a given algorithm suite uses specific algorithms for asymmetric encryption, symmetric encryption, asymmetric key wrap, and so on. Therefore, by specifying an algorithm suite, you are effectively selecting a whole suite of cryptographic algorithms to use.

If you want to use a particular WS-Security Policy algorithm suite, you can select it here. The **Encryption Algorithm**
and **Key Wrap Algorithm**
fields are automatically populated with the corresponding algorithms for that suite.

**Encryption Algorithm**:
The encryption algorithm selected is used to encrypt the data. The following algorithms are available:

* AES-256
* AES-192
* AES-128
* Triple DES

**Key Wrap Algorithm**:
The key wrap algorithm selected here is used to wrap (encrypt)the symmetric encryption key with the recipient's public key. The following key wrap algorithms are available:

* KwRsa15
* KwRsaOaep

**Generate a Reference List in WS-Security Block**:
When this option is selected, a `<xenc:ReferenceList>`
that holds a reference to all encrypted data elements is generated. The `<xenc:ReferenceList>`
element is inserted into the WS-Security block indicated by the specified actor.

**Insert Reference List into EncryptedKey**:
When this option is selected, a `<xenc:ReferenceList>`
that holds a reference to all encrypted data elements is generated. The `<xenc:ReferenceList>`
element is inserted into the `<xenc:EncryptedKey>`
element.

**Layout Type**:
Select the WS-SecurityPolicy layout type that you want the generated tokens to adhere to. This includes elements such as the `<EncryptedData>`, `<EncryptedKey>`, `<ReferenceList>`, `<BinarySecurityToken>`, and `<DerivedKeyToken>`
tokens, among others.

**Fail if no Nodes to Encrypt**:
Select this option if you want the filter to fail if any of the nodes specified on the **What to Encrypt**
tab are found in the message.

**Insert Timestamp**:
This option enables you to insert a WS-Security Timestamp as an encryption property.

**Indent**:
This option enables you to format the inserted `<EncryptedData>`
and `<EncryptedKey>`
blocks by indenting the elements.

**Insert CarriedKeyName for EncryptedKey**:
Select this option to insert a `<CarriedKeyName>`
element into the generated `<EncryptedKey>`
block.

### Auto-generation using the XML encryption settings wizard

Because the **XML-Encryption Settings**
filter must always be used in conjunction with the **XML-Encryption**
and **Find Certificate**
filters, the Policy Studio provides a wizard that can generate these three filters at the same time. Right-click a policy under the **Policies**
node in the Policy Studio, and select **XML Encryption Settings**.

## XML decryption settings filter

The API Gateway can decrypt an XML encrypted message on behalf of its intended recipients. XML Encryption is a W3C standard that enables data to be encrypted and decrypted at the application layer of the OSI stack, thus ensuring complete end-to-end confidentiality of data.

You should use the **XML-Decryption Settings**
in conjunction with the **XML-Decryption**
filter, which performs the decryption. The **XML-Decryption Settings**
generates the `decryption.properties`
message attribute, which is required by the **XML-Decryption**
filter.

{{< alert title="Note" color="primary" >}}The output of a successfully executed decryption filter is the original unencrypted message. Depending on whether the **Remove Encrypted Key used in decryption**
has been enabled, all information relating to the encryption key can be removed from the message. For more details, see [Options](#options).{{< /alert >}}

To understand how XML encryption works, see [XML encryption overview](#xml-encryption-overview). The following sections describe how to configure the **XML Decryption Settings**
filter to decrypt encrypted XML data.

### Nodes to decrypt

An XML message may contain several `EncryptedData`
blocks. The **Node(s) to Decrypt**
section enables you to specify which encryption blocks are to be decrypted using the following approaches:

* Decrypt all encrypted nodes
* Use XPath to select encrypted nodes

Configure one of the following settings:

**Decrypt All**:
The API Gateway attempts to decrypt *all* `EncryptedData`
blocks contained in the message.

**Use XPath**:
This option enables the administrator to explicitly choose the `EncryptedData`
block that the API Gateway should decrypt.

For example, the following skeleton SOAP message contains two `EncryptedData`
blocks:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>...<s:Header>
  <s:Body>
    <!-- 1st EncryptedData block -->
    <e:EncryptedData xmlns:e="http://www.w3.org/2001/04/xmlenc#"
       Encoding="iso-8859-1" Id="ENC_1" MimeType="text/xml"
       Type="http://www.w3.org/2001/04/xmlenc#Element">
        ...
    </e:EncryptedData>
    <!-- 2nd EncryptedData block -->
      <e:EncryptedData xmlns:e="http://www.w3.org/2001/04/xmlenc#"
        Encoding="iso-8859-1" Id="ENC_2" MimeType="text/xml"
        Type="http://www.w3.org/2001/04/xmlenc#Element">
         ...
    </e:EncryptedData>
  </s:Body>
</s:Envelope>
```

The `EncryptedData`
blocks are selected using XPath. You can use the following XPath expressions to select the respective `EncryptedData`
blocks:

|EncryptedData Block|XPath Expression|
|-------------------|----------------|
|1st                |`//enc:EncryptedData[@Id='ENC_1']`|
|2nd                |`//enc:EncryptedData[@Id='ENC_2']`|

Click the **Add**, **Edit**, or **Delete**
buttons to add, edit, or remove an XPath expression.

### Decryption key

The **Decryption Key**
section enables you to specify the key to use to decrypt the encrypted nodes. As discussed in [XML encryption wizard](#xml-encryption-wizard), data encrypted with a public key can only be decrypted with the corresponding private key. The **Decryption Key**
settings enable you to specify the private (decryption) key from the `<KeyInfo>`
element of the XML Encryption block, or the certificate stored in the Axway message attribute can be used to lookup the private key of the intended recipient of the encrypted data in the Certificate Store.

**Find via KeyInfo in Message**:
Select this option if you wish to determine the decryption key to use from the `KeyInfo`
section of the `EncryptedKey`
block. The `KeyInfo`
section contains a reference to the public key used to encrypt the data. You can use this `KeyInfo`
section reference to find the relevant private key (from the Axway Certificate Store) to use to decrypt the data.

**Find via certificate from Selector Expression**:
Select this option if you do not wish to use the `KeyInfo`
section in the message. Enter a selector expression that contains a certificate, (for example, `${certificate}`) whose corresponding private key is stored in the Axway Certificate Store.

**Extract nodes from Selector Expression**:
Specify whether to extract nodes from a specified selector expression (for example, `${node.list}`). This setting is not selected by default.

Typically, a **Find Certificate**
filter is used in a policy to locate an appropriate certificate and store it in the `certificate`
message attribute. When the certificate has been stored in this attribute, the **XML Decryption Settings**
filter can use this certificate to look up the Certificate Store for a corresponding private key for the public key stored in the certificate. To do this, select the `certificate`
attribute from the drop-down list.

### Options

The following configuration settings are available in the **Options**
section:

**Fail if no encrypted data found**:
If this option is selected, the filter fails if no `<EncryptedData>`
elements are found within the message.

**Remove the Encrypted Key used in decryption**:
Select this option to remove information relating to the decryption key from the message. When this option is selected, the `<EncryptedKey>`
block is removed from the message.

{{< alert title="Note" color="primary" >}}In cases where the `<EncryptedKey>`
block has been included in the `<EncryptedData>`
block, it is removed regardless of whether this setting has been selected.{{< /alert >}}

**Default Derived Key Label**:
If the API Gateway consumes a `<DerivedKeyToken>`, the default value entered is used to recreate the derived key that is used to decrypt the encrypted data.

**Algorithm Suite Required**:
Select the WS-Security Policy *Algorithm Suite*
that must have been used when encrypting the message. This check ensures that the appropriate algorithms were used to encrypt the message.

### Auto-generation using the XML decryption settings wizard

Because the **XML-Decryption Settings**
filter must always be paired with an **XML-Decryption**
filter, it makes sense to have a wizard that can generate both of these filters at the same time. To use the wizard, right-click the name of the policy in the tree view of the Policy Studio, and select the **XML Decryption Settings**
menu option.

Configure the fields on the **XML Decryption Settings**
dialog as explained in the previous sections. When finished, an **XML-Decryption Settings**
filter is created along with an **XML-Decryption**
filter.

## XML encryption filter

The **XML-Encryption**
filter is responsible for encrypting parts of XML messages based on the settings configured in the **XML-Encryption Settings**
filter.

The **XML-Encryption Settings**
filter generates the `encryption.properties`
message attribute based on configuration settings. The **XML-Encryption**
filter uses these properties to perform the encryption of the data.

## XML decryption filter

The **XML-Decryption**
filter is responsible for decrypting data in XML messages based on the settings configured in the **XML-Decryption Settings**
filter.

The **XML-Decryption Settings**
filter generates the `decryption.properties`
message attribute based on configuration settings. The **XML-Decryption**
filter uses these properties to perform the decryption of the data.

## XML encryption wizard

The following filters are involved in encrypting a message using XML encryption:

* Find Certificate: Specifies the certificate that contains the public key to use in the encryption. The data is encrypted such that it can only be decrypted with the corresponding private key.
* XML-Encryption Settings: Specifies the recipient of the encrypted data, what data to encrypt, what algorithms to use, and other such options that affect the way the data is encrypted.
* XML-Encryption: Performs the actual encryption using the certificate selected in the **Find Certificate** filter, and the options set in the **XML-Encryption Settings** filter.

While these filters can be configured independently of each other, it makes sense to configure them all at the same time because they must play a role in the policy that XML-encrypts messages. You can do this using the **XML Encryption Wizard**.

The wizard is available by right-clicking the name of the policy in the tree view of the Policy Studio, and selecting the **XML Encryption Settings**
menu option. The next section describes how to configure the settings on this dialog.

The first step in configuring the **XML Encryption Wizard**
is to select the certificate that contains the public key to use to encrypt the data. When the data has been encrypted with this public key, it can only be decrypted using the corresponding private key. Select the relevant certificate from the list of **Certificates in the Trusted Certificate Store**.

When the wizard is completed, the information configured on this screen results in the auto-generation of a **Find Certificate**
filter. This filter is automatically configured to use the selected certificate from the Certificate Store. For more details,see *Find certificate.

After clicking **Next**
on the first screen of the wizard, the configuration options for the **XML-EncryptionSettings**
filter are displayed.

When you have completed all the steps in the wizard, a policy is created that comprises a **Find Certificate**, **XML-Encryption Settings**, and **XML-Encryption**
filter. You can insert other filters into this policy as required. However, the order of the encryption filters must be maintained as follows:

* Find Certificate
* XML-Encryption Settings
* XML-Encryption
