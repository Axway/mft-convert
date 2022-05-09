{
"title": "Authentication filters",
  "linkTitle": "Authentication filters",
  "weight": 30,
  "date": "2019-10-17",
  "description": "Commonly used authentication filters, including API key, HTTP basic, and SAML."
}

## API key authentication filter

API keys are supplied by client users and applications calling REST APIs to track and control how the APIs are used (for example, to meter access and prevent abuse or malicious attack). The **Authenticate API Key**
filter enables you to securely authenticate an API key with the API Gateway.

API keys include a key ID that identifies the client responsible for the API service request. This key ID is not a secret, and must be included in each request. API keys can also include a confidential secret key used for authentication, which should only be known to the client and to the API service. You can use the **Authenticate API Key**
filter to specify where to find the API key ID and secret key in the request message, and to specify timestamp and expiry options.

An example use case for this filter would be a client accessing a REST API service to invoke specific methods, for example, `startVM()`
or `stopVM()`. To invoke these methods, you are required to provide your API key ID and secret key to the API Gateway. You can keep the secret key private by sending the request over HTTPS.

Alternatively, you can use the secret key to generate an HMAC digital signature. This means that the secret key is not sent in the request, but is inferred instead, because the message must have been signed using the required secret key. When the API service receives the request, it uses the API key ID to look up the corresponding secret key, and uses it to validate the signature and confirm the request sender.

The API Gateway supports the following API key types:

* Simple API keys including a key ID only. The API key ID is included in all requests to authenticate the client.
* Amazon Web Services style API keys including a key ID and a secret key, which are used together to securely authenticate the client. The API key ID is included in all requests to identify the client. The secret key is known only to the client and the API Gateway.

