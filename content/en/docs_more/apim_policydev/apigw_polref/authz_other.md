{
"title": "Additional authorization filters",
  "linkTitle": "Additional authorization filters",
  "weight": 45,
  "date": "2019-10-17",
  "description": "Additional authorization filters, including attribute authorization, Axway Passport, SAML, and SAML PDP."
}

## Attribute authorization filter

The purpose of the filters in the **Attributes**
filter group is to extract user attributes from various sources. You can use these filters to retrieve attributes from the message, an LDAP directory, a database, the API Gateway user store, HTTP headers, a SAML attribute assertion, and so on.

After retrieving a set of user attributes, API Gateway stores them in the `attribute.lookup.list`
message attribute, which is essentially a map of name-value pairs. It is the role of the **Attributes**
authorization filter to check the value of these attributes to authorize the user.

Configure the following fields on the **Attributes**
configuration window:

**Name**:
Enter a suitable name for this filter to display in a policy.

**Attributes**:
The **Attributes**
table lists the checks that the API Gateway performs on user attributes stored in the `attribute.lookup.list`
message attribute. The API Gateway performs the following checks:

* The entries in the table are OR-ed together so that if any one of them succeeds, the filter returns a pass.
* The attribute checks listed in the table are run in series until one of them passes.
* You can add a number of attribute-value pairs to a single attribute check by separating them with commas (for example, `company=axway, department=engineering, role=engineer`).
* If multiple attribute-value pairs are present in a given attribute check, these pairs are AND-ed together so that the overall attribute check only passes if all the attribute-value pairs pass. For example, if the attribute check comprises, `department=engineering, role=engineer`, this check only passes if both attributes are found with the correct values in the `attribute.lookup.list`
    message attribute.

To add an attribute check to the **Attributes**
table, click **Add**, and enter attributes in the dialog. For attribute checks involving attributes extracted from a SAML attribute assertion, you must specify the namespace of the attribute given in the assertion. For example, API Gateway can extract the `role`
attribute from the following SAML `<Attribute Statement>`, and store it in the `attribute.lookup.list`
map:

```xml
<saml:AttributeStatement>
  <saml:Attribute Name="role" NameFormat="http://www.company.com">
    <saml:AttributeValue>admin</saml:AttributeValue>
  </saml:Attribute>
  <saml:Attribute Name="email" NameFormat="http://www.company.com">
    <saml:AttributeValue>joe@company.com</saml:AttributeValue>
  </saml:Attribute>
  <saml:Attribute Name="dept" NameFormat="">
    <saml:AttributeValue>engineering</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

The `NameFormat`
attribute of the `<Attribute>`
gives the namespace of the attribute name. You must enter this namespace (together with a corresponding prefix) in the **Add Attributes**
dialog. For example, to extract the `role`
attribute from the SAML attribute statement above, enter `pre:role=admin`
in the **Attribute Requirement**
field. Then you must also map the `pre`
prefix to the `http://www.company.com`
namespace, as specified by the `NameFormat`
attribute in the attribute statement.

## RSA Access Manager authorization filter

RSA Access Manager (formerly RSA ClearTrust) provides identity management and access control services for web applications. It centrally manages access to web applications, ensuring that only authorized users are allowed access to resources.

The **Access Manager** filter enables integration with RSA Access Manager. This filter can query Access Manager for authorization information for a particular user on a given resource. In other words, API Gateway asks Access Manager to make the authorization decision. If the user has been given authorization rights to the web service, the request is allowed through to the service. Otherwise, the request is rejected.

### Prerequisites

You must copy RSA Access Manager libraries to API Gateway, so you must have RSA Access Manager installed on a server.

1. Copy the following files from the `lib` directory on your RSA Access Manager installation:
    * `axm-core-6.2.jar`
    * `cryptojce-6.1.jar`
    * `cryptojcommon-6.1.jar`
    * `jcm-6.1.jar`
2. Add the files to the `INSTALL_DIR/apigateway/ext/lib` directory on API Gateway:
3. Restart API Gateway.

### Configure RSA Access Manager general settings

Configure the following general settings.

#### Connection Details

The **Connection Details** section enables you to specify a group of Access Manager servers to connect to in order to authenticate clients. You can select a group of Access Manager servers to provide failover in cases where one or more servers are not available.

