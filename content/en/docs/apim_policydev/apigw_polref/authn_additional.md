{
"title": "Additional authentication filters",
  "linkTitle": "Additional authentication filters",
  "weight": 35,
  "date": "2019-10-17",
  "description": "Additional authentication filters, including attribute authentication, SAML PDP,  and CA SOA Security Manager."
}

## Attribute authentication filter

In cases when user credentials are passed to the API Gateway in a non-standard way, the credentials can be copied into API Gateway message attributes, and authenticated against a specified authentication repository (for example, API Gateway user store, LDAP directory, or database) using an **Attribute Authentication** filter. For example, assume user credentials are passed to API Gateway in the following XML message:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Body>
    <ns:User xmlns:ns="http://www.user.com">
      <ns:Username>1</ns:Username>
      <ns:Password>2</ns:Password>
    </ns:User>
  </s:Body>
</s:Envelope>
```

In this example, the standard methods of passing credentials (for example, HTTP basic or digest authentication, SAML assertions, and WS-Security Username tokens) are bypassed, and the client sends the user name and password as parameters in a simple SOAP message.

When the API Gateway receives this message, it can extract the value of the `<Username>` and `<Password>` elements using an XPath expression configured in the **Retrieve from Message** filter. This filter uses an XPath expression to retrieve the value of an element or attribute, and can then store this value in the specified message attribute.

You can configure an instance of this filter to retrieve the value of the `<Username>` attribute, and store it in the `authentication.subject.id` message attribute. Similarly, you can configure another filter to retrieve the value of the `<Password>`, and store it in the `authentication.subject.password` message attribute.

The **Attribute Authentication** filter can then use the user name and password values stored in these message attributes to authenticate the user against the specified authentication repository.

Complete the following fields to configure this filter:

**Name**: Enter an appropriate name for this filter to display in a policy.

**Username**: Specify the API Gateway message attribute that contains the user name of the user to be authenticated. The default attribute is the `authentication.subject.id` attribute, which is typically used to store a user name.

**Password**: Enter the API Gateway message attribute that contains the password of the user to authenticate. The default message attribute is `authentication.subject.password`, which typically stores a password.

**Credential Format**: Select the format of the credential stored in the API Gateway message attribute specified in the **Username** field above. By default, `User Name` is selected.

**Repository Name**: Select an existing repository to authenticate the user against from the list. Alternatively, you can configure a new authentication repository by clicking the **Add** button.

## Insert timestamp filter

In any secure communications protocol, it is crucial that secured messages do not have an indefinite life span. In secure web services transactions, a WS-Utility (WSU) timestamp can be inserted into a WS-Security Header to define the lifetime of the message in which it is placed. A message containing an expired timestamp should be rejected immediately by any web service that consumes the message.

Typically, the timestamp contains `Created` and `Expires` times, which combine to define the lifetime of the timestamp. The following shows an example `wsu:Timestamp`:

```xml
<wsu:Timestamp xmlns:wsu="http://schemas.xmlsoap.org/ws/2002/07/utility">
  <wsu:Created>2009-03-16T16:32:22Z</wsu:Created>
  <wsu:Expires>2009-03-16T16:42:22Z</wsu:Expires>
