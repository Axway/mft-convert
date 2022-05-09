{
"title": "Certificate validation filters",
  "linkTitle": "Certificate validation filters",
  "weight": 70,
  "date": "2019-10-17",
  "description": "Validate X.509 certificates using OCSP, CRL, or XKMS."
}
Whenever API Gateway receives an X.509 certificate, either as part of the SSL handshake or as part of the XML message itself, it is important to be able to determine whether that certificate is legitimate or not.

Certificates can be revoked by their issuers if it becomes apparent that the certificate is being used maliciously. Such certificates should never be trusted, and so it is very important that API Gateway can perform certificate validation.

API Gateway uses the following methods/protocols to validate certificates:

* **Online Certificate Status Protocol** (OCSP) – An automated certificate checking network protocol. API Gateway can query the OCSP responder for the status of a certificate. The responder returns whether the certificate is still trusted by the CA that issued it.
* **Certificate Revocation List** (CRL) – A signed list indicating a set of certificates that are no longer considered valid (that is, revoked certificates) by the certificate issuer. API Gateway can query a CRL to find out if a given certificate has been revoked. If the certificate is present in the CRL, it should not be trusted.
* **XML Key Management Services** (XKMS) – An XML-based protocol for (amongst other things) establishing the trustworthiness of a certificate over the Internet. API Gateway can query an XKMS responder to determine whether or not a given certificate can be trusted or not.

The API Gateway can check the validity of a client certificate using any of the following filters.

## OCSP client filter

You can use the Online Certificate Status Protocol (OCSP) to retrieve the revocation status of a certificate, as an alternative to retrieving Certificate Revocation Lists (CRLs).

{{< alert title="Note" color="primary" >}} To validate a certificate using an OCSP request or CRL lookup, the issuing CA's certificate should be trusted by API Gateway.

For a CRL lookup, the CA's public key is needed to verify the signature on the CRL. For an OCSP request, the CA's public key must be submitted as part of the request. The issuing CA's public key is not always present in issued certificates, so it is necessary to retrieve it from the API Gateway certificate store instead. {{< /alert >}}

The **OCSP Client** filter enables you to retrieve certificate revocation status from an OCSP responder, such as Axway Validation Authority. The input to this filter is the certificate to be checked. You must specify the message attribute that contains the certificate (`java.security.cert.X509Certificate`).

This filter returns the following outputs:

* `True` if the certificate status is `GOOD`
* `False` if the certificate status is `REVOKED` or `UNKNOWN` (or if an exception occurs)

This filter also outputs the following message attributes:

* `ocsp.response.certificate.status`: The status of the OCSP responder certificate as an `java.lang.Integer`. The possible values are:

    * `0` (`GOOD`)
    * `1` (`REVOKED`)
    * `2` (`UNKNOWN`)

    You can use this attribute if the filter return value is not detailed enough.
* `ocsp.response.signing.certificate`: The optional certificate included in the OCSP response (`java.security.cert.X509Certificate`) used to sign the response. You can use an additional OCSP filter to verify the status of this certificate.

### Configure OCSP general settings

Configure the following general settings on the **OCSP Client** dialog:

**Name**:

Enter a suitable name for this OCSP client filter to display in a policy.

**OCSP Responder URL**:

Enter the URL of the OCSP responder.

### Configure OCSP message settings

Configure the following OCSP message settings on the **Settings** tab:

**The message attribute storing the certificate to validate**:

Enter the name of the attribute that contains the certificate to be checked (`java.security.cert.X509Certificate`). The default is `${certificate}`.

**The key to sign the request**:

Click the **Signing Key** button to open the list of certificates in the certificate store. You can then select the key to use to sign requests to the OCSP responder.

You can select a specific certificate from the certificate store in the dialog, or click **Create/Import** to create or import a certificate. Alternatively, you can specify a certificate to bind to at runtime using an environment variable selector (for example, `${env.serverCertificate}`).

**Validate response**:

Select the **Do not validate response** option to disable response validation. The response from the OCSP responder is not validated when this option is selected.

Select the **Validate response** option to enable response validation. Click one or more of the following options to specify how the response from the OCSP responder is validated:

* **Against the certificate contained in the response**: The response is validated against the certificate contained in the response. This option is selected by default.
* **Against the CA certificate of the certificate being validated**: The response is validated against the CA certificate of the certificate being validated. This option is selected by default.
* **Against the specified certificate**: Click **Signing Key**   to choose a certificate from the certificate store or to specify a certificate to bind to at runtime.