**Connection Group Type**:
API Gateway can connect to a group of Access Manager authorization servers or dispatcher servers. When multiple Access Manager authorization servers are deployed for load-balancing purposes, API Gateway should first connect to a dispatcher server, which returns a list of active authorization servers. An attempt is then made to connect to one of these authorization servers using round-robin DNS. If the first dispatcher server in the connection group is not available, API Gateway attempts to connect to the dispatcher server with the next highest priority in the group, and so on.

If a dispatcher server has not been deployed, API Gateway can connect directly to an authorization server. If the authorization server with the highest priority in the connection group is not available, API Gateway attempts to connect to the authorization server with the next highest priority, and so on. Select the type of the connection group (**Authorization Server** or **Dispatcher Server**). All servers in the group must be of the same type.

**Connection Group**:
Click the button on the right, and select the connection group to use for authenticating clients. To add a connection group, right-click the **RSA Access Manager Connection Sets** tree node, and select **Add a Connection Set**. Alternatively, you can configure a connection set under the **Environment Configuration > External Connections** node in the Policy Studio tree.

#### Authorization Details

The **Authorization Details** section describes the resource for which the user is requesting access.

* **Server**:
    Enter the name of the server that is hosting the requested resource. The name entered must correspond to a preconfigured server name in Access Manager.
* **Resource**:
    Enter the name of the requested resource. This resource must be preconfigured in Access Manager.

Alternatively, you can enter a selector representing a message attribute in the **Resource** field. API Gateway expands this selector at runtime to the value of the corresponding message attribute. API Gateway message attribute selectors take the following format:

```
${message.attribute}
```

The following example of a typical SOAP message received by API Gateway shows how this works:

```
POST /services/timeservice HTTP/1.0
Host:localhost:8095
Content-Length:374
SOAPAction:TimeService
Accept-Language:en-US
Content-Type:text/XML; utf-8
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns1:getTime xmlns:ns1="urn:timeservice"></ns1:getTime>
    </soap:Body>
</soap:Envelope>
```

The following table shows an example of selector expansion:

| Selector              | Expanded To             |
|-----------------------|-------------------------|
| `${http.request.uri}` | `/services/timeservice` |

## Axway PassPort authorization filter

Axway PassPort provides a central repository, identity broker, and security audit point for Business-to-Business Integration (B2Bi) or Managed File Transfer (MFT) solutions. Axway PassPort centralizes and simplifies provisioning and management for your entire online ecosystem, enabling secure collaboration between applications, divisions, customers, suppliers, and regulatory bodies.

An Axway Component Security Descriptor (CSD) file is an XML file that defines resources, privileges, and roles for each component. For more details, see the *Axway PassPort* user documentation. The **Axway PassPort Authorization**
filter checks if the specified user has the privileges to perform the action on the specified resource. The CSD file defines the available actions that a resource supports. It might also define privileges and roles, which can also be created in the PassPort administration user interface.

To configure integration between Axway PassPort and API Gateway, configure the following settings:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**User ID**:
Enter the ID of the user to authorize. You can enter a static name or a selector that specifies a message attribute. The selector is expanded at runtime to the value of the message attribute. Defaults to `${authentication.subject.id}`.

**Resource**:
Enter the name of the resource for which the user is seeking authorization. This resource must have been defined in the `<ResourceDefinition>`
in the PassPort repository CSD. You can enter a static name or a selector that specifies a message attribute. The selector is expanded at runtime to the value of the message attribute. Defaults to `${http.request.uri}`.

**Action**:
Enter the action being performed on the resource for which authorization is sought. This action must have been defined in the `<AvailableActions>`
section of the PassPort repository CSD. You can enter a static name or a selector that specifies a message attribute. The selector is expanded at runtime to the value of the message attribute. Defaults to `${http.request.verb}`.

**PassPort Repository**:
Select an existing connection to an Axway PassPort repository to use for authorization. To configure a connection in the Policy Studio tree, right-click **Environment Configuration** > **External Connections** > **Authentication Repository Profiles** > **Axway PassPort Repository**, and select **Add a new Repository**.

## CA SOA Security Manager authorization filter

