{
"title": "XML integrity filters",
"linkTitle": "XML integrity filters",
"weight": 96,
"date": "2019-10-17",
"description": "Sign and verify XML messages."
}

## XML signature generation filter

The API Gateway can sign both SOAP and non-SOAP XML messages. Attachments to the message can also be signed. The resulting XML signature is inserted into the message for consumption by a downstream web service. At the web service, the signature can be used to authenticate the message sender and verify the integrity of the message.

You can configure the **XML Signature Generation** filter to generate a symmetric key to sign the message symmetrically, and automatically populate the `symmetric.key` message attribute with the generated key, so that a successive filter, for example, the **XML-Encryption Settings** filter, can use that symmetric key.

For more details on XML signature validating the integrity of the message, see [XML signature verification](#xml-signature-verification).

### Signing key settings

On the **Signing Key**
tab, you can select either a symmetric or an asymmetric key to sign the message content. Select the appropriate radio button and configure the fields on the corresponding tab.

#### Asymmetric Key

With an asymmetric signature, the signatory's private key (from a public-private key pair) is used to sign the message. The corresponding public key is then used to verify the signature. The following fields are available for configuration on this tab:

**Private Key in Certificate Store**:
To use a signing key from the certificate store, select **Key in Store**, and click **Signing Key**. Select a certificate that has the required signing key associated with it. The signing key can also be stored on a Hardware Security Module (HSM).

The *Distinguished Name*
of the selected certificate appears in the `X509SubjectName`
element of the XML signature as follows:

```xml
<dsig:X509SubjectName>
    CN=Sample,OU=R&D,O=Company Ltd.,L=Dublin 4,ST=Dublin,C=IE
</dsig:X509SubjectName>
```

**Private Key from Selector Expression**:
Alternatively, the signing key might have already have been used by another filter and stored in a message attribute. To reuse this key, select **Private Key from Selector Expression**, and enter the selector expression (for example, `${asymmetric.key}`).

#### Symmetric Key

With a symmetric signature, the same key is used to sign and verify the message. Typically the client generates the symmetric key and uses it to sign the message. The key must then be transmitted to the recipient so that they can verify the signature.

It would be unsafe to transmit an unprotected key along with the message so it is usually encrypted (or wrapped) with the recipient's public key. The key can then be decrypted with the recipient's private key and can then be used to verify the signature.

The following configuration options are available on this window:

**Generate Symmetric Key, and Save in Message Attribute**:
If you select this option, the API Gateway generates a symmetric key, which is included in the message before it is sent to the client. By default, the key is saved in the `symmetric.key`
message attribute.

**Symmetric Key from Selector Expression**:
If a previous filter (for example, a **Sign Message**
filter) has already used a symmetric key, you can to reuse this key as proof that the API Gateway is the holder-of-key entity. Enter the name of the selector expression in the field provided, which defaults to `${symmetric.key}`.

**Include Encrypted Symmetric Key in Message**:
The symmetric key is typically encrypted for the recipient and included in the message. However, the initiator and recipient of the transaction may have agreed on a symmetric key using some out-of-bounds mechanism. In this case, it is not necessary to include the key in the message. However, the default option is to include the encrypted symmetric key in the message. The `<KeyInfo>`
section of the signature points to the `<EncryptedKey>`.

**Encrypt with Key in Store**:
Select this to encrypt the symmetric key with a public key from the certificate store. Click **Signing Key**
and select the certificate that contains the public key of the recipient. By encrypting the symmetric key with this public key, you are ensuring that only the recipient that has access to the corresponding private key can decrypt the encrypted symmetric key.

**Encrypt with Key from Selector Expression**:
You can also use a key stored in a message attribute to encrypt (or wrap) the symmetric key. Select this setting and enter the selector expression to obtain the public key you want to use to encrypt the symmetric key with.

**Use Derived Key**:
A `<wssc:DerivedKeyToken>`
token can be used to derive a symmetric key from the original symmetric key held in and `<enc:EncryptedKey>`. The derived symmetric key is then used to actually sign the message, instead of the original symmetric key. It must be derived again during the verification process using the parameters in the `<wssc:DerivedKeyToken>`. One of these parameters is the symmetric key held in `<enc:EncryptedKey>`.

The following example shows the use of a derived key:

```xml
<enc:EncryptedKey Id="Id-0000010b8b0415dc-0000000000000000">
<enc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#rsa-1_5"/>
  <dsig:KeyInfo>
    ...
  </dsig:KeyInfo>
  <enc:CipherData>
</enc:EncryptedKey>

<wssc:DerivedKeyToken wsu:Id="Id-0000010bd2b8eca1-0000000000000017"
  Algorithm="http://schemas.xmlsoap.org/ws/2005/02/sc/dk/p_sha1">
  <wsse:SecurityTokenReference wsu:Id="Id-0000010bd2b8ed5d-0000000000000018">
    <wsse:Reference URI="#Id Id-0000010b8b0415dc-0000000000000000"
    ValueType="..../oasis-wss-soap-message-security-1.1#EncryptedKey"/>
  </wsse:SecurityTokenReference>
  <wssc:Generation>0</wssc:Generation>
  <wssc:Length>32</wssc:Length>
  <wssc:Label>WS-SecureConverstaionWS-SecureConverstaion</wssc:Label>
  <wssc:Nonce>h9TTWKRylCOz87+mc1/7Pg==</wssc:Nonce>
</wssc:DerivedKeyToken>

<dsig:Signature Id="Id-0000010b8b0415dc-0000000000000004">
  <dsig:SignedInfo>
    <dsig:CanonicalizationMethod
    Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
    <dsig:SignatureMethod
    Algorithm="http://www.w3.org/2000/09/xmldsig#hmac-sha1"/>
    <dsig:Reference>...</dsig:Reference>
  </dsig:SignedInfo>
  <dsig:SignatureValue>...dsig:SignatureValue>
  <dsig:KeyInfo>
    <wsse:SecurityTokenReference wsu:Id="Id-0000010b8b0415dc-0000000000000006">
    <wsse:Reference
    URI="# Id-0000010bd2b8eca1-0000000000000017"
    ValueType="http://schemas.xmlsoap.org/ws/2005/02/sc/dk"/>
    </wsse:SecurityTokenReference>
  </dsig:KeyInfo>
</dsig:Signature>
```

**Symmetric Key Length**:
This enables the user to specify the length of the key to use when performing symmetric key signatures. It is important to realize that the longer the key, the stronger the encryption.

#### Key Info

This tab configures how the `<KeyInfo>`
block of the generated XML signature is displayed. Configure the following fields on this tab:

**Do Not Include KeyInfo Section**:
This option enables you to omit all information about the signatory's certificate from the signature. In other words, the `KeyInfo`
element is omitted from the signature. This is useful where a downstream web service uses an alternative method of authenticating the signatory, and uses the signature for the sole purpose of verifying the integrity of the message. In such cases, adding certificate information to the message is an unnecessary overhead.

**Include Certificate**:
This is the default option which places the signatory's certificate inside the XML signature itself. The following example shows an example of an XML signature using this option:

```xml
<dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
  ...
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
</dsig:Signature>
```

**Expand Public Key**:
The details of the signatory's public key are inserted into a `KeyValue`
block. The `KeyValue`
block is only inserted when this option is selected.

```xml
<dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
  ...
  <dsig:KeyInfo>
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=Sample...</dsig:X509SubjectName>
      <dsig:X509Certificate>
        MIIE ....... EQgJ
      </dsig:X509Certificate>
    </dsig:X509Data>
    <dsig:KeyValue>
      <dsig:RSAKeyValue>
        <dsig:Modulus>
          AMfb2tT53GmMiD
          ...
          NmrNht7iy18=
        </dsig:Modulus>
        <dsig:Exponent>AQAB</dsig:Exponent>
      </dsig:RSAKeyValue>
    </dsig:KeyValue>
  </dsig:KeyInfo>
</dsig:Signature>
```

**Include Distinguished Name**:
If this is selected, the Distinguished Name of the signatory's X.509 certificate is inserted in an `<X509SubjectName>`
element as shown in the following example:

```xml
<dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
  ...
  <dsig:KeyInfo>
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=Sample,C=IE...</dsig:X509SubjectName>
      <dsig:X509Certificate>
        MIIEZDCCA0yg
        ....
        RNp9aKD1fEQgJ
      </dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</dsig:Signature>
```

**Include Key Name**:
This option allows you insert a key identifier, or `KeyName`, to allow the recipient to identify the signatory. Enter an appropriate value for the `KeyName`
in the **Value**
field. Typical values include Distinguished Names (DName) from X.509 certificates, key IDs, or email addresses. Specify whether the specified value is a **Text value**
of a **Distinguished name attribute**
by selecting the appropriate setting.

```xml
<dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
  ...
  <dsig:KeyInfo>
    <dsig:KeyName>test@axway.com</dsig:KeyName>
  </dsig:KeyInfo>
</dsig:Signature>
```

**Put Certificate in an Attachment**:
The API Gateway supports SOAP messages with attachments. By selecting this option, you can save the signatory's certificate to the file specified in the input field. This file can then be sent along with the SOAP message as a SOAP attachment.

From previous examples, it is clear that the user's certificate is usually placed inside a `KeyInfo`
element. However, in this example, the certificate is contained in an attachment, and not in the XML signature itself. To reference the certificate from the XML signature, so that validating applications can process the signature correctly, is the role of the `SecurityTokenReference`
block.

The `SecurityTokenReference`
block provides a generic way for applications to retrieve security tokens in cases where these tokens are not contained in the SOAP message. The name of the security token is specified in the `URI`
attribute of the `Reference`
element:

```xml
<dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
  ...
  <dsig:KeyInfo>
    <wsse:SecurityTokenReference xmlns:wsse="http://schemas.xmlsoap.org/ws/...">
      <wsse:Reference URI="c:myCertificate.txt"/>
    </wsse:SecurityTokenReference>
  </dsig:KeyInfo>
</dsig:Signature>
```

When the message is sent, the certificate attachment is given a `Content-Id` corresponding to the `URI`
attribute of the `Reference`
element.

The following example shows what the complete multipart MIME SOAP message looks like as it is sent over the wire. This illustrates how the `Reference`
element refers to the `Content-Id` of the attachment:

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
  <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
    ...
    <dsig:KeyInfo>
      <ws:SecurityTokenReference xmlns:ws="http://schemas.xmlsoap.org/ws/...">
        <ws:Reference URI="c:myCertificate.txt"/>
      </ws:SecurityTokenReference>
    </dsig:KeyInfo>
  </dsig:Signature>
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
element can be used to point to the security token used in the generation of the signature. Select this option to use this element. The type of the reference must be selected from the **Reference Type**
field.

The `<wsse:SecurityTokenReference>` (in the `<dsig:KeyInfo>`) can contain a `<wsse:Embedded>`
security token. Alternatively, the `<wsse:SecurityTokenReference>`, (in the `<dsig:KeyInfo>`) can refer to a certificate via a `<dsig:X509Data>`. Select the appropriate button, **Embed**
or **Refer**, depending on whether you want to use an embedded security token or a referred one.

You can make sure to include a `<BinarySecurityToken>`
(BST) that contains the certificate used to wrap the symmetric key in the message by selecting the **Include BinarySecurityToken**
option. The BST is inserted into the WS-Security header regardless of the type of Security Token Reference selected.

When using the Kerberos Token Profile standard and the API Gateway is acting as the initiator of a secure transaction, it can use Kerberos session keys to sign a message. The `KeyInfo`
must be configured to use a Security Token Reference with a `ValueType`
of `GSS_Kerberosv5_AP_REQ`. In this case, the Kerberos token is contained in a `<BinarySecurityToken>`
in the message.

If the API Gateway is acting as the recipient of a secure transaction, it can also use the Kerberos session keys to sign the message returned to the client. However, in this case, the `KeyInfo`
must be configured to use a Security Token Reference with `ValueType`
of `Kerberosv5_APREQSHA1`. When this `ValueType`
is selected, the Kerberos token is not contained in the message. The Security Token Reference contains a SHA1 digest of the original Kerberos token received from the client, which identifies the session keys to the client.

Using the WS-Trust for SPNEGO standard, the Kerberos session keys are not used directly to sign messages because a security context with an associated symmetric key is negotiated. This symmetric key is shared by both client and service and can be used to sign messages on both sides.

### What to sign settings

The **What to Sign**
tab is used to identify parts of the message that must be signed. Each signed part is referenced from within the generated XML signature. You can use *any*
combination of **Node Locations**, **XPaths**, **XPath Predicates**, and the nodes contained in a **Message Attribute**
to specify what must be signed.

**XML Signing Mechanisms**
It is important to consider the mechanisms available for referencing signed elements from within an XML signature. For example, With WSU Ids, an Id attribute is inserted into the root element of the nodeset that is to be signed. The XML signature then references this Id to indicate to verifiers of the signature the nodes that were signed. The use of WSU Ids is the default option because these are WS-I compliant.

Alternatively, a generic `Id` attribute (not bound to the WSU namespace) can be used to dereference the data. The `Id` attribute is inserted into the top-level element of the nodeset that is to be signed. The generated XML signature can then reference this Id to indicate what nodes were signed. When XPath transforms are used, an XPath expression that points to the root node of the nodeset that is signed is inserted into the XML signature. When attempting to verify the signature, this XPath expression must be run on the message to retrieve the signed content.

**Id Attribute**:
Select the Id attribute used to dereference the signed element in the `dsig:Signature`. The available options are as follows:

* *wsu:Id*: The default option references the signed data using a `wsu:Id`
    attribute. A `wsu:Id`
    attribute is inserted into the root node of the signed nodeset. This Id is then referenced in the generated XML signature as an indication of which nodes were signed. For example:

    ```xml
    <soap:Envelope xmlns:soap="...">
      <soap:Header>
        <wsse:Security xmlns:wsse="...">
          <dsig:Signature xmlns:dsig="..." Id="Id-00000112e2c98df8-0000000000000004">
            <dsig:SignedInfo>
              <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                  xml-exc-c14n#" />
              <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                    xmldsig#rsa-sha1" />
              <dsig:Reference URI="#Id-00000112e2c98df8-0000000000000003">
                <dsig:Transforms>
                  <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                </dsig:Transforms>
                <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                <dsig:DigestValue>xChPoiWJJrrPZkbXN8FPB8S4U7w=</dsig:DigestValue>
              </dsig:Reference>
            </dsig:SignedInfo>
            <dsig:SignatureValue>KG4N .... /9dw==</dsig:SignatureValue>
            <dsig:KeyInfo Id="Id-00000112e2c98df8-0000000000000005">
              <dsig:X509Data>
                <dsig:X509Certificate>
                  MIID ... ZiBQ==
                </dsig:X509Certificate>
              </dsig:X509Data>
            </dsig:KeyInfo>
          </dsig:Signature>
        </wsse:Security>
      </soap:Header>
      <soap:Body xmlns:wsu="..." wsu:Id="Id-00000112e2c98df8-0000000000000003">
        <vs:getProductInfo xmlns:vs="http://www.axway.com">
          <vs:Name>API Gateway</vs:Name>
          <vs:Version>7.7</vs:Version>
        </vs:getProductInfo>
      </s:Body>
      </s:Envelope>
    ```

    In this example, a `wsu:Id`
    attribute has been inserted into the `<soap:Body>`
    element. This `wsu:Id`
    attribute is then referenced by the `URI`
    attribute of the `<dsig:Reference>`
    element in the actual signature. When the signature is being verified, the value of the `URI`
    attribute can be used to locate the nodes that have been signed.
* *Id*: Select the `Id`
    option to use generic Ids (not bound to the WSU namespace) to dereference the signed data. Under this schema, the `URI`
    attribute of the `<Reference>`
    points at an Id attribute, which is inserted into the top-level node of the signed nodeset. In the following example, the Id specified in the signature matches the Id attribute inserted into the `<Body>`
    element, indicating that the signature applies to the entire contents of the SOAP body:

    ```xml
    <soap:Envelope xmlns:soap="....">
      <soap:Header>
        <dsig:Signature xmlns:dsig="...." Id="Id-0000011a101b167c-0000000000000013">
          <dsig:SignedInfo>
            <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                 xml-exc-c14n#"/>
            <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                 xmldsig#rsa-sha1"/>
            <dsig:Reference URI="#Id-0000011a101b167c-0000000000000012">
              <dsig:Transforms>
                <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </dsig:Transforms>
              <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <dsig:DigestValue>JCy0JoyhVZYzmrLrl92nxfr1+zQ=</dsig:DigestValue>
            </dsig:Reference>
          </dsig:SignedInfo>
          <dsig:SignatureValue>
            ......
            <dsig:SignatureValue>
              <dsig:KeyInfo Id="Id-0000011a101b167c-0000000000000014">
                <dsig:X509Data>
                  <dsig:X509Certificate>......</dsig:X509Certificate>
                </dsig:X509Data>
              </dsig:KeyInfo>
        </dsig:Signature>
      </soap:Header>
      <soap:Body Id="Id-0000011a101b167c-0000000000000012">
        <product version="7.7">
          <name>API Gateway</name>
          <company>axway</company>
          <description>SOA Security and Management</description>
        </product>
      </soap:Body>
    </soap:Envelope>
    ```

* *ID*: Select this option to use generic IDs (not bound to the WSU namespace) to dereference the signed data. Under this schema, the URI attribute of the `Reference`
    points at an ID attribute, which is inserted into the top-level node of the signed nodeset. In the following example, the URI specified in the Signature Reference node matches the ID attribute inserted into the `Body`
    element, indicating that the signature applies to the entire contents of the SOAP body:

    ```xml
    <soap:Envelope xmlns:soap="....">
      <soap:Header>
        <dsig:Signature xmlns:dsig="....">
          <dsig:SignedInfo>
            <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                 xml-exc-c14n#"/>
            <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                 xmldsig#rsa-sha1"/>
            <dsig:Reference URI="#Id-0000011a101b167c-0000000000000012">
              <dsig:Transforms>
                <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              </dsig:Transforms>
              <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
              <dsig:DigestValue>JCy0JoyhVZYzmrLrl92nxfr1+zQ=</dsig:DigestValue>
            </dsig:Reference>
          </dsig:SignedInfo>
          <dsig:SignatureValue>
            ......
            <dsig:SignatureValue>
              <dsig:KeyInfo Id="Id-0000011a101b167c-0000000000000014">
                <dsig:X509Data>
                  <dsig:X509Certificate>......</dsig:X509Certificate>
                </dsig:X509Data>
              </dsig:KeyInfo>
        </dsig:Signature>
      </soap:Header>
      <soap:Body ID="Id-0000011a101b167c-0000000000000012">
        <product version="7.7">
          <name>API Gateway</name>
          <company>Axway</company>
          <description>SOA Security and Management</description>
        </product>
      </soap:Body>
    </soap:Envelope>
    ```

* *xml:id*: Select this option to use an `xml:id`
    to dereference the signed data. Under this schema, the URI attribute of the `Reference`
    points at an `xml:id`
    attribute, which is inserted into the top-level node of the signed nodeset. In the following example, the URI specified in the Signature Reference node matches the `xml:id`
    attribute inserted into the `Body`
    element, indicating that the signature applies to the entire contents of the SOAP body:

    ```xml
    <soap:Envelope xmlns:soap="....">
      <soap:Header>
        <dsig:Signature xmlns:dsig="...." Id="Id-0000011a101b167c-0000000000000013">
          <dsig:SignedInfo>
            <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                  xml-exc-c14n#"/>
            <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                  xmldsig#rsa-sha1"/>
            <dsig:Reference URI="#Id-0000011a101b167c-0000000000000012">
              <dsig:Transforms>
                <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </dsig:Transforms>
              <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <dsig:DigestValue>JCy0JoyhVZYzmrLrl92nxfr1+zQ=</dsig:DigestValue>
            </dsig:Reference>
          </dsig:SignedInfo>
          <dsig:SignatureValue>
            ......
            <dsig:SignatureValue>
              <dsig:KeyInfo Id="Id-0000011a101b167c-0000000000000014">
                <dsig:X509Data>
                  <dsig:X509Certificate>......</dsig:X509Certificate>
                </dsig:X509Data>
              </dsig:KeyInfo>
        </dsig:Signature>
      </soap:Header>
      <soap:Body ID="Id-0000011a101b167c-0000000000000012">
        <product version=7.7>
          <name>API Gateway</name>
          <company>Axway</company>
          <description>SOA Security and Management</description>
        </product>
      </soap:Body>
    </soap:Envelope>
    ```

* *No id (use with enveloped signature and XPath 'The Entire Document')*: Select this option to sign the entire document. In this case, the URI attribute on the `Reference`
    node of the signature is `“”`, which means that no id is used to refer to what is being signed. The `“”`
    URI means that the full document is signed. A signature of this type must be an enveloped signature. On the **Advanced** > **Options**
    tab, select **Create enveloped signature**. To sign the full document, on the **What to Sign** > **XPaths**
    tab, select the XPath named `The entire document`.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <soap:Envelope xmlns:soap="....">
      <soap:Header>
        <wsse:Security
          xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
          <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#"
                Id="Id-0001346926985531-fffffffff28f6103-1">
            <dsig:SignedInfo>
              <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                 xml-exc-c14n#" />
              <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                  xmldsig#rsa-sha1" />
              <dsig:Reference URI="">
                <dsig:Transforms>
                  <dsig:Transform Algorithm="http://www.w3.org/2000/09/
                      xmldsig#enveloped-signature" />
                  <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                </dsig:Transforms>
                <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                <dsig:DigestValue>
                  BAz3140AFAfBL/DIj9y+16TEJIU=
                </dsig:DigestValue>
              </dsig:Reference>
            </dsig:SignedInfo>
            <dsig:SignatureValue>........</dsig:SignatureValue>
            <dsig:KeyInfo Id="Id-0001346926985531-fffffffff28f6103-2">
              <dsig:X509Data>
                <dsig:X509Certificate>........</dsig:X509Certificate>
              </dsig:X509Data>
            </dsig:KeyInfo>
          </dsig:Signature>
        </wsse:Security>
      </soap:Header>
      <soap:Body>
        <product version=7.7>
          <name>API Gateway</name>
          <company>Axway</company>
          <description>SOA Security and Management</description>
        </product>
      </soap:Body>
    </soap:Envelope>
    ```

**Use SAML Ids for SAML Elements**:
This option is only relevant if a SAML assertion is required to be signed. If this option is selected, and the signature is to cover a SAML assertion, an `AssertionID`
attribute is inserted into a SAML version 1.1 assertion, or an `ID`
attribute is inserted into a SAML version 2.0 assertion. The value of this attribute is then referenced from within a `<Reference>` block of the XML signature. This option is selected by default.

**Add and Dereference Security Token Reference for SAML**:
This option is only relevant if a SAML assertion is required to be signed. This setting signs the SAML assertion using a Security Token Reference and an STR-Transform. The `Signature`
points to the id of the `wsse:SecurityTokenReference`, and applies the STR-Transform.

When signing the SAML assertion, this means to sign the XML that the `wsse:SecurityTokenReference`
points to, and not the `wsse:SecurityTokenReference`. This option is unselected by default.

The following shows an example SOAP header:

```xml
<soap:Envelope xmlns:soap="....">
  <soap:Header>
    <wsse:Security xmlns:wsse="...." xmlns:wsu="...." ;

      <dsig:Signature xmlns:dsig=".....">
        <dsig:SignedInfo>
          <dsig:CanonicalizationMethod
            Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
               xmldsig#rsa-sha1" />
          <dsig:Reference URI="#Id-0001347292983847-00000000530a9b1a-1">
            <dsig:Transforms>
              <dsig:Transform
                Algorithm="http://docs.oasis-open.org/wss/2004/01/
                   oasis-200401-wss-soap-message-security-1.0#STR-Transform">
                <wsse:TransformationParameters>
                  <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                      xml-exc-c14n#" />
                </wsse:TransformationParameters>
              </dsig:Transform>
            </dsig:Transforms>
            <dsig:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
            <dsig:DigestValue>
              6/aLwABWfS+9UiX7v39sLJw5MaQ=
            </dsig:DigestValue>
          </dsig:Reference>
        </dsig:SignedInfo>
        <dsig:SignatureValue>
          ......
        </dsig:SignatureValue>
        <dsig:KeyInfo Id="Id-0001347292983847-00000000530a9b1a-3">
          <dsig:X509Data>
            <dsig:X509Certificate>
              .....
            </dsig:X509Certificate>
          </dsig:X509Data>
        </dsig:KeyInfo>
      </dsig:Signature>
      <wsse:SecurityTokenReference wsu:Id="Id-0001347292983847-00000000530a9b1a-1">
        <wsse:KeyIdentifier
          ValueType="http://docs.oasis-open.org/wss/
               oasis-wss-saml-token-profile-1.0#SAMLAssertionID">
               Id-948d50f1504e0f3703e00000-1
        </wsse:KeyIdentifier>
      </wsse:SecurityTokenReference>
      <saml:Assertion xmlns:saml="...." IssueInstant="2012-09-10T16:03:03Z"
        Issuer="CN=AAA Certificate Services, O=Comodo CA Limited, L=Salford, ST=Greater Manchester, C=GB"
        MajorVersion="1" MinorVersion="1">
        <saml:Conditions NotBefore="2012-09-10T16:03:02Z" NotOnOrAfter="2012-12-18T16:03:02Z" />
        <saml:AuthenticationStatement
          AuthenticationMethod="urn:oasis:names:tc:SAML:1.0:am:password"
          AuthenticationInstant="2012-09-10T16:03:03Z">
          <saml:Subject>
            <saml:NameIdentifier Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">
                   admin
            </saml:NameIdentifier>
            <saml:SubjectConfirmation>
              <saml:ConfirmationMethod>
                urn:oasis:names:tc:SAML:1.0:cm:sender-vouches
              </saml:ConfirmationMethod>
            </saml:SubjectConfirmation>
          </saml:Subject>
        </saml:AuthenticationStatement>
      </saml:Assertion>
    </wsse:Security>
  </soap:Header>
  ....
</soap:Envelope>
```

#### Node locations

Node locations are perhaps the simplest way to configure the message content that must be signed. The table on this tab is prepopulated with a number of common SOAP security headers, including the SOAP Body, WS-Security block, SAML assertion, WS-Security UsernameToken and Timestamp, and the WS-Addressing headers. For each of these headers, there are several namespace options available. For example, you can sign both a SOAP 1.1 and/or a SOAP 1.2 block by distinguishing between their namespaces.

On the **Node Locations**
tab, you can select one or more nodesets to sign from the default list. You can also add more default nodesets by clicking the **Add**
button. Enter the **Element Name**, **Namespace**, and **Index**
of the nodeset in the fields provided. The **Index**
field is used to distinguish between two elements of the same name that occur in the same message.

#### XPaths

You can use an XPath expression to identify the nodeset (the series of elements) that must be signed. To specify that nodeset, select an existing XPath expression from the table, which contains several XPath expressions that can be used to locate nodesets representing common SOAP security headers, including SAML assertions.

Alternatively, you can add a new XPath expression using the **Add**
button. XPath expressions can also be edited and removed with the **Edit**
and **Remove** buttons.

An example of a SOAP message is as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Header>
    </soap:Header>
    <soap:Body>
    <product xmlns="http://www.axway.com">
        <name>SOA Product</name>
        <company>Company</company>
        <description>Web services Security</description>
    </product>
    </soap:Body>
</soap:Envelope>
```

The following XPath expression indicates that all the contents of the SOAP body, including the `Body` element itself, should be signed:

```
/soap:Envelope/soap:Body/descendant-or-self::node()
```

You must also supply the namespace mapping for the `soap` prefix, for example:

| Prefix | URI                      |
|--------|--------------------------|
| `soap` | `http://schemas.xmlsoap.org/soap/envelope/`|

#### XPath predicates

Select this option to use an XPath transform to reference the signed content. You must select an XPath predicate from the table to do this. The table is prepopulated with several XPath predicates that can be used to identify common security headers that occur in SOAP messages, including SAML assertions.

To illustrate the use of XPath predicates, the following example shows how the SOAP message is signed when the `Sign SOAP Body`
predicate is selected:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Body>
    <vs:getProductInfo xmlns:vs="http://www.axway.com">
        <vs:Name>API Gateway</vs:Name>
        <vs:Version>7.7</vs:Version>
    </vs:getProductInfo>
    </s:Body>
</s:Envelope>
```

The default XPath expression (Sign SOAP Body) identifies the contents of the SOAP `Body` element, including the `Body`
element itself. The following is the XML Signature produced when this XPath predicate is used:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <dsig:Signature id="Sample" xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
      <dsig:SignedInfo>
        ...
        <dsig:Reference URI="">
          <dsig:Transforms>
            <dsig:Transform Algorithm="http://www.w3.org/TR/1999/REC-xpath-19991116">
              <dsig:XPath xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
                ancestor-or-self::soap:Body
              </dsig:XPath>
            </dsig:Transform>
            <dsig:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n" />
          </dsig:Transforms>
          ...
        </dsig:Reference>
      </dsig:SignedInfo>
      ...
    </dsig:Signature>
  </s:Header>
  <s:Body>
    <vs:getProductInfo xmlns:vs="http://www.axway.com">
      <vs:Name>API Gateway</vs:Name>
      <vs:Version>7.7</vs:Version>
    </vs:getProductInfo>
  </s:Body>
</s:Envelope>
```

This XML Signature includes an extra `Transform` element, which has a child `XPath` element. This element specifies the XPath predicate that validating applications must use to identify the signed content.

#### Message attribute

Finally, you can use the contents of a message attribute to determine what must be signed in the message. For example, you can configure a **Locate XML Nodes** filter to extract certain content from the message and store it in a particular message attribute. You can then specify this message attribute on the **Message Attribute** tab.

To do this, select the **Extract nodes from Selector Expression** check box, and enter the name of the attribute that contains the nodes in the field provided. The default value is `${node.list}`.

### Where to place signature settings

**Append Signature to Root or SOAP Header**:
If the message is a SOAP message, the signature is inserted into the SOAP `Header` element when this radio button is selected. The XML signature is inserted as an immediate child of the SOAP `Header`
element. The following example shows a skeleton SOAP message which has been signed using this option:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
 <s:Header>
  <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/..." id="Sample">
...
  </dsig:Signature>
 </s:Header>
 <s:Body>
...
 </s:Body>
</s:Envelope>
```

If the message is just plain XML, the signature is inserted as an immediate child of the root element of the XML message. The following example shows a non-SOAP XML message signed using this option:

```xml
<PurchaseOrder>
    <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="Sample">
    ...
    </dsig:Signature>
    <Items>
    ...
    </Items>
</PurchaseOrder>
```

**Place in WS-Security Element for SOAP Actor/Role**:
By selecting this option, the XML signature is inserted into the WS-Security element identified by the specified SOAP *actor* or *role*. A SOAP actor/role is simply a way of distinguishing a particular WS-Security block from others which might be present in the message.

Enter the name of the SOAP actor or role of the WS-Security block in the field. The following SOAP message contains an XML signature within a WS-Security block identified by the "test" actor:

```
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Header>
    <ws:Security xmlns:ws="http://schemas.xmlsoap.org/..." s:actor="test">
        <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/..." id="Sample">
        ...
        </dsig:Signature>
    </ws:Security>
    </s:Header>
    <s:Body>
    ...
    </s:Body>
</s:Envelope>
```

**Use XPath Location**:
This option is useful in cases where the signature must be inserted into a non-SOAP XML message. In such cases, it is possible to insert the signature into a location pointed to by an XPath expression. Select or add an XPath expression in the field provided, and then specify whether the API Gateway should insert the signature *before*
the location to which the XPath expression points, or *append* it to this location.

### Advanced signature generation settings

The **Advanced**
tab enables you to set the following:

* Additional elements from the message to be signed.
* Algorithms and ciphers used to sign the message parts.
* Various advanced options on the generated XML signature.

#### Additional

The **Additional** tab allows you to select additional elements from the message that are to be signed. It is also possible to insert a WS-Security Timestamp into the XML signature, if necessary.

**Additional Elements to Sign**:
The options here allow you to select other parts of the message to sign.

* **Sign KeyInfo Element of Signature**:
    The `<KeyInfo>` block of the XML signature can be signed to prevent people cut-and-pasting a different `<KeyInfo>` block into the message, which might point to some other key material, for example.
* **Sign Timestamp**:
    As stated earlier, timestamps are used to prevent replay attacks. However, to guarantee the end-to-end integrity of the timestamp, it is necessary to sign it.

This option is only enabled when you have elected to insert a Timestamp into the message using the relevant fields on the **Timestamp Options**
section below.

* **Sign Attachments**:
    In addition to signing some or all contents of the SOAP message, you can also sign attachments to the SOAP message. To sign all attachments, select **Include Attachments**. A signed attachment is referenced in an XML signature using the *Content-Id* or *cid* of the attachment. The `URI` attribute of the `Reference` element corresponds to this Content-Id. The following example shows how an XML signature refers to a sample attachment. It shows the wire format of the message and its attachment as they are sent to the destination web service. Multiple attachments result in successive `Reference` elements.

    ```
    POST /myAttachments HTTP/1.0
    Content-Length: 1000
    User-Agent: API Gateway
    Accept-Language: en
    Content-Type: multipart/related; type="text/xml";
    boundary="----=Multipart-SOAP-boundary"

    ------=Multipart-SOAP-boundary
    Content-Id: soap-envelope
    SOAPAction: none
    Content-Type: text/xml;
    charset="utf-8"
    <s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
      <s:Header>
        <dsig:Signature id="Sample" xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
          <dsig:SignedInfo>
            <dsig:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/
                xml-exc-c14n"/>
            <dsig:SignatureMethod Algorithm="http://www.w3.org/2000/09/
                xmldsig#rsa-sha1"/>
            <dsig:Reference URI="cid:moredata.txt">...</dsig:Reference>
          </dsig:SignedInfo>
        </dsig:Signature>
      </s:Header>
      <s:Body>
        ...
      </s:Body>
    </s:Envelope>

    ------=Multipart-SOAP-boundary
    Content-Id: moredata.txt
    Content-Type: text/plain; charset="UTF-8"
    Some more data.
    ------=Multipart-SOAP-boundary--
    ```

**Transform**:
This field is only available when you have selected the **Sign Attachments**
box above. It determines the transform used to reference the signed attachments.

**Timestamp Options**:
It is possible to insert a timestamp into the message to indicate when exactly the signature was generated. Consumers of the signature can then validate the signature to ensure that it is not of date.

The following options are available:

* **No Timestamp**:
    No timestamp is inserted into the signature.
* **Embed in WSSE Security**:
    The `wsu:Timestamp`
    is inserted into a `wsse:Security` block. The `Security` block is identified by the SOAP actor/role specified on the **Signature** tab.
* **Embed in Signature Property**:
    The `wsu:Timestamp` is placed inside a signature property element in the `dsig:Signature`.

The **Expires In** fields enable the user to optionally specify the `wsu:Expires` for the `wsu:Timestamp`. If all fields are left at `0`, no `wsu:Expires` element is placed inside the `wsu:Timestamp`. The following example shows a `wsu:Timestamp` that has been inserted into a `wsse:Security` block:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Header>
    <wsse:Security>
        <wsu:Timestamp wsu:Id="Id-0000011294a0311e-000000000000003d">
        <wsu:Created>2007-05-16T11:22:45Z</wsu:Created>
        <wsu:Expires>2007-05-23T11:22:45Z</wsu:Expires>
        </wsu:Timestamp>
        <dsig:Signature .>
        ...
        </dsig:Signature ...>
    </wsse:Security>
    </s:Header>
    <s:Body>
    ...
    </s:Body>
</s:Envelope>
```

#### Algorithm Suite

The fields on this tab determine the combination of cryptographic algorithms and ciphers that are used to sign the message parts.

**Algorithm suite**:
WS-Security Policy defines a number of *algorithm suites* that group together a number of cryptographic algorithms. For example, a given algorithm suite uses specific algorithms for asymmetric signing, symmetric signing, asymmetric key wrap, and so on. Therefore, by specifying an algorithm suite, you are effectively selecting a whole suite of cryptographic algorithms to use.

To use a particular WS-Security Policy algorithm suite, you can select it here. The **Signature Method**, **Key Wrap Algorithm**, and **Digest Method** fields are then automatically populated with the corresponding algorithms for that suite.

**Signature Method**:
The **Signature Method** field enables you to configure the method used to generate the signature. Various strengths of the HMAC-SHA1 algorithms are available from the list.

**Key Wrap Algorithm**:
Select the algorithm to use to wrap (encrypt) the symmetric signing key. This option need only be configured when you are using a symmetric key to sign the message.

**Digest Algorithm**:
Select the digest algorithm to you to produce a cryptographic hash of the signed data.

#### Options

This tab enables you to configure various advanced options on the generated XML signature. The following fields can be configured on this tab:

**WS-Security Options**:
WSSE 1.1 defines a `<SignatureConfirmation>`
element that can be used as proof that a particular XML signature was processed. A recipient and verifier of an XML signature must generate a `<SignatureConfirmation>` element for each piece of data that was signed (for each `<Reference>` in the XML signature). A `<SignatureConfirmation>` element contains the hash of the signed data and must be signed by the recipient before returning it in the response to the initiator (the original signatory of the data).

When the initiator receives the `<SignatureConfirmation>` elements in the response, it compares the hash with the hash of the data that it produced initially. If the hashes match, the initiator knows that the recipient has processed the same signature.

Select the **Initiator** option if the API Gateway is the initiator as outlined in the scenario above. The API Gateway keeps a record of the signed data and compares it to the contents of the `<SignatureConfirmation>` elements returned from the recipient in the response message.

Alternatively, if the API Gateway is acting as the recipient in this transaction, you can select the **Responder** radio button to instruct the API Gateway to generate the `<SignatureConfirmation>` elements and return them to the initiator. The signature confirmations are added to the WS-Security header.

**Layout Type**:
Select the WS-SecurityPolicy layout type that you want the XML signature and any generated tokens to adhere to. This includes elements such as`<Signature>`, `<BinarySecurityToken>`, and `<EncryptedKey>`, which can all be generated as part of the signing process.

**Fail if No Nodes to Sign**:
Check this option if you want the filter to fail if it cannot find any nodes to sign as configured on the **What to Sign** tab.

**Add Inclusive Namespaces for Exclusive Canonicalization**:
You can include information about the namespaces (and their associated prefixes) of signed elements in the signature itself. This ensures that namespaces that are in the same scope as the signed element, but not directly or visibly used by this element, are included in the signature. This ensures that the signature can be validated as a standalone entity outside of the context of the message from which it was extracted.

{{< alert title="Note" color="primary" >}}The WS-I specification only permits the use of exclusive canonicalization in an XML signature. The `<InclusiveNamespaces>` element is an attempt to take advantage of some of the behavior of *inclusive* canonicalization, while maintaining the simplicity of *exclusive* canonicalization.{{< /alert >}}

A *PrefixList*
attribute is used to list the prefixes of in-scope, but not visibly used elements and attributes. The following example shows how the `PrefixList` attribute is used in practice:

```xml
<soap:Envelope xmlns:soap='http://schemas.xmlsoap.org/soap/envelope'>
  <soap:Header>
    <wsse:Security xmlns:wsse='http://docs.oasis-open.org/...
         ' xmlns:wsu='http://docs.oasis-open.org/...'>
      <wsse:BinarySecurityToken wsu:Id='SomeCert' ValueType="http://docs.oasis-open.org/...">
          lui+Jy4WYKGJW5xM3aHnLxOpGVIpzSg4V486hHFe7sH
      </wsse:BinarySecurityToken>
      <ds:Signature xmlns:ds='http://www.w3.org/2000/09/xmldsig#'>
        <ds:SignedInfo>
          <ds:CanonicalizationMethod Algorithm='http://www.w3.org/2001/10/
               xml-exc-c14n#'>
            <c14n:InclusiveNamespaces xmlns:c14n='http://www.w3.org/2001/10/
                xml-exc-c14n#'
              PrefixList='wsse wsu soap' />
          </ds:CanonicalizationMethod>
          <ds:SignatureMethod Algorithm='http://www.w3.org/2000/09/xmldsig#rsa-sha1' />
          <ds:Reference URI=''>
            <ds:Transforms>
              <dsig:XPath xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:m='http://example.org/ws'>
                //soap:Body/m:SomeElement
              </dsig:XPath>
              <ds:Transform Algorithm='http://www.w3.org/2001/10/xml-exc-c14n#'>
                <c14n:InclusiveNamespaces xmlns:c14n='http://www.w3.org/2001/10/
                  xml-exc-c14n#' PrefixList='soap wsu test' />
              </ds:Transform>
            </ds:Transforms>
            <ds:DigestMethod Algorithm='http://www.w3.org/2000/09/xmldsig#sha1'/>
            <ds:DigestValue>VEPKwzfPGOxh2OUpoK0bcl58jtU=</ds:DigestValue>
          </ds:Reference>
        </ds:SignedInfo>
        <ds:SignatureValue>+diIuEyDpV7qxVoUOkb5rj61+Zs=</ds:SignatureValue>
        <ds:KeyInfo>
          <wsse:SecurityTokenReference>
            <wsse:Reference URI='#SomeCert' />
          </wsse:SecurityTokenReference>
        </ds:KeyInfo>
      </ds:Signature>
    </wsse:Security>
  </soap:Header>
  <soap:Body xmlns:wsu='http://docs.oasis-open.org/...
    ' xmlns:test='http://www.test.com'  wsu:Id='TheBody'>
    <m:SomeElement xmlns:m='http://example.org/ws' attr1='test:fdwfde' />
  </soap:Body>
</soap:Envelope>
```

**Indent**:
Select this method to ensure that the generated signature is properly indented.

**Create Enveloped Signature**:
By selecting this option, an enveloped XML signature is generated. The following skeleton signed SOAP message shows the enveloped signature:

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#" id="Sample">
    <ds:SignedInfo>
    <ds:Reference URI="">
        <ds:Transforms>
        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
        </ds:Transforms>
    </ds:Reference>
    </ds:SignedInfo>
</ds:Signature>
```

This indicates to the application validating the signature that the signature itself should not be included in the signed data. In other words, to validate the signature, the application must first strip out the signature. This is necessary in cases where the entire SOAP envelope has been signed, and the resulting signature has been inserted into the SOAP header. In this case, the signature is over a nodeset which has been altered (the signature has been inserted), and so the signature breaks.

**Insert CarriedKeyName for EncryptedKey**:
Select this option to include a `<CarriedKeyName>`
element in the `<EncryptedKey>` block that is generated when using a symmetric signing key.

**Include Transforms**:
Select this option to include a `<Transforms>` element in the `<Reference>` block to explain the transformation chain applied before hashing the content.

{{< alert title="Note" color="primary" >}}This option must be selected when **Create Enveloped Signature** is selected.{{< /alert >}}

## XML signature verification filter

In addition to validating XML signatures for authentication purposes, the API Gateway can also use XML signatures to prove message integrity. By signing an XML message, a client can be sure that any changes made to the message do not go unnoticed by the API Gateway. Therefore by validating the XML signature on a message, the API Gateway can guarantee the *integrity*
of the message.

### Signature verification settings

The following sections are available on the **Signature Verification**
tab:

**Signature Location**:
Because there may be multiple signatures in the message, you must specify which signature API Gateway uses to verify the integrity of the message. The signature can be extracted from one of the following:

* From the SOAP header
* Using WS-Security actors
* Using XPath

For more details on signature location options, see [Signature location options](#signature-location-options).

**Find Signing Key**:
The public key used to verify the signature can be taken from the following locations:

* **Via KeyInfo in Message**:
    Typically, a `<KeyInfo>`
    block is used in an XML signature to reference the key used to sign the message. For example, it is common for a `<KeyInfo>`
    block to reference a `<BinarySecurityToken>`
    that contains the certificate associated with the public key used to verify the signature.
* **Via Selector Expression**:
    The certificate used to verify the signature can be extracted from a selector expression. For example, a previous filter (for example, **Find Certificate**) may have already located a certificate and populated the `certificate`
    message attribute. To use this certificate to verify the signature, specify the selector expression in the field provided (for example, `${certificate}`). Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, KPS, or environment variable).
* **Via Certificate in LDAP**:
    Clients may not always want to include their public keys in their signatures. In such cases, the public key can be retrieved from a specified LDAP directory. This setting enables you to select a previously configured LDAP directory from a list. You can add LDAP connections under the **Environment Configuration** > **External Connections**
    node in the Policy Studio tree. Right-click the **LDAP Connection**
    tree node, and select **Add an LDAP Connection**.
* **Via Certificate in Store**:
    Similarly, you can retrieve a certificate from the certificate store by selecting this option, and clicking the **Select**
    button. Select the check box next to the certificate that contains the public key to use to verify the signature, and click **OK**.

### What must be signed settings

The **What Must Be Signed**
tab defines the content that must be signed for a SOAP message to pass the filter. This ensures that the client has signed something meaningful (part of the SOAP message), instead of arbitrary data that would pass a blind signature validation. This further strengthens the integrity verification process. The nodeset that must be signed can be identified by a combination of XPath expressions, node locations, and the contents of a message attribute. For more details, see [What to sign settings](#what-to-sign-settings).

{{< alert title="Note" color="primary" >}}If all attachments are required to be signed, select **All attachments**
to enforce this.{{< /alert >}}

### Advanced signature verification settings

The following advanced configuration options are available on the **Advanced**
tab:

**Signature Confirmation**:
If this filter is configured as part of an initiator policy, where the API Gateway acts as the client in a web services transaction, select the **Initiator**
option. This means that the filter keeps a record of the signature that it has verified, and checks the `SignatureConfirmation`
returned by the recipient.

Alternatively, if the API Gateway acts as the recipient in the transaction, select the **Recipient**
option. In this case, the API Gateway returns the `SignatureConfirmation`
elements in the response to the initiator.

**Default Derived Key Label**:
If the API Gateway consumes a `DerivedKeyToken`, use the default value to recreate the derived key.

**Algorithm Suite**:
Select the WS-Security Policy *Algorithm Suite*
that must have been used when signing the message. This check ensures that the appropriate algorithms were used to sign the message.

**Fail if No Signatures to Verify**:
Select this to configure the filter to fail if no XML signatures are present in the incoming message.

**Verify Signature for Authentication Purposes**:
You can use the **XML Signature Verification**
filter to authenticate an end user. If the message can be successfully validated, it proves that only the private key associated with the public key used to verify the signature was used to sign the message. Because the private key is only accessible to its owner, a successful verification can be used to effectively authenticate the message signer.

**Retrieve DOM using Selector Expression**:
You can configure this field to verify the response from a SAML PDP. When the API Gateway receives a response from the SAML PDP, it stores the signature on the response in a message attribute. You can specify this attribute using a selector expression to verify this signature. Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store, or environment variable).

**Remove enclosing WS-Security element on successful verification**:
Select this check box if you wish to remove the enclosing WS-Security block when the signature has been successfully verified. This setting is not selected by default.

## Signature location options

A given XML message can contain several XML signatures. Consider an XML document (for example, a company policy approval form) that must be digitally signed by a number of users (for example, department managers) before being submitted to the destination web service (for example, a company policy approval web service). Such a message contains several XML signatures by the time it is ready to be submitted to the web service.

In such cases, where multiple signatures are present within a given XML message, it is necessary to specify which signature the API Gateway should use in the validation process. You can specify the location of the signature in the XML message in the **XML Signature Verification** filter.

The API Gateway can extract the signature from an XML message using several different methods:

* WS-Security block
* SOAP message header
* Advanced (XPath)

Select the most appropriate method from the **Signature Location**
field. Your selection depends on the types of SOAP messages that you expect to receive. For example, if incoming SOAP messages contain an XML signature within a WS-Security block, you should choose this option from the list.

### Use WS-Security actors

If the signature is present in a WS-Security block:

1. Select `WS-Security block`
    from the **Signature Location**
    field.
2. Select a SOAP actor from the **Select Actor/Role(s)**
    field. Each actor uniquely identifies a separate WS-Security block. By selecting `Current actor/role only`
    from the list, the WS-Security block with no actor is taken.
3. In cases where there might be multiple signatures within the WS-Security block, it is necessary to extract one using the **Signature Position**
    field.

The following is a skeleton version of a message where the XML signature is contained in the `sample`
WS-Security block, (`soap-env:actor="sample"`):

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Header>
    <wsse:Security xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/04/secext" s:actor="sample">
        <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="s1">
        ....
        </dsig:Signature>
    </wsse:Security>
    </s:Header>
    <s:Body>
    <ns1:getTime xmlns:ns1="urn:timeservice">
    </ns1:getTime>
    </s:Body>
</s:Envelope>
```

### Use SOAP header

If the signature is present in the SOAP header:

1. Select `SOAP message header`
    from the **Signature Location**
    field.
2. If there is more than one signature in the SOAP header, then it is necessary to specify which signature the API Gateway should use. Specify the appropriate signature by setting the **Signature Position**
    field.

The following is an example of an XML message where the XML signature is contained within the SOAP header:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Header>
    <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="s1">
        ....
    </dsig:Signature>
    </s:Header>
    <s:Body>
    <ns1:getTime xmlns:ns1="urn:timeservice">
    </ns1:getTime>
    </s:Body>
</s:Envelope>
```

### Use XPath expression

To use an XPath expression to locate the signature:

1. Select `Advanced (XPath)`
    from the **Signature Location**
    field.
2. Select an existing XPath expression from the list, or add a new one by clicking on the **Add**
    button. XPath expressions can also be edited or removed with the **Edit**
    and **Remove**
    buttons.

The `First signature in SOAP Header`
XPath expression takes the first signature from the SOAP header. The expression is as follows:

```
//s:Envelope/s:Header/dsig:Signature[1]
```

To edit this expression, click the **Edit**
button to display the **Enter XPath Expression**
dialog.

An example of a SOAP message containing an XML signature in the SOAP header is provided below.

```
<?xml version="1.0" encoding="UTF-8"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Header>
    <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" id="s1">
        ....
    </dsig:Signature>
    </s:Header>
    <s:Body>
    <product xmlns="http://www.axway.com">
        <name>SOA Product*</name>
        <company>Company</company>
        <description>Web Services Security</description>
    </product>
    </s:Body>
</s:Envelope>
```

Because the elements referenced in the expression (`Envelope`
and `Signature`) are prefixed
elements, you must define the namespace mappings for each of these elements as follows:

| Prefix | URI                                         |
|--------|---------------------------------------------|
| `s`    | `http://schemas.xmlsoap.org/soap/envelope/` |
| `dsig` | `http://www.w3.org/2000/09/xmldsig#`        |

When adding your own XPath expressions, you must be careful to define any namespace mappings in a manner similar to that outlined above. This avoids any potential clashes that might occur where elements of the same name, but belonging to different namespaces, are present in an XML message.