For more details on authenticating Amazon Web Services API keys, see [Authenticating REST Requests](http://s3.amazonaws.com/doc/s3-developer-guide/RESTAuthentication.html).

Configure the following general settings:

**Name**:
Enter a suitable name for this filter in your policy.

**KPS Alias**:
Enter the alias name of the Key Property Store (KPS) used to store the API keys. Defaults to the example `ClientRegistry`
supplied with the API Gateway.

**Field Containing Secret**:
Enter the name of the field in the KPS that contains the secret. Defaults to `secretKey`.

### Configure API key settings

Configure the following fields on the **API Key**
tab:

**Where to find API key**:
To specify where to find the API key in the request message, select one of the following options:

* **API key is located in**:
    Select one of the following from the list:
    * `Query String`
    * `Header`
    * `Parameter`
    The default option is `Query String`. Enter the name in the text box. Defaults to `KeyId`.
* **API key is in Authorization header with format**:
    Select one of the following Authorization headers from the list:
    * `Amazon AWS s3 Authorization Header - "AWS apiKey + ":" + base64(signature)"`
    * `HTTP Basic Authentication Header - "Basic base64(apiKey:secret)"`
    Defaults to the `Amazon AWS s3 Authorization Header`.
* **API key can be found using the following selector**:\
    Enter the selector value that specifies the location of the API key. Defaults to `${http.client.getCgiArgument("KeyId")}`.

**Where to find Secret key**:
To specify where to find the secret key in the request message, select the **Extract Secret**
setting, and select one of the following options:

* **Secret key is in**:
    Select one of the following from the list:
    * `Query String`
    * `Header`
    * `Parameter`
    The default option is `Query String`. Enter the name in the text box. Defaults to `SecretKey`.
* **Secret key is in Authorization header with format**:
    Select the Authorization header from the list. Defaults to `HTTP Basic Authentication Header - "Basic base64(apiKey:secret)"`.
* **Secret key can be inferred from signature**:
    The client can use the secret key to generate a digital signature that is included in the request. When the API Gateway receives the request, it uses the API key ID to identify the client and look up the corresponding secret key in the Axway Client Application Registry. The secret key is then used to validate the signature and authenticate the client. To specify the signature format, select one of the following from the list:
    * `Amazon AWS s3 Authorization Header Authentication - "AWS apiKey + ":" + base64(signature)"`
    * `Amazon AWS s3 REST Authentication - "?Signature=<base64(signature)> &Expires=<seconds since epoch>&AWSAccessKeyId=<aws-id>"`
    Defaults to `Amazon AWS s3 Authorization Header Authentication`.
* **Secret key can be found using the following selector**:
    Enter the selector value that specifies the location of the secret key. Defaults to `${http.client.getCgiArgument("SecretKey")}`.

**Authenticate API key and secret**:
Select whether to authenticate both the API key ID and the secret key. This means that the client must supply the API key ID and the secret key in the request message. This setting is selected by default.

### Configure advanced API key settings

Configure the following fields on the **Advanced**
tab:

**Validate Timestamp**:
Select whether to validate the API key timestamp using the settings specified below. This setting is unselected by default.

**Timestamp is located in**:
To specify where the timestamp is located in the request message, select one of the following from the list:

* `Header`
* `Parameter`
* `Query String`

The default option is `Header`. Enter the name in the text box. Defaults to `Date`.

**Timestamp format is**:
To specify the timestamp format, select one of the following from the list:

* `Simple Date Format`
* `Milliseconds since epoch`
* `Seconds since epoch`

The default option is `Simple Date Format`. Enter the format in the text box. Defaults to `EEE, dd MMM yyyy HH:mm:ss zzz`.

**Timestamp Drift +/-**:
You can specify a drift time in milliseconds to allow differences in the clock times between the machine on which the API key was generated and the machine on which the API Gateway is running. Defaults to +-`60000`
milliseconds (one minute).

**Validate Expires**:
Select whether to validate the API key expiry details using the settings specified below. This setting is unselected by default.

**Expires is located in**:
To specify the location of the expiry details in the request message, select one of the following from the list:

* `Query String`
* `Header`
* `Parameter`

The default option is `Query String`. Enter the name in the text box. Defaults to `Expires`.

**Expires format is**:
To specify the format of the expiry details, select one of the following from the list:

* `Milliseconds since epoch`
* `Seconds since epoch`
* `Simple Date Format`

The default option is `Milliseconds since epoch`. Enter the format in the text box.

**Timestamp Drift +/-**:
You can specify a drift time in milliseconds to allow differences in the clock times between the machine on which the API key was generated and the machine on which the API Gateway is running. Defaults to `60000`
milliseconds (one minute).

## HTTP Basic authentication filter

In HTTP Basic authentication, a client authenticate using an `Authorization` header. When you include a **HTTP Basic Authentication** filter in a policy, API Gateway can use the `Authorization` header to authenticate the client against a user profile stored in an authentication repository.

HTTP Basic authentication has two approaches:

* **Challenge-response handshake**: This is typical of situations where a browser is talking to a web server, for example, when a user uses the browser to access a web resource. The browser does not know that the server requires HTTP Basic authentication, so the initial request to the server does not include an `Authorization` header. The server responds with an `HTTP 401` response code, instructing the browser to authenticate by sending the `Authorization` header. The browser displays a dialog to the user to enter a user name and password combination, and passes the provided credentials to the `Authorization` header. The browser then sends a second request including the `Authorization` header to the server for authentication.
* **Direct authentication**: This is used mainly for machine-to-machine transactions with no human intervention. The client sends up the `Authorization` HTTP Basic authentication header in its first request to the server, so the challenge-response handshake is omitted.

Before being passed to the `Authorization` header, the credentials are concatenated, base64-encoded, for example:

```
Authorization:Basic dm9yZGVsOnZvcmRlbA==
```

The realm presented in the challenge for HTTP Basic authentication is the realm currently specified in **Environment Configuration > Server Settings > General**.

### Configure general HTTP basic settings

When you configure the **HTTP Basic Authentication** filter, you specify where API Gateway finds the user profiles for authentication. API Gateway can look up user profiles in a local repository on API Gateway, a database, or an LDAP directory.

Configure the following settings:

**Credential Format**:
This defines the format of the user name presented to API Gateway during the HTTP Basic handshake that API Gateway uses for authorization checks against third-party Identity Management servers. API Gateway has no way of telling one format from another, so you must specify the format of the credential the client presents. The most common formats are user name or Distinguished Name (DName).

**Allow client challenge**:
This option is selected by default, and the client performs the HTTP Basic authentication challenge-response handshake with API Gateway. If you deselect this option, the client always sends the `Authorization` header to API Gateway for direct authentication.

**Allow retries**:
If you select this option, the user can try to enter their user name and password again if, for example, the authentication fails or has not yet been provided. The number of retries is controlled by the browser (usually three times).

**Remove HTTP authentication header**:
Select this option to remove the `Authorization` header from the downstream message. If this option is not selected, the incoming `Authorization` header is forwarded to the destination web service.

**Repository Name**:
Select the authentication repository where the user profiles are stored. This can be a local repository, a database, or an LDAP directory.

{{< alert title="Note" color="primary" >}}If you want to authenticate against an LDAP repository, you must set the authentication type of the LDAP connection to `Simple` . HTTP Basic authentication is not compatible with the other LDAP authentication types (`None`,`External` or `Digest-MD5`).{{< /alert >}}

### Configure invalid attempts

The settings for invalid attempts specify how to handle invalid attempts: how many times authentication can be attempted, and what happens after this limit is crossed. By default, the number of invalid attempts is not limited.

You can select to lock user accounts, ban IP addresses, or both, and define how long that restriction lasts. In addition, you can define how many invalid attempts can be made and over what time period.

To ban IP addresses, you must also define the attribute that contains the IP address in the **Key is** field.

**Store invalid attempt information in cache**:
To store information on invalid attempts in cache, select this option, and choose the cache (local or distributed) you want to use, or add a new one. For more details on caches, see [Configure caching](/docs/apim_policydev/apigw_poldev/general_cache/).

#### Restrictions when using the same invalid attempts cache in multiple filters

You can configure a policy including more than one HTTP authentication filters (**HTTP Basic Authentication**, **HTTP Digest Authentication**, or **HTML Form based Authentication**), for example, to allow users to authenticate with either HTTP Basic or HTTP digest authentication. If the filters in your policy use the same cache for invalid attempts, you must use the same invalid attempts settings in all the filters sharing a cache. This ensures that if an user breaches the invalid attempts threshold, the lockout behavior and the user experience remains consistent.

## SAML authentication filter

A Security Assertion Markup Language (SAML) *authentication assertion*
is issued as proof of an authentication event. Typically, an end user authenticates to an intermediary, who generates a SAML authentication assertion to prove that it has authenticated the user. The intermediary inserts the assertion into the message for consumption by a downstream web service.

When the API Gateway receives a message containing a SAML authentication assertion, it does not attempt to authenticate the end user again. Instead, it authenticates the sender of the assertion (the intermediary) to ensure that only the intermediary could have issued the assertion, and then validates the authentication details contained in the assertion. Therefore, the API Gateway performs the following tasks in this scenario:

* Authenticates the sender of the message (the intermediary)
* Extracts the end user's identity from the authentication assertion and validates the authentication details

The **SAML Authentication**
filter performs the second task. A separate authentication filter must be placed before this filter in the policy to authenticate the sender of the assertion. The end user's identity is used in any subsequent authorization filters.

The following sample SOAP message contains a SAML authentication assertion:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
<soap-env:Header xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/04/secext">
<wsse:Security>
  <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
    AssertionID="axway-1056477425082"
    Id="axway-1056477425082"
    IssueInstant="2003-06-24T17:57:05Z"
    Issuer="CN=Sample User,....,C=IE"
    MajorVersion="1"
    MinorVersion="0">
    <saml:Conditions
      NotBefore="2003-06-20T16:20:10Z"
      NotOnOrAfter="2003-06-20T18:20:10Z"/>
      <saml:AuthenticationStatement
        AuthenticationInstant="2003-06-24T17:57:05Z"
        AuthenticationMethod="urn:oasis:names:tc:SAML:1.0:am:password">
        <saml:SubjectLocality IPAddress="192.168.0.32"/>
        <saml:Subject>
          <saml:NameIdentifier
            Format="urn:oasis:names:tc:SAML:1.0:assertion#X509SubjectName">
            sample
          </saml:NameIdentifier>
        </saml:Subject>
      </saml:AuthenticationStatement>
      <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#"
        id="Sample User">
       <dsig:SignedInfo>
         .....
       </dsig:SignedInfo>
       <dsig:SignatureValue>
         rpa/......0g==
       </dsig:SignatureValue>
       <dsig:KeyInfo>
         .....
       </dsig:KeyInfo>
     </dsig:Signature>
  </saml:Assertion>
</wsse:Security>
</soap-env:Header>
<soap-env:Body>
  <ns1:getTime xmlns:ns1="urn:timeservice">
  </ns1:getTime>
</soap-env:Body>
</soap-env:Envelope>
```

### Configure SAML authentication details settings

Configure the following fields on the **Details**
tab:

**SOAP Actor/Role**:
If you expect the SAML assertion to be embedded in a WS-Security block, you can identify this block by specifying the SOAP Actor or Role of the WS-Security header that contains the assertion.

**XPath Expression**:
Alternatively, if the assertion is not contained in a WS-Security block, you can enter an XPath expression to locate the authentication assertion. You can configure XPath expressions using the **Add**, **Edit**,
and **Delete**
buttons.

**SAML Namespace**:
Select the SAML namespace that must be used on the SAML assertion for this filter to succeed. If you do not wish to check the namespace, select the `Do not check version`
option from the list.

**SAML Version**:
Specify the SAML version that the assertion must adhere to by entering the major version in the first field, and the minor version in the second field. For example, for SAML 2.0, enter `2`
in the first field and `0`
in the second field.

**Drift Time**:
The *drift time*, specified in seconds, is used when checking the validity dates on the authentication assertion. The drift time allows for differences between the clock times of the machine on which the assertion was generated and the machine hosting the API Gateway.

**Remove enclosing WS-Security element on successful validation**:
Select this check box to remove the WS-Security block that contains the SAML assertion after the assertion has been successfully validated.

### Configure trusted issuer settings

You can use the table on the **Trusted Issuers**
tab to select the issuers that you consider trusted. In other words, this filter only accepts assertions that have been issued by the SAML authorities selected here.

Click the **Add**
button to display the **Trusted Issuers**
window. Select the Distinguished Name of a SAML authority whose certificate has been added to the certificate store, and click **OK**. Repeat this step to add more SAML authorities to the list of trusted issuers.

## WS-Security UsernameToken authentication filter

A WS-Security *UsernameToken*
enables an end user identity to be passed over multiple hops before reaching the destination web service. The user identity is inserted into the message and is available for processing at each hop on its path.

The client user name and password are encapsulated in a WS-Security `<wsse:UsernameToken>`. When the API Gateway receives this token, it can perform one of the following tasks, depending on the requirements:

* Ensure that the timestamp on the token is still valid
* Authenticate the user name against a repository
* Authenticate the user name and password against a repository

The following sample SOAP message contains two `<wsse:UsernameToken>`
blocks:

```xml
<?xml version="1.0" encoding="iso-8859-1"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
<soap:Header>
    <wsse:Security xmlns:wsse="http://schemas.xmlsoap.org/ws/2003/06/secext">
        <wsse:UsernameToken wsu:Id="sample"
          xmlns:wsu="http://schemas.xmlsoap.org/ws/2003/06/utility">
            <wsse:Username>sample</wsse:Username>
            <wsse:Password Type="wsse:PasswordText">axway</wsse:Password>
            <wsu:Created>2004-05-19T08:44:51Z</wsu:Created>
        </wsse:UsernameToken>
    </wsse:Security>
    <wsse:Security soap:actor="axway"
      xmlns:wsse="http://schemas.xmlsoap.org/ws/2003/06/secext">
        <wsse:UsernameToken wsu:Id="axway"
          xmlns:wsu="http://schemas.xmlsoap.org/ws/2003/06/utility">
            <wsse:Username>axway</wsse:Username>
            <wsse:Password Type="wsse:PasswordText">axway</wsse:Password>
            <wsu:Created>2004-05-19T08:46:04Z</wsu:Created>
        </wsse:UsernameToken>
    </wsse:Security>
</soap:Header>
<soap:Body>
    <getHello xmlns="http://www.axway.com"/>
</soap:Body>
</soap:Envelope>
```

### Configure actor details

**Actor**:
The example SOAP message at the top of this page contains two `<wsse:UsernameToken>`
blocks. You must specify which block contains the `<wsse:UsernameToken>`
used to authenticate the end user. Specify the SOAP Actor/Role of the WS-Security block that contains the token.

**Credential Format**:
The API Gateway can authenticate users against a user profile repository based on User Names, X.509 Distinguished Names, or email addresses. Unfortunately, the WS-Security specification does not provide a means of specifying the type of `<wsse:UsernameToken>`, and so it is necessary for the administrator to do so using the **Credential Format**
field. The type specified here is used internally by the API Gateway in subsequent authorization filters.

### Configure token validation

Each `wsse:UsernameToken`
contains a timestamp inserted into the `<wsu:Created>`
element. Using this timestamp together with the details entered in this section, the API Gateway can determine whether the WS-Security `UsernameToken`
has expired. The `<wsu:Created>`
element is as follows:

```xml
<wsse:UsernameToken wsu:Id="axway"
  xmlns:wsu="http://schemas.xmlsoap.org/ws/2003/06/utility">
    <wsu:Created>2006.01.13T-10:42:43Z</wsu:Created>
        ...
</wsse:UsernameToken>
```

To configure token validation settings, complete the following fields:

**Drift Time**:
Specified in seconds to account for differences in the clock times between the machine on which the token was generated and the machine running the API Gateway. Using the *start time*, *end time*, and *drift time*, the token is considered valid if the current time falls between the following times:

```
[start - drift] and [start + drift + end]
```

**Validity Period**:
Specifies the lifetime of the token, where the value of the `<wsu:Created>`
element represents the *start time*
of the assertion, and the time period entered represents the *end time*.

**Timestamp Required**:
Select this option to ensure that the UsernameToken contains a timestamp. If no timestamp is found in the UsernameToken, a SOAP Fault is returned.

### Configure nonce settings

**Nonce Required**:
Select this option to ensure that the UsernameToken contains a `<wsse:Nonce>`
element. This is a randomly generated number that is added to the message. You can use the combination of a timestamp and a nonce to help prevent replay attacks.

**Select cache to store WSS UsernameToken nonces in**:
Click the button on the right, and select the cache that stores the nonce value. Defaults to the local `WSS UsernameToken Nonce Cache`.

To add a cache, right-click the **Caches**
tree node, and select **Add Local Cache**
or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration** > **Libraries**
node in the Policy Studio tree.

### Configure token validation via repository

Having validated the timestamp on the token, the API Gateway can then optionally authenticate the user name and password contained in the token. The following options are available:

* **No Verification**
    No verification of the user name and password is performed. Only the timestamp on the token is validated. This is the default behavior.
* **Verify Username Only**
    Only the user name is looked up in the selected repository. If the user name is found in this repository, the user is authenticated. Select the **No password allowed**
    check box to block messages that contain a UsernameToken with a `<wsse:Password>`
    element.
* **Verify Username and Password**
    The user name is looked up in the selected repository and is only authenticated if the corresponding password matches the one configured in the repository. If you select this option, you must select the type of the password. Both clear text and digest formats are supported. Select the appropriate option.

**Repository Name**:
The API Gateway attempts to authenticate users against the selected **Authentication Repository**. User profiles can be stored in the local store, a database, or an LDAP directory.

### Configure advanced WS-Security settings

**Remove enclosing WS-Security element on successful validation**:
Select this option to remove the WS-Security block that contains the UsernameToken after the token has been successfully authenticated. For example, in the above sample SOAP message that contains two `<wsse:UsernameToken>`
elements in two different WS-Security blocks, you could configure the API Gateway to remove one of these on successful authentication.

## HTML form-based authentication filter

HTML form-based authentication enables users to supply their user name and password details in an HTML form, and submit them to login to a system. Using HTML form-based authentication, normal HTTP authentication features such as HTTP basic or HTTP digest are not used. Instead, the user name and password are typically sent as HTML `<FORM>`
data in an HTTP `POST`
over SSL.

When the **HTML Form based Authentication**
filter is configured, the API Gateway can authenticate the user details specified in the HTML form against a user profile stored in the API Gateway local repository, a database, or an LDAP directory. The **HTML Form based Authentication**
filter also enables you to specify how HTTP sessions are managed (for example, session expiry, and applicable API Gateway domain or relative path).

{{< alert title="Tip" color="primary" >}}For an alternative approach to HTTP session management, which also includes the ability to check or to end sessions, see [Create session](#create-session-filter). {{< /alert >}}

### Configure general HTML form settings

These settings enable you to configure general details such as the names of the HTML form fields, format of user credentials, and repository to validate credentials against. Complete the following settings:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Username**:
Enter the name of the HTML form field in which the user enters their user name. Defaults to `username`.

**Password**:
Enter the name of the HTML form field in which the user enters their password. Defaults to `password`.

**Format of Authentication Credentials**:
You must specify the format of the user credentials presented by the client because the API Gateway has no way of telling one credential format from another. Select one of the following from the list:

* `User Name`
* `X.509 Distinguished Name`

The selected format is then used internally by the API Gateway when performing authorization lookups against third-party Identity Management servers.

**Repository Name**:
This specifies the name of the authentication repository where all user profiles are stored. This can be in the API Gateway's local repository, a database, or an LDAP directory. Select a preconfigured repository from the list (for example, `Local User Store`).

You can add a new repository by right-clicking the appropriate node under **Environment Configuration** > **External Connections**
> **Authentication Repository Profiles**
(for example, **Database Repositories**), and selecting **Add a new Repository**.

### Configure session settings

The settings on the **Session** tab enable you to configure how HTTP sessions between the HTML form client and the API Gateway are managed. Complete the following settings:

**Create a session**:
Select whether to create an HTTP session. This setting is selected by default.

**Expiry of session in milliseconds**:
Enter the period of time in milliseconds before the session expires. Defaults to `600000`
(10 minutes).

**Session cookie**:
Enter the name of the cookie used to manage the session. The default is `VIDUSR`.

**Session applicable for this domain**:
Enter the API Gateway domain name to which the session applies (for example, `dmz`).

**Session applicable for this path**:
Enter the API Gateway relative path to which the session applies. Defaults to `/`.

**Session sent over SSL only**:
Select whether the session is sent over an SSL connection only. This setting is not selected by default.

**HTTP Only cookie**:
Select the check box to add a HTTPOnly flag to the session cookie. This is not selected by default.

### Configure invalid login attempts settings

The settings on the **Invalid Login Attempts**
tab enable you to specify how to handle invalid login attempts. You can choose to lock user accounts, ban IP addresses, or both, if a specified number of invalid attempts are made in a specified time period. The invalid attempt information is also stored in a cache.

{{< alert title="Note" color="primary" >}}If you are using two or more instances of HTTP basic, HTTP digest, or HTML form-based authentication filters in the same policy, and they share the same invalid attempts cache, you must use the same invalid attempts settings on each of the filters.{{< /alert >}}

For more details on the settings on this tab, see [Invalid attempts](#configure-invalid-attempts).

## IP address authentication filter

You can configure the API Gateway to allow or deny machines, or groups of machines, access to resources based on their IP addresses. The main table on the window shows the IP addresses from which the API Gateway accepts or denies messages depending on what is configured.

The **IP Address**
authentication filter uses the value stored in the `http.request.clientaddr`
message attribute to determine whether to allow or deny access. This message attribute contains the remote host address from the TCP socket used in the connection between the client and the API Gateway.

Configure the following fields:

**Name**:
Enter a name for the filter to display in a policy.

**IP Addresses**:
You can add IP addresses by clicking the **Add**
button, which displays the **Add IP Filter**
dialog. Enter an **IP Address**
and **Subnet Mask**
to indicate a network to filter.

Messages sent from hosts belonging to this network are accepted or rejected based on what is configured in the section below. A **Subnet Mask**
of `255.255.255.255`
can be used to filter specific IP addresses.

{{< alert title="Note" color="primary" >}}If requests are made across a proxy, portal, or other such intermediary, the API Gateway filters on the IP address of the intermediary. Therefore, you should enter the IP address of the intermediary on this window, and not that of the user or client machine.{{< /alert >}}

You can edit and remove existing IP addresses by selecting the **Edit**
and **Remove**
buttons.

**Access**:
Depending on whether the **Allow Access**
or **Deny Access**
radio button is checked, the IP addresses listed in the table are allowed or denied access to the web service.

### Configure subnet masks

An IP address is normally represented by a string of four numbers separated by periods (for example, `192.168.0.20`). Each number is normally represented as the decimal equivalent of an 8-bit binary number, which means that each number can take any value between 0 (all 8 bits cleared) and 255 (all 8 bits set).

A *subnet mask* (or netmask) is also a set of four number blocks separated by periods, each of which has a value in the range 0-255. Every IP address consists of two parts: the network address and the host number. The netmask is used to determine the size of these two parts. The positions of the bits set in the netmask represent the space reserved for the network address, while the bits that are cleared represent the space reserved for the host number. The netmask determines the range of IP addresses.

The following examples illustrate how netmasks work in practice.

#### Example – Specify a range of IP addresses

To allow requests from the following IP addresses:

`192.168.0.16`, `192.168.0.17`, `192.168.0.18`, and `192.168.0.19`.

Use the following address and netmask combination:

`192.168.0.16/255.255.255.252`

In more detail, the binary representation of the netmask is as follows:

`11111111.11111111.11111111.11111100`

The top 30 bits of the netmask indicate the network and the last 2 bits refer to the host on the network. These last 2 bits allow 4 different addresses as shown in the worked example below.

When the API Gateway receives a request from a certain IP address, the API Gateway performs a logical AND on the client IP address and the configured netmask. It also does a logical AND with the IP address entered in the IP Address filter and the configured subnet mask. If the AND-ed binary values are the same, the request from the IP address can be considered in the same network range as that configured in the filter.

The following worked example illustrates the mechanics of the IP address filtering. It assumes that you have entered the following in the IP Address and Netmask fields in the IP Address filter:

| Field          | Value             |
|----------------|-------------------|
| **IP Address** | `192.168.0.16`    |
| **Net Mask**   | `255.255.255.252` |

```
Step 1: AND the IP address and Netmask configured in the IP Address Filter:
11000000.10100000.00000000.00010000 (192.168.0.16)
AND
11111111.11111111.11111111.11111100 (255.255.255.252)
=========================================
11000000.10100000.00000000.00010000
Step 2: Request is received from 192.168.0.18:
11000000.10100000.00000000.00010010 (192.168.0.18)
AND
11111111.11111111.11111111.11111100 (255.255.255.252)
=========================================
11000000.10100000.00000000.00010000
===> AND-ed value is equal to the result for 192.168.0.16.
===> Therefore the client IP address is inside the configured range.
Step 3: Request is received from 192.168.0.20:
11000000.10100000.00000000.00010100 (192.168.0.20)
AND
11111111.11111111.11111111.11111100 (255.255.255.252)
=========================================
11000000.10100000.00000000.00010100
===> AND-ed value is NOT equal to the result for 192.168.0.16.
===> Therefore the client IP address is NOT inside the configured range.
```

#### Example – Specify an exact IP address

You can also specify an exact IP address by using a netmask of`255.255.255.255`. When this netmask is used, only requests from this client IP address is allowed or blocked, depending on what is configured in the filter. This example assumes that the following details have been configured in the IP Address filter:

| Field          | Value             |
|----------------|-------------------|
| **IP Address** | `192.168.0.36`    |
| **Net Mask**   | `255.255.255.255` |

```
Step 1: AND the IP address and Netmask configured in the IP Address Filter:
11000000.10100000.00000000.00100100 (192.168.0.36)
AND
11111111.11111111.11111111.11111111 (255.255.255.255)
=========================================
11000000.10100000.00000000.00100100
Step 2: Request is received from client with IP address of 192.168.0.37:
11000000.10100000.00000000.00100101 (192.168.0.37)
AND
11111111.11111111.11111111.11111111 (255.255.255.255)
=========================================
11000000.10100000.00000000.00100101
===> AND-ed value is NOT equal to the result for 192.168.0.36
===> Therefore the client IP address is NOT inside the configured range.
```

## SSL authentication filter

A client can mutually authenticate to the API Gateway through the exchange of X.509 certificates. An X.509 certificate contains identity information about its owner and is digitally signed by the Certificate Authority (CA) that issued it.

A client presents such a certificate to the API Gateway while the initial SSL/TLS session is being negotiated, in other words, during the *SSL handshake*. The **SSL Authentication**
filter extracts this information from the client certificate and sets it as message attributes. These attributes can then be used by subsequent filters in the policy.

The **SSL Authentication**
filter can be used as a decision-making node on the policy. For example, it can be used to determine a path through a policy based on how users authenticate to the API Gateway.

## STS client authentication filter

The **Security Token Service Client**
filter enables the API Gateway to act as a client to a Security Token Service (STS). An STS is a third-party web service that authenticates clients by validating credentials and issuing security tokens across different formats (for example, SAML, Kerberos, or X.509). The API Gateway can use the **Security Token Service Client**
filter to request security tokens from an STS using WS-Trust. WS-Trust specifies the protocol for issuing, exchanging, and validating security tokens.

An STS has its own security requirements for authenticating and authorizing requests for tokens. This means that the API Gateway might need to insert tokens, digitally sign, and encrypt the request that it sends to the STS for the required token. Because the STS is exposed as a web service, it should have a WSDL file with WS-Policies that describe its security requirements.

For example, the API Gateway can use the **Security Token Service Client**
filter to request tokens that it cannot issue itself, and which might be required by an endpoint service. The endpoint service might require tokens to be signed by a particular authority (STS), or there might be a requirement for a token that contains a key encrypted for the endpoint service, and which only the STS can generate. You can also use the **Security Token Service Client**
filter to virtualize an STS using the API Gateway.

### Example request

Using WS-Trust, requests for tokens are placed in a `RequestSecurityToken`
(RST) element in the SOAP `Body`
element. The STS returns the requested token in a `RequestSecurityTokenResponse`
(RSTR) element in the SOAP `Body`. The following example is an extract from a token request message sent from the API Gateway to the STS:

```xml
<soap:Body
  xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/
  oasis-200401-wss-wssecurity-utility-1.0.xsd"
  wsu:Id="Id-0000012e71431904-00000000011d5641-19">
    <wst:RequestSecurityToken
      xmlns:wst="http://docs.oasis-open.org/ws-sx/ws-trust/200512"
      Context="Id-0000012e71431904-00000000011d5641-15">
        <wst:RequestType>
          http://docs.oasis-open.org/ws-sx/ws-trust/200512/Issue
        </wst:RequestType>
        <wst:TokenType>
          http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1
        </wst:TokenType>
        <wst:KeyType>
          http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey
        </wst:KeyType>
        <wst:Entropy>
            <wst:BinarySecret
              Type="http://schemas.xmlsoap.org/ws/2005/02/trust/SymmetricKey">
              WLQmo5mRYiBRqq2D7677Dg==
            </wst:BinarySecret>
        </wst:Entropy>
        <wsp:AppliesTo
          xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
            <wsa:EndpointReference
              xmlns:wsa="http://www.w3.org/2005/08/addressing">
                <wsa:Address>default</wsa:Address>
            </wsa:EndpointReference>
        </wsp:AppliesTo>
    </wst:RequestSecurityToken>
</soap:Body>
```

In this simple example, the client (API Gateway) requests a SAML token with a symmetric `KeyType`. The SAML token is requested for an endpoint service named `default`. An optional `OnBehalfOf`
token is not supplied.

### Configure request settings

Configure the following general request settings on the **Request**
tab:

**Request Type**:
Select one of the following request types:

* **Issue**: A request to issue a token. This is the default request type.
* **Validate**: A request to validate a token.

**Token Type to Request**:
Select the token type to request from the STS (for example, `SAML 1.0`, `SAML 1.1`, `SAML 2.0`, or `UsernameToken`). You can also request a custom token type by entering the custom token URI (for example, `http://www.mycustomtoken.com/EmailToken`). The default is `SAML 1.1`.

#### Issue: POP Key

A *proof-of-possession*
(POP) security token contains secret data used to demonstrate authorized use of an associated security token. Typically, the POP data is encrypted with a key known only to the recipient of the POP token. For **Issue**
requests, you can configure the following POP key settings on the **Issue: POP Key**
tab:

**Proof of Possession Key Type**:
Select the POP key type for the token you are requesting. This only applies to certain types of tokens (for example, SAML tokens). Select one of the following key types from the list:

* `SymmetricKey`: When a SAML token is requested with a symmetric POP key, the SAML assertion returned by the STS has a subject confirmation type of `holder-of-key`. The subject confirmation data contains a symmetric key encrypted for the endpoint service. The API Gateway (the client) can request the SAML token from the STS with the endpoint service specified as the token scope, so the STS knows what certificate to use to encrypt the symmetric key it places in the SAML assertion’s subject confirmation data.

    The API Gateway cannot decrypt the symmetric key in the SAML assertion because it is encrypted for the endpoint service. The STS passes the symmetric key to the requesting API Gateway in the RSTR so that the API Gateway also has the symmetric key. It can then use the SAML assertion (symmetric key) to sign the message to the endpoint service, proving that it holds the key in the SAML assertion. The endpoint service can verify the signature because it can decrypt the key in the SAML assertion. This is the default POP key type.
* `PublicKey`: When a SAML token is requested with a public asymmetric POP key, the SAML assertion returned by the STS has a subject confirmation type of `holder-of-key`. The subject confirmation data contains a public key or certificate. The API Gateway (the client) can also use this SAML assertion to sign messages to the endpoint service using the related private key, thus proving they hold the key referenced in the SAML assertion. The public key in the SAML assertion is not encrypted because it is not sensitive data.

    This SAML assertion can be used to sign messages to multiple endpoint services because it does not contain a key encrypted for a specific service. The API Gateway can specify the public key used in the **Public Proof of Possession Key** settings. This public key can be associated with a certificate in the certificate store, or generated on-the-fly using the **Generate Key** filter.
* `Bearer`: When a SAML token is requested with a bearer POP key, the SAML assertion returned by the STS has a subject confirmation type of `bearer`. In this case, the SAML token does not contain a POP key.

{{< alert title="Note" color="primary" >}}An STS can also generate a SAML token with a subject confirmation type of `sender-vouches`. In this case, the endpoint service trusts the client directly, the SAML assertion does not need to be signed by the STS. The client signs the SAML assertion and the SOAP `Body`
before sending the message to the endpoint service. This type of SAML assertion does not map to a value for **Proof of Possession Key Type**, but can be returned from the STS if no key type is specified. {{< /alert >}}

**Key Size**:
Enter the key size in bits to indicate the desired strength of the security. Defaults to `256`
bits.

**Entropy Type**:
If the **Proof of Possession Key Type**
requested is a `SymmetricKey`, you must specify an **Entropy Type**. If the API Gateway provides *entropy*, this means that it provides some of the binary material used to generate the symmetric key. In general, both the API Gateway and the STS provide some entropy for the symmetric key (a computed key). However, either side can also fully generate the symmetric key. Select one of the following options:

* **None**: The API Gateway does not provide any entropy, so the STS must fully generate the symmetric key.
* **Binary Secret**: The API Gateway provides entropy in the form of a Base64-encoded binary secret (or key). You must specify a **Binary Secret Type**. For details, see the next setting.
* **Encrypted Key**: The API Gateway provides entropy in the form of an `EncryptedKey`
    element. You must configure an **XML-Encryption**
    filter in the policy, which applies security before creating the WS-Trust message. This filter generates a symmetric key and encrypts it, but does not encrypt any data. The key must be encrypted with the STS certificate.

**Binary Secret Type**:
If the **Entropy Type**
is **Binary Secret**, you must specify a **Binary Secret Type**. Select one of the following:

* Nonce: The API Gateway generates a nonce value and places it in the RST.
* SymmetricKey: The **Binary Secret Message Attribute**
    value must be specified. In this case, this is the name of the message attribute that contains the symmetric key passed to the STS to be used as entropy for generating the POP symmetric key. The type of this message attribute must be `byte[]`
    when the **Binary Secret Type**
    is `SymmetricKey`.
* AsymmetricKey: The **Binary Secret Message Attribute**
    value must be specified. In this case, this is the name of the message attribute that contains the private asymmetric key passed to the STS to be used as entropy for generating the POP symmetric key. The type of this message attribute must be `byte[]`, `PrivateKey`, `KeyPair`, or `X509Certificate`
    when the **Binary Secret Type**
    is `AsymmetricKey`. In each case, the private key is used.

**Binary Secret Message Attribute**:
Enter or select the message attribute that contains the binary secret. This setting is required when the **Binary Secret Type**
is `SymmetricKey`
or `AsymmetricKey`.

**Computed Key Algorithm**:
When both the API Gateway and STS provide entropy values for the symmetric POP key, you can specify a computed key algorithm (for example, `PSHA1`). This is used when the key resulting from the token request is not directly returned, and is computed.

**Public Proof of Possession Key**:
If the **Proof of Possession Key Type**
requested is a `PublicKey`, you can specify what public key to include in the token using the following settings:

* **Use Key Format**: Select how the `UseKey`
    element in the RST formats the public key from the list (for example, `PublicKey`, `Certificate`, `BinarySecurityToken`, and so on).
* **Use Key Selector Expression**: Select or enter the selector expression that contains the public key. The public key can be of type `X509Certificate`, `PublicKey`, or `KeyPair`.

#### Issue: On Behalf Of Token

For **Issue**
requests, you can optionally configure the `OnBehalfOf`
token for the RST. If an `OnBehalfOf`
token is in the RST, this means you are requesting a token on behalf of the subject identified by the token or endpoint reference in the `OnBehalfOf`
element. You can configure the following settings on the **Issue: On Behalf Of Token**
tab:

**On Behalf Of**:
Select one of the following options:

* **None**: No `OnBehalfOf`
    token is specified. This is the default.
* **Token**: The token is embedded directly under the `<OnBehalfOf>`
    element in the RST.
* **EmbeddedSTR**: The token is placed in the `<OnBehalfOf><SecurityTokenReference><Embedded>`
    element in the RST.
* **Endpoint Reference**: A reference to the token is placed in the `<OnBehalfOf><SecurityTokenReference>`
    element. The token is placed in the WS-Security header.

**On Behalf Of Token Selector Expression**:
Enter the selector expression for the message attribute that contains the `OnBehalfOf`
token. This can be a `UsernameToken`, SAML token, X.509 certificate, and so on. The type of this message attribute can be `Node`, `List`
of Nodes, `String`, or `X509Certificate`. This message attribute must be populated using a filter configured in the policy that applies security before creating the WS-Trust message. For example, this includes a filter to extract a `UsernameToken`
from the incoming message, or a **Find Certificate**
filter.

**Endpoint Address**:
When the **On Behalf Of**
type is **Endpoint Reference**, no token is placed in the `OnBehalfOf`
element. Instead, you can enter an endpoint address in this field that identifies the subject on whose behalf you are requesting the token.

**Identity Type**:
When the **On Behalf Of**
type is **Endpoint Reference**, you can select an identity type from the list (for example, `DNSName`, `ServicePrincipaName`, or `UserPrincipalName`).

**Identity**:
When the **Identity Type**
is set to `DNSName`, `ServicePrincipaName`, or `UserPrincipalName`, you must specify a value in this field.

**Identity Selector Expression**:
When the selected **Identity Type**
is one of `PublicKey`, `Certificate`, `BinarySecurityToken`, `SecurityTokenReference_x509v3`, or `SecurityTokenReference_ThumbprintSHA1`, you must specify a selector expression in this field. This specifies the name of the message attribute that contains the certificate for the subject on whose behalf you are requesting the token. The type of this message attribute must be `X509Certificate`.

#### Issue: Token Scope and Lifetime

For **Issue**
requests, you can optionally specify details for the scope of the requested token (for example, the endpoint service this token is used for). These details are placed in the `AppliesTo`
element of the RST. You can configure the following settings on the **Issue: Token Scope and Lifetime**
tab:

**Endpoint Address**:
Enter an address for the endpoint.

**Identity Type**:
Select an identity type from the list (for example, `Certificate`, `BinarySecurityToken``DNSName`, `ServicePrincipalName`, or `UserPrincipalName`).

**Identity**:
When the **Identity Type**
is set to `DNSName`, `ServicePrincipaName`, or `UserPrincipalName`, you must specify a value in this field.

**Identity Selector Expression**:
When the **Identity Type**
selected is one of `PublicKey`, `Certificate`, `BinarySecurityToken`, `SecurityTokenReference_x509v3`, or `SecurityTokenReference_ThumbprintSHA1`, you must specify a selector expression in this field. This specifies the name of the message attribute that contains the certificate for the endpoint service that the token is sent to. The type of this message attribute must be `X509Certificate`.

**Expires In**:
Specify when the token is due to expire in the fields provided.

**Lifetime Format**:
Enter the date and time format in which the token lifetime is specified. Defaults to `yyyy-MM-dd'T'HH:mm:ss.SSS'Z'`.

{{< alert title="Note" color="primary" >}}The STS can choose to ignore the token lifetime specified in the RST.{{< /alert >}}

#### Validate:Target

If the request type is set to **Validate**, you can use the **Validate:Target**
tab to specify the token that you require the STS to validate. In this case, the STS does not issue a token. It validates the token passed to it in the RST and returns a status. The STS response is placed in the `sts.validate.code`
and `sts.validate.reason`
message attributes.

You can configure the following settings on the **Validate:Target**
tab:

**Token**:
Specifies that the token is placed directly under the `<ValidateTarget>`
element in the RST.

**EmbeddedSTR**:
Specifies that the token is placed in the `<ValidateTarget><SecurityTokenReference><Embedded>`
element.

**STR**:
Specifies that a reference to the token is placed in the `<ValidateTarget><SecurityTokenReference>`
element. The token is placed in the WS-Security header.

**Validate Target Selector Expression**:
Enter a selector expression for the message attribute that contains the token to validate. The attribute type can be `Node`, a `List`
of Nodes, or `String`. This message attribute must be populated using a filter configured in the policy that applies security before creating the WS-Trust message. For example, you can run a filter to extract a SAML token from the incoming message.

### Configure policies settings

The **Policies**
tab enables you to specify the policies that the **Security Token Service Client**
filter delegates to. You can configure the following settings on this tab by clicking the button next to each field:

**Policy apply security before creating the WS-Trust message**:
Specifies the policy that runs before the **Security Token Service Client**
filter creates the RST (the WS-Trust request message for the STS). The filters in this policy are used to set up message attribute values that the STS client filter requires (for example, the `OnBehalfOf`
token).

**Policy to apply security to the WS-Trust request**:
Specifies the policy that runs after the **Security Token Service Client**
filter has created the RST. The filters in this policy can sign and/or encrypt the message as required by the STS. It can also inject other security tokens into the WS-Security header if required.

**Policy to apply security to the WS-Trust response**:
Specifies the policy that runs to apply security to the WS-Trust response. This policy runs when the response is received from the STS. The filters in this policy can decrypt and verify signatures on the response message.

### Configure routing settings

When routing to an STS, you can specify a direct connection to the web service endpoint by entering a URL on the **Routing**
tab. Alternatively, when the routing behavior is more complex, you can delegate to a custom routing policy to handle the added complexity. The options on the **Routing**
tab allow for these alternative routing configurations.

**Use the following URL**:
Select this option to route to the specified URL. You can enter the URL in the text box, or specify the URL as a selector so that the URL is built dynamically at runtime from the specified message attributes (for example `${host}:${port}`, or `${http.destination.protocol}://${http.destination.host}:${http.destination.port}`).

You can configure SSL settings, credential profiles for authentication, and other settings for the direct connection using the tabs in the **Connection Details**
group. For more details, see [Connect to URL filter](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter).

**Delegate routing to the following policy**:
Select this option to use a dedicated routing policy to send messages on to the STS. Click the browse button next to the **Routing policy**
field to select the policy to use to route messages.

**No routing**:
Select this option to only allow request reflection for test purposes.

### Configure response settings

The **Response**
tab enables you to specify options for processing the response message from the STS. You can configure the following settings on this tab:

**Verify returned security token type**:
When selected, the filter checks that the `TokenType`
returned is what was requested. This is selected by default.

**Put security token into message attribute**:
When specified, the token returned from the STS is placed in the specified message attribute. The type of this attribute is `String`. Defaults to `sts.security.token`. An element version of the token is placed in a message attribute named `attrname.element`.

**Insert security token into original message in SOAP Actor/Role**:
When specified, the token returned from the STS is inserted into the original message. This is the original message received by the API Gateway (was the current message before the **Security Token Service Client**
filter ran). Defaults to `Current actor/role only`.

**Extract Token Lifetime**:
When selected, the token lifetime is extracted from the response, and the `sts.token.lifetime.created`
and `sts.token.lifetime.expires`
message attributes are populated. This setting is selected by default.

### Configure advanced STS client settings

The **Advanced**
tab enables you to specify the following options:

**Versions and Namespaces**:
The version and namespace options are as follows:

* **WS-Trust Version**: Specifies the WS-Trust namespace to use in the generated RST. Defaults to `WS-Trust 1.3`.
* **SOAP version**: Specifies the SOAP version to use in the generated RST. Defaults to `SOAP 1.1`.
* **WS-Addressing Namespace**: Specifies the WS-Addressing namespace to use in the generated RST. Defaults to `http://www.w3.org/2005/08/addressing`.
* **WS-Policy Namespace**: Specifies the WS-Policy namespace to use in the generated RST. Defaults to `WS-Policy 1.2`.
* **WS-Security Actor**: Specifies the actor in which to place tokens that are referred to from the RST using STRs (for example, `OnBehalfOf`). Defaults to `Current actor/role only`.

**Algorithms**:
The algorithm options are as follows:

* **Canonicalization Algorithm**: When selected, additional elements are added to the RST, which specify a client-requested canonicalization algorithm (for example, `ExC14n`).
* **Encryption Algorithm**: When selected, additional elements are added to the RST, which specify a client-requested encryption algorithm (for example, `Aes256`).
* **Encrypt with**: When selected, specifies the encryption algorithm with which to encrypt the RSTR (for example, `Aes256`).
* **Sign with**: When selected, specifies the signature algorithm with which to digitally sign the RSTR (for example, `RsaSha256`).

**Advanced Settings**:
The advanced options are as follows:

**Content-Type**: Specifies the `Content-Type`
of the message to be sent to the STS. For example, for Microsoft Windows Communication Foundation (WCF), select `application/soap+xml`. Defaults to `text/xml`.

**Store and restore original message**: When selected, the original message is saved before messages sent from the API Gateway to the STS and messages sent from the STS to the API Gateway are processed. It is then reinstated after this filter finishes processing the STS response. This is the default behavior.

For debug purposes, you might wish to return the STS response from your policy. In this case, deselect this setting, and the current message after this filter completes should then be the STS response. You might also wish to debug the RST (the request to the STS), and return that from your policy. In this case, disable this setting, click the **Routing**
tab, and select the **No routing**
option.

## Kerberos service authentication filter

The API Gateway can act as a Kerberos service to consume Kerberos tokens sent from a client in the HTTP header or in the message itself. The Kerberos client must have obtained a ticket from the Ticket Granting Server (TGS) for this Kerberos service. The service ticket makes up part of the Kerberos client-side token that is sent to the Kerberos service. If the service can validate the token, the client is authenticated successfully.

For more details on different Kerberos setups with API Gateway, see [API Gateway Kerberos Integration Guide](/docs/apigtw_kerberos/).

To add a **Kerberos Service** filter, open the **Authentication** category, and drag the filter onto the policy canvas. The following sections describe how to configure the different fields of this filter.

Configure the following fields:

**Kerberos Service**:
The **Kerberos Service**
you select in this field is responsible for consuming the Kerberos client's Kerberos token. The Kerberos client must have obtained a ticket for the Kerberos service's principal name to be able to authenticate to the Kerberos service.

Click the **...** button, and select a previously configured Kerberos service. To add a Kerberos service, right-click **Kerberos Services**, and select **Add Kerberos Service**. You can also add Kerberos services under **Environment Configuration > External Connections** in the node tree.

### Configure Kerberos standard settings

Configure the following fields on the **Kerberos Standard**
tab:

**Kerberos Standard**:
Select one of the following Kerberos standards:

* Kerberos Token Profile
* WS-Trust for SPNEGO
* SPNEGO over HTTP

{{< alert title="Note" color="primary" >}}The Kerberos Service filter consumes the Kerberos client-side tokens regardless of whether the token is sent at the message layer in the SOAP message, or at the transport layer in an HTTP header. {{< /alert >}}

**Client Token Location for Message-Level Standards**:
The Kerberos service ticket can be sent in the `Authorization`
HTTP header, or inside the message itself (for example, inside a `<BinarySecurityToken>` element). Alternatively, the Kerberos token can be in a message attribute.

Select one of the following options:

* **Message Body**: Select this option if you expect the Kerberos service ticket to be contained in the message. You must enter an XPath expression to point to the expected location of the Kerberos token. You can select some default expressions that point to common locations from the list. To add a new XPath expression, click **Add**. You can also edit or delete existing expressions, if needed.
* **Selector Expression**: When using the **WS-Trust for SPNEGO**
    standard, the **Consume WS-Trust**
    filter places the client-side Kerberos token inside the `ws.trust.spnego.token`
    message attribute.

### Configure message level settings

You can configure settings adhering to the message-level standards (for example, Kerberos Token Profile and WS-Trust for SPNEGO) on the **Message Level**
tab.

**Extract Session Keys**:
You must select this option to use the Kerberos/SPNEGO session keys to sign, encrypt, or decrypt a message in a subsequent filter. This option is only available when the token is extracted from the message body.

**Key Length**:
When using **WS-Trust for SPNEGO** standard, the **Kerberos Service**
filter generates a new symmetric key and wraps it using the Kerberos session key. This setting determines the length of the new symmetric key.

**Cache Security Context Session Key**:
The service-side policy might need to cache the session key in order to process (decrypt and verify) multiple requests from a Kerberos client. Use this field to select a cache for the session key.

### Configure transport level settings

The options on the **Transport Level**
tab are specific to Kerberos tokens received over HTTP, and are only relevant if you selected to use **SPNEGO Over HTTP**
standard.

**Cookie Name**:
The initial handshake between a Kerberos client and a Kerberos service can sometimes involve the exchange of a series of request and responses until the secure context has been established. In such cases, you can use a HTTP cookie to keep track of the context across multiple request and response messages. Enter the name of the cookie in the text field.

**Allow Client Challenge**:
In some cases, a Kerberos client might not authenticate (send the `Authorization`
HTTP header) to the Kerberos service on first request. The Kerberos service then responds with an HTTP 401 response code, instructing the client to authenticate to the server by sending the `Authorization`
header. The Kerberos client sends a second request, this time with the `Authorization`
header containing the relevant Kerberos token. Select this option to allow this kind of negotiation between the Kerberos client and service.

**Client Sends Body Only After Context is Established**:
The Kerberos client might wait to mutually authenticate the Kerberos service before sending the body of the message. Select this option to enable the Kerberos service to accept the body after the context has been established if the Kerberos client provides the known cookie. The cookies are cached in the cache you configure.

### Configure advanced SPNEGO settings

The settings on the **Advanced SPNEGO**
tab apply only to the **WS-Trust for SPNEGO**
and **SPNEGO over HTTP**
standards.

**Cache Partially Established Contexts**:
In theory, a Kerberos client and a Kerberos service may need to send and receive a number of tokens between each other to authenticate. In this case, the **Kerberos Service**
filter must cache the partially established context for each Kerberos client. The contexts are only cached during the establishment of the context.

In practice, however, a single client-side Kerberos token is normally enough to establish a context on the service-side, making this setting redundant.

## HTTP header authentication filter

You can use the **HTTP Header**
filter in cases where the API Gateway receives end user authentication credentials in an HTTP header. A typical scenario would see the end user (or message originator) authenticating to an intermediary. The intermediary authenticates the end user and, to propagate the end-user credentials to the destination web service, the intermediary inserts the credentials into an HTTP header and forwards them onwards.

When the API Gateway receives the message, it performs the following tasks:

* Authenticates the sender of the message (the intermediary)
* Extracts the *end user* identity from the token in the HTTP header for use in subsequent authorization filters

{{< alert title="Note" color="primary" >}}In the case outlined above, the API Gateway does *not* attempt to reauthenticate the end user. It trusts that the intermediary has already authenticated the end user, and so the API Gateway does not authenticate the user again. However, it is good practice to authenticate the message sender (the intermediary). Any subsequent authorization filters use the end user credentials that were passed in the HTTP header.{{< /alert >}}

The following configuration fields are available on this window:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**HTTP Header Name**:
Enter the name of the HTTP header that contains the end user credentials.

**HTTP Header Type**:
Select the type of credentials that are passed in the named HTTP header. The following types are supported:

* X.509 Distinguished Name
* Certificate
* User Name

## Insert WS-Security UsernameToken filter

When a client has been successfully authenticated, the API Gateway can insert a *WS-Security UsernameToken*
into the downstream message as proof of the authentication event. The `<wsse:UsernameToken>`
token enables a user's identity to be inserted into the XML message so that it can be propagated over a chain of web services.

A typical example would see a user authenticating to the API Gateway using HTTP digest authentication. After successfully authenticating the user, the API Gateway inserts a WS-Security UsernameToken into the message and digitally signs it to prevent anyone from tampering with the token.

The following example shows the format of the `<wsse:UsernameToken>`
token:

```xml
<wsse:UsernameToken wsu:Id="axway"
  xmlns:wsu="http://schemas.xmlsoap.org/ws/2003/06/utility">
    <wsu:Created>2006.01.13T-10:42:43Z</wsu:Created>
    <wsse:Username>axway</wsse:Username>
    <wsse:Nonce EncodingType="UTF-8">
        KFIy9LgzhmDPNiqU/B9ZiWKXfEVNvFyn6KWYP+1zVt8=
    </wsse:Nonce>
    <wsse:Password Type="wsse:PasswordDigest">
        CxWj1OMnYj7dddMnU/DrOhyY3j4=
    </wsse:Password>
</wsse:UsernameToken>
```

To configure general settings, complete the following fields:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Actor**:
The UsernameToken is inserted into the WS-Security block identified by the specified SOAP *Actor*.

### Configure credential details

To configure the credential details, complete the following fields:

**Username**:
Enter the name of the user included in the UsernameToken. By default, the `authentication.subject.id`
message attribute is stored, which contains the name of an authenticated user.

**Include Nonce**:
Select this option to include a nonce in the UsernameToken. A nonce is a random number that is typically used to help prevent replay attacks.

**Include Password**:
Select this option if you wish to include a password in the UsernameToken.

**Password**:
If the **Include Password**
check box is selected, the API Gateway inserts the user's password into the generated WS-Security UsernameToken. It can insert a **Clear**
or **SHA1 Digest**
version of the password, depending on which radio button you select. Axway recommends the digest form of the password to avoid potential eavesdropping.

You can either explicitly enter the password for this user in the **Password**
field, or use a message attribute by selecting the **Wildcard**
option, and entering the message attribute selector in the field provided. The default is `${authentication.subject.password}`, which contains the user password to authenticate to the API Gateway.

#### Advanced options

To configure advanced options, complete the following field:

**Indent**:
Select this option to add indentation to the generated `UsernameToken`
and `Signature`
blocks. This makes the security tokens more human-readable.

## Create session filter

The **Create Session**
filter enables the API Gateway to create an HTTP session and configure various session attributes (for example, expiry, domain, and security). This filter requires an `authentication.subject.id`
attribute for the user, and stores it in the HTTP session. This session ID is used to create a cookie with a specified name, which is stored in the generated `http.session.cookie.name`
attribute. The cookie is then sent to the user specified by the `authentication.subject.id`
attribute.

The **Create Session**
filter should be used with the **Check Session**
and **End Session**
filters to manage HTTP sessions.

{{< alert title="Tip" color="primary" >}}The **Create Session**
filter offers a more flexible approach to managing HTTP sessions than using the **HTTP Form-Based Authentication**
filter. For example, the form-based approach does not include the ability to check or end sessions, and sessions are auto-renewed on each invocation of the filter. For more details, see [HTML form-based authentication](#html-form-based-authentication-filter).{{< /alert >}}

Complete the following fields to configure this filter:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**Expiration time of session in milliseconds**:
Enter the HTTP session expiry timeout in milliseconds. When the session reaches the specified lifetime, it is automatically invalidated, and can no longer pass the **Check Session**
filter.

**Session cookie**:
Enter the name of the HTTP cookie used for the session. This filter uses the HTTP session ID to create the cookie named by this field. The specified cookie name is output in the generated `http.session.cookie.name`
message attribute. Defaults to `VIDUSR`.

**Session cookie domain**:
Enter the domain value for the `Set-Cookie`
header (for example, `example.com`).This informs the browser that cookies should be sent back to the server for the specified domain only.

**Session cookie path**:
Enter the path value for the `Set-Cookie`
header (for example, `/sales`). This informs the browser that cookies should be sent back to the server for the specified path only. Defaults to `/`.

**Session sent over SSL only**:
Select whether the session uses SSL only. When selected, this adds a `Secure`
flag to the cookie.

**HTTP-only cookie**:
Select whether the session uses HTTP only. When selected, this adds an `HTTPOnly`
flag to the cookie.

## Insert SAML authentication assertion filter

After successfully authenticating a client, the API Gateway can insert a SAML (Security Assertion Markup Language) authentication assertion into the SOAP message. Assuming all other security filters in the policy are successful, the assertion is eventually consumed by a downstream web service.

You can refer to the following example of a signed SAML authentication assertion when configuring the **Insert SAML Authentication Assertion**
filter:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
<soap-env:Header xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/04/secext">
<wsse:Security>
  <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
    AssertionID="axway-1056477425082"
    Id="axway-1056477425082"
    IssueInstant="2003-06-24T17:57:05Z"
    Issuer="CN=Sample User,....,C=IE"
    MajorVersion="1"
    MinorVersion="0">
    <saml:Conditions
      NotBefore="2003-06-20T16:20:10Z"
      NotOnOrAfter="2003-06-20T18:20:10Z"/>
      <saml:AuthenticationStatement
        AuthenticationInstant="2003-06-24T17:57:05Z"
        AuthenticationMethod="urn:oasis:names:tc:SAML:1.0:am:password">
        <saml:SubjectLocality IPAddress="192.168.0.32"/>
        <saml:Subject>
          <saml:NameIdentifier
            Format="urn:oasis:names:tc:SAML:1.0:assertion#X509SubjectName">
            sample
          </saml:NameIdentifier>
        </saml:Subject>
      </saml:AuthenticationStatement>
      <dsig:Signature xmlns:dsig="http://www.w3.org/2000/09/xmldsig#"
        id="Sample User">
       <dsig:SignedInfo>
         .....
       </dsig:SignedInfo>
       <dsig:SignatureValue>
         rpa/......0g==
       </dsig:SignatureValue>
       <dsig:KeyInfo>
         .....
       </dsig:KeyInfo>
     </dsig:Signature>
  </saml:Assertion>
</wsse:Security>
</soap-env:Header>
<soap-env:Body>
  <ns1:getTime xmlns:ns1="urn:timeservice">
  </ns1:getTime>
</soap-env:Body>
</soap-env:Envelope>
```

### Configure assertion details settings

Configure the following fields on the **Assertion Details**
tab:

**Issuer Name**:
Select the certificate containing the Distinguished Name (DName) to use as the Issuer of the SAML assertion. This DName is included in the SAML assertion as the value of the `Issuer`
attribute of the `<saml:Assertion>`
element. For an example, see the sample SAML assertion above.

**Expire In**:
Specify the lifetime of the assertion in this field. The lifetime of the assertion lasts from the time of insertion until the specified amount of time has elapsed.

**Drift Time**:
The drift time
is used to account for differences in the clock times of the machine hosting the API Gateway (that generate the assertion) and the machines that consume the assertion. The specified time is subtracted from the time at which the API Gateway generates the assertion.

**SAML Version**:
You can create SAML 1.0, 1.1, and 2.0 attribute assertions. Select the appropriate version from the list.

{{< alert title="Note" color="primary" >}}
SAML 1.0 recommends the use of the `http://www.w3.org/TR/2001/REC-xml-c14n-20010315`
XML Signature Canonicalization algorithm. When inserting signed SAML 1.0 assertions into XML documents, it is quite likely that subsequent signature verification of these assertions will fail. This is due to the side effect of the algorithm including inherited namespaces into canonical XML calculations of the inserted SAML assertion that were not present when the assertion was generated.

For this reason, Axway recommend that SAML 1.1 or 2.0 is used when signing assertions as they both use the exclusive canonical algorithm `http://www.w3.org/2001/10/xml-exc-c14n#`, which safeguards inserted assertions from such changes of context in the XML document. See section 5.4.2 of the `oasis-sstc-saml-core-1.0.pdf`
and section 5.4.2 of `sstc-saml-core-1.1.pdf`
documents, both of which are available at [http://www.oasis-open.org](http://www.oasis-open.org/).
{{< /alert >}}

### Configure assertion location settings

The options on the **Assertion Location**
tab specify where the SAML assertion is inserted in the message. By default, the SAML assertion is added to the WS-Security block with the current SOAP actor/role. The following options are available:

**Append to Root or SOAP Header**:
Appends the SAML assertion to the message root for a non-SOAP XML message, or to the SOAP Header for a SOAP message. For example, this option may be suitable for cases where this filter may process SOAP XML messages or non-SOAP XML messages.

**Add to WS-Security Block with SOAP Actor/Role**:
Adds the SAML assertion to the WS-Security block with the specified SOAP actor (SOAP 1.0) or role (SOAP 1.1). By default, the assertion is added with the current SOAP actor/role only, which means the WS-Security block with no actor. You can select a specific SOAP actor/role when available from the list.

**XPath Location**:
To insert the SAML assertion at an arbitrary location in the message, you can use an XPath expression to specify the exact location in the message. You can select XPath expressions from the list. The default is the `First WSSE Security Element`, which has an XPath expression of `//wsse:Security`. You can add, edit, or remove expressions by clicking the relevant button.

You can also specify how exactly the SAML assertion is inserted using the following options:

* **Append to node returned by XPath expression** (the default)
* **Insert before node returned by XPath expression**
* **Replace node returned by XPath expression**

**Insert into Message Attribute**:
Specify a message attribute to store the SAML assertion from the drop-down list (for example, `saml.assertion`). Alternatively, you can also enter a custom message attribute in this field (for example, `my.test.assertion`). The SAML assertion can then be accessed downstream in the policy.

### Configure subject confirmation method settings

The settings on the **Subject Confirmation Method**
tab determine how the `<SubjectConfirmation>`
block of the SAML assertion is generated. When the assertion is consumed by a downstream web service, the information contained in the `<SubjectConfirmation>`
block can be used to authenticate the end user that authenticated to the API Gateway, or the issuer of the assertion, depending on what is configured.

The following is a typical `<SubjectConfirmation>`
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

The following configuration fields are available on the **Subject Confirmation Method**
tab:

**Method**:
The selected value determines the value of the `<ConfirmationMethod>`
element. The following table shows the available methods, their meanings, and their respective values in the `<ConfirmationMethod>`
element:

* Holder Of Key: The API Gateway includes the key used to prove that the API Gateway is the holder of the key, or it includes a reference to the key. Value: `urn:oasis:names:tc:SAML:1.0:cm:holder-of-key`
* Bearer: The subject of the assertion is the bearer of the assertion. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`
* SAML Artifact: The subject of the assertion is the user that presented a SAML Artifact to the API Gateway. Value: `urn:oasis:names:tc:SAML:1.0:cm:artifact`
* Sender Vouches: Use this confirmation method to assert that the API Gateway is acting on behalf of the authenticated end user. No other information relating to the context of the assertion is sent. It is recommended that both the assertion and the SOAP Body must be signed if this option is selected. These message parts can be signed by using the **XML Signature Generation** filter. Value: `urn:oasis:names:tc:SAML:1.0:cm:bearer`

You can also leave the **Method** field blank, in which case no `<ConfirmationMethod>` block is inserted into the assertion.

**Holder-of-Key Configuration**:
When you select `Holder of Key`
as the SAML subject confirmation in the **Method**
field, you must configure how information about the key is included in the message. There are a number of configuration options available depending on whether the key is a symmetric or asymmetric key.

To use an asymmetric key as proof that the API Gateway is the holder-of-key entity, you must select the **Asymmetric Key**
radio button and then configure the following fields on the **Asymmetric**
tab:

* **Certificate from Store**:
    To select a key that is stored in the Certificate Store, select this option and click the **Signing Key**
    button. On the **Select Certificate**
    window, select the check box next to the certificate that is associated with the key to use.
* **Certificate from Selector Expression**:
    Alternatively, the key may have already been used by a previous filter in the policy (for example, to sign a part of the message). In this case, the key can be retrieved using the selector expression entered in this field. Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store, or environment variable).

To use a symmetric key as proof that the API Gateway is the holder of key, select the **Symmetric Key**
radio button, and configure the fields on the **Symmetric**
tab:

* **Generate Symmetric Key, and Save in Message Attribute**:
    If you select this option, the API Gateway generates a symmetric key, which is included in the message before it is sent to the client. By default, the key is saved in the `symmetric.key`
    message attribute.
* **Symmetric Key Selector Expression**:
    If a previous filter (for example, an **XML Signature Generation**
    filter) has already used a symmetric key, you can reuse this key as proof that the API Gateway is the holder-of-key entity. Enter the name of the selector expression (for example, message attribute) in the field provided, which defaults to `${symmetric.key}`. Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store, or environment variable).
* **Encrypt using Certificate from Certificate Store**:
    When a symmetric key is used, you must assume that the recipient has no prior knowledge of this key. It must, therefore, be included in the message so that the recipient can validate the key. To avoid meet-in-the-middle style attacks, where a hacker could eavesdrop on the communication channel between the API Gateway and the recipient and gain access to the symmetric key, the key must be encrypted so that only the recipient can decrypt the key.

    One way of doing this is to select the recipient's certificate from the Certificate Store. By encrypting the symmetric key with the public in the recipient's certificate, the key can only be decrypted by the recipient's private key, to which only the recipient has access. Select the **Signing Key**
    button, and select the recipient's certificate on the Select Certificate
    dialog.
* **Encrypt using Certificate from Selector Expression**:
    Alternatively, if the recipient's certificate has already been used (perhaps to encrypt part of the message), the certificate can be retrieved using the selector expression entered in this field. Using a selector enables settings to be evaluated and expanded at runtime based on metadata (for example, in a message attribute, Key Property Store, or environment variable).
* **Symmetric Key Length**:
    Enter the length (in bits) of the symmetric key to use.
* **Key Wrap Algorithm**:
    Select the algorithm to use to encrypt (*wrap*) the symmetric key.

The **Key Info**
tab must be configured regardless of whether you have elected to use symmetric or asymmetric keys. It determines how the key is included in the message. The following options are available:

* **Do Not Include Key Info**:
    Select this option if you do not wish to include a `<KeyInfo>`section in the SAML assertion.
* **Embed Public Key Information**:
    If this option is selected, details about the key are included in a `<KeyInfo>`
    block in the message. You can include the full certificate, expand the public key, include the distinguished name, and include a key name in the `<KeyInfo>`
    block by selecting the appropriate check boxes. When selecting the **Include Key Name**
    field, you must enter a name in the **Value**
    field, and then select the **Text Value**
    or **Distinguished Name Attribute**
    radio button, depending on the source of the key name.
* **Put Certificate in Attachment**:
    Select this option to add the certificate as an attachment to the message. The certificate is then referenced from the `<KeyInfo>`
    block.
* **Security Token Reference**:
    The Security Token Reference (STR) provides a way to refer to a key contained within a SOAP message from another part of the message. It is often used in cases where different security blocks in a message use the same key material and it is considered an overhead to include the key more than once in the message.\

    When this option is selected, a `<wsse:SecurityTokenReference>`
    element is inserted into the `<KeyInfo>`
    block. It references the key material using a `URI`
    to point to the key material and a `ValueType`
    attribute to indicate the type of reference used. For example, if the STR refers to an encrypted key, you should select `EncryptedKey`
    from the list, whereas if it refers to a `BinarySecurityToken`, you should select `X509v3`
    from the list. Other options are available to enable more specific security requirements.

### Configure advanced SAML settings

The settings on the **Advanced**
tab include the following fields.

**Select Required Layout Type**:
WS-Policy and SOAP Message Security define a set of rules that determine the layout of security elements that appear in the WS-Security header within a SOAP message. The SAML assertion is inserted into the WS-Security header according to the layout option selected here. The available options correspond to the WS-Policy Layout assertions of `Strict`, `Lax`, `LaxTimestampFirst`, and `LaxTimestampLast`.

**Insert SAML Attribute Statement**:
You can insert a SAML attribute statement into the generated SAML authentication assertion. If you select this option, a SAML attribute assertion is generated using attributes stored in the `attribute.lookup.list`
message attribute and subsequently inserted into the assertion. The `attribute.lookup.list`
attribute must have been populated previously by an attribute lookup filter for the attribute statement to be generated successfully.

**Indent**:
Select this method to ensure that the generated signature is properly indented.

**Security Token Reference**:
The generated SAML authentication assertion can be encapsulated within a `<SecurityTokenReference>`
block. The following example demonstrates this:

```xml
<soap:Header>
  <wsse:Security
    xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/12/secext"
    soap:actor="axway">
    <wsse:SecurityTokenReference>
      <wsse:Embedded>
        <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
          AssertionID="Id-00000109fee52b06-0000000000000012"
          IssueInstant="2006-03-15T17:12:45Z"
          Issuer="axway" MajorVersion="1" MinorVersion="0">
          <saml:Conditions NotBefore="2006-03-15T17:12:39Z"
            NotOnOrAfter="2006-03-25T17:12:39Z"/>
            <saml:AuthenticationStatement
              AuthenticationMethod="urn:oasis:names:tc:SAML:1.0:am:password"
              AuthenticationInstant="2006-03-15T17:12:45Z">
              <saml:Subject>
                <saml:NameIdentifier Format="Axway-Username-Password">
                  admin
                </saml:NameIdentifier>
                <saml:SubjectConfirmation>
                  <saml:ConfirmationMethod>
                    urn:oasis:names:tc:SAML:1.0:cm:artifact
                  </saml:ConfirmationMethod>
                </saml:SubjectConfirmation>
              </saml:Subject>
            </saml:AuthenticationStatement>
        </saml:Assertion>
      </wsse:Embedded>
    </wsse:SecurityTokenReference>
  </wsse:Security>
</soap:Header>
```

To add the SAML assertion to a `<SecurityTokenReference>`
block as in the example above, select the **Embed SAML assertion within Security Token Reference**
option. Otherwise, select **No Security Token Reference**.

## HTTP digest authentication filter

A client can authenticate to API Gateway with a user name and password digest using *HTTP digest authentication*. When an **HTTP Digest Authentication**
filter is configured, API Gateway requests the client to present a user name and password digest as part of the *HTTP digest challenge-response*
mechanism. API Gateway can then authenticate this user against a user profile stored in the API Gateway's local repository.

The realm presented in the challenge for HTTP digest authentication is the realm currently specified in **Environment Configuration** > **Server Settings > General**.

The **HTTP Digest Authentication**
filter enables you to specify where API Gateway can find user profiles for authentication purposes. API Gateway can look up user profiles in the API Gateway's local repository.

Complete the following settings:

**Name:**
Enter an appropriate name for the filter to display in a policy.

**Credential Format**:
The user name presented to API Gateway during the HTTP digest handshake can be of many formats, usually user name or Distinguished Name (DName). Because API Gateway has no way of inherently telling one format from another (for example, the client's user name could be a DName), you must specify the format of the credential presented by the client. This format is then used internally by API Gateway when performing authorization lookups against third-party Identity Management servers.

**Session Timeout**:
As part of the HTTP digest authentication protocol, API Gateway must generate a *nonce*
(number used once) value, and send it to the client. The client uses this nonce to create the digest of the user name and password. However, it should only be allowed a certain amount of time to do so. The **Session Timeout**
field specifies the length of time (in milliseconds) for which the nonce is valid.

**Allow retries**:
Select this option to allow the user to retry their user name and password in the browser when an HTTP 401 response code is received (for example, if authentication fails, or is not yet provided). The number of times that the browser displays the user name and password dialog when an HTTP 401 is received is controlled by the browser (usually three times). This setting is not selected by default.

**Remove HTTP authentication header:**
Select this option to remove the HTTP `Authorization`
header from the downstream message. If this option is not selected, the incoming `Authorization`
header is forwarded on to the destination web service.

**Repository Name**:
Select the name of the local authentication repository where all user profiles are stored.

You can add a new repository under the **Environment Configuration** > **External Connections**
node. Right-click the **Local Repositories**
node under **Authentication Repositories**, and select **Add a new repository**.

The **Invalid Attempts**
section enables you to specify how to handle invalid attempts. You can choose to lock user accounts, ban IP addresses, or both, if a specified number of invalid attempts are made in a specified time period. The invalid attempt information is also stored in a cache.

{{< alert title="Note" color="primary" >}}If you are using two or more instances of HTTP basic, HTTP digest, or HTML form-based authentication filters in the same policy, and they share the same invalid attempts cache, you must use the same invalid attempts settings on each of the filters.{{< /alert >}}

For more details on the fields in this section, see [Invalid attempts](#configure-invalid-attempts).

## Check session filter

The **Check Session**
filter checks for the presence of a valid cookie-based HTTP session. This filter tries to locate a valid session based on the value of a specified cookie name. If the session is found, the filter retrieves the user and sets it in the `authentication.subject.id`
attribute.

The **Check Session**
filter should be used with the **Create Session**
and **End Session**
filters to manage HTTP sessions.

Complete the following fields to configure this filter:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**Session cookie**:
Enter the name of the HTTP cookie used for the session. This filter tries to locate a valid session based on the value of the specified cookie name. The cookie name is output in the generated `http.session.cookie.name`
message attribute. Defaults to `VIDUSR`.

## End session filter

The **End Session**
filter terminates a cookie-based HTTP session. This filter tries to locate the session based on the value of a specified cookie name, and then invalidates it.

The **End Session**
filter should be used with the **Create Session**
and **Check Session**
filters to manage HTTP sessions.

Complete the following fields to configure this filter:

**Name**:
Enter an appropriate name for this filter to display in a policy.

**Remove session cookie**:
Select whether to try and remove the session cookie. When selected, the API Gateway sends a new cookie with the expiry time set in the past. You must also set the same domain and path values that were used to create the session using the **Create Session**
filter.

**Session cookie**:
Enter the name of the HTTP cookie used for the session. This filter tries to locate the session specified by this cookie name. This is output in the generated `http.session.cookie.name`
message attribute. Defaults to `VIDUSR`.

**Session cookie domain**:
When **Remove session cookie**
is selected, enter the same domain that was used to create the session in the **Create Session**
filter (for example, `example.com`). This removes the cookie for the specified domain only.

**Session cookie path**:
When **Remove session cookie**
is selected, enter the same path that was used to create the session in the **Create Session**
filter (for example, `/sales`). This removes the cookie for the specified path only. Defaults to `/`.

## Kerberos client authentication filter

You can configure the API Gateway to act as a Kerberos client and to obtain a service ticket for a specific Kerberos service. The service ticket makes up part of the Kerberos client-side token that is sent to the Kerberos service. If the service can validate the token, the client is authenticated successfully.

For more details on different Kerberos setups with API Gateway, see [API Gateway Kerberos Integration Guide](/docs/apigtw_kerberos/).

There are two filters you can use to configure the client-side transaction:

* Use a **Connection**
    filter to authenticate to a Kerberos service by inserting a client-side Kerberos token into the `Authorization`
    HTTP header.
* Use a **Kerberos Client**
    filter to send the client-side Kerberos token in a `BinarySecurityToken`
    block in the SOAP message.

To add a **Kerberos Client** filter, open the **Authentication** category, and drag the filter onto the policy canvas. The following sections describe how to configure the different fields of this filter.

### Configure Kerberos client settings

The fields configured on the **Kerberos Client**
tab determine how the Kerberos client obtains a service ticket for a specific Kerberos service.

Configure the following fields:

**Kerberos Client**:
The role of the Kerberos client selected in this field is twofold. First, it must obtain a Kerberos Ticket Granting Ticket (TGT) and second, it uses this TGT to obtain a service ticket for the selected **Kerberos Service Principal**. The TGT is acquired at server startup, server refresh (for example, when an update to configuration is deployed), and when the TGT expires.

To select which Kerberos client to use, click the **...** button, and select a previously configured Kerberos client. To add a Kerberos client, right-click **Kerberos Clients**, and select **Add Kerberos Client**. You can also add Kerberos clients under **Environment Configuration > External Connections** in the node tree.

**Kerberos Service Principal**:
The Kerberos client must obtain a service ticket from the Kerberos Ticket Granting Server (TGS) for the Kerberos service principal you set in this field. The TGS grants the Kerberos client a ticket for the selected principal, and the client can then send this ticket to the Kerberos service. The principal in the ticket must match the Kerberos service's principal for the client to be successfully authenticated.

The service principal name (SPN) can be used to uniquely identify the Kerberos service in the Kerberos realm.

To select which Kerberos service principal to use, click the **...** button on the right, and select a previously configured Kerberos principal in the tree (for example, the default `HTTP/host Service Principal`). To add a Kerberos principal, right-click **Kerberos Principals**, and select **Add Kerberos Principal**. You can also add Kerberos principals under **Environment Configuration > External Connections** in the node tree.

**Kerberos Standard**:
When using the **Kerberos Client**
filter to insert Kerberos tokens into SOAP messages, the Kerberos client can authenticate to Kerberos services using to two standards:

* **Web Services Security Kerberos Token Profile 1.1** – When using the Kerberos Token Profile, the client-side Kerberos token is inserted into a `BinarySecurityToken`
    block in the SOAP message. If you select this option, you must configure the fields on the **Kerberos Token Profile**
    tab. You can use signing and encryption filters to sign and encrypt the SOAP message using the Kerberos session key.
* **WS-Trust for Simple and Protected Negotiation Protocol (SPNEGO)** – When using the WS-Trust for SPNEGO standard, a series of requests and responses occur between the Kerberos client and the Kerberos service to establish a secure context using WS-Trust and WS-SecureConversation. After establishing the secure context, a further series of requests and responses produce a shared secret key that can be used to sign and encrypt *real* requests to the Kerberos service.If you select this option, it is not necessary to configure the fields on the **Kerberos Token Profile**
    tab, but you must configure the **Kerberos Client** filter as a part of a complicated policy set up to handle the multiple request and response messages involved in establishing the secure context between the Kerberos client and service.

### Configure Kerberos token profile settings

You only need to configure the fields on the **Kerberos Token Profile** tab if you set **Kerberos Standard** to **Web Services Security Kerberos Token Profile 1.1** on the **Kerberos Client**
tab. This tab allows you to configure where to insert the `BinarySecurityToken`
in the SOAP message.

**Where to Place BinarySecurityToken**:
You can insert the `BinarySecurityToken`
inside a named WS-Security Actor/Role in the SOAP message, or you can specify an XPath expression to indicate where the token should be inserted.

To insert the token into a WS-Security element in the SOAP Header element, select **WS-Security Element**. The `BinarySecurityToken`
is inserted into a WS-Security block for the specified actor/role. You can use the default option `Current actor/role only`,
or enter a named actor/role in the field provided.

Alternatively, to use an XPath expression to specify where to insert the `BinarySecurityToken`, select **XPath Location**. Click the **Add**
button to add a new XPath expression, or select an existing XPath expression from the list. You can also edit or delete existing expressions, if needed.

You can control inserting the `BinarySecurityToken` relative to the node pointed to by the XPath expression. Select the **Append**
to insert the token after or **Before** to insert the token before the node.

**BinarySecurityToken Value Type**:
Currently, the only supported `BinarySecurityToken` type is `GSS_Kerberosv5_AP_REQ`. The selected type is specified in the generated `BinarySecurityToken`.