CA SOA Security Manager can authenticate end users and authorize them to access protected web resources. The API Gateway can interact directly with CA SOA Security Manager by asking it to make authorization decisions on behalf of end users that have successfully authenticated to the API Gateway. CA SOA Security Manager decides whether to authorize the user, and relays the decision back to the API Gateway where the decision is enforced. The API Gateway acts as a Policy Enforcement Point (PEP) in this situation, enforcing the authorization decisions made by the CA SOA Security Manager, which acts a Policy Decision Point (PDP).

{{< alert title="Note" color="primary" >}}A **CA SOA Security Manager**
authentication filter must be invoked before a **CA SOA Security Manager**
authorization filter in a given policy. In other words, the end user must authenticate to CA SOA Security Manager before they can be authorized for a protected resource.{{< /alert >}}

### CA SOA Security Manager prerequisites

Integration with CA SOA Security Manager requires CA TransactionMinder SDK version 6.0 or later. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

#### Add third-party binaries to API Gateway

To add third-party binaries to API Gateway, perform the following steps:

1. Add the binary files as follows:
    * Add `.jar`
        files to the `INSTALL_DIR/apigateway/ext/lib`
        directory.
    * Add `.so`
        files to the `INSTALL_DIR/apigateway/<platform>/lib` directory.

2. Restart API Gateway.

#### Add third-party binaries to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies**
    in the Policy Studio main menu.
2. Click **Add**
    to select a JAR file to add to the list of dependencies.
3. Click **Apply**
    when finished. A copy of the JAR file is added to the `plugins`
    directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio with the `-clean` option. For example:

    ```
    cd INSTALL_DIR/policystudio/
    policystudio -clean
    ```

### Configure CA SOA Security Manager

Configure the following fields on the **CA SOA Security Manager**
authorization filter:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Attributes**:
If the end user is successfully authorized, the attributes listed here are looked up in CA SOA Security Manager, and returned to the API Gateway. These attributes are stored in the `attributes.lookup.list`
message attribute. They can be retrieved at a later stage to generate a SAML attribute assertion.

Select the **Set attributes for SAML Attribute token**
check box, and click the **Add**
button to specify an attribute to fetch from CA SOA Security Manager.

## Certificate attribute authorization filter

The API Gateway can authorize access to a web service based on the X.509 attributes of an authenticated client's certificate. For example, a simple **Certificate Attributes**
filter might only authorize clients whose certificates have a Distinguished Name (DName) containing the following attribute: `O=axway`. In other words, only `axway`
users are authorized to access the web service.

An X.509 certificate consists of a number of fields. The `Subject`
field is most relevant. It gives the DName of the client to which the certificate belongs. A DName is a unique name given to an X.500 directory object. It consists of a number of attribute-value pairs called Relative Distinguished Names (RDNs). Some of the most common RDNs and their explanations are as follows:

* `CN`: CommonName
* `OU`: OrganizationalUnit
* `O`: Organization
* `L`: Locality
* `S`: StateOrProvinceName
* `C`: CountryName

For example, the following is the DName of the *sample.p12*
client certificate supplied with API Gateway:

```
CN=Sample Cert, OU=R&D, O=Company Ltd., L=Dublin 4, S=Dublin, C=IE
```

Using the **Certificate Attributes**
filter, it is possible to authorize clients based on attributes (for example, the `CN`, `OU`, or `C`
in the DName).

Configure the following settings:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**X.509 Attributes**:
To add a new X.509 attribute check, click the **Add**
button. In the **Add X.509 Attributes**
dialog, enter a comma-separated list of name-value pairs representing the X.509 attributes and their values (for example, `OU=dev,O=Company`).

The new attribute check is displayed in the **X.509 Attributes**
table. You can edit and delete existing entries by clicking the **Edit**
and **Remove**
buttons.

The **X.509 Attributes**
table lists a number of attribute checks to be run against the client certificate. Each entry tests a number of certificate attributes in such a way that the check only passes if all of the configured attribute values match those in the client certificate. In effect, the attributes listed in a single attribute check are AND-ed together.

For example, imagine the following is configured as an entry in the **X.509 Attributes**
table:

```
OU=Eng, O=Company Ltd
```

If the API Gateway receives a certificate with the following DName, this attribute check passes because *all*
the configured attributes match those in the certificate DName:

```
CN=User1, OU=Eng, O=Company Ltd, L=D4, S=Dublin, C=IECN=User2, OU=Eng, O=Company Ltd, L=D2, S=Dublin, C=IE
```