</wsu:Timestamp>
```

Because the WS-Utility timestamp is inserted into the WS-Security header block, it is also referred to as a WSS timestamp. For example, see [Extract WSS timestamp](/docs/apim_policydev/apigw_polref/attributes_manipulate/#extract-wss-timestamp-filter).

Complete the following fields to configure the API Gateway to insert a timestamp into the message:

**Name**: Enter an intuitive name for the filter to display in a policy.

**Actor**: The timestamp is inserted into the WS-Security header identified by the SOAP Actor selected here.

**Expires In**: Configure the lifetime of the timestamp (and hence the message into which the timestamp is inserted) by specifying the expiry time of the assertion. The expiry time is expressed in days, hours, minutes, and seconds.

**Layout Type**: In cases where the timestamp must adhere to a particular layout as mandated by the WS-Policy `<Layout>` assertion, you must select the appropriate layout type. A web service that enforces a WS-Policy might reject the message if the layout of security elements in the SOAP header is incorrect. Therefore, you must ensure that you select the correct layout type.

## SAML PDP authentication filter

The API Gateway can request an authentication decision from a Security Assertion Markup Language (SAML) Policy Decision Point (PDP) for an authenticated client using the SAML Protocol (SAMLP). In such cases, the API Gateway presents evidence to the PDP in the form of some user credentials, such as the Distinguished Name of a client's X.509 certificate.

The PDP decides whether to authenticate the end user. It then creates an authentication assertion, signs it, and returns it to the API Gateway in a SAMLP response. The API Gateway can then perform a number of checks on the response, such as validating the PDP signature and certificate, and examining the assertion. It can also insert the SAML authentication assertion into the message for consumption by a downstream web service.

### Configure request settings

The **Request** tab describes how the API Gateway should package the SAMLP request before sending it to the SAML PDP.

You can configure the following fields on the **Request** tab:

**SAML PDP URL Set**: You can configure a group of SAML PDPs to which the API Gateway connects in a round-robin fashion if one or more of the PDPs are unavailable. This is known as a SAML PDP URL set. Click the button on the right, and select a previously configured SAML PDP URL set in the tree. To add a URL set, right-click the **SAML PDP URL Sets** tree node, and select **Add a URL Set**. Alternatively, you can configure a SAML PDP URL set under the **Environment Configuration** > **External Connections** node in the Policy Studio tree.

**SOAP Action**: Enter the SOAP action required to send SAMLP requests to the PDP. Click the **Use Default** button to use the following default SOAP action as specified by SAMLP:

```
http://www.oasis-open.org/committees/security
```

**SAML Version**: Select the SAML version to use in the SAMLP request.

**Signing Key**: If the SAMLP request is to be signed, click the **Signing Key** button, and select the appropriate signing key from the certificate store.

#### SAML subject settings

You can describe the *subject* of the SAML assertion on the **SAML Subject** tab. Complete the following fields:

**Subject Selector Expression**: Enter a selector expression for the message attribute that contains the user name of an authenticated user. The default value is `${authentication.subject.id}`.

**Subject Format**: Select the format of the subject selected in the **Subject Selector Expression** field above.

There is no need to select a format here if the **Subject Selector Expression** field is set to `authentication.subject.id`.

#### Subject confirmation settings

The settings on the **Subject Confirmation** tab determine how the `<SubjectConfirmation>` block of the SAML assertion is generated. When the assertion is consumed by a downstream web service, the information contained in the `<SubjectConfirmation>` block can be used to authenticate the end user that authenticated to the API Gateway, or the issuer of the assertion, depending on what is configured.

The following is a typical `<SubjectConfirmation>` block:

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

You must configure the following fields on the **Subject Confirmation** tab:

**Method**: The selected value determines the value of the `<ConfirmationMethod>` element. The following table shows the available methods, their meanings, and their respective values in the `<ConfirmationMethod>` element:

* **Holder Of Key**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The `<SubjectConfirmation>` contains a `<dsig:KeyInfo>` section with the certificate of the user selected to sign the SAMLP request. The user selected to sign the SAMLP request must be the authenticated subject (`authentication.subject.id`). Select the **Include Certificate** option if the signer's certificate is to be included in the `SubjectConfirmation` block. Alternatively, select the **Include Key Name** option if only the key name is to be included. Value: `urn:oasis:names:tc:SAML:1.0:cm:holder-of-key`
* **Bearer**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`
* **SAML Artifact**: A `<SubjectConfirmation>` is inserted into the SAMLP request. Value: `urn:oasis:names:tc:SAML:1.0:cm:artifact`
* **Sender Vouches**: A `<SubjectConfirmation>` is inserted into the SAMLP request. The SAMLP request must be signed by a user. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`

If the **Method** field is left blank, no `<ConfirmationMethod>` block is inserted into the assertion.

**Include Certificate**: Select this option to include the SAML subject's certificate in the `<KeyInfo>` section of the `<SubjectConfirmation>` block.

**Include Key Name**: Alternatively, to not include the certificate, select this option to only include the key name in the `<KeyInfo>` section.

### Configure response settings

The **Response** tab configures the SAMLP response returned from the SAML PDP. The following fields are available:

**SOAP Actor/Role**: If the SAMLP response from the PDP contains a SAML authentication assertion, the API Gateway can extract it from the response and insert it into the downstream message. The SAML assertion is inserted into the WS-Security block identified by the specified SOAP actor/role.

**Drift Time**: The SAMLP request to the PDP is time stamped by the API Gateway. To account for differences in the times on the machines running the API Gateway and the SAML PDP the specified time is subtracted from the time at which the API Gateway generates the SAMLP request.

## CA SOA Security Manager authentication filter

CA SOA Security Manager can authenticate end users and authorize them to access protected web resources. When the API Gateway receives a message containing user credentials, it can forward the message to CA SOA Security Manager where the passed credentials are extracted from the message to authenticate the end user. When the message has been passed to CA SOA Security Manager, it can authenticate the user by the following methods:

* **XML Document Credential Collector**: Gathers credentials from the message and maps them to fields within a user directory.
* **XML Digital Signature**: Validates the X.509 certificate contained within an XML-Signature on the message.
* **WS-Security**: Extracts user credentials from WS-Security tokens contained in the message.
* **SAML Session Ticket**: Consumes a SAML session ticket from an HTTP header, SOAP envelope, or session cookie to authenticate the end user.

By delegating the authentication decision to CA SOA Security Manager, the API Gateway acts as a Policy Enforcement Point (PEP). It *enforces* the decisions made by the CA SOA Security Manager, which acts a Policy Decision Point (PDP).

### Prerequisites

Integration with CA SOA Security Manager requires CA TransactionMinder SDK version 6.0 or later. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

#### Add third-party binaries to API Gateway

To add third-party binaries to API Gateway, perform the following steps:

1. Add the binary files as follows:

   * Add `.jar`   files to the `INSTALL_DIR/apigateway/ext/lib`   directory.
   * Add `.so`   files to the `INSTALL_DIR/apigateway/<platform>/lib` directory.
2. Restart API Gateway.

#### Add third-party binaries to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies** in the Policy Studio main menu.
2. Click **Add** to select a JAR file to add to the list of dependencies.
3. Click **Apply** when finished. A copy of the JAR file is added to the `plugins` directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio with the `-clean` option. For example:

   ```
   cd INSTALL_DIR/policystudio/
   policystudio -clean
   ```

### Configure CA SOA Security Manager authentication

**Name**: Enter a name for this filter to display in a policy.

**Agent Name**: To act as a PEP for the CA SOA Security Manager, the API Gateway must have been set up as a *SOA Agent* with the Policy Server. For more details on how to do this, see the *CA SOA Security Manager Agent Configuration Guide*.

Click the button on the right to select a previously configured agent to connect to SOA Security Manager. This name *must* correspond with the name of an agent previously configured in the SOA Security Manager **Policy Server**. At runtime, the API Gateway connects as this agent to a running instance of SOA Security Manager.

To add an agent, right-click the **SiteMinder/SOA Security Manager Connections** tree node, and select **Add a SOA Security Manager Connection**. Alternatively, you can add SOA Security Manager connections under the **Environment Configuration** > **External Connections** node in the Policy Studio tree. For details on how to configure SOA Security Manager connections, see
[Configure SiteMinder/SOA Security Manager connections](/docs/apim_policydev/apigw_external_connections/connector_ca_connection/).

#### Message details settings

While authenticating the user against CA SOA Security Manager, the user can also be authorized for a specified action on a particular resource. Configure the following fields in the **Message Details** section:

**Resource**: Enter the name of the resource for which you want to ensure that the user has access to. By default, the `http.request.uri` message attribute is used, which contains the relative path on which the request was received by the API Gateway.

**Action**: Specify the action that the user is attempting to perform on the specified resource. The API Gateway checks the user's entitlements in CA SOA Security Manager to ensure that the user is allowed to perform this action on the resource entered above. By default, the `http.request.verb` message attribute is used, which stores the HTTP verb used by the client when sending up the message.

**Protocol**: Enter the protocol used by the client to access the requested resource. Users can have different access rights depending on their roles in the organization. For example, managers might be allowed to FTP to a given resource, but more junior employees might only be allowed to GET a resource using HTTP. Defaults to `http`.

**Headers**: To carry out further authorization checks on the message, it is possible to forward the HTTP headers associated with the client message to the CA SOA Security Manager. By default, the `http.headers` message attribute is used to ensure that the original client headers are send to the CA SOA Security Manager.

### `XmlToolkit.properties` file

The `XmlToolkit.properties` file contains default properties used by the SOA agent, such as the URL of the CA SOA Manager, an identifier for the SOA agent, and an indication to the SOA Manager if it should perform fine-grained resource identification or not. The `XmlToolkit.properties` file can be found in the `/lib/modules/soasm` directory of your API Gateway installation.

```
#Wed Jul 18 15:02:16 BST 2007
WSDMResourceIdentification=yes
WS_UT_CREATION_EXPIRATION_MINUTES=60
```

The following properties are available:

* `WSDMResourceIdentification`: This value cannot be configured from the Policy Studio, and so can only be set directly in the properties file. If this property is set to `no`   (or if the properties file cannot be found) only a coarse-grained resource identification is performed on the requested URL. If this value is set to `yes`, a fine-grained resource identification including the requested URL, web service name, and SOAP operation (`<url>/<web service name>/<soap operation>`).
* `WS_UT_CREATION_EXPIRATION_MINUTES`: Specifies the WS-Username Token age limit restriction in minutes. This setting helps prevent against replay attacks. The default token age limit is 60 minutes. See the following section for more information on modifying this setting.

#### Configure the user name and password digest token age restriction

By default, the WS-Security authentication scheme imposes a 60 minute restriction on the age of user name and password digest tokens to protect against replay attacks.

You can configure a different value for the token age restriction for the API Gateway by setting the `WS_UT_CREATION_EXPIRATION_MINUTES` parameter in the `XmlToolkit.properties` file for that API Gateway. To configure the API Gateway to use a non-default age restriction for user name and password token authentication, complete the following steps:

1. Navigate to the`<INSTALL_DIR>/system/lib/modules/soasm` directory, where `INSTALL_DIR` points to the root of your API Gateway installation.
2. Open the `XmlToolkit.properties` file in a text editor.
3. Add the following line, where `token_age_limit` specifies the token age limit in minutes:

   ```
   WS_UT_CREATION_EXPIRATION_MINUTES=token_age_limit
   ```
4. Save and close the `XmlToolkit.properties` file.
5. Restart API Gateway.

{{< alert title="Note" color="primary" >}}

* The properties file is written to the `/lib/modules/soasm`   directory when a SOA Security Manager Authentication or Authorization filter is loaded at startup, or on server refresh (for example, when a configuration update is deployed), but only if the file does not already exist in this location.
* If the properties file already exists in the `/lib/modules/soasm`   directory, the `WSDMResourceIdentification`   property is *not*   overwritten. In other words, the user is allowed to manually set this property independently of the Policy Studio.
* If the `WSDMResourceIdentification`  property does not exist, it is given a default value of `yes`     and written to the file.
  {{< /alert >}}