You can select any combination of these options. If multiple options are selected, the filter continues as soon as the response is successfully validated against one of the selected options.

When the **Validate response** option is selected, you can also use the following time validation options:

* **Allowable time difference between this system and update time in the response**: Enter a value in milliseconds. You can use this field to allow for drift on server and client machines. The default value is 3000 (3 seconds).
* **The response is valid until**: Select the **Expiration date** check box to use the `nextUpdate` field in the OCSP response as the expiration date. This is the default.   Alternatively, to override the expiration date in the response (for example, to set a longer expiration), deselect the **Expiration date** check box, enter a value in the text box, and select a time unit (days, hours, minutes, or seconds).
* **A response with no expiration date is valid for**: Enter a value in the text box and select a time unit (days, hours, minutes, or seconds). The default value is 6 hours.

For more information on the time validation logic, see [Time validation logic](#Time).

**Use nonce to prevent reply attack**:

Select this option to include a nonce in the request. This is a randomly generated number that is added to the message to help prevent reply attacks.

**Store results of certificate status in**:

Click the browse button to select the cache in which to store the certificate status result. The list of currently configured caches is displayed in the tree. To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration** > **Libraries** node in the Policy Studio tree.

Storing the certificate status in the cache enables the certificate status to be retrieved without having to return to the OCSP responder.

#### Time validation logic

When you enable response validation, the following time validation logic is applied in the following order:

1. The `thisUpdate` field in the response is checked against the value you entered in **Allowable time difference between this system and update time in the response**. The response is valid if the current time on this system does not differ from the `thisUpdate` field in the response by more than the value entered (or the default value of 3000 milliseconds). For example:

   ```
   thisUpdate + allowable time difference < current time on this system
   ```
2. If you selected the **Expiration date** check box in the **The response is valid until** field, the response is checked against the `nextUpdate` field in the response. The response is considered *valid until* the `nextUpdate` time in the response. For example:

   ```
   thisUpdate + allowable time difference < current time on this system < nextUpdate
   ```
3. If you did not select the **Expiration date** check box in the **The response is valid until** field, the response is checked against the expiration value you entered in the text box. The response is considered valid if the current time on this system is less than the sum of the `thisUpdate` time in the response and the value you entered. For example:

   ```
   thisUpdate + allowable time difference < current time on this system < thisUpdate + valid until
   ```

   {{< alert title="Note" color="primary" >}}The **The response is valid until** settings are only applicable when the `nextUpdate` field is present in the response and the **Expiration date** check box is not selected.{{< /alert >}}
4. If no expiration date is available (that is, if the `nextUpdate` field is not present in the response), the response is checked against the value you entered in **A response with no expiration date is valid for**. The response is considered valid if the current time on this system is less than the sum of the `thisUpdate` time in the response and the value you entered (or the default value of 6 hours). For example:

   ```
   thisUpdate + allowable time difference < current time on this system < thisUpdate + valid for
   ```

   {{< alert title="Note" color="primary" >}}The **A response with no expiration date is valid for** settings are only applicable when the `nextUpdate` field is not present in the response.{{< /alert >}}

### Configure OCSP routing settings

You can configure the settings for routing the OCSP request to the OCSP responder on the **Routing** tab.

You can configure SSL settings, credential profiles for authentication, and other settings for the connection using the **SSL**, **Authentication**, and **Settings** tabs. For more details, see [Connect to URL](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter).

### Configure OCSP advanced settings

On the **Advanced** tab, you can enable a specific policy to run after the message is created, or after the response is received.

Configure the following advanced settings:

* **Run this policy after the message has been created**: Click the browse button to select a policy to be run after the message has been created.
* **Run this policy after a response has been received**: Click the browse button to select a policy to be run after a response has been received.
* **Record outbound transactions**: Select this option to enable recording of outbound transactions on the **Traffic** tab in API Gateway Manager. This field is not selected by default.

### Integration with Axway Validation Authority

When using the **OCSP client** with Axway Validation Authority (VA) as an OCSP responder, you can use the following trust models:

* **Direct trust**

    In this model, OCSP responses are signed with the OCSP signing certificate of the VA server. The signing certificate is not included in the OCSP response.
* **VA delegated trust**

    In this model, the signing certificate is included in the OCSP response. API Gateway might not have this certificate. If not, it must have the issuer (CA) certificate of the signing certificate.

You can import certificates into the API Gateway trusted certificate store under the **Environment Configuration** > **Certificates and Keys** node in the Policy Studio tree.

For more information on Axway Validation Authority, see the Axway Validation Authority user documentation.

## Extract certificate attributes filter

You can use the **Extract Certificate Attributes** filter to extract the X.509 attributes from a certificate stored in a specified API Gateway message attribute.

Typically, this filter is used in conjunction with the **Find Certificate** filter, which is found in the **Certificates** category of message filters. In this case, the **Find Certificate** filter can locate a certificate from one of many possible sources (for example, the message itself, an HTTP header, or the API Gateway certificate store), and store it in a message attribute, which is usually the `certificate` attribute.

The **Extract Certificate Attributes** filter can then retrieve this certificate and extract the X.509 attributes from it. For example, you can then use a **Validate Message Attribute** filter to check the values of the attributes.

### Generated message attributes

The **Extract Certificate Attributes** filter extracts the X.509 certificate attributes and populates a number of API Gateway message attributes with their respective values. The following table lists the message attributes that are generated by this filter, and shows what each of these attributes contains after the filter has executed:

`attribute.lookup.list` : This user attribute list contains an attribute for each Distinguished Name (DName) attribute for the subject (`cn`, `o`, `l`, and so on). The user attributes are named `cn`, `o`, and so on.

`attribute.subject.id` : The DName of the subject of the cert.

`attribute.subject.format` : Set to `X509DName`.

`cert.basic.constraints` : If the subject is a Certificate Authority (CA), and the `BasicConstraints` extension exists, this field gives the maximum number of CA certificates that may follow this certificate in a certification path. A value of zero indicates that only an end-entity certificate may follow in the path. This contains the value of `pathLenConstraint` if the `BasicConstraints` extension is present in the certificate and the subject of the certificate is a CA, otherwise its value is -1. If the subject of the certificate is a CA and `pathLenConstraint` does not appear, there is no limit to the allowed length of the certification path.

`cert.extended.key.usage` : A String representing the OBJECT IDENTIFIERs of the `ExtKeyUsageSyntax` field of the extended key usage extension (`OID = 2.5.29.37`). It indicates a purpose for which the certified public key may be used, in addition to, or instead of, the basic purposes indicated in the key usage extension field.

`cert.hash.md5` : An MD5 hash of the certificate.

`cert.hash.sha1` : A SHA1 hash of the certificate.

`cert.issuer.alternative.name` : An alternative name for the certificate issuer from the `IssuerAltName` extension (`OID = 2.5.29.18`).

`cert.issuer.id` : The DName of the issuer of the certificate.

`cert.issuer.id.c` : The `c` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.cn` : The `cn` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.emailaddress` : The `email` or `emailaddress` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.l` : The `l` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.o` : The `o` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.ou` : The `ou` attribute of the issuer of the certificate, if it exists.

`cert.issuer.id.st` : The `st` attribute of the issuer of the certificate, if it exists.

`cert.key.usage.cRLSign` : Set to `true` or `false` if the key can be used for `crlSign`.

`cert.key.usage.dataEncipherment` : Set to `true` or `false` if the key can be used for `dataEncipherment`.

`cert.key.usage.decipherOnly` : Set to `true` or `false` if the key can be used for `decipherOnly`.

`cert.key.usage.digitalSignature` : Set to `true` or `false` if the key can be used for digital signature.

`cert.key.usage.encipherOnly` : Set to `true` or `false` if the key can be used for `encipherOnly`.

`cert.key.usage.keyAgreement` : Set to `true` or `false` if the key can be used for `keyAgreement`.

`cert.key.usage.keyCertSign` : Set to `true` or `false` if the key can be used for `keyCertSign`.

`cert.key.usage.keyEncipherment` : Set to `true` or `false` if the key can be used for `keyEncipherment`.

`cert.key.usage.nonRepudiation` : Set to `true` or `false` if the key can be used for non-repudiation.

`cert.not.after` : Not after validity period date.

`cert.not.before` : Not before validity period date.

`cert.serial.number` : Certificate serial number.

`cert.signature.algorithm` : The signature algorithm for certificate signature.

`cert.subject.alternative.name` : An alternative name for the subject from the `SubjectAltName` extension (`OID = 2.5.29.17`).

`cert.subject.id` : The DName of the subject of the certificate.

`cert.subject.id.c` : The `c` attribute of the subject of the certificate, if it exists.

`cert.subject.id.cn` : The `cn` attribute of the subject of the certificate, if it exists.

`cert.subject.id.emailaddress` : The `email` or `emailaddress` attribute of the subject of the certificate, if it exists.

`cert.subject.id.l` : The `l` attribute of the subject of the certificate, if it exists.

`cert.subject.id.o` : The `o` attribute of the subject of the certificate, if it exists.

`cert.subject.id.ou` : The `ou` attribute of the subject of the certificate, if it exists.

`cert.subject.id.st` : The `st` attribute of the subject of the certificate, if it exists.

`cert.version` : The certificate version.

### Configure extract certificate attributes

Configure the following fields:

**Name**: Enter a name for the filter to display in a policy.

**Certificate Attribute**: The **Extract Certificate Attributes** filter extracts the attributes from the certificate contained in the message attribute selected or entered here. The selected attribute must contain a single certificate only.

**Include Distribution Points**: If the certificate contains CRL Distribution Point X.509 extension attributes (which point to the location of the certificate issuer's CRL), you can also extract these and store them in message attributes by selecting this check box. The extracted distribution points are stored in message attributes with a `distributionpoint.` prefix.

## Validate certificate store filter

The **Validate Server's Certificate Store** filter checks API Gateway's certificate store for certificates that are due to expire before a specified number of days. This enables you to monitor the certificates that API Gateway is running with.

For example, you can configure a policy that includes a **Validate Server's Certificate Store** filter and an **Alert** filter, which sends an email alert when it finds certificates that are due to expire. You can also configure this policy to run at regular intervals using the policy execution scheduler provided with API Gateway.

### Configure validate certificate store

Configure the following fields:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Days before expires**: Enter the number of days before the certificates are due to expire.

**Check Server's Certificate Store**: Select whether to check the certificates in API Gateway's certificate store. This is selected by default.

**Check Server's Java Keystore**: Select whether to check the certificates in API Gateway's Java Keystore. This is not selected by default. When selected, you must enter the password for this keystore.

**Check Java Keystore**: Select whether to check the certificates in the specified Java Keystore. This is not selected by default. When selected, you must configure the following fields:

* **Keystore Location**: Specify the path to this keystore (for example, `/home/oracle/osr-client.jks`).
* **Password**: Enter the password for this keystore.

### Deployment example

The following example shows a **Validate Certificates** policy that includes a **Validate Server's Certificate Store** filter and an **Alert** filter. This policy sends an email alert when it finds certificates that are due to expire:

![Validating Gateway Certificates](/Images/docbook/images/certs/validate_certs_policy.gif)

#### Configure an email alert

When this filter is successful, and finds certificates that are due to expire, it generates an `expired.certs.summary` attribute, which contains a summary of certificates due to expire. You can then use this attribute in the **Alert** filter to send an email alert to the API Gateway administrators, as shown in the following example:

![Configuring an Alert Message](/Images/docbook/images/certs/validate_certs_alert_message.gif)

You must also select a preconfigured email alert destination on the **Destination** tab (for example, **Email API Gateway Administrators**).

#### Configure a policy execution schedule

You can configure this policy to run at regular intervals (for example, once every day) using the policy scheduler provided with API Gateway. Under the **Environment Configuration** > **Listeners** node, right-click the API Gateway instance node, and select **Add policy execution scheduler**. The following example runs the policy at 12 noon every day:

![Configuring a Policy Schedule](/Images/docbook/images/certs/validate_certs_schedule.gif)

#### Example email alert

An email alert is sent if any certificates that are due to expire are detected. The contents of the email are obtained from the `expired.certs.summary` message attribute. For example:

```
Axway API Gateway running on Roadrunner contains certificates that will expire in 730 days.
2 expired certificates in API Gateway certificate store:
1. Cert details:
Cert issued to: CN=CA
Cert issued by: CN=CA
SHA1 fingerprint: 72:04:35:7C:A1:B1:C2:F5:E2:86:75:C4:83:12:9C:70:A8:D6:21:8E
MD5 fingerprint: 82:23:6F:59:F2:8F:C3:95:56:87:70:B5:51:3F:53:05
Subject Key Identifier (SKI): dfABenFoM0r7iJ3E1ZqU7HmKiyY=
Expires on: 2012-04-20
2. Cert details:
Cert issued to: CN=John Doe
Cert issued by: CN=CA
SHA1 fingerprint: 83:32:EB:3F:9C:15:87:FB:81:E1:D5:AC:CC:35:C3:F8:21:BB:DF:CD
MD5 fingerprint: 48:02:F6:3F:B9:64:EB:DA:DF:CF:F9:82:AC:CC:13:AB
Subject Key Identifier (SKI): HabJNMjAsBAWp4AcCq8yZkTEJKQ=
Expires on: 2012-04-20
```

## Static CRL certificate validation filter

A Certificate Authority (CA) might wish to publish a Certificate Revocation List (CRL) to a file. In this case, API Gateway can load the revoked certificates from the file-based CRL and validate user certificates against it.

{{< alert title="Note" color="primary" >}}Because the CRL is typically signed by the CA that owns it, the CA certificate that issued the CRL *must*
first be imported into the certificate store. In addition, the **CRL (Static)**
filter requires the `certificates`
message attribute to be set by a preceding filter.{{< /alert >}}

### Example static CRL-based validation policy

Typically, a **Find Certificate** filter is first used to find the certificate, which is stored in a `certificate` message attribute. You can then use a **Copy / Modify Attributes** filter to copy the `certificate` attribute to the `certificates` attribute by selecting its **Create list attribute** setting.

The following example policy shows the filters used:

![Static CRL Policy](/Images/docbook/images/certs/static_crl_policy.gif)

The following example shows the settings used in the **Copy / Modify Attributes** filter:

![Copy / Modify Attributes Filter](/Images/docbook/images/certs/copy_modfiy_attributes_filter.gif)

{{< alert title="Note" color="primary" >}}
Typically, a CA publishes a new CRL, containing the most up-to-date list of revoked certificates at regular intervals. However, the **CRL (Static)**
filter does not automatically update the CRL when it is loaded from a local file. If you need to automatically retrieve updated CRLs from a particular URL, you should use the **CRL (Dynamic)**
filter instead. For more details, see [Dynamic CRL certificate validation](#dynamic-crl-certificate-validation-filter).
{{< /alert >}}

### Configure static CRL validation

Enter a name for the filter to display in a policy in the **Name** field.

Click the **Load CRL** button to browse to the location of the CRL file. When the CRL has been loaded from the selected location, read-only information regarding revoked certificates and update dates is displayed in the other fields on the window.

## Dynamic CRL certificate validation filter

This filter enables validation of certificates against a Certificate Revocation List (CRL) that has been published by a Certificate Authority (CA). The CRL is retrieved from the specified URL and is cached by the server for certificate validation. The filter automatically fetches a potentially updated CRL from this URL when the criteria specified in the **Automatic CRL Update Preferences** section are met.

{{< alert title="Note" color="primary" >}}Because the CRL is typically signed by the CA that owns it, the CA certificate that issued the CRL *must*
first be imported into the certificate store. In addition, the **CRL (Dynamic)**
filter requires the `certificates`
message attribute to be set by a preceding filter.{{< /alert >}}

### Example dynamic CRL-based validation policy

The topic on the **CRL (Static)** filter shows a typical CRL-based policy that first uses a **Find Certificate** filter to find the certificate, which is stored in a `certificate` message attribute. It then uses a **Copy/Modify Attributes** filter to copy this `certificate` attribute to the `certificates` attribute by selecting its **Create list attribute** setting. This `certificates` attribute is then used by the CRL-based filter.

You can use the same approach with the **CRL (Dynamic)** filter instead of the **CRL (Static)** filter. For more details, see [Static CRL certificate validation](#static-crl-certificate-validation-filter).

### Configure dynamic CRL validation

Configure the following fields on the **CRL (Dynamic)** window:

**Name**: Enter an appropriate name for the filter to display in a policy.

**CRL Import URL**: Enter the full URL of the CRL to use to validate the certificate, or browse to the CRL location.

**Automatic CRL Update Preferences**: Typically, a CA publishes an updated CRL at regular intervals. You can configure the filter to dynamically pull the latest CRL published by the CA at specified intervals. Select one of the following update options:

* **Do not update**: The filter never attempts to automatically retrieve the latest CRL.
* **Update on "next update" date**: The CRL published by the CA contains a *Next Update*   date, which indicates the next date on which the CA publishes the CRL. You can choose to dynamically retrieve the updated CRL on the Next Update date by selecting this option. This effectively synchronizes the server with the CA updates.
* **Update every number of days**: The filter retrieves the CRL every number of days specified.
* **Trigger update on cron expression**: You can enter a cron expression to determine when to perform the automatic update.

## CRL LDAP validation filter

A Certificate Revocation List (CRL) is a signed list indicating a set of certificates that are no longer considered valid (revoked certificates) by the certificate issuer. API Gateway can query a CRL to find out if a given certificate has been revoked. If the certificate is present in the CRL, it should not be trusted.

To validate a certificate using a CRL lookup, the certificate's issuing CA certificate should be trusted by API Gateway. This is because for a CRL lookup, the CA public key is needed to verify the signature on the CRL. The issuing CA public key is not always included in the certificates that it issues, so it is necessary to retrieve it from API Gateway's certificate store instead.

The **Name** and **URL** of all currently configured LDAP directories are displayed in the table on the **CRL Certificate Validation** window. API Gateway checks the CRL of all selected LDAP directories to validate the client certificate. The filter fails as soon as API Gateway determines that one of the CRLs has revoked the certificate.

To configure LDAP connection information, complete the following fields:

**Name**: Enter an appropriate name for the filter to display in a policy.

**LDAP Connection**: Click the button on the right, and select the LDAP directory to check its CRL. To use an existing LDAP directory, (for example, `Sample Active Directory Connection`), you can select it in the tree. To add an LDAP directory, right-click the **LDAP Connections** tree node, and select **Add an LDAP Connection**.

Alternatively, you can add LDAP connections under the **Environment Configuration** > **External Connections** node in the Policy Studio tree.

## CRL responder filter

This filter enables API Gateway to behave as Certificate Revocation List (CRL) responder, which returns CRLs to clients. This filter imports the CRL from a specified URL. You can also configure it to periodically retrieve the CRL from this URL to ensure that it always has the latest version.

Configure the following fields on the **CRL Responder** window:

**Name**: Enter an appropriate name for the filter to display in a policy.

**CRL Import URL**: Enter the full URL of the CRL that you want to return to clients. Alternatively, browse to the location of the CRL file by clicking the browse button on the right.

**Automatic CRL Update Preferences**: Because keeping up-to-date with the latest list of revoked certificates is crucial in any *trust network* , it is important that you configure the filter to retrieve the latest version of the CRL on a regular basis. The following automatic update options are available:

* **Do not update**: The CRL is not automatically updated.
* **Update on "next update" date**: The CRL published by the CA contains a *Next Update*   date, which indicates the next date on which the CA publishes the CRL. You can choose to dynamically retrieve the updated CRL on the Next Update date by selecting this option. This effectively synchronizes the server with the CA updates.
* **Update every number of days**: The CRL is updated after the specified number of days has elapsed (for example, every `3` days).
* **Trigger update on cron expression**: You can enter a cron expression to determine when to perform the automatic update.

## XKMS certificate validation filter

XML Key Management Specification (XKMS) is an XML-based protocol that enables you to establish the trustworthiness of a certificate over the Internet. API Gateway can query an XKMS responder to determine whether a given certificate can be trusted.

Configure the following fields:

**Name**: Enter an appropriate name for this XKMS filter to display in a policy.

**XKMS Connection**: Click the button on the right, and select an XKMS connection in the tree. To add an XKMS connection, right-click the **XKMS Connections** node, and select **Add an XKMS Connection**. Alternatively, you can configure an XKMS connection under the **Environment Configuration** > **External Connections** node in the Policy Studio tree.

## Find certificate filter

The **Find Certificate** filter locates a certificate and sets it in the message for use by other certificate-based filters. Certificates can be extracted from several different locations. For example, this includes the certificate store, user store, selector expressions, HTTP headers, and message attachments.

By default, API Gateway stores the extracted certificate in the `certificate` message attribute. However, it can store the certificate in any message attribute, including any arbitrary attribute (for example, `user_certificate`). The certificate can be extracted from this attribute by a successor filter in the policy.

Complete the following fields:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Attribute Name**: Enter or select the name of the message attribute to store the extracted certificate in. When the target message attribute has been selected, the next step is to specify the location of the certificate from one of the following options. Defaults to `certificate`.

**Certificate Store**: Click **Select**, and select a certificate from the certificate store.

**User**: This field provides an alternative way to specify the user certificate. You can enter an explicitly named user in the API Gateway user store, or you can enter a selector expression that evaluates to a string that contains the name of a user in the user store. For example:

```
${authentication.subject.id}
```

You can also associate a certificate with this user using the **Signing Key** option in the user configuration. In this way, the specified user name is used to retrieve the certificate associated with that user.

**Selector Expression**: You can specify a selector expression by enclosing the message attribute name that contains the certificate in curly brackets, and prefixing this with `$` (for example, `${certificate}`). Using a selector is a more flexible way of locating certificates than specifying the user directly.

This selector expression must return an X.509 certificate. Selectors used in other fields return a string that is then used to look up the certificate (for example, in the certificate store, user store, HTTP header, or attachment).

**HTTP Header Name**: Enter the name of the HTTP header that contains the certificate. Alternatively, you can enter a selector expression that evaluates to a string that contains the HTTP header name, and whose value is the certificate.

**Attachment Name**: Enter the name of the attachment (`Content-Id`) that contains the certificate. Alternatively, you can enter a selector expression that evaluates to a string that contains the ID of the attachment that encapsulates the certificate.

**Certificate Alias**: Enter the alias name of the certificate. Alternatively, you can enter a selector expression that evaluates to a string that contains the certificate alias in the certificate store.

This certificate alias refers to a certificate in the API Gateway's trusted certificate store, and not to a certificate in the Java keystore.

## Certificate chain check filter

It is a trivial task for a user to generate a X.509 certificate and use it to negotiate mutually authenticated connections to publicly available services. Allowing every user to generate their own certificate and use it on the Internet is a risk scenario, and should not be allowed. To address this, API Gateway can perform a *certificate chain check* on the client certificate to establish its authenticity by ensuring that the certificate was originated from a trusted source.

The main purpose of the certificate chain validation is to ensure that a certificate has been issued by a trusted source. Typically, in a Public Key Infrastructure (PKI), a Certificate Authority (CA) is responsible for issuing and distributing certificates. This infrastructure is based on the premise of *transitive trust* - if everybody trusts the CA, everybody transitively trusts the certificates issued by that CA. If entities only trust certificates that have been issued by the CA, they can reject certificates that have been self-generated by clients.

When a CA issues a certificate, it digitally signs the certificate and inserts a copy of its own signature into it. Whenever an application, such as API Gateway, receives a client certificate, it can extract the signature of the issuing CA certificate from it, and run a certificate chain check to determine whether the signature was created by a trusted CA. If the application trusts the CA, it also trusts any client certificate with a valid signature from that CA. This process goes on recursively with each certificate's issuer, until a trusted root CA, which lists the same CN name as both subject and issuer, is encountered. This list of certificates, from self-signed root CA to the leaf certificate, is a certificate chain and all valid certificate chains end with a root CA.

API Gateway maintains a repository of trusted CA certificates, which is known as the certificate store. To *trust* a specific CA, that CA certificate must be imported into the certificate store.

You can configure the following settings on the **Certificate Chain Check** window:

**Name**: Enter an appropriate name for this filter to display in a policy.

**Certificates Message Attribute**: You can specify a message attribute that contains the certificate or certificates to check. The message attribute type can be an `X509Certificate` object, or an `ArrayList` of `X509Certificate` objects.

**Distinguished Name**: This table lists the Distinguished Names of the certificates currently in the certificate store. Select the check box beside a CA to enable this filter to consider it as *trusted* when performing the certificate chain check. You can select multiple CAs in the table.

## Certificate validity filter

The validity period of an X.509 certificate is encoded in the certificate. The **Certificate Validity** filter performs a simple check on a certificate to ensure that it has not expired.

By default, the **Certificate Validity** filter searches for the X.509 certificate in the `certificate` message attribute, which must be set by a predecessor filter in the policy (for example, by an **SSL Authentication** filter).

Configure the following fields on the **Certificate Validity** window:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Certificate Selector Expression**: Enter the selector expression that specifies where to obtain the certificate (for example, from a message attribute). The filter checks the validity of the specified certificate. If no certificate is found, the filter returns an error. Defaults to `${certificate}`.

Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store (KPS), or environment variable).

## Create thumbprint from certificate filter

The **Create Thumbprint** filter can be used to create a human-readable thumbprint (or fingerprint) from the X.509 certificate that is stored in the `certificate` message attribute. The generated thumbprint is stored in the `certificate.thumbprint` attribute.

Configure the following fields on this filter:

**Name**: Enter a name for this filter to display in a policy.

**Digest Algorithm**: Select the digest algorithm to create the thumbprint of the certificate from the list.