However, if the API Gateway receives a certificate with the following DName, the attribute check fails because the attributes in the DName do not match *all*
the configured attributes (the `OU`
attribute has the wrong value):

```
CN=User1, OU=qa, O=Company Ltd, L=D4, S=Dublin, C=IE
```

The **X.509 Attributes**
table can contain several attribute check entries. In such cases, the attribute checks (the entries in the table) are OR-ed together, so that if any of the checks succeed, the overall **Certificate Attributes**
filter succeeds.

To summarize:

* Attribute values within an attribute check only succeed if *all*
    the configured attribute values match those in the DName of the client certificate.
* The filter succeeds if *any*
    of the attribute checks listed in the **X.509 Attributes**
    table succeed.

## Entrust GetAccess authorization filter

Entrust GetAccess provides identity management and access control services for web resources. It centrally manages access to web applications, enabling users to benefit from a single sign-on capability when accessing the applications that they are authorized to use.

The **GetAccess**
filter enables integration with Entrust GetAccess. This filter can query GetAccess for authorization information for a particular user for a given resource. In other words, the API Gateway asks GetAccess to make the authorization decision. If the user has been given authorization rights to the web service, the request is allowed through to the service. Otherwise, the request is rejected.

### Configure GetAccess WS-Trust STS settings

This tab enables you to configure how the API Gateway authenticates to the GetAccess WS-Trust Security Token Service (STS). You can configure the API Gateway to connect to a group of GetAccess STS servers in a round-robin fashion. This provides the necessary failover capability when one or more STS servers are not available.

Configure the following fields:

* **URL Group**:
    Click the button on the right, and select an STS URL group in the tree. This group consists of a number of GetAccess STS servers to which the API Gateway round-robins connection attempts. To add a URL group, right-click the **Entrust GetAccess URL Sets**
    node, and select **Add a URL Set**. Alternatively, you can configure a URL connection set under the **Environment Configuration** > **External Connections**
    node in the Policy Studio tree.
* **Drift Time**:
    Having successfully authenticated to a GetAccess STS server, the STS server issues a SAML authentication assertion and returns it to the API Gateway. When checking the validity period of the assertion, the specified **Drift Time**
    is used to account for a possible difference between the time on the STS server and the time on the machine hosting the API Gateway.
* **WS-Trust STS Attribute Field Name**:
    Specify the field name for the `Id`
    field in the WS-Trust request. The default is `Id`.

### Configure GetAccess SAML PDP settings

When the API Gateway has successfully authenticated to a GetAccess STS server, it can then obtain authorization information about the end user from the GetAccess SAML PDP. The authorization details are returned in a SAML authorization assertion, which is then validated by the API Gateway to determine whether the request should be denied.

Configure the following fields:

* **URL Group**:
    Click the button on the right, and select an SAML PDP URL group in the tree. This group consists of a number of GetAccess SAML PDP servers to which the API Gateway round-robins connection attempts. To add a URL group, right-click the **Entrust GetAccess URL Sets**
    node, and select **Add a URL Set**. Alternatively, you can configure a URL connection set under the **Environment Configuration** > **External Connections**
    node in the Policy Studio tree.
* **Drift Time**:
    The specified **Drift Time**
    is used to account for the possible difference between the time on the GetAccess SAML PDP and the time on the machine hosting the API Gateway. This comes into effect when validating the SAML authorization assertion.
* **Resource**:
    This is the resource for which the client is requesting access. You can enter a selector representing a message attribute, which is looked up and expanded to a value at runtime. For example, to specify the original path on which the request was received by the API Gateway as the resource, enter the selector `${http.request.uri}`.
* **Actor/Role**:
    To add the SAML authorization assertion to the downstream message, select a SOAP actor/role to indicate the WS-Security block where the assertion is added. Leave this field blank if the assertion is not to be added to the message.

## SAML authorization filter

A Security Assertion Markup Language (SAML) authorization assertion contains proof that a certain user has been authorized to access a specified resource. Typically, such assertions are issued by a SAML Policy Decision Point (PDP) when a client requests access to a specified resource. The client must present identity information to the PDP, which ensures that the client does have permission to access the resource. The PDP then issues a SAML authorization assertion stating whether the client is allowed access the resource.

When the API Gateway receives a request containing such an assertion, it performs the following validation on the assertion details:

* Ensures the resource in the assertion matches that configured in the **SAML Authorization**
    filter
* Checks the client's access permissions for the resource
* Ensures the assertion has not expired

If the information validates, the API Gateway authorizes the message for the resource specified in the assertion.

The following example of a SAML authorization assertion might be useful when configuring the **SAML Authorization**
filter.

```xml
<saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
  xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
  MajorVersion="1" MinorVersion="0"
  AssertionID="192.168.0.131.1010924615489"
  Issuer="AA" IssueInstant="2002-03-26 16:23:35">
  <saml:Conditions NotBefore="2002-04-18T09:19:00Z"
    NotOnOrAfter="2003-06-28T09:21:00Z"/>
  <saml:AuthorizationDecisionStatement
    Resource="http://www.abc.org/services/getPrice"
    Decision="Permit">
      <saml:Action>Read</saml:Action>
  </saml:AuthorizationDecisionStatement>
</saml:Assertion>
```

### Configure details settings

The following fields are available on the **Details**
tab:

**SOAP Actor/Role**:
There might be several authorization assertions contained in a message. You can identify the assertion to validate by entering the name of the SOAP actor/role of the WS-Security header that contains the assertion.

**XPath Expression**:
Alternatively, you can enter an XPath expression to locate the authorization assertion. You can configure XPath expressions using the **Add**, **Edit**,
and **Delete**
buttons.

**SAML Namespace**:
Select the SAML namespace that must be used on the SAML assertion for this filter to succeed. To not check the namespace, select the `Do not check version`
option from the list.

**SAML Version**:
Enter the SAML version that the assertion must adhere to by entering the major version in the first field, followed by the minor version in the second field. For example, for SAML version 2.0, enter `2`
in the first field and `0`
in the second field.

**Drift Time**:
The *drift time*, specified in seconds, is used when checking the validity dates on the authorization assertion. The drift time allows for differences between the clock times of the machine on which the assertion was generated and the machine hosting the API Gateway.

**Remove enclosing WS-Security element on successful validation**:
Select this check box to remove the WS-Security block that contains the SAML assertion after the assertion has been successfully validated.

### Configure trusted issuer settings

You can use the table on the **Trusted Issuers**
tab to select the issuers that you consider trusted. In other words, this filter only accepts assertions that have been issued by the selected SAML authorities.

Click the **Add**
button to display the **Trusted Issuers**
dialog. Select the Distinguished Name of a SAML authority whose certificate has been added to the certificate store, and click **OK**. Repeat this step to add more SAML authorities to the list of trusted issuers.

### Configure optional settings

The optional settings enable further examination of the contents of the authorization assertion. The assertion can be checked to ensure that the authorized subject matches a specified value, and that the resource specified in the assertion matches the one entered here.

The API Gateway can verify that the subject in the SAML assertion (the `<NameIdentifier>`
) matches one of the following options:

* **The subject of the authentication filter**
* **The following value**: (for example, `user@axway.com`)
* **Neither of the above**

The API Gateway examines the `<Resource>`
tag inside the SAML authorization assertion. By default, it compares the `<Resource>`
to the `destination.uri`
attribute that is set in the policy. If they are identical, this filter passes. Otherwise, it fails.

You can enter a value for the resource in the **Resource**
field. The API Gateway then compares the `<Resource>`
in the assertion to this value instead of the `destination.uri`
attribute. The filter passes if the `<Resource>`
value matches the value entered in the **Resource**
field.

## SAML PDP authorization filter

API Gateway can request an authorization decision from a Security Assertion Markup Language (SAML) Policy Decision Point (PDP) for an authenticated client using the SAML Protocol (SAMLP). In such cases, the API Gateway presents evidence to the PDP in the form of some user credentials, such as the Distinguished Name of a client's X.509 certificate.

The PDP decides whether the user is authorized to access the requested resource. It then creates an authorization assertion, signs it, and returns it to the API Gateway in a SAML Protocol response. The API Gateway can then perform a number of checks on the response, such as validating the PDP signature and certificate, and examining the assertion. It can also insert the SAML authorization assertion into the message for consumption by a downstream web service.

### Configure SAML PDP request settings

The **Request** tab describes how the API Gateway packages the SAMLP request before sending to the SAML PDP.

You can configure the following fields on the **Request**
tab:

**SAML PDP URL Set**:
You can configure a group of SAML PDPs to which the API Gateway connects in a round-robin fashion if one or more of the PDPs are unavailable. This is known as a SAML PDP URL set. Click the button on the right, and select a previously configured SAML PDP URL set in the tree. To add a URL set, right-click the **SAML PDP URL Sets**
tree node, and select **Add a URL Set**. Alternatively, you can configure a SAML PDP URL set under the **Environment Configuration** > **External Connections**
node in the Policy Studio tree.

**SOAP Action**:
Enter the SOAP action required to send SAMLP requests to the PDP. Click the **Use Default**
button to use the following default SOAP action as specified by SAMLP:

```
http://www.oasis-open.org/committees/security
```

**SAML Version**:
Select the SAML version to use in the SAMLP request.

**Signing Key**:
If the SAMLP request is to be signed, click the **Signing Key**
button, and select the appropriate signing key from the certificate store.

**Resource**:
Enter the resource to obtain the authorization assertion for. You should specify the resource as a URI (for example, `http://www.axway.com/TestService`). The name of the resource is then included in the assertion.

**Evidence**:
SAMLP stipulates that proof of identity in the form of a SAML authentication assertion must be presented to the SAML PDP as part of the SAMLP request. API Gateway can use an existing SAML authentication assertion that is already present in the message, or generate one based on the user that authenticated to it.

Select the **Use SAML Assertion in message**
option to include an existing assertion in the SAMLP request. Specify the actor/role of the WS-Security block where the assertion is found in the **SOAP Actor/Role**
field.

Alternatively, select the **Create SAML Assertion from authenticated client**
radio button to generate a new authentication assertion for inclusion in the SAMLP request. To sign the newly generated assertion with a private key from the certificate store, click **Signing Key**. Alternatively, you can enter a selector expression for the signing certificate (for example, `${certificate}`).

The specified **Drift Time**
is subtracted from the time that API Gateway generates the authentication assertion. This accounts for any possible difference in the times of the machines hosting the SAML PDP and the API Gateway.

#### SAML subject settings

You can describe the *subject*
of the SAML assertion on the **SAML Subject**
tab. Complete the following fields:

**Subject Selector Expression**:
Enter a selector expression for the message attribute that contains the user name of an authenticated user. The default value is `${authentication.subject.id}`.

**Subject Format**:
Select the format of the subject selected in the **Subject Selector Expression**
field above.

There is no need to select a format here if the **Subject Attribute**
field is set to `authentication.subject.id`.

#### Subject confirmation settings

The settings on the **Subject Confirmation**
tab determine how the `SubjectConfirmation`
block of the SAML assertion is generated. When the assertion is consumed by a downstream web service, the information contained in the `SubjectConfirmation`
block can be used to authenticate the end user that authenticated to the API Gateway, or the issuer of the assertion, depending on what is configured.

The following is a typical `SubjectConfirmation`
block:

```xml
<saml:SubjectConfirmation>
  <saml:ConfirmationMethod>
    urn:oasis:names:tc:SAML:1.0:cm:holder-of-key
  </saml:ConfirmationMethod>
  <dsig:KeyInfo xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
    <dsig:X509Data>
      <dsig:X509SubjectName>CN=axway</dsig:X509SubjectName>
      <dsig:X509Certificate>
        MIICmzCCAY ...... mB9CJEw4Q=
      </dsig:X509Certificate>
    </dsig:X509Data>
  </dsig:KeyInfo>
</saml:SubjectConfirmation>
```

You must configure the following fields on the **Subject Confirmation**
tab:

**Method**:\
The selected value determines the value of the `<ConfirmationMethod>`
element. The following table shows the available methods, their meanings, and their respective values in the `<ConfirmationMethod>`
element:

* **Holder Of Key**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The `<SubjectConfirmation>` contains a `<dsig:KeyInfo>` section with the certificate of the user selected to sign the SAMLP request. The user selected to sign the SAMLP request must be the authenticated subject (`authentication.subject.id`). Select the **Include Certificate** option if the signer's certificate is to be included in the `SubjectConfirmation` block. Alternatively, select the **Include Key Name** option if only the key name is to be included. Value: `urn:oasis:names:tc:SAML:1.0:cm:holder-of-key`
* **Bearer**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`
* **SAML Artifact**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:artifact`
* **Sender Vouches**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The SAMLP request must be signed by a user. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`

If the **Method**
field is left blank, no `<ConfirmationMethod>`
block is inserted into the assertion.

**Include Certificate**:
Select this option to include the SAML subject's certificate in the `<KeyInfo>`
section of the `<SubjectConfirmation>`
block.

**Include Key Name**:
Alternatively, if you do not want to include the certificate, you can select this option to only include the key name in the `<KeyInfo>`
section.

### Configure SAML PDP response settings

The **Response**
tab enables you to configure the API Gateway to perform a number of checks on the SAMLP response from the PDP by examining the contents of various key elements in the authorization assertion.

**SOAP Actor/Role**:
If the SAMLP response from the PDP contains a SAML authorization assertion, the API Gateway can extract it from the response and insert it into the downstream message. The SAML assertion is inserted into the WS-Security block identified by the specified SOAP actor/role.

**Drift Time**:
The SAMLP request to the PDP is time stamped by the API Gateway. To account for differences in the times on the machines running the API Gateway and the SAML PDP the specified time is subtracted from the time at which the API Gateway generates the SAMLP request.

**Subject in the assertion (the NameIdentifier) must match**:
The authorization assertion can be checked to ensure that the authorized subject matches a specified value, and that the resource specified in the assertion matches the one entered here. API Gateway can verify that the subject in the SAML assertion (the `NameIdentifier`
) matches one of the following options:

* **The subject of the authentication filter**
* **The following value** (for example, `CN=sample, O=Company, C=ie`)
* **Neither of the above**

## Tivoli authorization filter

IBM Tivoli Access Manager for e-business (TAM) provides authentication and access control services for web resources. It also stores policies describing the access rights of users.

API Gateway can integrate with this product through the **Tivoli** filter. This filter can query Tivoli for authorization information for a particular user on a given resource. In other words, API Gateway asks Tivoli to make the authorization decision. If the user has been given authorization rights to the web service, the request is allowed through to the service. Otherwise, the request is rejected.

### Tivoli prerequisites

Before you can configure the **Tivoli** filter, you must configure API Gateway for integration with TAM. For more details, see the
[API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).

### Configure Tivoli settings

Configure the following fields on the **Tivoli Authorization** window:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Object Space**:
The object space represents the resource for which the client must be authorized. Enter the name of the resources in the **Object Space** field. You can also enter selectors that represent the values of message attributes. At runtime, API Gateway expands the selector to the current value of the corresponding message attribute. For example, to specify the original path on which the request was received by API Gateway as the resource, enter the selector `${http.request.uri}`.

**Permissions**:
Clients can access a resource with a number of permissions such as read, write, and execute. A client is only authorized to access the requested resource if it has the relevant permissions checked in the table. To edit the existing permissions, click **Edit**.

**Attributes**:
You can specify a list of user attributes to retrieve from the Tivoli server. To add attributes to be retrieved that are not listed, click **Add**, and enter the attribute name in the dialog. If you want to retrieve all attributes, leave the table blank, and select **Set attributes for SAML Attribute token**. You can then use these attributes in a **Insert SAML Attribute Assertion** filter at a later stage. If you do not want to retrieve any attributes, leave the table blank and deselect **Set attributes for SAML Attribute token**.

{{< alert title="Note" color="primary" >}}The permissions for the `primary` action group are available by default. You can also configure custom action groups and make them available for selection in the filter. The groups created here reflect custom groups created on the Tivoli server. To create a new group with custom action bits, click the **Edit** button to display the **Tivoli Action Group** dialog.{{< /alert >}}

Enter a name for the group in the **Name** field. Click **Add** to add a new action bit to the group. The **Tivoli Action** dialog is displayed. You must enter an **Action Bit** (for example, `r`) and a **Description** (for example, `Read permission`) for the new action bit. Click **OK** on the **Tivoli Action** dialog to return to the **Tivoli Action Group** dialog.

Add as many action bits as required to your new group before clicking **OK** on the **Tivoli Action Group**
dialog. The new action bits are then available for selection in the table on the main filter window.

**Tivoli Configuration Files**:
A Tivoli configuration file that contains all the required connection details is associated with a particular API Gateway instance. Click **Settings** to display the **Tivoli Configuration** dialog.

On the **Tivoli Configuration** dialog, select the API Gateway instance whose connection details you want to configure. For more information see, [Integrate with Identity Management servers](/docs/apigtw_auth_auth/).
